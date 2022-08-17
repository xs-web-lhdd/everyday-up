##### 24、padding 对行内元素有效果吗？
- 行内元素同样具有盒子模型
- 行内元素的margin-left、margin-right和padding-left、padding-right属性设置都是有效的
- 行内元素的margin-top、margin-bottom和padding-top、padding-bottom属性设置是无效的，但是必须注意的是，对于padding-top和padding-bottom的设置，从显示效果上来看是增加的，但其实设置是无效的，因为它们没有撑大盒子，并不会对周围的元素产生影响。
- 特别要注意，img是一个特例，它虽然是行内元素，但它的性质不同于行内元素。对于img设置padding和margin都是有效的。