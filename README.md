# Angular

## 基础知识

### Angular是什么——MVVM

		原始：
			MVC模式
				M   Model       模型-数据
				V   View        视图-表现层        HTML/CSS
				C   Controller  控制器-业务逻辑
				
				缺点:
					1.M和V耦合度高
					2.C特别臃肿
				
				M和V直接打交道，就会出现一个问题（耦合的问题），视图或者数据的变化都会影响另一方，C特别臃肿
		
		变种：
			MVP
				M   Model       模型-数据
				V   View        视图-表现层        HTML/CSS
				P   Presenter   主持人
				
				缺点:
				1.M和V没有耦合
				2.P特别特别臃肿
				
				M和V没有直接关系，P从中间控制，视图或者数据的变化，只需要P做修改，P特别特别臃肿

			MVVM	Angular属于典型的MVVM模式
			
				M   Model       模型-数据
				V   View        视图-表现层        HTML/CSS
				VM    ViewModel
				
				缺点:
					1.M和V没有耦合
					2.VM还好（不是那么臃肿）
					3.把一部分简单逻辑放进HTML里面
				
				把一部分归视图层管的事拉到自己身上，把一部分归M层管的事拉到自己身上

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

> 双向绑定			ng-model

	  	数据	<->	UI
	  	
	  	$scope	<->	HTML

> 依赖注入(依赖反转)

	 	函数：参数由调用方决定
	 	
	 	依赖注入：参数由定义方决定（要什么参数，就能得到什么参数）

> $scope		作用域：ng数据

		$scope.$apply()		数据的检查
		
		$scope.$watch('数据',function(){},false)		数据的监听
		最后一个参数默认为false，如果是true的情况，则是监控内容

> $http		数据请求

		$http.get
		
			let mod = angular.module('test',[])
			mod.controller('main',function($scope,$http,$q){
				$q.all([
					$http.get('data/arr.txt'),
					$http.get('data/json.txt')
				]).then(res=>{
					//console.log(res)
					let [data1,data2] = res;
					console.log(data1.data,data2.data)
				},err=>{
					alert('读取失败')
				})
			})
		
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
			  	$http.jsonp(res, {jsonpCallbackParam: 'cb'}).then();
			
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

### encodeURIComponent( )函数

> 作用：可把字符串作为URI组件进行编码

> 说明：

		1.该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码：- _ . ! ~ * ' ( )
		
		2.其他字符（比如 ：; / ? : @ & = + $ , # 这些用于分隔 URI 组件的标点符号），都是由一个或多个十六进制的转义序列替换的
		
		3.encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号

### 指令总结

		ng-app        	范围；引入模块
		
		ng-model      	双向绑定    数据<->UI
		
		ng-bind       	输出
		
		ng-事件       		ng-click/dblclick/mousedown...
		
		ng-init       	初始化
		
		ng-controller 	控制器

> 

		ng-show/hide  显示隐藏    block/none
		
		ng-if         元素是否存在

> 

		ng-src		不报错
		
		ng-href		和href类似

> 

		ng-style
		  	<div style="width:200px;height:200px;background:#ccc;">
		  	style={width: '200px', height: '200px', background: '#ccc'}
		
		  	style="{{a}}"
		  	ng-style="json"
		
		ng-class
		  	<div class="aaa bbb ccc">
		  	class=['aaa', 'bbb', 'ccc']
		
		  	class="{{a}}"
		  	ng-class="[...]"

> 

		ng-cloak      没加载完之前，先隐藏

> 

		ng-value    	value="xxx"
		
		ng-readonly 	readonly		禁止用户输入
		
		ng-selected 	selected		默认显示哪个
		
		ng-disabled 	disabled		默认不能点选
		
		ng-checked  	checked			默认选中

> 

		ng-switch
		  	switch(xxx){
		    	case xxx:
		      	break;
		    	case xxx:
		      	break;
		  	}
		
	  	ng-switch="xxx"
	  	ng-switch-when="xxx"

> 

		ng-jq           指定另一个jq兼容库作为angular内部使用

> 

		ng-bind         输出，把HTML按照文字输出
		
		ng-bind-html    输出，把HTML本身输出到页面
		  	*$sce.trustAsHtml(html代码)
		  	
		  	用依赖项$sce的trustAsHtml包一下
			1.6.4 有这问题，低版本没有问题
		
		ng-bind-template
			ng-bind-template="sfassdasf{{z}}dswerr{{a}}"

> 

		ng-include      包含一个文件
		  	ng-include="a"
		  	ng-include="'1.html'"
		  	
		  	*注意地址——localhost

> 

		ng-options      <select>
		  	ng-options="值  as  字  for  xxx  in  arr"
		
	  		ng-options——替你组织option
		  	1.必须有ng-model
		  	2.值  as  字  for  item  in  arr
		
		group
		
		值  as  字  group  by  分类依据  for  名字  in  数组
		
		item.id  as  item.name  group  by  item.type  for  item  in  arr
		
		[{id: 1, name: '北京', type: '直辖市'}, 
		{id: 2, name: '上海', type: '直辖市'}, 
		{id: 3, name: '深圳', type: '直辖市'}, 
		{id: 4, name: '武汉', type: '省会'}, 
		{id: 5, name: '郑州', type: '河南省'}, 
		{id: 6, name: '洛阳', type: '河南省'}]

> 

		<ng-form name="xxx">
		
		</ng-form>

> 

		表单校验：
			1.改用ng-form
			  	<form></form>           ×
			  	<ng-form></ng-form>     √
			  
			2.ng-form要加name
			
			3.每个表单元素需要ng-model
			
			4.校验的结果
			  	form1.email.$error

### 自定义过滤器(filter)

		mod.filter(名字, function (){
		  return function (){
		    //代码...
		  };
		});

### 自定义指令(directive)

		app.directive('名字', function (){
			return {
				restrict: E/A/C/M		标签/属性/class/注释
				template: '插入的内容'
			};
		});
		
		
		restrict
			E	Element			元素
			A	Attribute		属性
			C	Class			类
			M	Comment			注释
				*必须加上replace:true
				*必须两边空格
		
		
		transclude-嵌入、包裹
			1.transclude: true
			2.占位符——原始内容
				<ng-transclude></ng-transclude> 
			3.属性(可作为某个标签的属性)
				<span ng-transclude><span>
		针对C：
			HTML里：中划线		my-close
			JS里：驼峰命名		myClose

### 模块依赖

		angular.module('名字', [])
		.controller()
		.filter();
		
		1.页面引用
		ng-app="名字"
		
		2.模块依赖
		angular.module('名字2', ['名字']);	
			注意：
				同名：后面的模块会覆盖前面的模块

### 自定义依赖项

		1.最简单——factory
			app.factory(名字, function (){
				return 内容;
			});
		
		2.强大——provider
			//provider		供应者
			app.provider(名字, function (){
				this.$get=function (){
					return 内容
				};
			});
		
			大致意思(只是帮助理解，不真实)
				//factory：
				app.factory()->1个
				app.factory()->2个
			
				//angular会：
				var pro=new provider();
				pro.$get()->1个
				pro.$get()->2个
		
		3.服务——service
			app.service(名字, function (){
				this...
			});
		
		三者区别：
			factory——简单
			app.factory(名字, function (){
				return {...};
			});
		
			provider——强大：可配置的
			app.provider(名字, function (){
				this.$get=function (){
					return {...};
				};
			});
		
			service——类似于构造函数
			app.service(名字, function (){
					this...
			});
		
		4.constant——常量(不可装饰)
		
		5.value——变量
		
		依赖项只会创建一次：
			依赖项的状态在不同的地方是共享的

### 依赖修饰

		修改依赖
			会修改原始的依赖，原来的依赖就变了
		
		app.decorator('依赖的名字', function ($delegate){
			$delegate	依赖项的东西
		
			return		修改后的依赖($delegate);
		});

### 数据共享

		数据共享——多个Controller之间
		1.父子Controller
			$scope能继承——复制了$scope
			*不能叫同步，只是复制
		
		消息机制(事件)：
			$scope.$emit('名字', 数据);		触发：自己+父级，向上发送
			$scope.$broadcast('名字', 数据);	触发：自己+子级，向下放松
			$scope.$on('名字', 数据);			接收
		
		2.无关（毫无关联）Controller
			用自定义依赖来存储数据
			*factory、service、provider——创建的依赖只有一个

### ng-route(路由)

