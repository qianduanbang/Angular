# Angular
## 基础知识

### Angular是什么——MVVM

	MVC   经典——数据(ajax、jsonp、数组、ng-model)、视图(HTML、CSS、动画、用户操作)、控制器
	MVP   c->p    M和V不耦合
	MVVM  M V VM

### 指令（*为常用指令）

	*ng-model               双向绑定
	ng-bind                 单向绑定(只输出)
	{{}}					表达式
	
	ng-init                 初始化
	*ng-repeat              循环
	ng-repeat="item in arr/json" $index
	ng-repeat="(key,value) in arr/json"
	
	*ng-click/mouseover...   事件
	*ng-controller           控制器
	*ng-app                  引入模块

### controller——功能、大段代码

> $scope		作用域：ng数据

	$scope.$apply()		数据的检查
	$scope.$watch('数据',function(){})		数据的监听

> $http		数据请求

	$http.get
	
	$http.post		头、transform
	
		a.改个头
		    $http({
		      	url,
		      	data,
		      	method,
		      	headers: {'content-type': 'application/x-www-form-urlencoded'}
		    });
		改内容
		    $http({
		      	transformRequest(obj){
		        	return "要传输的字符串"
		      	}
		    });
	    
		b.引入一个模块
			let commonMod=angular.module('common', []);
			commonMod.config(function ($httpProvider){
			  	$httpProvider.defaults.transformRequest=function (obj){
			    	let arr=[];
			
				    for(let name in obj){
				      	arr.push(`${encodeURIComponent(name)}=${encodeURIComponent(obj[name])}`);
				    }
			
			    	return arr.join('&');
			  	};
			  	$httpProvider.defaults.headers.post['Content-Type']='application/x-www-form-urlencoded';
			});
	
	$http.jsonp		ng的版本
	
	    v1.6.4		改了一堆东西
		
		v1.5.*
			$http.jsonp('xxx?cb=JSON_CALLBACK').then();
			
		v1.6.4之后
		  	let res=$sce.trustAsResourceUrl('xxx');
		  	$http.jsonp(, {jsonpCallbackParam: 'cb'}).then();
		
		三种：
			$http.get()
			$http.post()      config()修改配置，可以配置依赖项
			$http.jsonp()
			
			另外的写法：
				$http({
				  method: 'xxx',
				  url: 'xxx'
				})
				
	$interval			重复定时器
	
	$timeout			延时定时器
	
	$q.all				ng版的Promise
	
	$q.race				ng版的Promise
	
	$sce				ng内置的依赖，$sce专门处理不安全的事项（jsonp是不安全的）

### 解决ng做post请求的方法

```
	let commonMod=angular.module('common', []);
	commonMod.config(function ($httpProvider){
	  $httpProvider.defaults.transformRequest=function (obj){
	    let arr=[];
	
	    for(let name in obj){
	      arr.push(`${encodeURIComponent(name)}=${encodeURIComponent(obj[name])}`);
	    }
	
	    return arr.join('&');
	  };
	  $httpProvider.defaults.headers.post['Content-Type']='application/x-www-form-urlencoded';
	});
```

> encodeURIComponent( )函数

> 作用：可把字符串作为URI组件进行编码

> 说明：

	1.该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码：- _ . ! ~ * ' ( )
	
	2.其他字符（比如 ：; / ? : @ & = + $ , # 这些用于分隔 URI 组件的标点符号），都是由一个或多个十六进制的转义序列替换的
	
	3.encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号
