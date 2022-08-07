##### 46. 全排列

```js
/**
 * @description 46-全排列
 * @author 氧化氢
 */

/**
 * @param {number[]} nums
 * @return {number[][]}
 */
 var permute = function(nums) {
  var length = nums.length
  var ans = []

  function dfs(nums, arr) {
    if(arr.length === length) {
        ans.push(arr)
        return
    }
    for(var i = 0; i < nums.length; i++) {
        var target = arr.slice()
        target.push(nums[i])
        var newNums = nums.slice(0, i).concat(nums.slice(i+1))
        dfs(newNums, target)
    }
  }

  dfs(nums, [])

  return ans
};
```

