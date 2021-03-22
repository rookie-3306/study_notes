首先最重要的可以查看官方文档:https://uniapp.dcloud.io/component/text

	text组件:
	属性:
		selectable:文本是否可选
		user-select:文本是否可选
		space:显示连续空格
			值:ensp(中文字符空格一半大小),emsp(中文字符空格大小),nbsp(根据字体设置的空格大小)
		decode:是否解码
	例:
	-------------------------------------------
	<template>
	<view>
		<view>
			<text space="emsp" selectable="true">唱歌 打 篮球</text>
		</view>
		<view>
			<text space="ensp">喝 快乐水</text>
		</view>
	</view>
	</template>

	<script>
	</script>

	<style>
	</style>
	-------------------------------------------
	