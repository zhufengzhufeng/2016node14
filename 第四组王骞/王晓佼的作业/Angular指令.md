####Angular指令
- 1.特点
   - 1.不依赖控制器
   - 2.指令默认不会产生作用域，可以声明创建作用域，一般不声明
- 2.指令分类
  - 装饰性
    - 负责添加功能
  - 组件式
    - 替换成一个完整的组件
    - 有template的一般是组件
- 3.指令格式
  - 属性  A  Attribute
  
  ```
    <div drag></div>
  ```
  - 元素 E  Element
  
  ```
   <drag></drag>
  ```
  - 类 C Class
  
  ```
   <div class="drag"></div>
  ```
  - 注释  M Mommit
  
  ```
  <!--directive:drag-->
  ```
- 4.命名规范
   - HTML中，用短线连接
   - JS 采用驼峰命名法
- 5.directive指令

```
  var app=angular.module('appModule',[]);
    app.directive("myDrag",function () {  //JS采用驼峰命名法*
        return {//默认生效的 AE
            restrict:'ACME',//限制生效范围,每个字母代表一个格式
            replace:true,  //替换外层原有标签，要求模板必须有一个根节点  template中的标签是根节点
            template:`<div><h1>Hello Angular</h1><p>你好</p></div>`  //这里的div是根节点，涉及到模板替换要有一个根节点

        }
    })
```
- 6.装饰性指令

```
  var app=angular.module('appModule',[]);
    app.directive('colorRed',function () {
        return {
            restrict:'A',//写代码之前，记得限制范围
        //操作DOM
            link:function (scope,element,attr) {   //连接函数  连接作用域和视图  scope-->形参 element--》当前指令所在元素,angular要加单位   attr 当前指令上的所有属性
                //scope代表当前指令所在作用域，指令没有创建作用域，使用的是上一级
             element.css({'color':'red','font-size':'100px'})

            }
        }
    })
```
- 7.组件
  - （1）特点：
     - a.通过属性传递数据
     
     ```
      <div class="col-md-12">
            <!--通过属性  传递数据-->
            <panel title="这是标题1">这是内容1</panel>
            <panel title="这是标题2">这是内容2</panel>
            <panel title="这是标题3">这是内容3</panel>
        </div>
     ```
     - b.封装好的组件可以多次调用
     - c.作用域
   - （2）组件中的域 scope
       - a.默认不会创建作用域
       - b.书写方式1：scope：true ---》【true（产生）  false（不产生  默认）】【存在父子关系，可以获取值】
       - c.书写方式2：scope：{}    --》创建独立作用域，自己的属性放在自己的域中（可以防止值的覆盖），不存在父子关系，无法获取值，和$rootScope平级
       - 指令中独立的作用域，利用属性进行方法的传递时，采用的方式：属性="作用域上的方法（）"   指令中用& 接收
     
     ```
     <script>
    var app=angular.module('appModule',[]);
    app.directive('panel',function () {
        return{
            restrict:'E',
            //封装好一个组件可以多次调用
            templateUrl:`tmpl/panel.html`,//通过路径引用HTML
            transclude:true,//将内容内嵌到panel中
            link:function (scope, element, attr) {
                //想拿到attr 中的属性放在panel的heading中  视图可以通过{{}}来获取值
                scope.title=attr.title;
            },
            scope:true///产生作用域scope默认是false 不产生、     scope:{} -》这样也可以    创建独立作用域属性放在自己的域中，防止覆盖
        }
    })
</script>
panel:<div class=panel-body ng-transclude></div>

     ```
- 8.指令符号
   - （1）@
     - a.单向修改（根变枝变，枝变根不变）
     - b.取得是字符串,声明的字符串和外边的值无关
     - c.前后名字相同可以去掉
     
     ```
      scope:{
                title:'@title'     // <=> title:'@'
            }
     ```
  - （2）=
      - 双向数据改变
      - 互相影响，一个变另一个也跟着变
      - 变量，相当于赋值
      
      ```
        scope:{
                title:'=tit'//变量   双向改变
            }
      ```
  - （3）&
      - 对外部方法的调用
  
```
<body>
<input type="text" ng-model="title">
<!--这里传的是变量  -->
<panel ss="say(a,b)"></panel>
<script src="node_modules/angular/angular.js"></script>
<script>
    /*指令中有独立作用域，将方法通过属性传到指令中，传递方式（属性=“作用域上的方法（）”）  指令中用&接收*/
    var app=angular.module('appModule',[]);
    app.run(['$rootScope',function ($rootScope) {
        $rootScope.say=function (who,where) {
            alert(who);
            alert(where)
        }
    }]);
    app.directive('panel',function () {
        return{
            restrict:'E',
            template:`<button ng-click="s({a:'你好',b:'hao'})">say</button>`,
            scope:{
               s:'&ss'
            }
        }
    })
</script>
```
- 9.指令的交互

```
指令模板1：(控制器一般放置在父级上，方便对子元素整体操作，避免代码冗余)
创建控制器，后边放置将要对他进行引用是要执行的方法
控制器有可能是构造函数，在进行引用的时候最好用this，以保证方法的正确执行
多个元素同时使用一个指令模板时，应注意创建单独域，以保证各个元素之间互不影响
指令模板2：
1.引用上级控制器
2.link函数引用执行。
```