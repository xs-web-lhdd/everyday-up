##### 283. 移动零
```js
var moveZeroes = function(nums) {
    let j = 0
    for(let i = 0; i < nums.length; i++) {
        if(nums[i] !== 0) {
            nums[j] = nums[i]
            j++
        }
    }

    for(let k = j; k < nums.length; k++) {
        nums[k] = 0
    }

    return nums
};
```