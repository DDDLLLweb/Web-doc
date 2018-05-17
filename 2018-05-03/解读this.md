# 通俗易懂
this一般有几种调用场景     
var obj = {a: 1, b: function(){console.log(this);}}
1. 作为方法调用时，指向该调用对象 obj.b(); // 指向obj
2. 作为函数调用, var b = obj.b; b(); // 指向全局window
3. 作为构造函数调用 var b = new Fun(); // this指向当前实例对象
4. 作为call与apply调用 obj.b.apply(object, []); // this指向当前的object