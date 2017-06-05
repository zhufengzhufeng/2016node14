### directive(指令)
- 1.指令是不依赖于控制器的
- 2.指令默认不会产生作用域 也可以声明创建作用域
>
- 1） 指令的分类 装饰型（负责添加功能的）  组件式（替换成一个完整的组件） 复用
- 2） 指令的格式
>
- 创建指令

```
    var app = angular.module('appModule',[]);
    app.directive('myDire',function () {
        return {

        }
    });

```

- 模板

```
   var app= angular.module('appModule',[]);
       app.directive('myDrag',function () {  //ECMA 命名规范多个单词以-连接，js中采用驼峰命名
          return { //默认生效的是AE
              restrict:'ACEM', //限制使用范围 每一个字母代表一种格式
              replace:true,//替换外层的原有标签，要求模板必须有一个根节点
              template:`<div>
                           <h1>HELLO angular</h1>
                           <p>你好</p>
                        </div>`
              //只要涉及到模板替换,外面就要有一个根节点
          }
       });
```

```
    Attribute   简写 A  属性
    Element     简写 E  元素
    Class       简写 C 名字
    Comment     简写 M 注释
    directive:my-drag
```
### 装饰型指令
- restrict  限制范围
- link  连接作用域和视图的
  + link中的scope --》代表当前指令所在作用域，指令没有创建作用域，使用的是上一级
  + 当前指令所在的元素,angular中要求增加单位px
  + attr 代表的是当前指令上的所有属性
```
    var app = angular.module('appModule',[]);
        app.directive('colorRed',function () {
            return {
                restrict:'A',//限制范围
                //操作dom
                link:function (scope,element,attr) { //链接 连接作用域和视图的
                    //scope代表当前指令所在作用域，指令没有创建作用域，使用的是上一级
                    //当前指令所在的元素,angular中要求增加单位px
                    element.css({'color':'red','font-size':'100px'});
                    //attr 代表的是当前指令上的所有属性
                    console.log(attr);
                    //1.mousedown mousemove mouseup   //DOMMouseScroll  mousewheel
                }
            }
        })
```
### 组件式
- attr props 想传递一些数据可以通过属性传递
```
    var app = angular.module('appModule',[]);
        app.directive('panel',function () {
            return {
                restrict:'E',
                //封装好了一个组件 可以多次调用
                templateUrl:'tmpl/panel.html',//通过路径 引用html
                transclude:true,//保留内容，嵌入到panel中
                /*link:function (scope,element,attr) {
                    //想拿到attr中的属性放到heading中
                    scope.title = attr.title; //将数据挂在作用域上 视图可以通过{{}}来获取值
                },*/
                scope:{
                    title:'@title' //scope.title = attr.title;
                }  // {} 通过true {} 创建一个独立作用域
            }
        })
```
### scope
- scope:true  不断绝父子关系，仍然可以向上查找
- scope:{}    断绝父子关系，就是和$rootScope平级

```
    <body>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <input type="text" ng-model="title">
                <!--tit和say的名字 是为了做传递   后面的内容都是作用域上定义的数据或者方法-->
                <panel tit="title" say="say(tit)">这是内容1</panel>
                <panel tit="title" say="say(tit)">这是内容2</panel>
                <panel tit="title" say="say(tit)">这是内容3</panel>
            </div>
        </div>
    </div>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.run(['$rootScope',function ($rootScope) {
            $rootScope.title = 'zfpx';
            $rootScope.say = function (title) {
                alert(title);
            }
        }]);

        app.directive('panel',function () {
            return {
                restrict:'E',
                templateUrl:'tmpl/panel.html',//通过路径 引用html
                transclude:true,//保留内容，嵌入到panel中
                scope:{  //scope.title = 'zfpx1';//声明的是一个字符串 和外面作用域上的值没关系
                    title:'@tit',//@符号取得是 title对应的字符串 ,前后名字相同可以去掉
                    sayTitle:'&say'
                } // title sayTitle是当前视图调用时用的名字
                //默认不产生作用域 而且拥有父子关系
                //true不断绝父子关系，仍然可以向上查找
                //{} 断绝父子关系，就是和$rootScope平级
            }
        })
    </script>
    </body>
```
### @符号

```
    <body>
    <input type="text" ng-model="zfpx">
    <panel tit="{{zfpx}}"></panel>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.run(['$rootScope',function ($rootScope) {
            $rootScope.zfpx = 'zfpx'
        }]);
        //@符号取得是字符串  @符是单项的
        app.directive('panel',function () {
            return {
                restrict:'E',
                template:'<input ng-model="title"/>',//通过路径 引用html
                scope:{
                    //属性的值发生变化 会重新给指令赋新的值
                    title:'@tit' //scope.title = 'zfpx'
                }
            }
        })
    </script>
    </body>
```

### = 符号

```
    <body>
    <input type="text" ng-model="title">
    <panel tit="title"></panel>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.run(['$rootScope',function ($rootScope) {
            $rootScope.title = 'zfpx'
        }]);
        //@符号取得是字符串  @符是单项的
        //=号传递的是变量    =是双向的
        app.directive('panel',function () {
            return {
                restrict:'E',
                template:'<input ng-model="bb"/>',//通过路径 引用html
                scope:{
                    //属性的值发生变化 会重新给指令赋新的值
                    bb:'=tit' //scope.bb =$rootScope.title =
                }
            }
        })
    </script>
    </body>
```
### &符号(and符)

```
    <body>
    <input type="text" ng-model="title">
    <panel ss="say(a,b)"></panel>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //指令中拥有独立作用域，将方法通过属性传递到指令中 传递的时候需要用(属性="作用域上的方法()",指令中用&符号来接收
        var app = angular.module('appModule',[]);
        app.run(['$rootScope',function ($rootScope) {
            $rootScope.say = function (who,where) {
                alert(who);
                alert(where)
            }
        }]);
        app.directive('panel',function () {
            return {
                restrict:'E',
                template:`<button ng-click="s({a:'你好',b:'不好'})">say</button>`,
    //            template:'<button ng-click="s({a:\'你好\'})">say</button>',
                scope:{
                    s:'&ss'
                }
            }
        })
    </script>
    </body>
```

### open

- ng-hide或者ng-show操作的是样式
- ng-if操作的是dom元素 （会产生一个作用域）如果if为false则内部代码不执行

```
    <script>
        var app = angular.module('appModule',[]);
        app.directive('listGroup',function () {
           return {
               restrict:'E',
               controller:function ($scope) {
                   $scope.lists = [];
                   this.collect = function (s) {
                       $scope.lists.push(s); //收集两个人作用域 可以控制两个人的flag属性
                   };
                   this.tell = function (s) {
                       $scope.lists.forEach(function (item) {
                           if(s!=item){
                               item.flag = false;
                           }
                       });
                   }
               },
               scope:true
           }
        });
        app.directive('listOne',function () {
            return {
                restrict:'E',
                require:'^?listGroup', //加上?如果找不到则是null
                templateUrl:'tmpl/open.html',
                transclude:true,
                link:function (scope,element,attrs,groupCtrl) {
                    groupCtrl.collect(scope);
                    scope.flag = false;
                    scope.toggle = function () { //点击时 告诉父亲 自己是谁，除了自己的都关闭掉
                        groupCtrl.tell(scope); //将自己的作用域传到父亲那
                        scope.flag =!scope.flag
                    }
                },
                scope:{
                    title:'@type'
                }
            }
        });
        /*app.controller('toggleCtrl',function ($scope) {
            $scope.flag = true;
            $scope.toggle = function () {
                $scope.flag =! $scope.flag;
            }
        })*/
    </script>
```
&
```
    <script>
        var app = angular.module('appModule',[]);
        app.directive('listGroup',function () {
            return {
                controller:function () {
                    var arr = [];
                    this.collect = function (s) {
                       arr.push(s);
                    };
                    this.tell = function (s) {
                        arr.forEach(function (item) {
                            if(item!=s){
                                item.flag = false;
                            }
                        })
                    }
                },
                scope:true
            }
        });
        app.directive('listOne',function () {
            return {
                require:'^?listGroup',
                templateUrl:'tmpl/open.html',
                transclude:true,
                link:function (s,e,a,groupCtrl) {
                    groupCtrl.collect(s);
                    s.flag = false;
                    s.toggle = function () {
                        groupCtrl.tell(s);
                        s.flag = !s.flag
                    }
                },
                scope:{
                    title:'@type'
                }
            }
        })
    </script>

```

### 指令和指令间的交互

```
    <body ng-controller="my">
    <girl cry eat>XXXX</girl>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.directive('girl',function () {
            return {
                //这个控制器 代表的就是指令的控制器
                controller:function ($scope) {
                   $scope.arr = []; //存放数据
                   this.say = function (kinds) { //点击xxxx弹出eat和cry
                       $scope.arr.push(kinds);
                   }
                },
                link:function (scope,element,attrs) {
                    element.on('click',function () {
                        alert(scope.arr);
                    });
                },
                scope:{}
            }
        });
        app.directive('eat',function () {
            return {
                require:'^girl',// ^ 找不到则向上找，如果找不到不想报错^?
                link:function (scope,element,attrs,gCtrl) {//会将girl的controller进行实例化，并且放在第四个参数上
                    //默认指令的作用域会向上查找，跳过包含他的指令
                    gCtrl.say('eat');
                },
            }
        });
        app.directive('cry',function () {
            return {
                require:'girl',
                link:function (scope,element,attrs,gCtrl) {
                    gCtrl.say('cry');
                },
            }
        });
    </script>
    </body>
```
### $watch

```
    <body ng-controller="myCtrl">
    <input type="text" ng-model="name" ng-change="change()">
    <span ng-bind="age"></span>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.controller('myCtrl',function ($scope) {
            //当输入的name值为偶数的时候 让age+1,如果age增加了 监控age如果age>10 弹层说age大于10了
            $scope.age = 1;
            //监控的第一个参数可以为作用域上属性名，还可以是函数
            //$scope.$watch('age');
            $scope.$watch(function () {
                console.log(1);
                //只要页面有任何一个值变化就会执行此函数,并且将函数的返回结果传入到另一个函数中
                return $scope.age;
            },function (newVal,oldVal) {
                //age发生变化就会执行
                if(newVal>4){
                    alert('age大于4了')
                }
            });
            $scope.change = function () {
                if($scope.name%2==0){
                    $scope.age++;
                }
            };
        })
    </script>
    </body>
```

### 例子 满百包邮

```
    <body ng-controller="myCtrl">
    商品名称 {{product.name}} <br>
    商品价格 {{product.price}}<br>
    商品数量 <input type="text" ng-model="product.count"><br>
    邮费 {{product.free}}<br>
    总额 {{sum()+product.free}}<br>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.controller('myCtrl',function ($scope) {
            $scope.sum = function () {
                return $scope.product.count*$scope.product.price
            };
            //脏值检查  效率低   数据劫持+观察者模式(on run ) Object.defineProperty
            $scope.$watch($scope.sum,function (newVal) {
                $scope.product.free = newVal>=100?0:10
            });
            /*$scope.$watch('sum()',function (newVal) {
                console.log(newVal);
            })*/
            $scope.product = {
                name:'可乐',
                price:2.5,
                count:1,
                free:10
            }
        })
    </script>
    </body>
```
### $apply

```
    <body ng-controller="myCtrl">
    {{date | date:'hh:mm:ss'}}
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.controller('myCtrl',function ($scope,$interval,$timeout) {
            var timer = $interval(function () {
                $scope.date = Date.now();
                $interval.cancel(timer); //取消定时器
            },1000); //angular提供的服务 都可以自动调用$apply,不需要手动调用   watch+apply就实现了 双向数据绑定
            /*setInterval(function () {
                $scope.date = Date.now();
                //不是angular中的方法，改变数据，不会刷新视图
                $scope.$apply();//告诉angular数据有变化，刷新视图
            },1000);*/
        });
    </script>
    </body>
```
### 自定义model

```
    <body>
    <input type="text" zf-model="aa">
    {{age}}
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //1.ng-model不会产生作用域
        //2.监控输入框中的输入的值 将值赋到作用域上  v->m
        //3.监控作用域上的值，将值赋给输入框  m->v
        var app = angular.module('appModule',[]);
        app.run(function ($rootScope) {
            $rootScope.aa = 'zfpx'
        });
        app.directive('zfModel',function () {
            return {
                restrict:'A',
                link:function (scope,element,attrs) {
                    //监控输入框中的输入事件         v->m
                    element.on('input',function () {
                        scope[attrs.zfModel] = element.val();
                        scope.$apply();//强制让视图刷新
                    });
                    //监控作用域上的值，将值赋给输入框 m->v
                    scope.$watch(attrs.zfModel,function (newVal) {
                        element.val(newVal);
                    });
                }
            }
        })
    </script>
    </body>
```
### 服务

### provider

- 最大的服务，可以配置
```
 app.config(function(CalcProvider){
       CalcProvider.age = 10;
 })
 app.provider('Calc',function(){
        this.age = 8;
        this.$get = function(){
            return {a:1}
        }
 })
```

### factory

```
  app.factory('Calc',function(){
      return {a:1}
  })
```

### service
```
   app.service('Clac',function(){
       this.a = 1;
   })
```

### value/constant
```
   app.value('Calc',{a:1});
   app.constant('Calc',{a:1});
```

### 事件例子

- 父子级关系

```
    <body>
    <div ng-controller="parent">
        <input type="text" ng-model="total">
        <div ng-controller="child">
            {{name}} <br>
            {{price}} <br>
            <input type="text" ng-model="count">
        </div>
    </div>
    <!--通过总价算数量，通过数量还能算总价-->
    <!--控制器 可以进行交互 $rootScope,服务,通过事件-->
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //交互方式 $on 监听 $emit 向上发射并且包括自己  $broadcast 向下发射并且包括自己
        var app = angular.module('appModule',[]);
        app.controller('parent',function ($scope) {
            //$scope.total = 10;
            $scope.$on('爸爸我买了雪碧',function (e,data) {
                $scope.total = data
            });
            $scope.$watch('total',function () {
                $scope.$broadcast('给儿子200块',$scope.total);
            });
        });
        app.controller('child',function ($scope) {
            $scope.name = '雪碧';
            $scope.price = 2.5;
            $scope.count = 4;
            $scope.$watch('count',function () {
                //当数量变化的时候 向上发射
                $scope.$emit('爸爸我买了雪碧',$scope.count*$scope.price);
            });
            $scope.$on('给儿子200块',function (e,data) {
                $scope.count = data/$scope.price;
            });
        });
    </script>
    </body>
```

- 平级关系
```
    <body>
    <div ng-controller="parent">
        <input type="text" ng-model="total">
    </div>
    <div ng-controller="child">
        {{name}} <br>
        {{price}} <br>
        <input type="text" ng-model="count">
    </div>
    <!--通过总价算数量，通过数量还能算总价-->
    <!--控制器 可以进行交互 $rootScope,服务,通过事件-->
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //交互方式 $on 监听 $emit 向上发射并且包括自己  $broadcast 向下发射并且包括自己
        var app = angular.module('appModule',[]);
        //发布订阅模式
        app.controller('parent',function ($scope,$rootScope) {
            //$scope.total = 10;
            $rootScope.$on('爸爸我买了雪碧',function (e,data) {
                $scope.total = data
            });
            $scope.$watch('total',function () {
                $scope.$emit('给儿子200块',$scope.total);
            });
        });
        app.controller('child',function ($scope,$rootScope) {
            $scope.name = '雪碧';
            $scope.price = 2.5;
            $scope.count = 4;
            $scope.$watch('count',function () {
                //当数量变化的时候 向上发射
                $scope.$emit('爸爸我买了雪碧',$scope.count*$scope.price);
            });
            $rootScope.$on('给儿子200块',function (e,data) {
                $scope.count = data/$scope.price;
            });
        });
    </script>
    </body>
```

### 百度搜索框

```
    <!DOCTYPE html>
    <html lang="en" ng-app="appModule">
    <head>
        <meta charset="UTF-8">
        <title>百度搜索框</title>
        <link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.css">
    </head>
    <body ng-controller="searchCtrl" class="container">
    <div class="text-center h2 text-danger">搜索框</div>

    <input type="text" ng-model="query" ng-keyup="search($event)" class="form-control" ng-keydown="stop($event)">

    <ul class="list-group">
        <!--当前被选中的增加active属性-->
        <li ng-repeat="list in lists track by $index" class="list-group-item " ng-class="{active:index==$index}">{{list}}</li>
    </ul>

    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.controller('searchCtrl',function ($scope,$http,$sce) {
            //服务 提取公有方法，是单例的（只有一个）
            //$http  - > $.ajax
            //后台会返回一个方法执行 xxx() 必须叫cb
            $scope.index = -1;
            $scope.query = '';
            $scope.lists = [];
            $scope.stop = function (e) {
                var code = e.keyCode;
                if(code==38)
                e.preventDefault();
            };
            $scope.search = function (e) {
                var code = e.keyCode;
                if(code==13){//回车时打开新的页面
                    return window.open('https://www.baidu.com/s?wd='+$scope.query);
                }
                if(code==38 || code == 40){
                    if(code==38){ //上键
                        $scope.index--;//当等于第一个或者小于第一个(默认可能会按上)
                        if($scope.index<-1){
                            $scope.index = $scope.lists.length-1;
                        }
                        if($scope.index == -1 ){
                            $scope.query = $scope.temp;//越界后返回默认搜索的内容
                        }else{
                            $scope.query = $scope.lists[$scope.index];//正常取值
                        }
                    }
                    if(code==40){ //下键
                        $scope.index++;//当等于最后一项时回到-1
                        if($scope.index>$scope.lists.length-1){
                            $scope.index = -1;
                            $scope.query = $scope.temp
                        }else{
                            $scope.query = $scope.lists[$scope.index];
                        }
                    }
                    return;
                }
                $http.jsonp(
                    $sce.trustAsResourceUrl('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query)
                    ,{jsonpCallbackParam:'cb'})
                .then(function (res) {//第一个函数就是成功后的回调
                    $scope.temp = $scope.query;//查询过一次 记录住查询的内容
                   $scope.lists = res.data.s
                },function (err) {
                    console.log(err);
                });
            };
        })
    </script>
    </body>
    </html>
```













