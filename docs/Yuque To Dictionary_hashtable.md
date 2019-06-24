### 一、字典和散列表的概念

1. 字典是什么？    

字典是一种以 **键-值对** 形式存储数据的数据格式，其中键名用来查询特定元素。

2. 字典和集合有什么异同？

相同：都是用来存储不同元素的数据格式；   
区别：集合是以 **值-值** 的数据格式存储，而字典是以 **键-值** 的数据格式存储。   

3. 什么是散列表和散列函数？

哈希表（`Hash table`，也叫散列表），是根据关键码值(·Key value·)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做**散列函数**，存放记录的数组叫做**散列表**。   

4. 散列表的特点是什么？

特点：数组和链接优点的结合，查询速度非常的快，几乎是O(1)的时间复杂度，并且插入和删除也容易。


### 二、请实现一个字典
```js
class Dictionary {
    constructor(){
        this.items = []
    }

    set (key, value) {
        if ( !key ) return new Error('请指定插入的key')
        this.items[key] = value
    }

    has (key) {
        return key in this.items 
    }

    delete (key) {
        if(this.has(key)){
            delete this.items[key]
            return true
        }
        return false
    }

    get (key) {
        return this.has(key) ? this.items[key] : undefined
    }
    
    clear () {
        this.items = {}
    }
    
    keys () {
        return Object.keys(this.items)
    }

    values () {
        let result = []
        for(let k in this.items){
            if(this.has[k]){
                result.push(this.items[k])
            }
        }
        return result
    }

    size () {
        const values = this.values()
        return values.length
    }
}
```

### 三、请实现一个散列表
```js
class HashTable {
    constructor(){
        this.table = []
    }

    hashCode(key) {
        let hash = 0;
        for (let i = 0; i < key.length; i++) {
          hash += key.charCodeAt(i);
        }
        return hash % 37;
    }

    put (key, value) {
        let position = this.hashCode(key)
        this.table[position] = value
    }

    remove (key) {
        if ( !key ) return false
        let position = this.hashCode(key)
        this.table[position] = undefined
    }

    get (key) {
        let position = this.hashCode(key)
        return this.table[position]
    }

    print () {
        return this.table
    }
}
```


### 四、请利用之前已实现的链表，实现一个分离链接的散列表

**链表的实现代码省略 可以查看之前的代码**
```js
let ValuePair = function (key, value){
    this.key = key
    this.value = value
    this.toString = function(){
        return `[${this.key} - ${this.value}]`
    }
}
class HashTable {
    constructor(){
        this.table = []
    }
    
    hashCode(key) {
        let hash = 0;
        for (let i = 0; i < key.length; i++) {
          hash += key.charCodeAt(i);
        }
        return hash % 37;
    }
    
    put (key, value) {
        let position = this.hashCode(key)
        if(this.table[position] == undefined){
            this.table[position] = new LinkedList()
        }
        this.table[position].append(new ValuePair(key, value))
    }

    remove (key) {
        let position = this.hashCode(key)
        if ( !key || this.table[position] === undefined ) return false
        let current = this.table[position].getHead()
        while(current.next){
            if(current.element.key === key){
                this.table[position].remove(current.element)
                if(this.table[position].isEmpty){
                    this.table[position] = undefined
                }
                return true
            }
            current = current.next
        }
    }

    get (key) {
        let position = this.hashCode(key)
        if(this.table[position] === undefined) return undefined
        let current = this.table[position].getHead()
        while(current.next()){
            if(current.element.key === key){
                return current.element.value
            }
            current = current.next
        }
    }

    print () {
        return this.table
    }
}
```


### 五、实现一个线性探查的散列表

```js
let ValuePair = function (key, value){
    this.key = key
    this.value = value
    this.toString = function(){
        return `[${this.key} - ${this.value}]`
    }
}
class HashTable {
    constructor(){
        this.table = []
    }
    
    hashCode(key) {
        let hash = 0;
        for (let i = 0; i < key.length; i++) {
          hash += key.charCodeAt(i);
        }
        return hash % 37;
    }

    put (key, value) {
        let position = this.hashCode(key)
        if(this.table[position] == undefined){
            this.table[position] = new ValuePair(key, value)
        }else{
            let index = ++position
            while(this.table[index] !== undefined){
                index ++
            }
            this.table[index] = new ValuePair(key, value)
        }
    }

    remove (key) {
        let position = this.hashCode(key)
        if( !key || this.table[position] === undefined ) return undefined
        if(this.table[position].key === key){
            this.table[index] = undefined
        }else{
            let index = ++position
            while(
                this.table[index] === undefined ||
                this.table[index].key !== key
            ){
                index ++
            }
            if(this.table[index].key === key){
                this.table[index] = undefined
            }
        }
    }

    get (key) {
        let position = this.hashCode(key)
        if( !key || this.table[position] === undefined ) return undefined
        if(this.table[position].key === key){
            return this.table[position].value
        }else{
            let index = ++position
            while(
                this.table[index] === undefined ||
                this.table[index].key !== key
            ){
                index ++
            }
            if(this.table[index].key === key){
                return this.table[index].value
            }
        }
    }
    
    print () {
        return this.table
    }
}
```