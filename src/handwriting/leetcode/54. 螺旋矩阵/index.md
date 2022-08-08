##### 54. 螺旋矩阵

```js
/**
 * @description 54-螺旋矩阵
 * @author 氧化氢
 */

/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
 var spiralOrder = function(matrix) {
  var length =  matrix.length
  var size = matrix[0].length
  var arr = []
  for(var i = 0; i < length; i++) {
    arr.push(new Array(size).fill(0))
  }

  var ans = []
  var direction = 'right'
  var i = 0, j = 0
  ans.push(matrix[i][j])
  arr[i][j] = 1
  while(ans.length < length * size) {
    if(direction === 'right') {
      if(j < size - 1) {
        if(!arr[i][j+1]) j++
        else {
          direction = 'down'
          i++
        }
      }
      else if(j === size - 1) {
        direction = 'down'
        i++
      }
    }
    else if(direction === 'down') {
      if(i < length - 1) {
        if(!arr[i+1][j]) i++
        else {
          direction = 'left'
          j--
        }
      }
      else if(i === length - 1) {
        direction = 'left'
        j--
      }
    }
    else if(direction === 'left') {
      if(j > 0) {
        if(!arr[i][j-1]) j--
        else {
          direction = 'up',
          i--
        }
      }
      else if(j === 0) {
        direction = 'up'
        i--
      }
    }
    else if(direction === 'up') {
      if(i > 0) {
        if(!arr[i-1][j]) i--
        else {
          direction = 'right',
          j++
        }
      }
    }
    ans.push(matrix[i][j])
    arr[i][j] = 1
  }

  console.log(ans);

  return ans
};
```

