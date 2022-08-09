##### 121. 买卖股票的最佳时机
```js
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
  // minIndex 用来存储每一位之前最小值的下标
  var minIndex = []
  minIndex.push(0)
  for(var i = 1; i < prices.length; i++) {
    if(prices[i] < prices[minIndex[i-1]]) minIndex[i] = i
    else minIndex[i] = minIndex[i - 1]
  }

  // 这里是拿到当前位置的值和前面最小值做差，求出最大差值 
  var ans = 0
  for(var i = 0; i < minIndex.length; i++) {
    ans = Math.max(prices[i] - prices[minIndex[i]], ans)
  }

  return ans
};
```