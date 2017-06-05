##### angular指令

> 指令不依赖于控制器（比如ng-model）

> 指令默认不会产生作用域，也可以声明创建作用域

- 指令的分类
  - 装饰型（负责添加功能）
  - 组件式（替换成一个完整的组件）
    + 组件的特点：复用
- 指令的格式
```
<!-- Attribute A -->
<div drag></div>
<!-- Class C -->
<div class="my-drag"></div>
<!-- Element E -->
<drag></drag>
<!-- directive:drag -->
<!-- Comment M -->
```

  > 指令默认返回一个对象
  
```
app.directive('myDrag', function () {
        //指令默认返回一个对象
        return {
            //默认生效的是A和E
            restrict: 'ACEM',  
            replace: true, 
            template: `<div>
                         <h1>hello angular</h1>
                         <p>good morning</p>
                       </div>`
        }
    });
```
   > 指令属性
   
   - restrict:限制使用范围，每一个字母代表一种格式
   - replace:替换外层原有标签，要求模板必须有一个根节点
   
   > 只要涉及到模板替换，外面要有一个根节点

   ```
app.directive('drag',function(){
     return {
         template: `<div>
         <h1>hello angular</h1>
         <p>good morning</p>
         </div>`
     }
})

   ```

   > 在页面中，先引用jquery，再引用angular，这样angular就不会加载自己的jquery;
   
##### 装饰型指令
```
app.directive('colorRed', function () {
return {
restrict: 'A', //限制范围
//操作DOM
link: function (scope, element, attr) { 
element.css({'color':'red','font-size':'100px'});
console.log(attr);
}
}
});
```
   > 装饰型指令说明
   
   - link:是个函数，用来链接作用域和试图,这个函数中可以操作DOM
   - scope:当前指令所在作用域，指令没有创建作用域，使用的是上级作用域，此例中上级作用域是根作用域（$rootScope）
   - element:当前指令所在元素
   - attr:当前指令身上的属性,比如指令panel身上的属性title
   ```
<panel title="标题1">这是内容1</panel>

   ```
##### 组件式指令

> html中的内容
```
<!--attr props 想传递数据可以通过属性传递-->
<panel title="标题1">这是内容1</panel>
<panel title="标题2">这是内容2</panel>
<panel title="标题3">这是内容3</panel>
<!-- <div class="panel panel-danger">
<div class="panel-heading">标题</div>
<div class="panel-body">内容</div>
<div class="panel-footer"></div>
</div>-->
//eg:panel是指令，目的是生成下面注释掉内容的格式的模板，并将指令身上的属性放在生成模板的头部，内容在body中
```

> js中的内容

```
app.directive('panel', function () {
return {
restrict: 'E',
templateUrl: "template/panel.html",
transclude: true, 
link:function (scope,element,attr) {
scope.title=attr.title; //将数据挂在作用域上，试图可以通过{{}}获取数据
},
// scope:true //也可写成{}，表示产生独立作用域
scope:{
title:'@title'
}
}
});
```
- transclude: true,表示保留指令内容，指定放在哪个位置
- templateUrl:引用模板的路径，直接引用html文件，而不是字符串拼接模板
- link:用来链接试图和作用域。
- scope：true/{}， 产生独立的作用域
##### scope

> scope:true和{}的区别

- scope本来默认不产生作用域，而且拥有父子关系
- true不断绝父子关系，找不到会向上查找
- {}会断绝父子关系，和$rootScope平级，而且这个对象中只能放通过=、&、@传递进来的属性。
##### scope{}中传递属性的方式
- @
  + 传递的是字符串值
  + 如果前后名字相同，后面可以省掉，
  + 在指令中只能通过{{}}获取属性值
  + @是单向绑定
```
<input type="text" ng-model="word">
<panel title="{{word}}">这是内容1</panel>
//js中
link:function (scope,element,attr) {
scope.title=attr.title;
},
scope:{
title:'@title',//也可写成如下格式
title:'@'
}
```
> 只有经指令panel的属性title的值挂在scope这个作用域上，才能通过@传递，

```
<input type="text" ng-model="word" class="outer">
            <panel title="{{word}}">这是内容1</panel>
<!-- <div class="panel panel-danger">
                 <div class="panel-heading"></div>
                 <div class="panel-body"></div>
                 <div class="panel-footer"><input type="text" ng-model="title"></div>
     </div>-->
<script>
    var app = angular.module('appModule', []);
    app.run(['$rootScope',function ($rootScope) {
        $rootScope.word='hello';
    }]);
    app.directive('panel', function () {
        return {
            restrict: 'E',
            templateUrl: "template/panel.html",
            transclude: true,
            link:function (scope,element,attr) {
                scope.title=attr.title;
            },
            scope:{
                title:'@title',
            }
        }
    });
</script>
```
> 1.如果在页面中修改最外层input中的值，试图改变，数据也会改变，相当于修改了根作用域$rootScope上word的值，导致指令中title属性绑定的值也发生改变，所以模板中所有input的
值也跟着改变。。
2.@是单向绑定，所以在改变模板中input的值的时候，相当于在它自己的域scope上重新声明了一个值scope.title=newValue,但是不会影响根作用域上word的值，所以模板中input值改变，
最外层input的值不会变

- =
  + 是双向绑定
  + 取到的是变量
```
<input type="text" ng-model="word" class="outer">
            <panel title="word">这是内容1</panel>
<script>
    var app = angular.module('appModule', []);
    app.run(['$rootScope',function ($rootScope) {
        $rootScope.word='hello';
    }]);
    app.directive('panel', function () {
        return {
            restrict: 'E',
            template: `<input type="text" ng-model="title">`,
            scope:{
                title:'=title'
            }
        }
    });
</script>
```
> 指令的属性title绑定的是根作用域$rootScope下的word变量，则在模板中取值时用=而不用@，因为用@相当于取到的是变量名word，而不是它的值，
1.这种绑定方法，导致scope上绑定的title和$rootScope上的word指向同一个空间，里面存着相同的值。所以一边改变，全部都改变，所以=是双向绑定，取到的是变量，@取到的是字符串值。

- &
  + 一般用来传递方法

> 指令中拥有独立作用域，将方法通过属性传递到指令中，传递的时候需要用（属性=‘作用域上的方法（）’，指令中用&符号来接受）
```
<panel title="word" ss="say(c,l)">这是内容1</panel>
<script>
    var app = angular.module('appModule', []);
    app.run(['$rootScope',function ($rootScope) {
        $rootScope.say=function (who,where) {
            alert(who);
            alert(where)
        };
    }]);
    app.directive('panel', function () {
        return {
            restrict: 'E',
            template: `<button ng-click="s({c:\'你好\',l:'老地方'})">say</button>`,
            scope:{
                s:'&ss'
            }
        }
    });
</script>
```
##### 指令与指令间的相互交互

> 给div设置显示方式的方法

- ng-hide操作的是样式
- ng-if操作的是DOM元素，会产生一个作用域，如果if为false，则内部代码不执行
```
<style>
        .show {display: block;}
        .hidden {display: none;}
</style>
<div ng-hide="flag"><div>
<div ng-if="flag"></div>
<div ng-class="{true:'hidden',false:'show'}[flag]"><div>
```
- 指令间的交互
  + 指令有控制器controller，而且会产生一个作用域$scope,它和scope指的是同一个作用域
  + ^?'girl'表示先找当前作用域，找不到往上级找，？的作用是找不到还不会报错
  + 指令cry和eat中的link函数会将girl的controller进行实例化，并且放在cry和eat指令link函数的第四个参数
```
<girl cry eat>xxx</girl>
app.directive('girl', function () {
        return {
            //这个控制器代表的就是指令的控制器
            controller: function ($scope) {
                $scope.arr=[];
                this.say = function (kinds) {
                    //功能：点击xxx，弹出eat和cry
                    $scope.arr.push(kinds);
                }
            },
            scope:{},
            link:function (scope, element, attr) {
                element.on('click',function () {
                    alert(scope.arr);
                });
            }
        }
    });
    app.directive('cry', function () {
        return {
            require: 'girl',   //^girl表示先找当前域，找不到往上级找，如果找不到不想报错，可在^后加？
            //link函数会将girl的controller进行实例化，并且放在第四个参数上
            link: function (scope, element, attr, gCtrl) {
                gCtrl.say('eat');
            }
        }
    });
    app.directive('eat', function () {
        return {
            require: '^girl',
            link: function (scope, element, attr, gCtrl) {
                //默认指令的作用域会向上查找，跳过包含的指令
//                scope.a=100;
                gCtrl.say('cry');
            }
        }
    })

```
##### provider
- 最大的服务，可以配置
```
app.config(function(){
    NameProvider.age=10
})
//服务名首字母须大写
app.provider('Name',function(){
     this.age=8;
     this.$get=function(){
     return {}
     }
})
```
##### factory
```
app.factory('Calc',function () { 
        this.currency='$';
        return {
            '+':(a,b)=> {
                return this.currency+(a+b);
            }
        }
    });
```
##### service
```
 app.service('Calc',function () {
        this['+']=(a,b)=>a+b;
        //factory返回的对象是service函数实例化的结果
    });
```
##### value
```
 app.value('Calc',{
        //调用的是factory
        '+': (a,b)=>a+b
    });
```
##### constant
```
app.constant('Calc',{
        //调用的是factory
        '+': (a,b)=>a+b 
    });
```
##### $watch

> 监控的第一个参数可以为作用域上的属性名，还可以是函数

- 第一个参数是作用域上的属性名
```
 app.controller('myCtrl', function ($scope) {
        $scope.age = 1;
       $scope.$watch('age', function (newVal, oldVal) {
            //age发生变化就会执行
            if (newVal > 4) {
                alert('age大于4了')
            }
        })
        $scope.change = function () {
            if ($scope.name % 2 == 0) {
                $scope.age++;
            }
        }
    });
```
- 第一个参数是个函数

> 只要监听的页面内容有任何变化就会执行$watch中的第一个函数，并且将函数的返回结果传入到第二个函数中
```
app.controller('myCtrl', function ($scope) {
        $scope.age = 1;
        $scope.$watch(function () {
            //只要页面有任何一个值变化就会执行此函数，并且将函数的返回结果传入到另一个函数中
            console.log(1);
            return $scope.age;
        }, function (newVal,oldVal) {
            if (newVal > 4) {
                alert('age大于4了')
            }
        });
        $scope.change = function () {
            if ($scope.name % 2 == 0) {
                $scope.age++;
            }
        }
    });
```
##### $apply
> $apply作用：告诉angular数据有变化，刷新视图
```
app.controller('myCtrl',function ($scope,$interval,$timeout) {
        //angular提供的服务（$interval）都可以自动调用$apply,不需要手动调用
        var timer=$interval(function () {
            $scope.date=Date.now();
            $interval.cancel(timer);  //取消定时器
        },1000);
        //watch+apply就实现了双向数据绑定
        /*setInterval(function () {
            //不是angular的方法，改变数据不会刷新试图
            $scope.date=Date.now();
            $scope.$apply();
        },1000);*/
    })
```

> setInterval不是angular的方法，所以使用$apply后，改变数据不会刷新视图，angular提供的$interval
服务可以自动调用$apply，不需要手动调用就可以实现视图和数据同步。

##### 事件

> 控制器可以通过$rootScope,服务，事件进行交互

- 交互方式
  + $on监听 
  + $emit向上发射并且包括自己 
  + $broadcast向下发射并且包括自己

> 监听和发射要保证事件是同一个。
```
 app.controller('parent', function ($scope) {
        $scope.total = 10;
        $scope.$on('买了雪碧', function (e, data) {
            $scope.total = data;
        });
        $scope.$watch('total', function () {
            $scope.$broadcast('买几瓶', $scope.total)
        })
    });
    app.controller('child', function ($scope) {
        $scope.name = '雪碧';
        $scope.price = 3;
        $scope.count = 4;
        //on须再emit前
        $scope.$watch('count', function () {
            $scope.$emit('买了雪碧', $scope.count * $scope.price)
        });
        $scope.$on('买几瓶', function (e, data) {
            $scope.count = data/$scope.price;
        });
    })
```
##### 平级交互

> 两个控制器是平级，则只能向上发射（$rootScope）,所以只能通过$rootScope.$on()监听。
```
app.controller('parent', function ($scope,$rootScope) {
        $scope.total = 10;
        $rootScope.$on('买了雪碧', function (e, data) {
            $scope.total = data;
        });
        $scope.$watch('total', function () {
            $scope.$emit('买几瓶', $scope.total)
        })
    });
    app.controller('child', function ($scope,$rootScope) {
        $scope.name = '雪碧';
        $scope.price = 3;
        $scope.count = 4;
        $scope.$watch('count', function () {
            $scope.$emit('买了雪碧', $scope.count * $scope.price)
        });
        $rootScope.$on('买几瓶', function (e, data) {
            $scope.count = data/$scope.price;
        });
    });

```


