# 版权声明
版权是一件很重要的事情，能写一本书或者翻译一本书需要付出艰辛的劳动。刚刚我已经跟机械工业出版社的人联系，欣喜的是他们说可以发布学习笔记，我才贴出来。
虽然贴出来了，但是如果有任何不妥的地方，本人承诺立即删除文章。也希望各位读者能够尊重别人的劳动果实。

# 前言

为什么要读《数据结构和算法》，或者把这个问题推广下为什么要不断的读书呢？是工作做不下去了吗？其实不是的，工作已经稳稳的。是热爱吗？也不是的，其实我心里也很不想读。因为一个很直观的感受，读了这些东西感觉也没啥用，真的，这些知识点不会直接对工作产生任何影响，读了感觉也就是在浪费时间。而且读书确实是一件蛮痛苦的事情。那说了这么多，为什么还要读呢？其实是面试刺激的，找工作经历了一段时间的面试，让自己发现了自己原来是个渣渣，尤其是在算法方面。所以现在要重新读书，把那些自己薄弱的地方补起来，让自己变得更强大。也让自己的视野能够更开阔，看到更大的风景。

# 4 栈

## 4.1 对栈的操作

栈是一种特殊列表，栈内的元素只能通过列表的一段进行访问，这一端称为栈顶。栈是一种后入先出的数据结构。

## 4.2 栈的实现

栈的方法和属性
 

栈的方法和属性 | 含义
---|---
top(属性) | 栈顶的位置
push(方法) | 向栈中压入一个新元素
pop(方法) | 弹出栈顶的一个元素
peek(方法) | 访问栈顶的元素，但是不弹出

```javascript
class Stack {
  constructor() {
    this.dataStore = []
    this.top = 0
  }
  // 向栈顶压入一个元素
  push(element) {
    this.dataStore[this.top++] = element
  }
  // 弹出栈顶元素。原书有错误，原书只是返回栈顶元素并没有弹出，订正如下
  /*
  pop() {
    return this.dataStore[--this.top]
  }
  */
  pop() {
    return this.dataStore.splice(--this.top, 1)
  }
  // 访问栈顶元素
  peek() {
    return this.dataStore[this.top - 1]
  }
  // 清空站栈内所有元素
  clear() {
    this.dataStore = []
    this.top = 0
  }
  // 判断栈是否为空
  empty() {
    return this.dataStore.length == 0
  }
  set length(value) {

  }
  get length() {
    return this.dataStore.length
  }
  toString() {
    return this.dataStore.toString()
  }
}

// 对栈进行测试

let stack = new Stack()

// 测试push方法
stack.push('a')
stack.push('b')
stack.push('c')

console.log(stack) // ["a", "b", "c"]

// 测试pop方法
console.log(stack.pop())
console.log(stack) // ["a", "b"]

// 测试length属性
console.log(stack.length) // 2

// 测试peek方法
console.log(stack.peek()) // b
```

## 4.3 使用Stack类

栈的使用还是蛮简单的，这一节举了几个使用栈的例子，不过有些例子看不懂啥意思，涉及到数学知识了。我把看得懂的例子举一个出来。大家看下，知道栈的用法即可。

回文是指这样一种现象：一个单词、短语或者数字，从前往后看或者从后往前看都是一样的。其实就是左右对称。比如dad racecar 就是回文

判断是否是回文

```javascript
function isPalindrome(word) {
  var s = new Stack()
  for (let i = 0; i < word.length; i++) {
    s.push(word[i])
  }
  var rword = ''
  while (s.length > 0) {
    rword += s.pop()
  }
  if (word == rword) {
    return true
  } else {
    return false
  }
}

console.log(isPalindrome('hello')) // false
console.log(isPalindrome('dad')) // true
```

**个人总结：栈是相对容易的一种数据结构，在生活中似乎用户也蛮多的，可以学习下。另外为作者js编程能力捉急**




