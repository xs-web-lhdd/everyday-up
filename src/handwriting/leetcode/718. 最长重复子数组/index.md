##### 718. 最长重复子数组:
```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findLength = function(nums1, nums2) {
    var ans = 0
    // B 不动，A 移动，然后逐个进行比较：
    for(let i = 0; i < nums1.length; i++) {
        let arr = nums1.slice(i)
        ans = Math.max(ans, getMaxArr(arr, nums2))
    }
    // B 动，A 不动，然后逐个进行比较：
    for(let i = 0; i < nums2.length; i++) {
        let arr = nums2.slice(i)
        ans = Math.max(ans, getMaxArr(arr, nums1))
    }

    // 为什么要 A、B 都移动一次呢？看下面的测试用例：
    // [1, 0, 0, 0] [0, 0, 0, 1]

    return ans

    function getMaxArr(arr, min) {
        let flagArr = []
        let ans = 0
        for(let i = 0, j = 0; i < arr.length && j < min.length; i++, j++) {
            if(arr[i] === min[j]) ans++
            else {
                flagArr.push(ans)
                ans = 0
            }
        }

        flagArr.push(ans)

        return Math.max(...flagArr)
    }
};
```