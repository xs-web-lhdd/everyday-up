##### 179. 最大数
```js
/**
 * @description 179. 最大数
 * @author 氧化氢
 */
var largestNumber = function(nums) {
	// 从大到小排序
    let arr = nums.sort((a,b)=>{
        return `${b}${a}`-`${a}${b}`
    })
	// 下面的代码是为了去掉数字开头的0。避免类似[0,0]这样的用例报错。
    let i = 0
    while(arr[i]==0 && i!==arr.length-1) {
        i++
    }
    return arr.slice(i).join('')
};
```