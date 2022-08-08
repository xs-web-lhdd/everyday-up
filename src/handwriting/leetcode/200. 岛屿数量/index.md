##### 200. 岛屿数量

```js
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    var size = grid.length
    var length = grid[0].length
    var count = 0
    
    var fArr = []
    for(var i = 0; i < size; i++) {
        fArr.push(new Array(length).fill(0))
    }

    var dirs = [
        [0, 1],
        [0, -1],
        [1, 0],
        [-1, 0]
    ]
    function dfs(x, y, grid, fArr) {
        for(let k = 0; k < 4; k++) {
            let i = x + dirs[k][0]
            let j = y + dirs[k][1]
            if(i < 0 || i >= size) continue
            if(j < 0 || j >= length) continue
            if(fArr[i][j]) continue
            if(grid[i][j] === '0') continue
            fArr[i][j] = 1
            dfs(i, j, grid, fArr)
        }
    }

    for(let i = 0; i < size; i++) {
        for(let j = 0; j < length; j++) {
            if(fArr[i][j] === 1) continue
            if(grid[i][j] === '1') {
                count++
                dfs(i, j, grid, fArr)
            }
        }
    }

    return count
};
```

