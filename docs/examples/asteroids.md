# asteroids

```typescript
const spaceship = sprites.create(img`
 . . . . d
 . . . . d
 . . . d d d
 . . d d d d d
 . d a d a d a d
 d a a a a a a a d
 d a a a a a a a d
 `)
spaceship.y = 110

const meteor = img`
. . . 8 8 8 
. . 8 8 1 8 8 8
. 8 8 1 1 8 8 8 8
. . 8 8 8 8 1 1 8 8
8 8 8 8 8 8 1 1 8
. 8 8 8 8 8 8 8 8
. . 8 8 1 8 8 8
. . . 8 8 8 
`

const meteorDeath = [
    img`
. . . 8 8 8 
. . . 8 1 8 . 8
. 8 8 . 1 . 8 8 8
. . 8 8 . 8 1 1 8 8
8 8 8 . 8 . 1 1 8
. 8 . 8 8 8 . 8 8
. . 8 8 1 8 8 .
. . . 8 8 8 
`,
    img`
. . . 8 8 8 
. . . 8 1 8 . .
. 8 8 . . . . 8 8
. . 8 . . . 1 1 8 8
8 8 8 . . . . 1 8
. . . . . . . . .
. . . 8 1 8 . .
. . . 8 8 8 
`,
    img`
. . . . 8 . 
. . . . . . . .
. 8 . . . . . . .
. . . . . . . . . 8
. . . . . . . . .
. . . . . . . . .
. . . . . . . .
. . . . 8 . 
`]


const rocketImg = img`
. 3 
. 3
3 3 3
3 3 3
`

spaceship.onOverlap(function (other: Sprite) {
    game.over()
})
spaceship.z = 10

game.update(function () {
    spaceship.x += keys.dx(70)
    spaceship.x = Math.clamp(10, 118, spaceship.x)

    // metero
    if (Math.random() < 0.05) {
        let m = sprites.createProjectile(meteor, 0, Math.randomRange(30, 80))
        m.x = Math.randomRange(10, 140)
    }
    // stars
    if (Math.random() < 0.1) {
        let m = sprites.createProjectile(img`1`, 0, 40)
        m.x = Math.randomRange(0, 128)
        m.life = Math.randomRange(100, 120)
        m.setFlag(SpriteFlag.Ghost, true);
    }
    let now = control.millis()
    if (keys.A.wasPressed()) {
        let r = sprites.createProjectile(rocketImg, 0, -90)
        r.x = spaceship.x
        r.y = spaceship.y - 10
        r.onOverlap(function (other: Sprite) {
            other.destroy()
            const o = sprites.createWithAnimation(meteorDeath)
            o.x = other.x;
            o.y = other.y;
            o.vy = -5;
            o.life = 20;
            o.setFlag(SpriteFlag.Ghost, true)
            info.changeScoreBy(1)
        })
    }
})
```
