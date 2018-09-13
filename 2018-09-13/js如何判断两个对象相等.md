```js
function equalsObj(x,y){
    var f1 = x instanceof Object;
    var f2 = x instanceof Object;
    //如果二者中有一者不是引用数据类型，或者都不是引用数据类型，直接用===判断
    if(!f1||!f2){
        return x===y;
    }
    //如果二者都是引用数据类型，判断二者的属性长度是否一样
    if(Object.keys(x).length!=Object.keys(y).length){
        return false;
    }
    //若属性长度相同，遍历，属性判断相同属性对应的属性值是否相同，
    //还是先看里边的属性是哪一个类型，如果全是引用类型，那就接着对里边的属性调用equals递归函数。
    //如果不全是引用类型，那就比较这两个值是否相等，若不相等就直接false啦
    for(key in x){
        var a = x[key] instanceof Object;
        var b = y[key] instanceof Object;
        if(a&&b){
            equalsObj(x[key],y[key]);
        }else if(x[key]!=y[key]){
            return false;
        }
    }
    return true;
}
```