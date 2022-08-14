##### 674. 最长连续递增序列:
DP 解法:
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findLengthOfLCIS = function (nums) {
  let arr = [1]
  for (let i = 1; i < nums.length; i++) {
    if (nums[i] > nums[i - 1]) arr[i] = arr[i - 1] + 1
    else arr[i] = 1
  }

  return Math.max(...arr)
};
```