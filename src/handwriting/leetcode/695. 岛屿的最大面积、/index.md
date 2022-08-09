##### 695. 岛屿的最大面积
```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxAreaOfIsland = function (grid) {
  var size = grid.length
  var length = grid[0].length

  var dirs = [
    [0, 1],
    [0, -1],
    [1, 0],
    [-1, 0]
  ]

  var arr = []
  var area = 0
  for (var i = 0; i < size; i++) {
    arr.push(new Array(length).fill(0))
  }

  var ans = 0
  for (var i = 0; i < size; i++) {
    for (var j = 0; j < length; j++) {
      if (arr[i][j]) continue
      if (grid[i][j]) {
        dfs(i, j, grid, arr)
        ans = Math.max(ans, area)
        area = 0
      }
    }
  }


  function dfs(x, y, grid, arr) {
    area++
    arr[x][y] = 1
    for (let k = 0; k < 4; k++) {
      let i = x + dirs[k][0]
      let j = y + dirs[k][1]
      if (i < 0 || i >= size) continue
      if (j < 0 || j >= length) continue
      if (arr[i][j]) continue
      if (grid[i][j]) {
        arr[i][j] = 1
        dfs(i, j, grid, arr)
      }
    }
  }

  return ans
};
```