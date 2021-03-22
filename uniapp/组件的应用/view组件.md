首先最重要的可以查看官方文档:https://uniapp.dcloud.io/component/view

	view组件:
	属性:
		hover-class:指定按下去的样式类。当 hover-class="none" 时，没有点击态效果.
		hover-stop-propagation:默认值为false,指定是否阻止本节点的祖先节点出现点击态(防止冒泡).
		hover-start-time:按住后多久出现点击态，单位毫秒.
		hover-stay-time:手指松开后点击态保留时间，单位毫秒.
		注:在需要String转为指定类型的属性的前面需要加上:号,类似于vue语法中v-bind:的效果(貌似高版本不加也可以自动识别).
	例:
	-------------------------------------------
	<template>
		<view>
			<view class="box1" hover-class="box1_active">
				<view class="box2" hover-class="box2_active" hover-stop-propagation="true"></view>
			</view>
			<view class="box3" hover-class="box3_active" :hover-start-time="2000" :hover-stay-time="1000"></view>
		</view>
	</template>

	<script>
	</script>

	<style>
		.box1{
			width: 200px;
			height: 200px;
			background-color: #4CD964;
		}
		.box2{
			width: 100px;
			height: 100px;
			background-color: #555555;
		}
		.box3{
			width: 150px;
			height: 150px;
			background-color: #000000;
		}
		.box1_active{
			background-color: #DD524D;
		}
		.box2_active{
			background-color: #FFA500;
		}
		.box3_active{
			background-color: #FF0000;
		}
	</style>
	-------------------------------------------