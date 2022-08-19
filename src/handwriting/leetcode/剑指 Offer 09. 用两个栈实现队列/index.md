##### 剑指 Offer 09. 用两个栈实现队列:
```js
var CQueue = function() {
    this.leftS = []
    this.rightS = []
};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    this.leftS.push(value)

    return null
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    var i = this.leftS.length
    if(i <= 0) return -1
    while(i) {
        if(i === 1) break
        this.rightS.push(this.leftS.pop())
        i--
    }
    var ans = this.leftS.pop()
    while(this.rightS.length) {
        this.leftS.push(this.rightS.pop())
    }

    return ans
};

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```