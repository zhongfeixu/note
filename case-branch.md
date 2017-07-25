## public   单文件组件 含有内部html、css、js逻辑代码
## controll  业务逻辑代码  
## portalService 入口服务判断
## lib 静态文件资源
##  



学习任务：
1.express
2.dustJS

图片生成工具
## routes/index.js 路由层
##

路由：
入口主文件
```
			var birds = require('./文件名');
			app.use('/birds', birds);
```
```
		var express = require('express');
		var router = express.Router();

		// 该路由使用的中间件
		router.use(function timeLog(req, res, next) {
		  console.log('Time: ', Date.now());
		  next();
		});
		// 定义网站主页的路由
		router.get('/', function(req, res) {
		  res.send('Birds home page');
		});
		// 定义 about 页面的路由
		router.get('/about', function(req, res) {
		  res.send('About birds');
		});
		module.exports = router;
```




生成活动页文件，是save.ejs文件为模板；
路由触发：
模板替换后成为商城的新模板

没有template.html，直接渲染App.vue.
vue文件中：public中，window.datas来自于哪里？？？？

this.$store.state.editor.data---->指的的事vuex内部的默认保存参数

vuex的里面这里this.$store.state.editor指的是如下
```
export default {
  modules: {
    editor
  }
}

```
