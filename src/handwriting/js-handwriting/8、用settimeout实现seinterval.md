##### 通过 setTimeout 实现 setInterval
```js
/**
 * @description 通过 setTimeout 实现 setInterval
 * @author 氧化氢
 */

let timeMap = {}
let id = 0 // 简单实现id唯一
const mySetInterval = (cb, time) => {
  let timeId = id // 将timeId赋予id
  id++ // id 自增实现唯一id，为每一次创建的定时器都定义一个唯一的编号（id），这样就可以在全局多次定义 setInterval 而不是只能定义一个
  let fn = () => {
    cb()
    timeMap[timeId] = setTimeout(() => {
      fn()
    }, time)
  }
  timeMap[timeId] = setTimeout(fn, time)
  return timeId // 返回timeId
}
 
const myClearInterval = (id) => {
  clearTimeout(timeMap[id]) // 通过timeMap[id]获取真正的id
  delete timeMap[id]
}
 
```

```js
/**
 * @description 通过 setTimeout 实现 setInterval
 * @author 氧化氢
 */


let idMap = {}
let id = 0

function mySetInterval(fn, timer) {
  let timeId = id
  id++
  function func() {
    fn()
    idMap[timeId] = setTimeout(func, timer)
  }

  idMap[timeId] = setTimeout(func, timer)
  return timeId
}


function myClearInterval(id) {
  clearTimeout(idMap[id])
  delete idMap[id]
}
```