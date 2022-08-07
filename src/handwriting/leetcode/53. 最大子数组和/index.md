##### 53. 最大子数组和

###### dp:

```js
/**
 * @description 53-最大子数组和
 * @author 氧化氢
 */

/**
 * @param {number[]} nums
 * @return {number}
 */
 var maxSubArray = function(nums) {
    var arr = [nums[0]]
    for(var i = 1; i < nums.length; i++) {
        arr[i] = Math.max(arr[i - 1] + nums[i], nums[i])
        // if(arr[i-1] > 0) arr[i] = arr[i-1] + nums[i]
    	// else arr[i] = nums[i]
    }

    return Math.max(...arr)
};
```

###### 前缀和：

```js
/**
 * @description 53-最大子数组和
 * @author 氧化氢
 */

/**
 * @param {number[]} nums
 * @return {number}
 */
 var maxSubArray = function(nums) {
  var pre = [0]
  for(var i = 1; i <= nums.length; i++) {
    pre[i] = pre[i - 1] + nums[i - 1]
  }

  pre.shift()
  
  var min = 0
  var ans = Number.MIN_SAFE_INTEGER
  for(var num of pre) {
    ans = Math.max(ans, num - min)
    min = Math.min(num, min)
  }

  return ans
};
```

