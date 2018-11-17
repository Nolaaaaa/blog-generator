---
title: LeetCode答题记录 201811
date: 2018-10-30 22:54:10
tags: JavaScript
categories: 前端
---

LeetCode上的答题记录，方便日后来围观这个愚蠢的我哈哈
<escape><!-- more --></escape>
### 53 Maximum Subarray
给定整数数组nums，找到具有最大总和并返回其总和的连续子数组（包含至少一个数字）。[题目链接](https://leetcode.com/problems/maximum-subarray/description/)
Exp 1:  `Input: [-2,1,-3,4,-1,2,1,-5,4]  Output: 6`
Explanation: [4,-1,2,1] has the largest sum = 6
```JavaScript
// ● success Runtime: xx ms
```   



### 136 Single Number
给一个非空的整数数组，除了一个元素外，每个元素都会出现两次。找到那一个。要求时间复杂度是O(n)，空间复杂度是O(1)。[题目链接](https://leetcode.com/problems/single-number/description/)
Exp 1:  `Input: [2,2,1]  Output: 1`
Exp 2:  `Input: [4,1,2,1,2]  Output: 4`
```JavaScript
// 第一次尝试：用indexOf查找的方法
// ● success Runtime: 188 ms     
var singleNumber = function(nums) { 
	var result = []
	nums.forEach((value,index) => { 
		var idx = result.indexOf(value)
		if(idx < 0) {
				result.push(value)
		} else {
				result.splice(idx,1)
		}
	})
	return Number(result)
}

// 第二次尝试：用sort()排序后判断的方法
// ● success Runtime: 80 ms    
var singleNumber = function(nums) { 
    nums.sort((a,b) => a-b)
    for(let i = 0; i < nums.length; i++) {
        if(nums[i] !== nums[i-1] && nums[i] !== nums[i+1]) return nums[i]
    }
}


// 第三次尝试：利用eval()对字符串中的内容进行位运算
// ● success Runtime: 68 ms  
var singleNumber = function(nums) { 
    return eval(nums.join('^'))
}

// 第四次尝试：使用位运算，'^'是一种运算符，运算符！！！
// ● success Runtime: 56 ms 
var singleNumber = function(nums) { 
    var result
    for(let i = 0; i < nums.length; i++) result = nums[i] ^ result
    return result
}

// 大神的代码
// ● success Runtime: 56 ms 
var singleNumber = function(nums) { 
    return nums.reduce((a,b) => a ^ b)
}
```


### 929 Unique Email Addresses
在电子邮件地址的本地名称部分中的某些字符之间添加句点（'。'），点会被忽略掉。在本地名称中添加加号（'+'），会忽略第一个加号后面的所有内容。给定电子邮件列表，有多少不同的地址实际接收邮件？[题目链接](https://leetcode.com/problems/unique-email-addresses/description/)
Exp 1:
`Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]`
`Output: 2`
```JavaScript
// 1 先用正则把@后面的值匹配出来，2 再把匹配出的@前面的值变成数组，3 找到第一个"+"的index位置，
// 4 截取从0到index范围的数组，5 变成字符串并用正则去掉"."，6 最后把前后拼起来放到一个数组，最后去重
// ● success Runtime: 108 ms
var numUniqueEmails = function(emails) {
    var result = []
    emails.forEach((value,index)=>{
        let end = value.match(/@.*/)[0]
        let tempArr = value.match(/.*\@/)[0].split('')
        let start = tempArr.slice(0,tempArr.indexOf("+")).join('').match(/\w/g).join('') 
        result.push(start.concat(end))
    })
    return Array.from(new Set(result)).length
}

// 以下是来自大神的代码
// ● success Runtime: 68 ms
var numUniqueEmails = function(emails) {
	return Array.from(new Set(emails.map(email => email.split('@')[1]))).length
}
```


### 617 Merge Two Binary trees
给定两个二叉树，将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。注意: 合并必须从两个树的根节点开始。[题目链接](https://leetcode.com/problems/merge-two-binary-trees/)
```JavaScript
// ● success Runtime: 92 ms
var mergeTrees = function(t1, t2) {
	// 加外面这层的目的是为了处理t1,t2都是空的情况
	if(!t1 && !t2) return []
	if(!t1) return t2
	if(!t2) return t1
	function merge(t1, t2) {
		if(!t1) return t2
		if(!t2) return t1
		t1.left = merge(t1.left, t2.left)
		t1.right = merge(t1.right, t2.right)
		t1.val = t1.val + t2.val
		return t1
	}
	merge(t1, t2)
	return t1
}
```

### 198 House Robber
你是一个专业的强盗，计划在街上抢劫房屋。每个房子都藏着一定数量的钱，阻止你抢劫他们的唯一限制因素是相邻的房屋有连接的安全系统，如果两个相邻的房子在同一个晚上被闯入，它将自动联系警方。给出一个代表每个房子的金额的非负整数列表，确定今晚可以抢劫的最大金额而不警告警察。[题目链接](https://leetcode.com/problems/house-robber/description/)
Exp 1：`Input: [1,2,3,1]     Output: 4`
Exp 2：`Input: [2,7,9,3,1]   Output: 12`
```JavaScript
// ● success Runtime: 48 ms
var rob = function(nums) {
	var even = 0, odd = 0
	for (let i = 0; i < nums.length; i++) {
		if (i % 2 === 0) {
			even = Math.max(even + nums[i], odd)
		} else {
			odd = Math.max(even, odd + nums[i])
		}
	}
	return Math.max(even, odd)
}
```

### 20 Valid Parentheses
给定一个只包含字符’（’，’）’，’{‘，’}’，’[‘和’]’的字符串，如果对应的括号能正常关闭，则字符串有效。[题目链接](https://leetcode.com/problems/valid-parentheses/description/)
Exp 1:  `Input: "()"     Output: true`
Exp 2:  `Input: "()[]{}" Output: true`
Exp 3:  `Input: "(]"     Output: false`
Exp 4:  `Input: "([)]"   Output: false`
Exp 5:  `Input: "{[]}"   Output: true`
```JavaScript
// ● success Runtime: 56 ms
var isValid = function(s) {
	s = Array.from(s)
	var stack= [],contrast  = []
	var map = {
		"(" : ")",
		"{" : "}",
		"[" : "]"
	}
	s.forEach( (value, index) => {
		// 是左括号就压栈，是右括号就出栈，出栈时判断左右括号是否匹配
		if(map[value]) {
			stack.push(value)
		} else {
			var temp = stack.pop()
			if(map[temp] !== value) {
				contrast.push("false")
			}
		}
	})
	// 如果栈中长度或者不匹配元素的长度不等于0，则返回false
	if(contrast.length > 0 || stack.length > 0) {
		return false
	} else {
		return true
	}
}
// 不知道为什么，把contrast.push("false")换成return false总是会出问题，只能增加一个contrast数组用来存那些不匹配的，最后再进行判断
```

### 203 Remove Linked List Elements
给一个val，删掉链表中值和val一样的所有元素。[题目链接](https://leetcode.com/problems/remove-linked-list-elements/)
Exp 1:  `Input:  1->2->6->3->4->5->6, val = 6   Output: 1->2->3->4->5`
```JavaScript
// 第一次尝试链表变成数组进行操作，返回一个数组，虽然测试成功，但并不是在链表中移除元素
// ● success Runtime: 96 ms
var removeElements = function(head, val) {
	// 链表变成数组
	var headArr = []
	while(head !== null) {
		headArr.push(head.val)
		head = head.next
	}

	//  删除数组中的匹配元素
	var tempArr = [];
	var idx = headArr.indexOf(val);
	while (idx != -1) {
		tempArr.push(idx);
		idx = headArr.indexOf(val, idx + 1);
	}
	tempArr.sort((a,b) => b-a)
	tempArr.forEach( (value, index) => {
		headArr.splice(value,1)
	})
	return headArr
}

// 第二次尝试直接在链表中操作
// ● success Runtime: 76 ms
var removeElements = function(head, val) {
	var node = val
	var curNode = head

	// 如果head不存在，返回[]
	if(!head) return []

	// 查找和删除
	while(curNode.next != null) {
		while (curNode.next != null && curNode.next.val !== node) {
				curNode = curNode.next
		}
		if(curNode.next !== null ) {
				curNode.next = curNode.next.next
		}
	}

	// 判断head.val，即第一个元素，如果等于node就删除
	if(head.val === node) head = head.next

	return head
}
```

### 504 Base 7
实现一个七进制。[题目链接](https://leetcode.com/problems/base-7/description/)
Exp 1:  `Input: 100    Output: "202"`
Exp 2:  `Input: -7     Output: "-10"`
```JavaScript
// ● success Runtime: 64 ms
var convertToBase7 = function(num) {
	// num为0，直接返回0
	if(num === 0) return "0"

	// num大于0，取num的绝对值
	nums = num > 0? num:-num

	// 算出来的结果存到数组
	var result = []
	while(nums > 0){
		res = nums % 7
		nums = parseInt(nums/7)
		result.unshift(String(res))
	}

	// 判断num的符号，是负数就加上负号
	if(Math.sign(num) === -1) result.unshift('-')

	// return字符串形式
	return result.join('')
}
```

### 859 Buddy Strings
改变A中随意两个元素的位置，使A和B相等，能相等true，不能false。[题目链接](https://leetcode.com/problems/buddy-strings/description/)
Exp 1:  `Input: A = "ab", B = "ba"   Output: true`
Exp 2:  `Input: A = "ab", B = "ab"   Output: false`
Exp 3:  `Input: A = "aa", B = "aa"   Output: true`
Exp 4:  `Input: A = "aaaaaaabc", B = "aaaaaaacb"   Output: true`
Exp 5:  `Input: A = "", B = "aa"     Output: false`
```JavaScript
// ● success Runtime: 60 ms
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
	var tempA = [],tempB = []
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
// 传说中的送分题，我挂了6次才success，还有谁？
```

### 438 (未完成)Find All Anagrams in a String
给定一个字符串s和一个非空字符串p，找到s中p的的所有起始索引，只要和p的元素一样就可以，不管顺序。[题目链接](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
Exp 1:  `Input: s: "cbaebabacd" p: "abc"  Output: [0, 6]`
Exp 2:  `Input: s: "abab" p: "ab"         Output: [0, 1, 2]`
```JavaScript
// 第一次尝试，在数组中循环遍历查找
// ○ failed Time Limit Exceeded
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

// 第二次尝试，滑动窗口(sliding window)解决字符串查找匹配的问题

```

### 7 Reverse Integer
给一个32位有符号整数，返回这个整数的反向数字。[题目链接](https://leetcode.com/problems/reverse-integer/)
Exp 1:  `Input: 123    Output: 321`
Exp 2:  `Input: -123   Output: -321`
Exp 3:  `Input: 120    Output: 21`
```JavaScript
// ● success Runtime: 88 ms
var reverse = function(x) {
  let num = parseInt(x.toString().split('').reverse().join(''))
  if (num > Math.pow(2,31) - 1) return 0;
  return Math.sign(x) * num
}
// Math.sign() 函数返回一个数字的符号, 指示数字是正数，负数还是零
```

### 707 Design Linked List
设计一个链表，单链表具有两个属性：`val`和`next`。 `val`是当前节点的值，`next`是指向下一个节点的指针/引用。如果要使用双向链表，需要一个属性`prev`以指示链接列表中的上一个节点。假设链表中的所有节点都是0索引的。[题目链接](https://leetcode.com/problems/design-linked-list/)
在链表类中实现这些功能：
`get（index）`：获取链表中索引节点的值。如果索引无效，则返回-1。
`addAtHead（val）`：在链表的第一个元素之前添加值为val的节点。插入后，新节点将成为链表的第一个节点。
`addAtTail（val）`：将值val的节点追加到链表的最后一个元素。
`addAtIndex（index，val）`：在链表中的索引节点之前添加值为val的节点。如果index等于链表的长度，则该节点将附加到链表的末尾。如果index大于长度，则不会插入节点。
`deleteAtIndex（index）`：如果索引有效，则删除链表中的索引节点。
```JavaScript
// ● success Runtime: 72 ms
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
// 虽然测试成功了，但、好像、似乎...并不是可以用于链表的方法
```

### 665 Non-decreasing Array
给定一个包含n个整数的数组，通过修改最多1个元素来检查它是否可以变为非递减。如果`array [i] <= array [i + 1]`为每个i`1 <= i <n`保持，则这个数组是非递减的。[题目链接](https://leetcode.com/problems/non-decreasing-array/)
Exp 1:  `Input: [4,2,3]  Output: True`
Exp 2:  `Input: [4,2,1]  Output: False`
```JavaScript
// ● success Runtime: 132 ms
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
```