# 과제 3 (난이도 상)

https://github.com/udacity/asteroids 에 들어가 해당 리포지토리를 로컬 머신에 클론 받고 다음 과제를 수행해주세요.

## 버그를 유발한 커밋 찾기

asteroids를 실행하면 우주선과 소행성이 나타납니다. 키보드에서 ←와 →를 누르면 우주선이 가리키는 방향이 바뀌고, ↑를 누르면 우주선이 전진합니다. 스페이스 바를 누르면 로켓이 발사되죠. 

index.html 파일을 열고 게임을 직접 실행해 봅시다.

![asteroids-intro](../resources/asteroids-intro.png)

엇! 그런데 에러가 있네요. 스페이스 바에서 손을 떼지 않고 계속 눌러봅시다. 쉼 없이 총알이 발사됩니다. 

![asteroids-bug](../resources/asteroids-bug.png)

이렇게 끊임없이 총알이 발사되면 게임을 금방 깰 수 있어서 안 됩니다. 처음엔 총알이 끊임없이 발사되지 않도록 구현했었는데, 어디선가 총알이 끊임없이 발사되도록 하는 코드가 들어간 것 같네요.

어떤 커밋때문에 버그가 생겼는지 찾아봅시다. 그리고 버그를 수정하려면 어떻게 해야 하는지 적어봅시다.

### 정답

(여기에 버그를 유발한 커밋의 id와 어떻게 하면 버그를 수정할 수 있는지 적어주세요.)

#### 버그를 유발한 커밋의 id: 25ede83

#### 버그를 수정 방법

가장 최근 커밋(커밋 id: 3884eab)에서 game.js 파일의 코드행 422 ~ 441의 코드는 다음과 같다:
```JavaScript
if (KEY_STATUS.space) {
  if (this.delayBeforeBullet <= 0) {
    for (var i = 0; i < this.bullets.length; i++) {
      if (!this.bullets[i].visible) {
        SFX.laser();
        var bullet = this.bullets[i];
        var rad = ((this.rot-90) * Math.PI)/180;
        var vectorx = Math.cos(rad);
        var vectory = Math.sin(rad);
        // move to the nose of the ship
        bullet.x = this.x + vectorx * 4;
        bullet.y = this.y + vectory * 4;
        bullet.vel.x = 6 * vectorx + this.vel.x;
        bullet.vel.y = 6 * vectory + this.vel.y;
        bullet.visible = true;
        break;
      }
    }
  }
}
```
1. 코드행 423과 424 사이에 코드`this.delayBeforeBullet = 10;`를 추가한다.

수정된 코드는 다음과 같다:
```JavaScript
if (KEY_STATUS.space) {
  if (this.delayBeforeBullet <= 0) {
    this.delayBeforeBullet = 10;
    for (var i = 0; i < this.bullets.length; i++) {
      if (!this.bullets[i].visible) {
        SFX.laser();
        var bullet = this.bullets[i];
        var rad = ((this.rot-90) * Math.PI)/180;
        var vectorx = Math.cos(rad);
        var vectory = Math.sin(rad);
        // move to the nose of the ship
        bullet.x = this.x + vectorx * 4;
        bullet.y = this.y + vectory * 4;
        bullet.vel.x = 6 * vectorx + this.vel.x;
        bullet.vel.y = 6 * vectory + this.vel.y;
        bullet.visible = true;
        break;
      }
    }
  }
}
```
2. 수정된 `game.js`파일 커밋할 목록에 추가 `git add game.js` 한다.
3. 커밋 `git commit`하고 커밋 메세지를 작성하고 저장한다.
4. `git push origin master`해서 깃허브에 커밋들을 밀어넣는다.



### 힌트

과제 2를 통해 커밋도 체크아웃 할 수 있다는 것을 배웠습니다. 이전 커밋을 체크하웃하면 타임머신을 타고 과거로 돌아갈 수 있습니다!
