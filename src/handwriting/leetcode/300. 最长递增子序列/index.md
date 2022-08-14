##### 300. 最长递增子序列:
```js
/**
 * @description 300. 最长递增子序列
 * @author 氧化氢
 */

/**
 * @param {number[]} nums
 * @return {number}
 */
 var lengthOfLIS = function(nums) {
    var arr = new Array(nums.length).fill(1)

    for(var i = 1; i < nums.length; i++) {
        for(var j = 0; j < i; j++) {
            if(nums[j] < nums[i]) arr[i] = Math.max(arr[j] + 1, arr[i])
        }
    }


    return Math.max(...arr)
};
```