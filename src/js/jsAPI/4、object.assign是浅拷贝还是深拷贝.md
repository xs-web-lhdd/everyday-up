##### 4、object.assign 是深拷贝还是浅拷贝：
Object.assign()方法可以把源对象自身的任意多个的可枚举属性拷贝给目标对象，然后返回目标对象，但是Object.assign()进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。此方法对于Array和Object均可适用。Array.prototype.concat()和Array.prototype.slice()也为浅拷贝，只适用于Array。`展开运算符`也是浅拷贝。

- splice 进行删除时的元素数组也是浅拷贝


Object.assign 第一参是目标对象，返回的就是目标元素拷贝完的对象，第二参数后面是其他对象