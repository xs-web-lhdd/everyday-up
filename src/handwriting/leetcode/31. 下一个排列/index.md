##### 31、下一个排列
acwing 讲解，3 分钟 搞定
```js
/**
 * @description 31-下一个排列
 * @author 氧化氢
 */

/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
 var nextPermutation = function(nums) {
  var k = nums.length - 1
  // 找到后面递减的最大值下标 k
  while(k >= 0 && nums[k-1] >= nums[k]) k--
  
  if(k - 1 < 0) {
    return nums.reverse()
  } else {
    var t = nums.length-1
    while(nums[t] <= nums[k-1]) t--

    var temp = nums[k-1]
    nums[k-1] = nums[t]
    nums[t] = temp
    

    var left = k, right = nums.length - 1
    while(left < right) {
        [nums[left], nums[right]] = [nums[right], nums[left]]
        left++
        right--
    }

    return nums
  }
};
```