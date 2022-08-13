##### 136. 只出现一次的数字:
```js
/**
 * @description 136-只出现一次的数字
 * @author 氧化氢
 */

/**
 * @param {number[]} nums
 * @return {number}
 */
 var singleNumber = function(nums) {
  var ans = 0
  for(var i = 0; i < nums.length; i++) {
    ans ^= nums[i]
  }

  return ans
};
```