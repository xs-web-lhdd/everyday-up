### 1、cvte 算法题？
题目：目前存在一批任务，这些任务不存在循环依赖，要求实现一个函数，得到一批任务最长耗时，并给任务执行顺序？
例如：
```js
const arr = [{
    id: '1',
    cost: 100
  },
  {
    id: '2',
    cost: 100,
    dep: '4'
  },
  {
    id: '4',
    cost: 100,
    dep: '3'
  },
  {
    id: '3',
    cost: 100,
  }
]

// 输出：
300
3 4 2
```




```js
function getTime(arr) {
  const ans = []
  for (let i = 0; i < arr.length; i++) {
    const task = arr[i]
    if (task.dep) {
      const queue = []
      let dep = arr[i].id
      let costSum = 0
      let flag = 1
      while (flag) {
        arr.forEach((element, index) => {
          if (element.id === dep) {
            queue.unshift(element.id)
            costSum += element.cost
            if (element.dep) flag = 1, dep = element.dep
            else flag = 0
            arr.splice(index, 1)
          }
        });
      }
      ans.push({
        id: queue.join(','),
        cost: costSum
      })
    } else {
      ans.push(arr[i])
      arr.splice(i, 1)
      i--
    }
  }

  return ans
}

const arr = [{
    id: '1',
    cost: 100
  },
  {
    id: '2',
    cost: 100,
    dep: '4'
  },
  {
    id: '4',
    cost: 100,
    dep: '3'
  },
  {
    id: '3',
    cost: 100,
  }
]

console.log(getTime(arr));
```