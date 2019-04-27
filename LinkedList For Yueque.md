### 一、链表是什么？与数组有什么区别？生活中有什么案例？

**1.概念：**   

链表（Linked list）是一种上一个元素的引用指向下一个元素的存储结构，链表通过指针来连接元素与元素；   

链表是线性表的一种，所谓的线性表包含顺序线性表和链表，顺序线性表是用数组实现的，在内存中有顺序排列，通过改变数组大小实现。而链表不是用顺序实现的，用指针实现，在内存中不连续。意思就是说，链表就是**将一系列不连续的内存联系起来**，将那种碎片内存进行合理的利用，解决空间的问题。     

所以，链表允许插入和删除表上任意位置上的节点，但是不允许随即存取。链表有很多种不同的类型：**单向链表**、**双向链表**及**循环链表**。   

**2.与数组的区别：**   
* 相同：   
两种结构均**可实现数据的顺序存储**，构造出来的模型呈**线性结构**。   

* 不同：   
链表是**链式的存储结构**；数组是**顺序的存储结构**。   
链表通过**指针**来连接元素与元素，数组则是把所有元素按**次序**依次存储。 

链表的插入删除元素相对数组较为简单，不需要移动元素，且较为容易实现长度扩充，但是寻找某个元素较为困难。   

数组寻找某个元素较为简单，但插入与删除比较复杂，由于最大长度需要再编程一开始时指定，故当达到最大长度时，扩充长度不如链表方便。   
   

**数组和链表一些操作的时间复杂度对比：**   
数组：   
* 查找复杂度：O(1)   
* 添加/删除复杂度：O(n)   

链表：   
* 查找复杂度：O(n)  
* 添加/删除复杂度：O(1)   

**3.生活中的案例：**   
火车，是由一些列车厢连接起来；   
寻宝游戏，每个线索都是下一个线索地点的指针。   


### 二、请事先一个链表，并实现以下方法

* `append(element)`：向链表尾部添加一个新的元素。  
* `insert(position, element)`：向链表指定位置插入一个新的元素。  
* `remove(element)`：从链表中移除并返回已删除的元素（若有多个相同元素则取第一次出现的情况）。  
* `getNode(index)`：获取指定索引位置的节点。  
* `indexOf(element)`：返回元素在链表中的索引（若有多个相同元素则取第一次出现的情况），如果列表中没有该元素则返回 -1。  
* `removeAt(position)`：从链表特定的位置移除一项。  
* `getHead()`：获取链表头部。  
* `getTail()`：获取链表尾部。  
* `isEmpty()`：如果链表中不包含任何元素，返回 true，否则返回 false。  
* `size()`：返回链表中元素个数，与数组的 length 属性类似。  
* `print()`：打印链表中的所有元素，如 "0@1-->1@2-->2@3-->3@4-->4@5"（"index@element"）。  

![链表1](https://cdn.nlark.com/yuque/0/2019/png/102778/1555493335785-a20c2097-cc77-466a-b8ad-4d12523d91ea.png)

**提示：Web 端优先使用 ES6 以上的语法实现，可定义一个 Node 类来表示节点，该类中用于存储数据的属性可以为 data 或 element。** 

----
解题：   
```js
class Node {
    constructor(element){
        this.element = element
        this.next = null
    }
}
class LinkedList {
    constructor(){
        this.length = 0
        this.head = null
    }
    /**
     * 添加元素（末尾添加）
     * @param {*} element 添加的元素
     */
    append(element){
        let node = new Node(element)
        if(!this.head){
            this.head = node
        }else{
            let current = this.head
            // 查找最后一项
            while(current.next){
                current = current.next
            }
            // 将最后一下的next赋值为node，实现追加元素
            current.next = node
        }
        this.length ++
    }
    /**
     * 添加元素（指定位置）
     * @param {Number} position 添加的位置
     * @param {*} element  添加的元素
     */
    insert(position, element){
        if(!position || position < 0 || position > this.length) return new Error('请输入正确的数值！')
        let node = new Node(element),
            index = 0,
            previous = null
        if(position === 0){
            node.next = this.head
            this.head = node
        }else{
            let current = this.head
            while(index++ < position){
                previous = current
                current = current.next
            }
            previous.next = node
            node.next = current
        }
        this.length ++
    }
    /**
     * 删除元素
     * @param {*} element 删除的元素
     * @return {*}  被删除的元素
     */
    remove(element){
        if (!element) return {}
        let current = this.head,
            previous = null
        if(element === this.head.element){
            this.head = current.next
        }else{
            while(current.next && current.element !== element){
                previous = current
                current = current.next
            }
            previous.next = current.next
            this.length --
            return current.element
        }
    }
    /**
     * 删除元素（指定位置）
     * @param {Number} position 删除元素的位置
     * @return {*}  被删除的元素
     */
    removeAt(position){
        if (Number(position) === NaN || position < 0 || position > this.length) return {}
        let current = this.head,
            index = 0,
            previous = null
        if(position === 0){ // 删除第一项
            this.head = current.next
        }else{
            while(index++ < position){
                previous = current
                current = current.next
            }
            previous.next = current.next
        }
        this.length --
        return current.element
    }
    /**
     * 查找指定位置的元素
     * @param {Number} index 查找的元素的下标
     * @return {*} element 查找的元素
     */
    getNode(index){
        if (Number(index) === NaN || index < 0 || index > this.length) return {}
        if (index === 0) return this.head.element;
        let current = this.head,
            curIndex = 1
        while (curIndex < index) {
            current = current.next
            curIndex ++
        }
        return current.element;
    }
    /**
     * 查找指定元素的位置
     * @param {*} element 查找的元素
     * @return {Number} 查找的元素的下标
     */
    indexOf(element){
        if (!element) return -1
        let current = this.head, 
            index = 0
        while(current.next && current.element !== element){
            current = current.next
            index ++
        }
        return index === 0 ? -1 : index
    }
    /**
     * 获取头元素
     * @return {Number}
     */
    getHead(){
        return this.getNode(0)
    }
    /**
     * 获取尾元素
     * @return {Number}
     */
    getTail(){
        let current = this.head
        while (current.next) {
            current = current.next
        } 
        return current
    }
    /**
     * 链表是否为空
     * @return {Boolean}
     */
    isEmpty(){
        return this.length === 0
    }
    /**
     * 链表的长度
     * @return {Number}
     */
    size(){
        return this.length
    }
    /**
     * 将链表转成字符串
     * @return {String}
     */
    toString(){
        let current = this.head,
            arr = new Array()
        while(current.next){
            arr.push(current.element)
            current = current.next
        }
        return arr.toString()
    }
    /**
     * 将链表转成字符串
     * @return {String}
     */
    print(){
        let current = this.head,
            arr = [],
            index = 0
        while(current.next){
            arr.push(index + '@' + current.element)
            current = current.next
        }
        return arr.join('-->')
    }
}
```

### 三、基于链表分别实现 Stack 和 Queue


**提示：Stack 的特点是后进先出，Queue 的特点是先进先出，可以只实现其中几个重要的方法。**

**实现Stack：**   
```js
class Stack {
    constructor(){
        this.linkedList = new LinkedList()
    }
    /**
     * 在链表尾部 添加一个元素
     * @param {Object} element 需要添加的元素
     */
    push(element){
        this.linkedList.append(element)
    }
    /**
     * 在链表头部 删除一个元素
     * @return {Object} 推出的元素 
     */
    pop(){
        let index = this.linkedList.length - 1;
        return this.linkedList.removeAt(index)
    }
    /**
     * 返回链表头部的元素
     * @return {Object} 头部的元素 
     */
    peek(){
        return this.linkedList.getHead()
    }
    /**
     * 返回是否是空链表
     * @return {Boolean}
     */
    isEmpty(){
        return this.linkedList.isEmpty()
    }
    /**
     * 返回链表长度
     * @return {Number} 链表长度
     */
    size(){
        return this.linkedList.size()
    }
}
```

**实现Queue：**   
```js
class Queue {
    constructor(){
        this.linkedList = new LinkedList()
    }
    /**
     * 在链表头部 添加一个元素
     * @param {Object} element 需要添加的元素
     */
    enqueue(element){
        this.linkedList.append(element)
    }
    /**
     * 在链表尾部 删除一个元素
     * @return {Object} 推出的元素 
     */
    dequeue(){
        return this.linkedList.removeAt(0)
    }
    /**
     * 返回链表头部的元素
     * @return {Object} 头部的元素 
     */
    front(){
        return this.linkedList.getHead()
    }
    /**
     * 返回是否是空链表
     * @return {Boolean}
     */
    isEmpty(){
        return this.linkedList.isEmpty()
    }
    /**
     * 返回链表长度
     * @return {Number} 链表长度
     */
    size(){
        return this.linkedList.size()
    }
}
```


### 四、链表反转
请实现一个 `reverseLinkedList()` 方法，用于实现链表反转。   

![链表2](https://cdn.nlark.com/yuque/0/2019/jpeg/102778/1555294131053-6d7c8e1f-1984-4fb3-a309-3804706f6dfe.jpeg?x-oss-process=image/resize,w_1492)   

示例：   
```js
let linkedlist = {
    'element': 1,'next': {
        'element': 2,'next': {
            'element': 3,'next': {
                'element': 4,'next': {
                    'element': 5,
                    'next': null
               }
           }
       }
   }
};

reverseLinkedList(linkedlist);

linkedlist = {
    'element': 5,'next': {
        'element': 4,'next': {
            'element': 3,'next': {
                'element': 2,'next': {
                    'element': 1,
                    'next': null
               }
           }
       }
   }
};

// 反转前 linkedList.print() 输出：0@1-->1@2-->2@3-->3@4-->4@5
// 反转后 linkedList.print() 输出：0@5-->1@4-->2@3-->3@2-->4@1
```

**1.使用迭代：**   
在遍历列表时，将当前节点的 `next` 指针改为**指向前一个元素**。由于节点没有引用其上一个节点，因此必须**先存储其前一个元素**。在更改引用之前，还需要另一个指针来存储下一个节点。**不要忘记在最后返回新的头引用**！
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
let reverseList = function(head) {
    let pre = null, curr = head
    while (curr) {
        next = curr.next
        curr.next = pre
        pre = curr
        curr = next
    }
    return pre
};
```
**复杂度分析**    

**时间复杂度**：`O(n)`。 假设 `n` 是列表的长度，时间复杂度是 `O(n)`。    
**空间复杂度**：`O(1)`。    


**2.使用递归：**  

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
let reverseList = function(head) {
    if(head == null || head.next == null) return head
    let pre = reverseList(head.next)
    head.next.next = head
    head.next = null
    return pre
};
```


### 五、请实现一个双向链表 DoublyLinkedList。

双向链表和普通链表的区别在于，在链表中，一个节点只有链向下一个节点的链接，而在双向链表中，链接是双向的：一个链向下一个元素，另一个链向前一个元素，如下图所示：   
![链表3](https://cdn.nlark.com/yuque/0/2019/png/102778/1555294166670-7f715520-9195-4707-86ce-6e7fa6411a0f.png)  

正是因为这种变化，使得链表相邻节点之间不仅只有单向关系，还可以通过 `prev` 来访问当前节点的上一节点。

**提示：更新原有的 Node 类，新增 prev 属性，为了实现代码复用，可以继承前面已实现的LinkedList类。**


----
解题：   
```js
class Node {
    constructor(element){
        this.element = element
        this.next = null
        this.prev = null
    }
}
class DoubleLink {
    constructor(){
        this.length = 0
        this.head = null
        this.tail = null
    }
    /**
     * 添加元素（末尾添加）
     * @param {*} element 添加的元素
     */
    append(element){
        let node = new Node(element)
        let last = this.getTail()
        last.next = node
        node.previous = last
        this.length ++
    }
    /**
     * 添加元素（指定位置）
     * @param {Number} position 添加的位置
     * @param {*} element  添加的元素
     */
    insert(position, element){
        if(!position || position < 0 || position > this.length) return new Error('请输入正确的数值！')
        let node = new Node(element),
            index = 0,
            previous = null,
            current = this.head
        if(position === 0){
            if(!this.head){
                this.head = node
                this.tail = node
            }else{
                node.next = current
                current.prev = node
                this.head = node
            }
        }else if(position === this.length){
            current = this.tail
            current.next = node
            node.prev = current
            this.tail = node
        }else{
            while(index++ < position){
                previous = current
                current = current.next
            }
            node.next = current
            previous.next = node

            current.prev = node
            node.prev = previous
        }
        this.length ++
    }
    /**
     * 删除元素
     * @param {*} element 删除的元素
     * @return {*}  被删除的元素
     */
    remove(element){
        if (!element) return {}
        let current = this.head,
            previous = null
        if(element === this.head.element){
            this.head = current.next
        }else{
            while(current.next && current.element !== element){
                previous = current
                current = current.next
            }
            previous.next = current.next
            this.length --
            return current.element
        }
    }
    /**
     * 删除元素（指定位置）
     * @param {Number} position 删除元素的位置
     * @return {*}  被删除的元素
     */
    removeAt(position){
        if (Number(position) === NaN || position < 0 || position > this.length) return {}
        let current = this.head,
            index = 0,
            previous = null
        if(position === 0){ // 删除第一项
            this.head = current.next
            if(this.length === 1){
                this.tail = null
            }else{
                this.head.prev = null
            }
        }else if(position === this.length -1){
            current = this.tail
            this.tail = current.prev
            this.tail.next = null
        }else{
            while(index++ < position){
                previous = current
                current = current.next
            }
            previous.next = current.next
            current.next.prev = previous
        }
        this.length --
        return current.element
    }
    /**
     * 查找指定位置的元素
     * @param {Number} index 查找的元素的下标
     * @return {*} element 查找的元素
     */
    getNode(index){
        if (Number(index) === NaN || index < 0 || index > this.length) return {}
        if (index === 0) return this.head.element;
        let current = this.head,
            curIndex = 1
        while (curIndex < index) {
            current = current.next
            curIndex ++
        }
        return current.element;
    }
    /**
     * 查找指定元素的位置
     * @param {*} element 查找的元素
     * @return {Number} 查找的元素的下标
     */
    indexOf(element){
        if (!element) return -1
        let current = this.head, 
            index = 0
        while(current.next && current.element !== element){
            current = current.next
            index ++
        }
        return index === 0 ? -1 : index
    }
    /**
     * 获取头元素
     * @return {Number}
     */
    getHead(){
        return this.getNode(0)
    }
    /**
     * 获取尾元素
     * @return {Number}
     */
    getTail(){
        let current = this.head
        while (current.next) {
            current = current.next
        } 
        return current
    }
    /**
     * 链表是否为空
     * @return {Boolean}
     */
    isEmpty(){
        return this.length === 0
    }
    /**
     * 链表的长度
     * @return {Number}
     */
    size(){
        return this.length
    }
}
```
