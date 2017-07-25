
## 父模板语法：{+name}default Content{/name}  指的是
```
模板
<div class="page">
  <h1>{+pageHeader}LinkedIn{/pageHeader}</h1>
  <div class="bodyContent">
    {+bodyContent/}
  </div>
  <div class="footer">
    {+pageFooter}
       <a href="/contactUs">Contact Us</a>
    {/pageFooter}
  </div>
</div>
```
```
数据
{}
```
```
<div class="page">
	<h1>LinkedIn</h1>
	<div class="bodyContent"></div>
	<div class="footer">
		<a href="/contactUs">Contact Us</a>
	</div>
	</div>
```
如果数据没有或是传入为空，则标签内容为默认存在


## 子模板嵌套父模板
```
{>"base_template"/}//父模板 ，这里是个文件路径
{<bodyContent}
<p>Your body content</p>
{/bodyContent}
{<pageFooter}
       This is a NEW footer
{/pageFooter}
```
```
{}
```
```
<div class="page">
<h1>LinkedIn</h1>
	<div class="bodyContent"></div>
	<div class="footer">
		<a href="/contactUs">Contact Us</a>
	</div>
</div>
```
## 对象替换变量语法----->传入的对象如果是深层对象，那么就打点去选择
```
	hello,{name}
```
```
{
 "name":"Ned",
 "amigo":{"name":"Dusty"}
}
```
## dust条件语法
```
{?属性名}{:else}{/属性名}

```
注意点：如果对应传入是除boolean值以外，会进行隐式转换处理，true为存在，fasle为不存在

## 对象（section）模板语法替换
	注意要点：这里就是如果传过来的的对象，内层还有深层对象的时候，就用{#属性名}{/属性名}
```
	Hello, {name} {#friend}{name}{/friend} {nickname}. That's my name, too.
```
```
{
  "name": "John",
  "nickname": "Jingleheimer Schmidt",
  "friend": {
    "name": "Jacob"
  }
}
```

```
Hello, John Jacob Jingleheimer Schmidt. That's my name, too.
```

## 循环语法（Looping）
	注意要点：数组要点：内层中{.}表示数值ele，{$idx}表示索引，{$len}表示数组长度

```
模板
<ul>
  {#languages}<li>{.}</li>{/languages}
</ul>
```
```
{
  "languages": [
    "HTML",
    "CSS",
    "JavaScript",
    "Dust"
  ]
}
```
```
<ul><li>HTML</li><li>CSS</li><li>JavaScript</li><li>Dust</li></ul>
```
### 数组里面有数组的语法，并且数据里面的值用固定的值链接，{@sep}固定链接值{/sep}，这里这个是固定写法
```
模板：
<ul>
  {#languages}
    <li>{name} by {#creators}{.}{@sep} and {/sep}{/creators}</li>
  {/languages}
</ul>
```
```
传入数据：
{
  "languages": [
    {
      "name": "HTML",
      "creators": ["Tim Berners Lee"]
    },
    {
      "name": "CSS",
      "creators": ["Håkon Wium Lie", "Bert Bos","Bert Bos2"]
    },
    {
      "name": "JavaScript",
      "creators": ["Brendan Eich"]
    },
    {
      "name": "Dust",
      "creators": ["akdubya"]
    }
  ]
}
```

```
显示结果：
<ul>
	<li>HTML by Tim Berners Lee</li>
	<li>CSS by Håkon Wium Lie and Bert Bos and Bert Bos2</li>
	<li>JavaScript by Brendan Eich</li>
	<li>Dust by akdubya</li>
</ul>
```

## Compiling and Rendering(模板的解析和渲染)
### 第一种最简单的浏览器模板渲染；
```
<script type="text/dust" id="hello">Hello {world}!</script>
    <script src="./dust-full.js"></script>
    <script type="text/javascript">
    	//获取模板字符串
        var src = document.getElementById('hello').innerHTML;
        //渲染解析一体化
        //renderSource三参数： 第一个参数：表示模板字符串， 第二个传入的对象，第三：回调函数，回调函数中第二个参数表示的是你渲染后的数据结果。
        dust.renderSource(src, { world: "Alpha Centauri" }, function(err, out) {
            var span=document.createElement("span");
            span.innerHTML=out;
            document.body.appendChild(span);
        });
    </script>
```
### 第二种使用requireJS异步加载模块方式加载，场外文件
   改造js为符合AMD的模块
   ```
	   (function (factory){
	      if ( typeof define === "function" && define.amd ) {
	         // AMD. Register as an anonymous module.
	         define(factory);
	     } else {

	         // Browser globals
	         // 以我的库为例  返回mTools
	        window.mTools = factory();
	    }
		})(function(){
	    我们的js库
	    return {
	       //模块返回值
	     }

		});

   ```
```
		require(["lib/dust-core", "tmpl/hello"], function(dust, helloTemplate) {
		  // Dust >= 2.6.0
		  dust.render('tmpl/hello', { world: "Mars" }, function(err, out) { ... });
		  // Dust >= 2.7.0
		  dust.render(helloTemplate, { world: "Pluto" }, function(err, out) { ... });
		})
```
### 第三种：基于node环境的

```
	var src = fs.readFileSync('/views/hello.dust', 'utf8');
	var compiled = dust.compile(src, 'hello');
	dust.loadSource(compiled);
	dust.render('hello', { world: "Venus" }, function(err, out) {
	  // `out` contains the rendered output.
	  console.log(out);
	});

```
