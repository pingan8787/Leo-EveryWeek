## 一、什么是树？   
* 1.树有什么特点，什么是二叉树和二叉搜索树：     

**树**是一种**非线性的数据结构**，以**分层方式存储数据**，用来表示**有层级关系的数据**。   

每棵树至多只有一个**根结点**，**根结点**会有很多**子节点**，每个**子节点只有一个父结点**。   

**父结点**和**子节点**是相对的。   

* 2.生活中的例子：    
如：家谱或公司组织架构图。    

## 二、请实现二叉搜索树（BST），并实现以下方法：   

```js
class Node {
    constructor(key){
        this.key = key
        this.left = null
        this.right = null
    }
}
class BST {
    constructor(){
        this.root = null
    }
    /**
     * 插入一个节点
     * @param {*} node 插入的位置节点
     * @param {*} newNode 插入的节点
     */
    insertNode (node, newNode){
        if(newNode.key < node.key){
            if(node.left === null && node.right === null){
                node.left = newNode
            }else if(node.left !== null && node.right === null){
                node.right = newNode
            }else{
                this.insertNode(node.left, newNode)
            }
        }else{
            if(node.left === null && node.right === null){
                node.left = newNode
            }else if(node.left !== null && node.right === null){
                node.right = newNode
            }else{
                this.insertNode(node.right, newNode)
            }
        }
    }
    /**
     * 插入操作
     * @param {*} key 
     */
    insert (key){
        let newNode = new Node(key)
        if(this.root === null){
            this.root = newNode
        }else{
            this.insertNode(this.root, newNode)
        }
    }
    searchNode (node, key){
        if(node === null) return false
        if(key < node.key){
            return this.searchNode(node.left, key)
        }else if(key > node.key){
            return this.searchNode(node.right, key)
        }else{
            return true
        }
    }
    /**
     * 搜索操作
     * @param {*} key 
     */
    search (key){
        return this.searchNode(this.root, key)
    }
    /**
     * 最小值的节点
     */
    min (){
        let node = this.root
        if(node === null) return null
        while(node && node.left !== null){
            node = node.left
        }
        return node.key
    }
    /**
     * 最大值的节点
     */
    max (){
        let node = this.root
        if(node === null) return null
        while(node && node.right !== null){
            node = node.right
        }
        return node.key
    }
    /**
     * 找到最小节点
     * @param {*} node 
     */
    findMinNode (node){
        if(node === null) return null
        while(node && node.left !== null){
            node = node.left
        }   
        return node
    }
    /**
     * 删除一个节点
     * @param {*} node 
     * @param {*} key 
     */
    removeNode (node, key){
        if(node === null) return null
        if(key < node.key){
            node.left = this.removeNode(node.left, key)
            return node
        }else if(key > node.key){
            node.right = this.removeNode(node.right, key)
            return node
        }else{
            // 1.叶节点
            if(node.left === null && node.right === null){
                node = null
                return node
            }
            // 2.只有一个子节点
            if(node.left === null){
                node = node.right
                return node
            }else if(node.right === null){
                node = node.left
            }
            // 3.有两个子节点
            let curNode = this.findMinNode(node.right)
            node.key = curNode.key
            node.right = this.removeNode(node.right, curNode.key)
            return node
        }
    }
    /**
     * 删除一个节点
     * @param {*} key 
     */
    remove (key){
        if(this.root === null) return null
        this.root = this.removeNode(this.root, key)
    }
}
```

## 三、基于题二实现二叉搜索树扩展以下方法：  
```js
// 1. 先序
BST.prototype.preOrderTraverseNode = function(node, callback){
    if(node !== null){
        callback(node.key)
        this.preOrderTraverseNode(node.left, callback)
        this.preOrderTraverseNode(node.right, callback)
    }
}
BST.prototype.preOrderTraverse = function(callback){
    this.preOrderTraverseNode(this.root, callback)
}

// 2. 中序
BST.prototype.inOrderTraverseNode = function(node, callback){
    if(node !== null){
        this.inOrderTraverseNode(node.left, callback)
        callback(node.key)
        this.inOrderTraverseNode(node.right, callback)
    }
}
BST.prototype.inOrderTraverse = function(callback){
    this.inOrderTraverseNode(this.root, callback)
}

// 3. 后序
BST.prototype.postOrderTraverseNode = function(node, callback){
    if(node !== null){
        this.postOrderTraverseNode(node.left, callback)
        this.postOrderTraverseNode(node.right, callback)
        callback(node.key)
    }
}
BST.prototype.postOrderTraverse = function(callback){
    this.postOrderTraverseNode(this.root, callback)
}
```

## 四、请实现从上往下打印二叉树   

* 方法一：  
```js
BST.prototype.printLevelOrder = function (root, arr = [], i = 0){
    if (root && (root.key || root.key === 0)) {
      !arr[i] && (arr[i] = [])
      arr[i].push(root.key)
      i++
      root.left && this.printLevelOrder(root.left, arr, i)
      root.right && this.printLevelOrder(root.right, arr, i)
    }
    return arr
}
```

* 方法二：  
```js
BST.prototype.printLevelOrder = function (){
    if(this.root === null) return []
    let result = [], queue = [this.root]
    while(true){
        let len = queue.length, arr = []
        while(len > 0){
            console.log(queue)
            let node = queue.shift()
            len -= 1
            arr.push(node.key)
            if(node.left !== null) queue.push(node.left)
            if(node.right !== null) queue.push(node.right)
        }
        if(arr.length === 0) return result
        result.push([...arr])
    }
}
```

## 五、给定一个二叉树，判断其是否是一个有效的二叉搜索树。

```js
function isValidBST(root) {
    let arr = []
    function inOrderTraverse(node){
        if(node === null) return;
        node.left && inOrderTraverse(node.left);
        arr.push(node.val);
        node.right && inOrderTraverse(node.right);
    }
    inOrderTraverse(root)
    for(let i = 0; i < arr.length - 1; i++){
        if(arr[i] >= arr[i+1]) return false
    }
    return true
};
```