### directive(ָ��)
- 1.ָ���ǲ������ڿ�������
- 2.ָ��Ĭ�ϲ������������ Ҳ������������������
>
- 1�� ָ��ķ��� װ���ͣ�������ӹ��ܵģ�  ���ʽ���滻��һ������������� ����
- 2�� ָ��ĸ�ʽ
>
- ����ָ��

```
    var app = angular.module('appModule',[]);
    app.directive('myDire',function () {
        return {

        }
    });

```

- ģ��

```
   var app= angular.module('appModule',[]);
       app.directive('myDrag',function () {  //ECMA �����淶���������-���ӣ�js�в����շ�����
          return { //Ĭ����Ч����AE
              restrict:'ACEM', //����ʹ�÷�Χ ÿһ����ĸ����һ�ָ�ʽ
              replace:true,//�滻����ԭ�б�ǩ��Ҫ��ģ�������һ�����ڵ�
              template:`<div>
                           <h1>HELLO angular</h1>
                           <p>���</p>
                        </div>`
              //ֻҪ�漰��ģ���滻,�����Ҫ��һ�����ڵ�
          }
       });
```

```
    Attribute   ��д A  ����
    Element     ��д E  Ԫ��
    Class       ��д C ����
    Comment     ��д M ע��
    directive:my-drag
```
### װ����ָ��
- restrict  ���Ʒ�Χ
- link  �������������ͼ��
  + link�е�scope --������ǰָ������������ָ��û�д���������ʹ�õ�����һ��
  + ��ǰָ�����ڵ�Ԫ��,angular��Ҫ�����ӵ�λpx
  + attr ������ǵ�ǰָ���ϵ���������
```
    var app = angular.module('appModule',[]);
        app.directive('colorRed',function () {
            return {
                restrict:'A',//���Ʒ�Χ
                //����dom
                link:function (scope,element,attr) { //���� �������������ͼ��
                    //scope����ǰָ������������ָ��û�д���������ʹ�õ�����һ��
                    //��ǰָ�����ڵ�Ԫ��,angular��Ҫ�����ӵ�λpx
                    element.css({'color':'red','font-size':'100px'});
                    //attr ������ǵ�ǰָ���ϵ���������
                    console.log(attr);
                    //1.mousedown mousemove mouseup   //DOMMouseScroll  mousewheel
                }
            }
        })
```
### ���ʽ
- attr props �봫��һЩ���ݿ���ͨ�����Դ���
```
    var app = angular.module('appModule',[]);
        app.directive('panel',function () {
            return {
                restrict:'E',
                //��װ����һ����� ���Զ�ε���
                templateUrl:'tmpl/panel.html',//ͨ��·�� ����html
                transclude:true,//�������ݣ�Ƕ�뵽panel��
                /*link:function (scope,element,attr) {
                    //���õ�attr�е����Էŵ�heading��
                    scope.title = attr.title; //�����ݹ����������� ��ͼ����ͨ��{{}}����ȡֵ
                },*/
                scope:{
                    title:'@title' //scope.title = attr.title;
                }  // {} ͨ��true {} ����һ������������
            }
        })
```
### scope
- scope:true  ���Ͼ����ӹ�ϵ����Ȼ�������ϲ���
- scope:{}    �Ͼ����ӹ�ϵ�����Ǻ�$rootScopeƽ��

```
    <body>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <input type="text" ng-model="title">
                <!--tit��say������ ��Ϊ��������   ��������ݶ����������϶�������ݻ��߷���-->
                <panel tit="title" say="say(tit)">��������1</panel>
                <panel tit="title" say="say(tit)">��������2</panel>
                <panel tit="title" say="say(tit)">��������3</panel>
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
                templateUrl:'tmpl/panel.html',//ͨ��·�� ����html
                transclude:true,//�������ݣ�Ƕ�뵽panel��
                scope:{  //scope.title = 'zfpx1';//��������һ���ַ��� �������������ϵ�ֵû��ϵ
                    title:'@tit',//@����ȡ���� title��Ӧ���ַ��� ,ǰ��������ͬ����ȥ��
                    sayTitle:'&say'
                } // title sayTitle�ǵ�ǰ��ͼ����ʱ�õ�����
                //Ĭ�ϲ����������� ����ӵ�и��ӹ�ϵ
                //true���Ͼ����ӹ�ϵ����Ȼ�������ϲ���
                //{} �Ͼ����ӹ�ϵ�����Ǻ�$rootScopeƽ��
            }
        })
    </script>
    </body>
```
### @����

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
        //@����ȡ�����ַ���  @���ǵ����
        app.directive('panel',function () {
            return {
                restrict:'E',
                template:'<input ng-model="title"/>',//ͨ��·�� ����html
                scope:{
                    //���Ե�ֵ�����仯 �����¸�ָ��µ�ֵ
                    title:'@tit' //scope.title = 'zfpx'
                }
            }
        })
    </script>
    </body>
```

### = ����

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
        //@����ȡ�����ַ���  @���ǵ����
        //=�Ŵ��ݵ��Ǳ���    =��˫���
        app.directive('panel',function () {
            return {
                restrict:'E',
                template:'<input ng-model="bb"/>',//ͨ��·�� ����html
                scope:{
                    //���Ե�ֵ�����仯 �����¸�ָ��µ�ֵ
                    bb:'=tit' //scope.bb =$rootScope.title =
                }
            }
        })
    </script>
    </body>
```
### &����(and��)

```
    <body>
    <input type="text" ng-model="title">
    <panel ss="say(a,b)"></panel>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //ָ����ӵ�ж��������򣬽�����ͨ�����Դ��ݵ�ָ���� ���ݵ�ʱ����Ҫ��(����="�������ϵķ���()",ָ������&����������
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
                template:`<button ng-click="s({a:'���',b:'����'})">say</button>`,
    //            template:'<button ng-click="s({a:\'���\'})">say</button>',
                scope:{
                    s:'&ss'
                }
            }
        })
    </script>
    </body>
```

### open

- ng-hide����ng-show����������ʽ
- ng-if��������domԪ�� �������һ�����������ifΪfalse���ڲ����벻ִ��

```
    <script>
        var app = angular.module('appModule',[]);
        app.directive('listGroup',function () {
           return {
               restrict:'E',
               controller:function ($scope) {
                   $scope.lists = [];
                   this.collect = function (s) {
                       $scope.lists.push(s); //�ռ������������� ���Կ��������˵�flag����
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
                require:'^?listGroup', //����?����Ҳ�������null
                templateUrl:'tmpl/open.html',
                transclude:true,
                link:function (scope,element,attrs,groupCtrl) {
                    groupCtrl.collect(scope);
                    scope.flag = false;
                    scope.toggle = function () { //���ʱ ���߸��� �Լ���˭�������Լ��Ķ��رյ�
                        groupCtrl.tell(scope); //���Լ��������򴫵�������
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

### ָ���ָ���Ľ���

```
    <body ng-controller="my">
    <girl cry eat>XXXX</girl>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.directive('girl',function () {
            return {
                //��������� ����ľ���ָ��Ŀ�����
                controller:function ($scope) {
                   $scope.arr = []; //�������
                   this.say = function (kinds) { //���xxxx����eat��cry
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
                require:'^girl',// ^ �Ҳ����������ң�����Ҳ������뱨��^?
                link:function (scope,element,attrs,gCtrl) {//�Ὣgirl��controller����ʵ���������ҷ��ڵ��ĸ�������
                    //Ĭ��ָ�������������ϲ��ң�������������ָ��
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
            //�������nameֵΪż����ʱ�� ��age+1,���age������ ���age���age>10 ����˵age����10��
            $scope.age = 1;
            //��صĵ�һ����������Ϊ�����������������������Ǻ���
            //$scope.$watch('age');
            $scope.$watch(function () {
                console.log(1);
                //ֻҪҳ�����κ�һ��ֵ�仯�ͻ�ִ�д˺���,���ҽ������ķ��ؽ�����뵽��һ��������
                return $scope.age;
            },function (newVal,oldVal) {
                //age�����仯�ͻ�ִ��
                if(newVal>4){
                    alert('age����4��')
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

### ���� ���ٰ���

```
    <body ng-controller="myCtrl">
    ��Ʒ���� {{product.name}} <br>
    ��Ʒ�۸� {{product.price}}<br>
    ��Ʒ���� <input type="text" ng-model="product.count"><br>
    �ʷ� {{product.free}}<br>
    �ܶ� {{sum()+product.free}}<br>
    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.controller('myCtrl',function ($scope) {
            $scope.sum = function () {
                return $scope.product.count*$scope.product.price
            };
            //��ֵ���  Ч�ʵ�   ���ݽٳ�+�۲���ģʽ(on run ) Object.defineProperty
            $scope.$watch($scope.sum,function (newVal) {
                $scope.product.free = newVal>=100?0:10
            });
            /*$scope.$watch('sum()',function (newVal) {
                console.log(newVal);
            })*/
            $scope.product = {
                name:'����',
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
                $interval.cancel(timer); //ȡ����ʱ��
            },1000); //angular�ṩ�ķ��� �������Զ�����$apply,����Ҫ�ֶ�����   watch+apply��ʵ���� ˫�����ݰ�
            /*setInterval(function () {
                $scope.date = Date.now();
                //����angular�еķ������ı����ݣ�����ˢ����ͼ
                $scope.$apply();//����angular�����б仯��ˢ����ͼ
            },1000);*/
        });
    </script>
    </body>
```
### �Զ���model

```
    <body>
    <input type="text" zf-model="aa">
    {{age}}
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //1.ng-model�������������
        //2.���������е������ֵ ��ֵ������������  v->m
        //3.����������ϵ�ֵ����ֵ���������  m->v
        var app = angular.module('appModule',[]);
        app.run(function ($rootScope) {
            $rootScope.aa = 'zfpx'
        });
        app.directive('zfModel',function () {
            return {
                restrict:'A',
                link:function (scope,element,attrs) {
                    //���������е������¼�         v->m
                    element.on('input',function () {
                        scope[attrs.zfModel] = element.val();
                        scope.$apply();//ǿ������ͼˢ��
                    });
                    //����������ϵ�ֵ����ֵ��������� m->v
                    scope.$watch(attrs.zfModel,function (newVal) {
                        element.val(newVal);
                    });
                }
            }
        })
    </script>
    </body>
```
### ����

### provider

- ���ķ��񣬿�������
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

### �¼�����

- ���Ӽ���ϵ

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
    <!--ͨ���ܼ���������ͨ�������������ܼ�-->
    <!--������ ���Խ��н��� $rootScope,����,ͨ���¼�-->
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //������ʽ $on ���� $emit ���Ϸ��䲢�Ұ����Լ�  $broadcast ���·��䲢�Ұ����Լ�
        var app = angular.module('appModule',[]);
        app.controller('parent',function ($scope) {
            //$scope.total = 10;
            $scope.$on('�ְ�������ѩ��',function (e,data) {
                $scope.total = data
            });
            $scope.$watch('total',function () {
                $scope.$broadcast('������200��',$scope.total);
            });
        });
        app.controller('child',function ($scope) {
            $scope.name = 'ѩ��';
            $scope.price = 2.5;
            $scope.count = 4;
            $scope.$watch('count',function () {
                //�������仯��ʱ�� ���Ϸ���
                $scope.$emit('�ְ�������ѩ��',$scope.count*$scope.price);
            });
            $scope.$on('������200��',function (e,data) {
                $scope.count = data/$scope.price;
            });
        });
    </script>
    </body>
```

- ƽ����ϵ
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
    <!--ͨ���ܼ���������ͨ�������������ܼ�-->
    <!--������ ���Խ��н��� $rootScope,����,ͨ���¼�-->
    <script src="node_modules/angular/angular.js"></script>
    <script>
        //������ʽ $on ���� $emit ���Ϸ��䲢�Ұ����Լ�  $broadcast ���·��䲢�Ұ����Լ�
        var app = angular.module('appModule',[]);
        //��������ģʽ
        app.controller('parent',function ($scope,$rootScope) {
            //$scope.total = 10;
            $rootScope.$on('�ְ�������ѩ��',function (e,data) {
                $scope.total = data
            });
            $scope.$watch('total',function () {
                $scope.$emit('������200��',$scope.total);
            });
        });
        app.controller('child',function ($scope,$rootScope) {
            $scope.name = 'ѩ��';
            $scope.price = 2.5;
            $scope.count = 4;
            $scope.$watch('count',function () {
                //�������仯��ʱ�� ���Ϸ���
                $scope.$emit('�ְ�������ѩ��',$scope.count*$scope.price);
            });
            $rootScope.$on('������200��',function (e,data) {
                $scope.count = data/$scope.price;
            });
        });
    </script>
    </body>
```

### �ٶ�������

```
    <!DOCTYPE html>
    <html lang="en" ng-app="appModule">
    <head>
        <meta charset="UTF-8">
        <title>�ٶ�������</title>
        <link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.css">
    </head>
    <body ng-controller="searchCtrl" class="container">
    <div class="text-center h2 text-danger">������</div>

    <input type="text" ng-model="query" ng-keyup="search($event)" class="form-control" ng-keydown="stop($event)">

    <ul class="list-group">
        <!--��ǰ��ѡ�е�����active����-->
        <li ng-repeat="list in lists track by $index" class="list-group-item " ng-class="{active:index==$index}">{{list}}</li>
    </ul>

    <script src="node_modules/angular/angular.js"></script>
    <script>
        var app = angular.module('appModule',[]);
        app.controller('searchCtrl',function ($scope,$http,$sce) {
            //���� ��ȡ���з������ǵ����ģ�ֻ��һ����
            //$http  - > $.ajax
            //��̨�᷵��һ������ִ�� xxx() �����cb
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
                if(code==13){//�س�ʱ���µ�ҳ��
                    return window.open('https://www.baidu.com/s?wd='+$scope.query);
                }
                if(code==38 || code == 40){
                    if(code==38){ //�ϼ�
                        $scope.index--;//�����ڵ�һ������С�ڵ�һ��(Ĭ�Ͽ��ܻᰴ��)
                        if($scope.index<-1){
                            $scope.index = $scope.lists.length-1;
                        }
                        if($scope.index == -1 ){
                            $scope.query = $scope.temp;//Խ��󷵻�Ĭ������������
                        }else{
                            $scope.query = $scope.lists[$scope.index];//����ȡֵ
                        }
                    }
                    if(code==40){ //�¼�
                        $scope.index++;//���������һ��ʱ�ص�-1
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
                .then(function (res) {//��һ���������ǳɹ���Ļص�
                    $scope.temp = $scope.query;//��ѯ��һ�� ��¼ס��ѯ������
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













