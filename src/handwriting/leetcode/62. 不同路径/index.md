##### 62. 不同路径:
```js
/**
 * @description 62-不同路径 动态规划 KO 解法
 * @author 氧化氢
 */

/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
 var uniquePaths = function(m, n) {
  let arr = []
  for(let i = 0; i < m; i++) {
      arr.push(new Array(n).fill(0))
  }
  for(let i = 0; i < m; i++) {
      for(let j = 0; j < n; j++) {
          if(!i && !j) arr[i][j] = 1
          else {
              // 从上边往下走:
              if(i) arr[i][j] += arr[i-1][j]
              // 从左边往右走:
              if(j) arr[i][j] += arr[i][j-1]
          }
      }
  }

  return arr[m-1][n-1]
};
```