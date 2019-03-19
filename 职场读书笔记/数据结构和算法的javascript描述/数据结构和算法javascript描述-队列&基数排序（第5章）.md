
# 5 队列

队列是一种队列，不同的是队列只能从队尾插入元素，在队首删除元素，队列用于存储按顺序排列的数据，先进先去

## 5.1 队列的方法和属性


队列的方法和属性 | 含义
---|---
enqueue(方法) | 在队列尾部插入一个元素
dequeue(方法) | 在队列头部删除一个元素
front(方法) | 返回队列头部的元素
back(方法) | 返回队列尾部的元素
toString(方法) | 返回队列的字符串形式
empty(方法) | 判断一个队列是否为空
length(属性) | 返回队列的长度

## 5.2 实现一个队列

```javascript
class Queue {
  constructor() {
    this.dataStore = []
  }
  enqueue(ele) {
    this.dataStore.push(ele)
  }
  dequeue() {
    return this.dataStore.shift()
  }
  front() {
    return this.dataStore[0]
  }
  back() {
    return this.dataStore[this.dataStore.length - 1]
  }
  toString() {
    return this.dataStore.join(' ')
  }
  empty() {
    return this.dataStore.length == 0
  }
}

// 测试
let q = new Queue()
q.enqueue('a')
q.enqueue('b')
q.enqueue('c')
console.log(q.toString()) // a b c
q.dequeue()
console.log(q.toString()) // b c
console.log('front:', q.front()) // front: b
console.log('back:', q.back()) // back: c

```

## 5.3 基数排序

基数排序又叫桶排序，是一种数值排序的方法。从低位开始将待排序的数按照这一位的值放到相应的编号为0~9的桶中。等到低位排完得到一个子序列，再将这个序列按照次低位的大小进入相应的桶中，一直排到最高位为止，数组排序完成。



假设有以下一串数字，我们对起进行基数排序.**基数排序要求数值的位数必须相同，不同的话，数值左边补0即可**

43 21 35 24 76 52 23 44


### 5.3.1 按照个位数把数值放入不同的桶中


基数 | 数值 | 数值
---|---|---
0 | 
1 | 21
2 | 52 
3 | 43 | 23
4 | 24 | 44
5 | 35
6 | 76
7 | 
8 | 
9 | 

从上到下得到一个排序后的数值。 
21 52 43 23 24 44 35 76

个人理解：个位数排序的意义是保证了如果2个数值十位数相同，那么他们肯定个数数值小的数在前面

### 5.3.2 按照十位数把数值放入不同的桶中

基数 | 数值 | 数值 | 数值
---|---|---|---
0 | 
1 | 
2 | 21 | 23 |24 
3 | 35 | 
4 | 43 | 44
5 | 52
6 | 
7 | 76
8 | 
9 | 

从上到下得到排序后的数值：21 23 24 35 43 44 52 76


21 52 43 23 24 44 35 76 经过基数排序后结果如下
21 23 24 35 43 44 52 76

**自此我们使用基数排序法完成了对数值的排序**

### 5.3.3 基数排序的类型

基数排序有2种类型

- LSD-从低位向高位排
- MSD-从高位向低位排

两个排序方法原理都一模一样，LSD 适用于数字位数比较少的数值排序，MSD适用于数值位数比较多的数值排序

## 5.4 使用队列实现基数排序

```javascript
let nums = [43, 21, 35, 24, 76, 52, 23, 44]

function createQueueArr() {
  let queueArr = []
  for (let i = 0; i < 10; i++) {
    queueArr[i] = new Queue()
  }
  return queueArr
}
/**
 * @description 对数组中的数值按照基数排序的方法分配到不同的桶子中
 * @param {Array} nums 需要排序的数字组成的数组 
 * @param {Array} queueArr 10个队列，相当于10个桶子，分别代表基数0-9
 * @param {Number} digit 需要按哪位进行排序，1是对位个位 10对应10位
 */
function distribute(nums, queueArr, digit) {
  for (let i = 0; i < nums.length; i++) {
    if (digit == 1) {
      queueArr[nums[i] % 10].enqueue(nums[i])
    } else {
      let tempNum = Math.floor(nums[i] / 10)
      queueArr[tempNum].enqueue(nums[i])
    }
  }
}
/**
 * @description 对基数排序后数值从上到下进行收集
 * @param {Array} queueArr 队列
 */

function collect(queueArr) {
  let tempArr = []
  for (let i = 0; i < queueArr.length; i++) {
    while (!queueArr[i].empty()) {
      tempArr.push(queueArr[i].dequeue())
    }
  }
  return tempArr
}

// 对基数排序进行测试
let queueArr = createQueueArr()
distribute(nums, queueArr, 1)
console.log(queueArr)  // [Queue, Queue, Queue, Queue, Queue, Queue, Queue, Queue, Queue, Queue]

let sortNum1 = collect(queueArr)
console.log(sortNum1) // [21, 52, 43, 23, 24, 44, 35, 76]

let queueArr2 = createQueueArr()
distribute(sortNum1, queueArr2, 10)

let sortNum2 = collect(queueArr2)
console.log(sortNum2) // [21, 23, 24, 35, 43, 44, 52, 76]
```
测试结果更预想的一样。说明我们使用队列成功实现了基数排序。

## 5.5 优先队列

在一般情况下，从队列中删除元素，一定是率先入队的元素，但是也有一些使用队列的应用，在删除元素的时候不必遵循先进先出的规则。而是根据优先级大小决定那个先出。比如去银行办理业务，vip的人总是比我们这些屌丝优先。

一个优先队列跟一个普通的队列的主要区别是dequeuq方法

优先队列的dequeue方法如下。

```javascript
// code 存储优先级的大小，越小越优先
function dequequ(){
  let entry = 0
  for(let i=0;i<this.dataStore.length;i++){
    if(this.dataStore[i].code<this.dataStore[entry].code){
      entry = i
    }
  }
  return this.dataStore.splice(entry,1)
}
```
自此关于队列的所有知识讲解完毕。

**个人总结：队列也是列表的一种特殊形式，比较简单，在生活中的用途似乎也蛮多的。值得学习下。队列中涉及到的基数排序方法很有趣的，也蛮难理解。值得好好研究。**










