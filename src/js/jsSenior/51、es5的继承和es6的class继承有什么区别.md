### es5的继承和es6的class继承有什么区别？
ES5的继承时通过prototype或构造函数机制来实现。ES5的继承实质上是`先创建子类的实例对象，然后再将父类的方法添加到this上`（Parent.apply(this)）。

ES6的继承机制完全不同，实质上是`先创建父类的实例对象this（所以必须先调用父类的super()方法），然后再用子类的构造函数修改this。`

具体的：ES6通过class关键字定义类，里面有构造方法，类之间通过extends关键字实现继承。子类必须在constructor方法中调用super方法，否则新建实例报错。因为子类没有自己的this对象，而是继承了父类的this对象，然后对其进行加工。如果不调用super方法，子类得不到this对象。

ps：super关键字指代父类的实例，即父类的this对象。在子类构造函数中，调用super后，才可使用this关键字，否则报错。


### es6 类的原理？


### es6 中 static 的 this 指向？