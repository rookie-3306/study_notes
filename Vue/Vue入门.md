Vue的数据绑定：
	当对象给Vue托管时,只要在{{}}中加入信息，Vue会自动帮你绑定
	-------------------------------------------
		<html>
		<head>
			<meta charset="utf-8">
			<title></title>
			<script src="../js/vue.js"></script>
		</head>
		<body>
			<div id="box">
				<h2>{{msg}}</h2>
				<ul>
					<li v-for="item in array">{{item}}</li>
				</ul>
			</div>
			<script>
				let box = new Vue({
					//获取托管元素
					el:"#box",
					data:{
						//数据
						msg:"你好Vue!"
					}
				})
			</script>
		</body>
	</html>
	-------------------------------------------
	
Vue的列表展示:
	-------------------------------------------
	......
	<body>
		<div id="box">
			<ul>
				<li>{{array[0]}}</li>
				<li>{{array[1]}}</li>
				<li>{{array[2]}}</li>
			</ul>
			<p>-------------------------------</p>
			<ul>
				<li v-for="item in array">{{item}}</li>
			</ul>
		</div>
		<script>
			let box = new Vue({
				el:"#box",
				data:{
					array:['测试01','测试02','测试03']
				}
			})
		</script>
	</body>
	-------------------------------------------

Vue的事件绑定:
	-------------------------------------------
	...
	<body>
		<div id="app">
			<h2>计数器数值:{{count}}</h2>
			<!-- v-on为监听,冒号后面跟着监听事件名,这里为click,然后根据监听事件触发在vue中定义的函数 -->
			<button v-on:click="add()">+</button>
			<button v-on:click="sub()">-</button>
		</div>
		<script>
			let app = new Vue({
				el:"#app",
				data:{
					// 定义自身变量
					count:0
				},
				methods:{
					add:function(){
						// 找到上方data中定义的变量,并且操作变量
						this.count++;
					},
					sub:function(){
						this.count--;
					}
				}
			})
		</script>
	</body>
	-------------------------------------------