##### 209. 长度最小的子数组

```js
/**
 * @param {number} target
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(target, nums) {
    var left = 0, right = 0, sum = 0, res = Number.MAX_SAFE_INTEGER
    for(var right = 0; right < nums.length; right++) {
        sum += nums[right]
        while(sum >= target) {
            res = Math.min(res, right - left + 1)
            sum -= nums[left]
            left++
        }
    }

    return res > nums.length ? 0 : res
};
```

