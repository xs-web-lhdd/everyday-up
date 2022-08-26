### 74. 搜索二维矩阵：
```js
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function(matrix, target) {
    var length = matrix.length, size = matrix[0].length
    var i = 0; j = length - 1

    while(i < size && j >= 0) {
        if(matrix[j][i] > target) j--
        else if(matrix[j][i] < target) i++
        else return true
    }

    return false
};
```