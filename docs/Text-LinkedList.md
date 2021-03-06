> 出题人：王平安
> 所属专题：算法专题篇
> 答题须知：答题者请于每周周会前，把答案提交到专题对应的 issue 下面，若有特殊情况，需通知出题人或部门负责人。

### 一、链表是什么？与数组有什么区别？生活中有什么案例？
概念参考阅读 [链表 —— 维基百科](https://zh.wikipedia.org/wiki/%E9%93%BE%E8%A1%A8)  

### 二、请事先一个链表，并实现以下方法
* `append(element)`：向列表尾部添加一个新的元素。   
* `insert(position, element)`：向列表指定位置插入一个新的元素。   
* `remove(element)`：从列表中移除并返回特定元素（若有多个相同元素则取第一次出现的情况）。   
* `indexOf(element)`：返回元素在列表的索引（若有多个相同元素则取第一次出现的情况），如果列表中没有该元素则返回 `-1`。
* `removeAt(position)`：从列表中，移除并返回特定位置的一项。   
* `isEmpty()`：如果列表不含任何元素，返回 `true`，否则返回 `false`。  
* `size()`：返回列表中元素个数，与数组的 `length` 属性类似。
* `toString()`：由于列表项使用 `Node` 类，需要重写继承自 JavaScript 对象默认的 `toString()` 方法，让其只输出元素的值。   
**提示：Web 端优先使用 ES6 以上的语法实现。**   


### 三、实现反转链表
用链表的方式，输出一个反转后的单链表。    

示例:   
```js
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
// input
let head = {
    'val': 1,'next': {
        'val': 2,'next': {
            'val': 3,'next': {
                'val': 4,'next': {
                    'val': 5,
                    'next': null
                }
            }
        }
    }
};
reverseList(head)

// output
head = {
    'val': 5,'next': {
        'val': 4,'next': {
            'val': 3,'next': {
                'val': 2,'next': {
                    'val': 1,
                    'next': null
                }
            }
        }
    }
};
```

**解题思路1.使用迭代：**   
在遍历列表时，将当前节点的 `next` 指针改为**指向前一个元素**。由于节点没有引用其上一个节点，因此必须**先存储其前一个元素**。在更改引用之前，还需要另一个指针来存储下一个节点。**不要忘记在最后返回新的头引用**！

**解题思路2.使用递归：**   
通过递归修改 `head.next.next` 和 `head.next` 指针来实现。   


**数组和链表一些操作的时间复杂度对比：**   

数组：   
* 查找复杂度：O(1)
* 添加/删除复杂度：O(n)
链表：   
* 查找复杂度：O(n)
* 添加/删除复杂度：O(1)


### 四、判断链表是否有环
设计一个函数 `hasCycle`，接收一个链表作为参数，判断链表中是否有环。   
为了表示给定链表中的环，我们使用整数 `pos` 来表示**链表尾**连接到**链表中的位置**（索引从 `0` 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。    

![linkedlist-cycle](http://images.pingan8787.com/linkedlist-cycle.png)

需要注意的是，不可能存在多个环，最多只有一个。  

**示例 1：**   
```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```
![示例 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**示例 2：**   
```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```
![示例 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

**示例 3：**   
```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```
![示例 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)



**解题思路1.判断是否有 null：**   
一直遍历下去，如果遍历到 `null` 则表示没有环，否则有环，但是考虑到性能问题，最好给定一段时间作为限制，超过时间就不要继续遍历。   

**解题思路2.标记法：**   
也是要遍历每个节点，并在遍历的节点添加标记，如果后面遍历过程中，遇到有这个标记的节点，即表示有环，反之没有环。   

**解题思路3.使用双指针（龟兔赛跑式）：**   
设置2个指针，一个 `快指针` 每次走 2 步，`慢指针` 每次走 1 步，如果没有环的情况，最后这两个指针不会相遇，如果有环，会相遇。   


### 五、实现两两交换链表中的节点
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。   

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。    
 
**示例:**   
```
给定 1->2->3->4, 你应该返回 2->1->4->3.
给定 1->2->3->4->5, 你应该返回 2->1->4->3->5.
```

**解题思路1.使用迭代：**   
和**反转链表**类似，关键在于有三个指针，分别指向前后和当前节点，而不同在于两两交换后，移动节点的步长为2，需要注意。   

**解题思路2.使用递归：**   
这里也可以使用递归，也可以参考**反转链表**的问题，终止条件是递归到链表为空，或者只剩下一个元素没得交换了，才终止。   