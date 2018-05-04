# 扁平化
数组的扁平化，就是将一个嵌套多层的数组 array (嵌套可以是任何层数)转换为只有一层的数组。    
# 递归
```js
// 方法 1
var arr = [1, [2, [3, 4]]];
function flatten(arr) {
    var result = [];
    for (var i = 0, len = arr.length; i < len; i++) {
        if (Array.isArray(arr[i])) {
            result = result.concat(flatten(arr[i]))
        }
        else {
            result.push(arr[i])
        }
    }
    return result;
}
console.log(flatten(arr)) //[1, 2, 3, 4]
```
# toString
如果数组的元素都是数字，可以用toString方法

    [1, [2, [3, 4]]].toString() // "1,2,3,4",返回一个扁平的字符串
```js
    // 方法2
    var arr = [1, [2, [3, 4]]];
    function flatten(arr) {
        return arr.toString().split(',').map(function(item){
            return +item
        })
    }
    console.log(flatten(arr)) //[1, 2, 3, 4]
```
# reduce
# undercore