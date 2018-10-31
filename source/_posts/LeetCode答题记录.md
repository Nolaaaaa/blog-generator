---
title: LeetCode答题记录
date: 2018-10-30 22:54:10
tags: JavaScript
categories: 前端
---

这篇主要是整理为了在LeetCode上的答题记录，方便以后有兴趣时重新码时顺便看一下自己以前是多么的愚蠢哈哈
<escape><!-- more --></escape>

### 504 Base 7
[题目链接](https://leetcode.com/problems/base-7/description/)
```JavaScript
var convertToBase7 = function(num) {
    // num为0，直接返回0
    if(num === 0) return "0"
    
    // num大于0，取num的绝对值
    nums = num > 0? num:-num
    
    // 算出来的结果存到数组
 var result = []
    while(nums > 0){
        res = nums % 7
        nums = parseInt(nums)/7
        nums = parseInt(nums)
        result.unshift(String(res))
    }
    
    // 判断num的符号，是负数就加上负号
    if(Math.sign(num) === -1) {
        result.unshift('-')
    }
    
    // return字符串形式
    return result.join('')
}
```

### 859 Buddy Strings
[题目链接](https://leetcode.com/problems/buddy-strings/description/)
```JavaScript
var buddyStrings = function(A, B) {
    // 两个字符串长度不一致，返回false
    if(A.length !== B.length) return false
    
    // 数组长度小于两个，返回false
    if(A.length < 2) return false
    
    // 变成数组
    A.split()   
    B.split()
    
    // 两个数组相等时
    if(String(A) === String(B)) {
        // 数组长度只有两个，A中的两个值相等，返回true
        if(A[0] === A[1] && A.length === 2) {
            return true
        }
        // A中有重复的值得话，返回true
        if(Array.from(new Set(A)).length < A.length) {
            return true
        }
        return false
    }
    
    // 设两个数组分别存A中和B中不一样的部分
    var tempA = []
    var tempB = []
    for(let i = 0; i < A.length; i++) {
        if(A[i] !== B[i]) {
            tempA.push(A[i])
            tempB.push(B[i])
        }
    }
    // 不同的数字超过两个就返回false
    if(tempA.length > 2)  return false
    
    // 如果A、B不一样的部分排序后还是一样，返回true
    if(tempA.sort().join('') === tempB.sort().join('')) {
        return true
    }else{
        return false
    }
}
// 以上代码测试成功。传说中的送分题，我挂了6次才success，还有谁？
```

### 438 Find All Anagrams in a String
[题目链接](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
```JavaScript
var findAnagrams = function(s, p) {
  // 把字符串变成数组
  s = s.split('')
  p = p.split('')

  // 把p数组排序并变成字符串，方便后面比较
  var strP = p.sort().join('') 

  // 定义result存最终的结果
  var result = []
  p.forEach(function(value, index) {
    // 遍历p数组
      s.forEach(function(val, ind) {
          // 遍历s，如果p中的值在s中匹配成功
          if(value === val) {
              // 设一个临时数组temp存匹配的数字的后面几个（长度和p的长度一样）
              let temp = [] 
              for (let i = 0; i < p.length; i++) {
                temp.push(s[ind + i])
              }
              // 把临时数组temp排序并变成字符串
              let strTemp = temp.sort().join('') 

              // 比较p数组和临时数组temp中的内容是否一样，一样就把对应的s数组中的指针存到result中
              if(strTemp === strP) {
                result.push(ind)
              }
          }
      })
  })
  // 把result数组去重并重新排序，sort默认是按unicode码顺序排列，数字排序有问题，需添加方法
  result = Array.from(new Set(result)).sort(function(a,b) {return a - b})
  return result
}
// 以上代码运行结果：超时
```

### 7 Reverse Integer
[题目链接](https://leetcode.com/problems/reverse-integer/)
```JavaScript
var reverse = function(x) {
  let num = parseInt(x.toString().split('').reverse().join(''))
  if (num > Math.pow(2,31) - 1) return 0;
  return Math.sign(x) * num
}
// Math.sign() 函数返回一个数字的符号, 指示数字是正数，负数还是零
// 以上代码测试成功
```

### 707 Design Linked List 
[题目链接](https://leetcode.com/problems/design-linked-list/)
```JavaScript
var MyLinkedList = function() {
  this.arr = []
}
MyLinkedList.prototype.get = function(index) {
  if(index < this.arr.length && index > -1){
    return this.arr[index]
  }else{
    return -1
  }
}
MyLinkedList.prototype.addAtHead = function(val) {
  this.arr.unshift(val)
  return this.arr
}
MyLinkedList.prototype.addAtTail = function(val) {
  this.arr.push(val)
  return this.arr
}
MyLinkedList.prototype.addAtIndex = function(index, val) {
  if(index <= this.arr.length && index > -1) { 
    this.arr.splice(index, 0, val)  
    return this.arr
  }else{
    return
  }
}
MyLinkedList.prototype.deleteAtIndex = function(index) {
  if(index < this.arr.length && index > -1){
    this.arr.splice(index, 1)
    return  this.arr 
  }else{
    return 
  }
}
// 以上代码测试成功，但、好像、似乎...并不是链表
```

### 665 Non-decreasing Array
[题目链接](https://leetcode.com/problems/non-decreasing-array/)
```JavaScript
var checkPossibility = function(nums) {
  // 前一个<=后一个，存一个"true"，反之存一个"false"到result数组中
  var result = []
  nums.forEach(function(value,index) {
    if(nums[index] <= nums[index + 1]) {
      result.push("true")
    }else{
      result.push("false")
    }
  })
  // 删除result数组的最后一个
  result.pop()
  // falseArr数组存result数组中"false"值的index
  var falseArr = [];
  var idx = result.indexOf("false");
  while (idx != -1) {
    falseArr.push(idx);
    idx = result.indexOf("false", idx + 1);
  }
  // 判断
  if(falseArr.length === 1) {
      let ind = falseArr[0]
      if(nums[ind] <= nums[ind + 2] || nums[ind - 1] <= nums[ind + 1] || ind === 0 || ind === result.length - 1) {
          return true
      }else{
          return false
      }
  }else if(falseArr.length < 1){
    return true
  }else{
      return false
  }
}

// 以上代码测试成功，但，逻辑好像还够不完善
```