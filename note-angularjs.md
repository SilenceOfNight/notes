# AngularJS

## <font id="service" style="color:#434A54">服务</font>
AngularJS支持使用服务的体系结构“关注点分离”的概念。服务是JavaScript函数，并负责只做一个特定的任务。这也使得他们即维护和测试的单独实体。服务使用AngularJS的依赖注入机制注入实例。
#### 内置服务
###### $timeout

###### $interval
window.setInterval的Angular包装形式。Fn是每次延迟时间后被执行的函数。  

间隔函数的返回值是一个承诺。  
这个承诺将在每个间隔刻度被通知，并且到达规定迭代次数后被取消，如果迭代次数未定义，则无限制的执行。通知的值将是运行的迭代次数。取消一个间隔，调用$intreval.cancel(promise)。  

备注：当你执行完这项服务后应该把它销毁。特别是当controller或者directive元素被销毁时而$interval未被销毁。你应该考虑到在适当的时候取消interval事件。

**使用：**$interval(fn,delay[,count[,invokeApply[,Pass]]]);
> ###### 参数说明
> * fn 将重复执行的函数。
> * delay 每次调用的间隔毫秒数。
> * count 循环次数。不设置，则无限循环。
> * invokeApply 如果设置为false，则避开脏值检查，否则将调用$apply。
> * Pass 函数的附加参数。

**暴露方法：**
*cancel(promise);*
取消与承诺相关联的任务。

``` html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Example for AngularJS</title>
    <script src="https://cdn.bootcss.com/angular.js/1.4.6/angular.min.js"></script>
</head>

<body ng-app="app">
    <div ng-controller="exampleCtrl">
        <p>
            {{message}}
        </p>
        <button ng-click="onClick()">点击</button>
    </div>
    <script>
    var app = angular.module('app', []);
    app.controller('exampleCtrl', ["$scope", "$interval", function($scope, $interval) {
        $scope.message = "点击下面按钮，将追加3次文本内容。每次追加间隔为1S。"
        $scope.onClick = function() {
            $interval(function() {
                $scope.message += '哈哈哈哈!';
            }, 1000, 3);
        }
    }])
    </script>
</body>

</html>
```

#### 自定义服务