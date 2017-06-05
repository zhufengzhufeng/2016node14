###指令
- 1.指令是不依赖于控制器的
- 2.指令默认不会产生作用域 也可以声明创建作用域
     - 1） 指令的分类 装饰型（负责添加功能的）  组件式（替换成一个完整的组件） 复用
     - 2) 指令的格式
- Attribute   A   属性
```angular2html
<div my-drag></div>
```
- Element     E   元素
```angular2html
<my-drag></my-drag>
```
- Class       C 
```angular2html
<div class="my-drag"></div>
```
- Comment     M   注释
```angular2html
<!-- directive:my-drag -->

```

```angular2html
var app= angular.module('appModule',[]);
    app.directive('myDrag',function () {  
       return { 
           restrict:'ACEM', 
           replace:true,
           template:`<div>
                        <h1>HELLO angular</h1>
                        <p>你好</p>
                     </div>`
          
       }
    });
```
- restrict 默认生效的是AE 限制使用范围 每一个字母代表一种格式
 ECMA 命名规范多个单词以-连接，js中采用驼峰命名
- replace:true,替换外层的原有标签，要求模板必须有一个根节点
- template   只要涉及到模板替换,外面就要有一个根节点


