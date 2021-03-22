首先最重要的可以查看官方文档:https://uniapp.dcloud.io/collocation/pages

	在pages.json中声明condition(与pages属性是同一级别)
	-------------------------------------------
	"condition":{
		//当前激活的模式，list节点的索引值
		"current":0,
		//启动模式列表
		"list":[
			{
				//启动模式名称
				"name":"详情页面",
				//启动页面路径
				"path":"pages/details/details",
				//启动参数，可在页面的 onLoad 函数里获得
				"query":"id=80"
			}
		]
	}
	-------------------------------------------
	
	配置完之后用微信小程序运行的时候能直接在普通编译的下拉菜单中选择刚刚设置的name属性--详情页面 来直接访问该页面.