# JavaScript Note

### Array
#### reduce
对数组中的所有元素调用指定的对调函数。该回调函数的返回值为累积结果，并且此返回值在下一次调用该回调函数时作为参数提供。

**语法**
``` javascript
array.reduce(callbackFn[, initiaValue])
```

**参数说明**

|     参数     | 是否必填 |                                                   定义                                                   |
|--------------|----------|----------------------------------------------------------------------------------------------------------|
| array        | 必填     | 数组对象                                                                                                 |
| callbackFn   | 必填     | 1个接受最多拥有4个参数的函数。对于数组中的每个元素，reduce方法都会调用callbackFn函数一次。               |
| initialValue | 可选     | 如果指定initialValue，则它将用初始值来启动积累。第一次调用callbackFn函数会将此值作为参数而非数组值提供。 |

> ###### callbackFn参数说明
> 
> ``` javascript
> function callbackfn(previousValue, currentValue, currentIndex, array)
> ```
> 
> |    回调参数   |                                                           定义                                                           |
> |---------------|--------------------------------------------------------------------------------------------------------------------------|
> | previousValue | 通过上一次调用回调函数获得的值。如果向 reduce 方法提供 initialValue，则在首次调用函数时，previousValue 为 initialValue。 |
> | currentValue  | 当前数组元素的值。                                                                                                       |
> | currentIndex  | 当前数组元素的数字索引。                                                                                                 |
> | array         | 包含该元素的数组对象                                                                                                     |

>> ###### 第一次调用回调函数
>> 在第一次调用回调函数时，作为参数提供的值取决于 reduce 方法是否具有 initialValue 参数。
>> 
>> 如果向 reduce 方法提供 initialValue：
>> 
>> * previousValue 参数为 initialValue。
>> * currentValue 参数是数组中的第一个元素的值。
>>
>> 如果未提供 initialValue：
>> 
>> * previousValue 参数是数组中的第一个元素的值。
>> * currentValue 参数是数组中的第二个元素的值。
>> 
>> ###### 修改数组对象
>> 
>> |      reduce方法启动后的条件      |       元素是否传递给回调函数       |
>> |----------------------------------|------------------------------------|
>> | 在数组的原始长度之外添加元素。   | 否。                               |
>> | 添加元素以填充数组中缺少的元素。 | 是，如果该索引尚未传递给回调函数。 |
>> | 元素被更改。                     | 是，如果该元素尚未传递给回调函数。 |
>> | 从数组中删除元素。               | 否，除非该元素已传递给回调函数。   |

**返回值**  
通过最后一次调用回调函数获得的累积结果。

**示例**
``` javascript
function appendCurrent (previousValue, currentValue) {
  var result = (previousValue + "->" + currentValue);
  console.log('previous: ' + previousValue + ', current: ' + currentValue + ', result: ' + result);

  return result;
}

console.log([1, 2, 3, 4].reduce(appendCurrent)); //回调函数调用3次，输出结果为1->2->3->4
console.log('-----------------------------------------------');
console.log([1, 2, 3, 4].reduce(appendCurrent, 0)); //回调函数调用4次，输出结果为0->1->2->3->4
```

### Generator 函数

##### 声明方式
``` javascript
function* gen(x) {
  var y = yield x + 2;
  return y;
}

var g = gen(1); //gen {[[GeneratorStatus]]: "suspended"}
g.next(); //Object {value: 3, done: false}
g.next(); //Object {value: undefined, done: true}
```
声明时，在函数名称前加" **\*** "，用以与普通函数以示区别。

[学习地址传送门](http://www.ruanyifeng.com/blog/2015/04/generator.html)

### Promise
ES6 原生提供了 Promise 对象。可以有效解决回调地狱问题。

##### 回调地狱
``` javascript
setTimeout(function (name) {
  var catList = name + ',';
  setTimeout(function (name) {
    catList += name + ',';
    setTimeout(function (name) {
      catList += name + ',';
      setTimeout(function (name) {
        catList += name + ',';
        setTimeout(function (name) {
          catList += name;
          console.log(catList);
        }, 1, 'Lion');
      }, 1, 'Snow Leopard');
    }, 1, 'Lynx');
  }, 1, 'Jaguar');}, 1, 'Panther');
```
>由于回调函数是异步的，在上面的代码中每一层的回调函数都需要依赖上一层的回调执行完，所以形成了层层嵌套的关系最终形成类似上面的回调地狱，但代码以此种形式展现时无疑是不利于我们阅读与维护的。  
> 回调函数真正的问题在于他剥夺了我们使用 return 和 throw 这些关键字的能力。而 Promise 很好地解决了这一切。

##### 概念
所谓 Promise，就是一个对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的 API，可供进一步处理。

##### 特点
**1. 对象的状态不受外界影响。**

Promise 对象代表一个异步操作，有三种状态：

* Pending ----------------进行中  
* Resolved ---------------已完成
* Rejected ---------------已失败

只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。  
这也是 Promise 这个名字的由来，它的英语意思就是「承诺」，表示其他手段无法改变。

**2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。**

Promise 对象的状态改变，只有两种可能：  

* Pending --> Resolved
* Pending --> Rejected

只要这两种情况发生，状态就固定不再发生改变。  
就算改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。  
这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

##### 使用
``` javascript
var promise = new Promise(function(resolve, reject) {
  ......
  if( /* 异步操作成功 */ ) {
    resolve( /*返回值*/ );
  } else {
    reject( /*错误信息*/ );
  }

});

promise.then(function(value) {
  // 成功操作
  ......
}, function(value) {
  //失败操作
  ......
});
```
> Promise 构造函数接受一个函数作为参数，该函数的两个参数分别是 resolve 方法和 reject 方法。  
> 如果异步操作成功，则用 resolve 方法将 Promise 对象的状态，从「未完成」变为「成功」（即从 pending 变为 resolved）；  
> 如果异步操作失败，则用 reject 方法将 Promise 对象的状态，从「未完成」变为「失败」（即从 pending 变为 rejected）。
 
##### API
1. Promise.resolve()
2. Promise.reject()
3. Promise.all() 
4. Promise.prototype.then()
5. Promise.prototype.catch()

### RxJS
RxJS，Reactive Extensions for JavaScript，即JavaScript的响应式扩展。其思路是把随时间不断变化的数据、状态、事件等等转成可以被观察的序列（Observable Sequence），然后订阅序列中那些Observable对象的变化。一旦变化，就会执行事件安排好的各种转换和操作。

[RxJS入门](https://yq.aliyun.com/articles/65027)


