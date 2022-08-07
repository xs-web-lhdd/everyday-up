##### 165. 测试版本号

```js
/**
 * @description 165-比较版本号
 * @author 氧化氢
 */

/**
 * @param {string} version1
 * @param {string} version2
 * @return {number}
 * @solution 官方题解就很通俗易懂！
 */
var compareVersion = function (version1, version2) {
  var l1 = version1.split('.')
  var l2 = version2.split('.')
  for (var i = 0, j = 0; i < l1.length || j < l2.length; i++, j++) {
    var c1 = l1[i] ? Number(l1[i]) : 0
    var c2 = l2[j] ? Number(l2[j]) : 0
    if (c1 < c2) return -1
    if (c1 > c2) return 1
  }

  return 0
};

export default compareVersion
```

