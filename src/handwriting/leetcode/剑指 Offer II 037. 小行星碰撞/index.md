##### 剑指 Offer II 037. 小行星碰撞
```js
/**
 * @param {number[]} asteroids
 * @return {number[]}
 */
var asteroidCollision = function(asteroids) {

    while(isCan(asteroids)) {
        for(var i = 0; i < asteroids.length; i++) {
            if(asteroids[i] > 0 && asteroids[i+1] < 0) {
                // 会发生碰撞：
                if(Math.abs(asteroids[i]) > Math.abs(asteroids[i+1])) {
                    asteroids.splice(i+1, 1)
                    i--
                } else if(Math.abs(asteroids[i]) < Math.abs(asteroids[i+1])) {
                    asteroids.splice(i, 1)
                    i--
                } else {
                    asteroids.splice(i, 2)
                    i--
                }
            }
        }
    }

    return asteroids
};


function isCan(arr) {
    for(var i = 0; i < arr.length; i++) {
        if(arr[i] > 0 && arr[i+1] < 0) return true
    }

    return false
}
```