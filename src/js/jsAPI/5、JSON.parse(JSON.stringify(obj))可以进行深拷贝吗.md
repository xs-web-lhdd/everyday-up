##### JSON.parse(JSON.stringify(obj))有什么不足之处？当object内存在循环引用时会报错吗？
JSON.parse(JSON.stringify(obj))可以实现数组和对象和基本数据类型的深拷贝，但不能处理函数。因为JSON.stringify()方法是将一个javascript值转换我一个JSON字符串，不能接受函数。其他影响如下：

- 如果对象中有时间对象，那么用该方法拷贝之后的对象中，时间是字符串形式而不是时间对象
- 如果对象中有RegExp、Error对象，那么序列化的结果是空
- 如果对象中有函数或者undefined，那么序列化的结果会把函数或undefined丢失
- 如果对象中有NAN、infinity、-infinity，那么序列化的结果会变成null
- JSON.stringfy()只能序列化对象的可枚举自有属性，如果对象中有是构造函数生成的，那么拷贝后会丢弃对象的constructor
- 如果对象中存在循环引用也无法正确实现深拷贝，会报错