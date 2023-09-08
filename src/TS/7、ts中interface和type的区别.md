### ts 中 interface 和 type 的区别？

1、interface 只能定义对象类型，type 可以声明任何类型、基础类型、联合类型、交叉类型。
2、定义两个同名的 type 会报异常，interface 定义两个同名的 interface 会合并。
3、interface 可以使用 extends、implements 进行扩展
