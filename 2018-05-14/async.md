# 含义
async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await。    

async函数对 Generator 函数的改进，体现在以下四点。

（1）内置执行器。     
Generator 函数的执行必须靠执行器，所以才有了co模块，而async函数自带执行器。也就是说，async函数的执行，与普通函数一模一样，只要一行。   

    asyncReadFile();

上面的代码调用了asyncReadFile函数，然后它就会自动执行，输出最后结果。这完全不像 Generator 函数，需要调用next方法，或者用co模块，才能真正执行，得到最后结果。  
（2）更好的语义。       
（3）更广的适用性。         
co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）。        
（4）返回值是 Promise。         

# 基本用法
async函数返回一个 Promise 对象，可以使用then方法添加回调函数。
当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

面试题
指定多少毫秒后输出一个值
```js
    function timeout(ms){
        return new Promise((resolve)=>{
            setTimeout(resolve,ms);
        });
    }
    async function asyncPrint(value,t){
        await timeout(t);
        console.log(value);
    }
    asyncPrint('hello world', 1000);
```