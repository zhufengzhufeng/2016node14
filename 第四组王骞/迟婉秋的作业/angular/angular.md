## angular 
#### 1.directive 指令
> 1,指令时不依赖于控制器的
> 2,指令默认不会产生作用域的，但可以声明创建作用域
####2，指令的分类
+ 装饰型；负责添加功能的
+ 组件式：替换成完整的组件，可以复用
#### 3,指令的格式
```
var app=angular.module('appModule',[]);
app.directive('myDrag',function(){
     restrict:'限制使用范围的的格式'

})
```
#### 4.常用的格式
+ 设置属性 Attribute   (A)
```
<div my-drag></div>
```
+ 标签 Element (E)
````
<my-drag></my-drag>
````
+ 类名 Class (C)
```
<div class='my-drag'></div>
```
+ 注释 Comment (M)
````
<!-- directive:my-drag -->
````

#### 模板替换
+ template:要替换的模板
+ 注意：替换外层的原有标签，要求模板必须有一个根节点 replace:true
####  装饰型指令
 ````
 <div color-red>hello zpfx</div>
 <div drag></div>
````
+  link:function(scope,element,attr){}作用：链接作用域和视图的
+ scope:代表当前指令的所在作用域，指令没有创建作用域，使用的是上级。
+ element:当前指令所在的元素，设置字体或是需要单位的时候，angular需要增加单位。
+ attr:代表当前指令上的所有属性。
#### 组件式指令
````
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <!--attr props 想传递一些数据可以通过属性传递-->
            <panel title="这是标题1">这是内容1</panel>
            <panel title="这是标题2">这是内容2</panel>
            <panel title="这是标题3">这是内容3</panel>
        </div>
    </div>
</div>
````
+ 上面就是简单的指令模型，保证指令之间夹着内容，指令放在哪个位置。
+ templateUrl:'tmpl/panel.html',//通过路径 引用html。
+ transclude:true,//保留内容，嵌入到panel中，在对应要填入的内容标签上填写ng-transclude.
#### scope
+ 声明的是一个简单的字符串，和外面作用域上的值没有关系，
+ '@':取的是对应属性的字符串，实现单向数据绑定，属性值发生改变，会重新给指令赋予新值。
+ '=':传递的是变量，是双向数据绑定，属性值发生改变，会重新给指令赋予新值。
+ '&':指令中拥有独立作用域，将方法通过属性传递到指令中 传递的时候需要用(属性="作用域上的方法()",指令中用&符号来接收
+ scope:{},断绝父子关系，和$rootScope平级。
+ scope：true,不断绝父子关系仍然可以向上级查找。
#### ng-
+ ng-hide或者ng-show操作的是样式.
+ ng-if操作的是dom元素 （会产生一个作用域）如果if为false则内部代码不执行.

### 监听
##### $watch
+ 监控的第一个参数可以是作用域上的属性名，还可以是函数
> $scope.$watch('age')/$scope.$watch(function(){})
+ 只要页面有任何一个值变化就会执行此函数，并将函数的返回结果传入到另一个函数中。
+ 实现双向数据绑定。
##### $apply
+ 告诉angular数据有变化，刷新视图。
### angular服务
+ angular提供5种服务，provider/factory/service/constant/value,一般采用factory.
#### provider
```
var app=angular.module('appModule',[]);
 app.config(function(CalcProvider){
   //CalcProvider  是Calc声明的函数实例。
   CalcProvider.currency='RMB';

 })
 app.provider('Cacle',function(){
      this.currency='$';
      this.$get=function(){
      return{}
      }
 })
 里面的this都是CacleProvider
```
#### factory
+ factory是基于provider的，不支持配置，因为配置在factory之前执行完成。
```
app.factory('Calc', function () { //会先new provider
        this.currency = '$';
        return {}
```


