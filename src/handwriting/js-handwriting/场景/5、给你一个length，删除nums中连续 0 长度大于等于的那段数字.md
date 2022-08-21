##### 5、删除 nums 中连续 0 长度大于 length 的数字段：
例子：
```js
//删除前： [1, 2, 3, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 3, 0]
//删除后： [1, 2, 3, 1, 0, 1, 3, 0]
```

代码：
```js
let arr = [1, 2, 3, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 3, 0]

function deleteZero(nums, length) {
  let arr = []
  if (arr[0] === 0) {
    arr.push({
      start: 0,
      end: 0,
      length: 1,
    })
  } else {
    arr.push({
      start: -1,
      end: -1,
      length: 0
    })
  }
  // 通过 DP 计算出每一位前面最长 0 的情况（包括自己）
  for (let i = 1; i < nums.length; i++) {
    if (nums[i] === 0) {
      if (arr[i - 1].length !== 0) {
        arr[i] = {
          start: arr[i - 1].start,
          end: i,
          length: arr[i - 1].length + 1
        }
      } else {
        arr[i] = {
          start: i,
          end: i,
          length: 1
        }
      }
    } else {
      arr[i] = {
        start: -1,
        end: -1,
        length: 0
      }
    }
  }

  // 将 nums 中符合条件的 0 数字段全部替换为 'xxx'
  for (let i = arr.length - 1; i >= 0;) {
    if (arr[i].length >= length) {
      for (let j = arr[i].start; j <= arr[i].end; j++) {
        nums[j] = 'xxx'
      }
      i = i - (arr[i].end - arr[i].start + 1)
    } else {
      i--
    }
  }

  // 进行删除：
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === 'xxx') nums.splice(i, 1), i--
  }

  return nums
}

console.log(deleteZero(arr, 3));
```