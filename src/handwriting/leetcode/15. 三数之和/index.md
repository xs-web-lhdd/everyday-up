##### 15. 三数之和
```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
    nums = nums.sort((a, b) => a - b)
    var ans = []
    for(let i = 0; i < nums.length; i++) {
        var ans1 = nums[i]
        var arr = nums.slice(0, i).concat(nums.slice(i+1))
        for(let i = 0, j = arr.length - 1; i < j;) {
            if(ans1 + arr[i] + arr[j] === 0) {
                var item = [arr[i],arr[j],ans1].sort((a, b) => a - b).join(',')
                if(!ans.includes(item)) ans.push(item)
                i++
            }
            else if(ans1 + arr[i] + arr[j] < 0) {
                i++
            }
            else {
                j--
            }
        }
    }

    ans = [...new Set(ans)]
    var res = []
    ans.forEach(item => {
        console.log(item)
        res.push(item.split(','))
    })


    return res
};
```