##### 3、slice、substr、substring的区别：
[CSDN](https://blog.csdn.net/qq_40662765/article/details/106037117)

**总结：**
- slice、substring第二个参数是结束的下标（不包括该位置），substr第二个参数是截取的长度，他们三个如果第参数都是大于字符串长度的那么都相当于第二个参数没传
- slice 第二个参数大于第一个参数时返回空字符串，substring 第二个参数大于第一个参数时相当于两个参数互换位置
- slice 第二个参数小于 0 时等于 length + 第二个参数，substring 第二个参数小于 0 时等于第二个参数是 0，substr 第二个参数小于 0 时返回空串
- slice 可以截取数组，但是 substring substr 就不可以