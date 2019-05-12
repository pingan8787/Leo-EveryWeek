### 一、集合是什么？与它相关数学概念有哪些？

 
**1.集合定义：**    
**集合（Set）**是一种包含不同元素的数据结构。集合中的元素称为**成员**，集合最重要的两个特点：  
* 集合中的成员是无序；
* 集合中不存在相同成员；

即：无序且唯一。
  
**2.集合相关的数学概念：**  

集合的概念，如数学中一个由大于或等于0的整数组成的自然数集合， `N = { 0, 1, 2, ...}`。    
还有如**空集**，表示不包含任何元素的集合。    
并且也有并集，交集，差集等操作。    


### 二、请实现一个集合，并实现以下方法：     
  
```js
class Sets {
    constructor(){
        this.items = {}
    }
    has(value){
        // return value in this.items
        return this.items.hasOwnProperty(value)
    }
    add(value){
        if(!this.has(value)) {
            this.items[value] = value
            return true
        }
        return false
    }
    delete(value){
        if(!this.has(value)){
            delete this.items[value]
            return true
        }
        return false
    }
    clear(){
        this.items = {}
    }
    size(){
        const values = this.values()
        return values.length
    }
    values(){
        return Object.keys(this.items)
    }
}
```

### 三、请实现集合的并集、交集、差集、子集操作   
   
```js
/**
 * union 并集
 * @param {Object} otherSet 其他集合
 */
Sets.prototype.union = function(otherSet){
    let result = new Sets(),
        current = this.values(),
        other = otherSet.values()
    for(let i = 0; i < current.length; i++){
        result.add(current[i])
    }
    for(let i = 0; i < other.length; i++){
        result.add(other[i])
    }
    return result
}


/**
 * intersection 交集
 * @param {Object} otherSet 其他集合
 */
Sets.prototype.intersection = function(otherSet){
    let result = new Sets(),
        current = this.values()
    for(let i = 0; i < current.length; i++){
        if(otherSet.has(current[i])){
            result.add(current[i])
        }
    }
    return result
}


/**
 * difference 差集
 * @param {Object} otherSet 其他集合
 */
Sets.prototype.difference = function(otherSet){
    let result = new Sets(),
        current = this.values()
    for(let i = 0; i < current.length; i++){
        if(!otherSet.has(current[i])){
            result.add(current[i])
        }
    }
    return result
}



/**
 * subset 子集
 * @param {Object} otherSet 其他集合
 */
Sets.prototype.subset = function(otherSet){
    let result = new Sets(),
        current = this.values()

    if(this.size() > otherSet.size()) return false
    for(let i = 0; i < current.length; i++){
        if(!otherSet.has(current[i])){
            return false
        }
    }
    return true
}
```

### 四、给定两个数组，编写一个 intersection() 函数来计算它们的交集。
   
```js
function intersection(arr1, arr2){
    if(!Array.isArray(arr1) || !Array.isArray(arr2)) return []
    let create = function(arr){
        let sets = new Sets()
        arr.map(item => sets.add(item))
        return sets
    }
    let Sets1 = create(arr1)
    let Sets2 = create(arr2)
    let result = Sets1.intersection(Sets2)
    return  result.values()
}
```

### 五、给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集。
 
```js
function subsets(nums){
    if(!nums || !Array.isArray(nums)) return []

    let result = [[]],
        len = nums.length
    if(len === 0) return result 
    for(let i = 0; i < len; i++){
        let l = result.length
        let num = nums[i]
        let array = [num]
        for(let j = 0; j < l; j++){
            let tmparray = result[j].concat(array)
            result.push(tmparray)
        }
    }
    return result
}
```