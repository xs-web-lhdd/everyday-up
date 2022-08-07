##### 88. 合并两个有序数组

###### 方法一：

```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    if(!n) return
    for(let num of nums2) {
        nums1.push(num)
    }
    nums1.sort((a, b) => a - b)

    for(var i = 0; i < nums1.length; i++) {
        if(nums1[i] === 0) break
    }

    nums1.splice(i, n)

    return nums1
};
```

###### 方法二：

```js
/**
 * @description 88 合并两个有序数组
 * @author 氧化氢
 */

/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
 var merge = function(nums1, m, nums2, n) {
  for(var i = m; i < m+n; i++) {
    nums1[i] = nums2[i-m]
  }
  nums1.sort((a, b) => a - b)
};
```

