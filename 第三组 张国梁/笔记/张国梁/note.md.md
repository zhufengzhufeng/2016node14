##### 服务器端没有window对象,后台最大的是global对象

- console.log(global) 在文件中运行this不是global

## global上的属性

- process 进程对象
    - 每次开启一个node程序 就会开一个进程
    - 代码跑完后,会自动关闭进程
    + nextTick 下一队列(当前队列的底部)
    + process.pid (进程的id)
    + process.kill() 杀死进程
    + process.cwd();  当前工作目录  (__dirname不可以更改的)
    + process.exit(); 退出进程
- Buffer  缓存 16进制  二进制
- clearImmediate: [Function], 立即清除
- clearInterval: [Function],  
- clearTimeout: [Function],
- setImmediate: [Function],   设置立即
- setInterval: [Function],
- setTimeout: [Function]
- console: [Getter] }          输出

## console

- console.log('log');
- console.dir({name:1});
- console.info('info');
- console.error('error');
- console.warn('warn');
- console.time('start');
- console.timeEnd('start');

> time和timeEnd的参数必须传入一个字符串,并且是同一个

> console.log的顺序是不定的

```
var a=100; 
console.log(global.a)  undefined
global.a=100;
```

> 通过 var 声明不能直接声明在global上

> 如果在global上声明属性global.xxx

###### const   常量  let  var
- 全部用const 如果需要变的值 let
- import export 不支持

###### setTimeout

> 异步的

```
function eat(who){
    console.log(this) setTimeout中的this是他自己
    console.log(who+'吃'+'foods')
}
setTimeout(eat,1000,'小猪','冰淇淋')
setTimeout(eat.bind(global),1000,'小猪','冰淇淋')
```
> 在第二个参数之后,都是给运行的函数传递值

> bind可以改变this执向

##### 异步的
- process.nextTick  同步代码执行后将会执行的方法(第一个小本尾部)
- setTimeout        唯一能设置时间的      (服务员第二个小本)
- setImmediate      异步在nextTick后执行 (服务员第二个小本)

##### 事件环
```
console.log('A');

process.nextTick(function () {
  console.log('去厕所');
});
console.log('B');
setImmediate(function () {
    console.log('立即去端饭');
})
setTimeout(function () {
    console.log('扫地');
    //如果不写时间约等于 setImmediate可能会在setImmediate之前执行
});
```
> 同步 > process.nextTick> setImmediate> setTimeout

- global上的属性可以在任意地方访问

##### 全局属性  五个形参;
- export
- require
- module
- __dirname     directory name(目录名)
- ____filename   绝对路径,不可变的 (当前文件)

> 在我们运行文件的过程中,会给这个文件套一层闭包函数,函数中拥有形参

##### process

- process.exit();退出进程 过三秒 关掉进程

```
setInterval(function () {
    process.exit();
},3000);
```
- process.kill(pid 12000) 杀死进程
- process.cwd(); current working directory当前工作目录
    + cwd代表的是当前执行代码的文件夹的绝对路径(可以改变)
## 模块化
- cmd(seajs)就近依赖  amd(requirejs) 依赖前置
- commonjs (node)  读写文件io操作
- 有利于分工协作
- 高内聚,低耦合
- 实现代码复用

> 闭包,命名空间(不可能完全避免命名冲突,调用过长)

## 模块化
- 定义模块  (创建一个JS文件)
- 使用模块  (require)
- 导出模块  (exports/module.exports)

## 文件模块
- 引用方式要通过相对路径引用 ('./');
- 引用多次 只执行一次

## 第三方模块
- npm管理后台文件的  管理前段文件bower
- 别人的模块 npm
- 下载文件通过npm
    + 全局下载      (只在命令行下使用)
        - nrm工具(切换源的)
        ```
        npm install nrm -g
        ```
        - 添加一个源
        ```
        nrm add zf1 http://172.18.1.139
        ```
        - 测试源
        ```
        nrm test
        ```
        - 切换源
        ```
        nrm use taobao
        ```
        - nodeppt start
        ```
        npm install nodeppt -g
        ```
    + 本地下载      (在代码里使用的)
        -记录安装过的所有模块
        ```
        npm init(记录所有的依赖关系) -y 默认确定
        ```
        - 依赖      (开发 上线都需要)
        ```
        npm install jquery --save 缩写(-S)
        ```
        - 开发依赖   (只在开发 使用)
        ```
        npm install babel-core --save-dev 缩写(-D)
        ```
- 卸载模块
```
npm uninstall nodeppt -g
npm uninstall jquery -S
npm uninstall gulp -D
```
#### 安装less webpack --save-dev
### 安装bootstrap --save
### 安装animate.css --save
### 查看版本
```
npm info jquery
```

> 默认安装到node_modules文件夹下,不指定版本默认为最新版本

## bower
- 先安装bower
```
npm install bower -g
```
- 初始化bower.json
```
bower init
```
- 下载插件
```
bower install jquery --save
```

> 默认安装到bower_components

### bower可以指定目录安装
```
touch  .bowerrc
{"directory":"lib/js"}
```

### 发包
- 找到带有package.json的文件夹
- 切换到官方npm上
```
nrm use npm 
```
- 登录账号
```
npm addUser
```
- 发布
```
npm publish
```
### 模块的查找方式

> 下载的第三方必须存放在 node_modules文件夹下

```
require('./node_modules/zfpxhello/index.js');
第三方模块可以直接使用包名  引用
var str = require('zfpxhello');
console.log(str);
console.log(module.paths);
```
- 先查找当前目录下Node_modules文件,
- 找到名字相同的文件夹,
- 找到package.json看里面的main,运行main对应的文件;
- 找不到会向上级查找node_modules文件,直到找到为止(根目录),
- 找不到则报错(如果找不到会默认找index.js);

## 核心模块/内置模块
- node自带的模块,使用不需要安装,可以直接
- util工具

## 搭建博客
```
npm install hexo-cli -g  //提供一个命令 hexo
```
- 初始化博客
```
hexo init
```
- 启用
```
hexo -serve  -p （该端口号 ）
```
- 下载主题
```
git clone
```
> themes 下 _config.yml  里面theme: hexo-theme-next （改变主题）

## hexo g
> 生成发布的内容

## 安装插件
```
npm install hexo-deployer-git --save
```
## 配置用户路径
## 新建仓库
> 名字要求: github账号名.github.io

## 指令
```
<!-- 属性 Attribute A-->
<div my-drag></div>
<!-- Element       E-->
<my-drag></my-drag>
<!-- Class         C-->
<div class="my-drag"></div>
<!-- comment       M-->
<!-- directive:my-drag -->
```
- 指令是不依赖于控制器的
- 指令默认不会产生作用域 也可以声明创建作用域()
    + 指令的分类 装饰型(负责添加功能的)  组件式(替换成一个完整的组件):特点 复用
    + 指令的格式
```
var app = angular.module('appModule', []);
    app.directive('myDrag', function () {  //ECMA 命名规范多个单词已 - 链接, js中采用驼峰命名
        return {                  //默认生效的是AE
            restrict: 'ACEM',      //限制使用范围 每一个字母代表一种格式
            replace: true,         //替换外层的原有标签,要求模板必须有一个根节点
            template: `<div>
                            <h1>Hello angular</h1>
                            <p>你好</p>
                       </div>`
            //只要涉及到模板替换 ,外面就要有一个根节点
        }
    });
```

#### 装饰性指令
```
 var app = angular.module('appModule',[]);
    app.directive('colorRed',function () {
       return{
           restrict:'A',//限制范围
           //操作 dom
           link:function (scope,element) { //链接 连接作用域和视图的
               // scope代表当前指令所在作用域,指令没有创建作用域,使用的是上一级
              // element当前指令所在的元素
               console.log(element);
               //当前指令所在的元素,angular中要求带单位
               element.css({'font-size':'10px'});
               //attr 代表的是当前指令上的所有属性
               //1. mousedown mousemove mouseup    // DOMMouseScroll mousewheel

           }
       }
    });
```

#### 组件式
- 先要保留指令之间夹着的内容,指定放在哪个位置
- attr props 想传递一些数据,可以通过属性传递

> 例子

```
 var app = angular.module('appModule',[]);
    app.directive('panel',function () {
        return{
            restrict:'E',
            //封装好了一个组件,可以多次调用
            templateUrl:`template/panel.html`, //通过路径 引用html
            transclude:true,  // 保留内容 嵌入到panel中  默认false
            link:function (scope,element,attr) {
                //想拿到attr中的属性 放到heading中
               scope.title=attr.title;  //将数据挂在作用域上,视图可以通过 {{}}来获取值
            },
            //scope:true    // 默认false不产生作用域   或者   {} 创建一个独立的作用域
            scope:{
                title:'@title'   //  scope.title=attr.title;
            }         //{} 通过true {}创建一个独立作用域
        }
    })
```
- 默认不产生作用域  而且拥有父子关系
-  true不断绝父子关系,仍然向上查找
- {}断绝父子关系, 就是和$rootScope平级

#### @ = &并且符
> @ 父级改变,子级 也变, 子变 父级作用域不变,符号取得的是 字符串    @符是单向的

- 将数据挂在作用域上,视图可以通过 {{}}来获取值
- @符合 取得的是title对应的字符串  名字相同可以删掉 title:'@'

```
scope:{//  scope.title=zzq;  声明的是一个字符串，和外面作用域上的值没关系
                title:'@title'   // @符合 取得的是title对应的字符串  名字相同可以删掉 title:'@'

                 //{} 通过true {}创建一个独立作用域
             }

```
> =  = 号 传递的是变量       =是双向的

- 属性的值发生变化  会重新给指令赋新的值
- 所以 无论传递的改变  还是原来的值改变,大家都会变

> & 并且符

- 指令中拥有 独立作用域,将方法通过属性传递到指令中,传递的时候 需要用(属性="作用域上的方法()",指令中用 &并且符来接收);
```
scope{
    s:&ss 方法名 是声明过的要一样
}
```

#### 手风琴angular
```
   var app = angular.module('appModule', []);
    app.controller('toggleCtrl',function ($scope) {
        $scope.flag=true;
        $scope.toggle=function () {
            $scope.flag=!$scope.flag;
        }
    });
```

#### 指令之间的交互
-  require:'^girl', // ^ 找不到则向上找,如果找不到 不想报错  ^?

```
 link:function (scope,element,attrs,gCtrl) {// 会将girl的controller进行实体化,并且放在第四个参数上;
                //默认指令的作用域会向上查找,跳过包含它的指令
                gCtrl.say('eat')
                
            }
```

#### $watch 
- 监控的第一个参数可以为作用域上的属性名,还可以是函数
- 只要页面有任何一个值变化就会执行此函数,并且将函数的返回结果传入到另一个函数里
```
$scope.$watch(function(){
       return $scope.age
},function(newVal,oldVal){
    newVal是传递的值  在操作一次 就是oldVal
})
```

#### $apply
```
setInterval(funcation(){
    $scope.data = Date.now();
    //不是angular中的方法,所以改变数据,不会刷新视图
    $scope.$apply(); //告诉angular数据有变化,刷新视图
},1000)
```

> 不是angular中的方法,需要$apply来应用;

```
 var app=angular.module('appModule',[]);
    app.controller('myCtrl',function ($scope,$interval,$timeout) {
        var timer=$interval(function () {
            $scope.date=Date.now();
            $interval.cancel(timer); //取消定时器
        },1000);   // angular提供的服务,都可以自动调用  $apply,不需要手动调用  watch+apply就实现了双向数据绑定
```

> angular里自带的方法;

#### 视图->模型  模型->视图
- ng-model不会产生作用域
- 监控输入框中的值,将值赋到做作用域上,  v-m
- 监控作用域上的值,将值赋给输入框,      m-v
```
var app = angular.module('appModule',[]);
app.directive('zfModel',function(){
    return {
        restrict:'A', (属性)
        link:function(scope,element,attrs){
        //监控输入框中的输入事件   V-M
            element.on('input',function(){
                scope[attrs.zfModel]=element.val();
                scope.$apply();
            });
            scope.$watch(attrs,zfModel,function(newVal){
            element.val(newVal);
            })
        }
    }
})
```

#### jsonp

> jsonp跨域 协议 域名 端口 有一个不满足就是跨域

```
 $scope.search = function () {
            // $http   ->  $.ajax
            //后台会返回一个  XXX()  必须叫 cb()
            $http.jsonp($sce.trustAsResourceUrl('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query),{
                jsonpCallbackParam: 'cb'
            }).then(function (res) {  //第一个函数就是成功后的回调
                $scope.lists=res.data.s;
            },function () {
                
            });
        };
```

> 后台返回一个回调函数 必须叫做 cb()

## provide
- 最大的服务,可以配置
```
app.config(function(CalcProvider){
    CalcProvider.age=10;
})
app.provider('Calc,function(){
       this.age=8;
       this.$get = function(){
       return {a:1}
       }
})
```

## factory
```
app.factory('Calc,function(){
        return {a:1}
})
```

## service
```
app.service('Calc,function(){
       this.a=1;
})
```

## value/constant
```
app.value('Calc,{a:1})
app.constant('Calc,{a:1})

```

#### 平级事件 发布订阅 >观察者模式
-  交互方式: $on 监听  
- $emit 向上发射并且包括自己   
- $broadcast 向下发射并且包括自己

