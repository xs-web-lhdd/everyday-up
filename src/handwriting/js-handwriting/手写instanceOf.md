```js
function instanceOf(child, father) {

  if((typeof child !== 'object' && typeof child !== 'function') || child === null) return false

  let fp = father.prototype
  let cp = child.__proto__
  while(cp) {
    if(cp === fp) return true
    cp = cp.__proto__
  }

  return false
}
```

