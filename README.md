# frontend
a frontend exercise!

日常开发

问题、强制换行 word-break:break-all

问题、一个ajax请求json数据 1.59M，一个测试小哥的电脑登录进去，刚一开始ajax请求就中断了。
数据请求是null，没有抱任何错误，不知道是什么原因，其他人都会发起请求一秒钟返回数据没有问题就他一个人有问题。
怀疑是不是他浏览器设置了请求大小限制，
	chrome,  firefox  ,以及其他国产浏览器都没找到哪儿可以设置的。
	卸载chrome重新安装，问题依然存在。
	正在焦虑万分时，一个老司机说如果浏览器阻断请求，应该会报错。
	既然没有报错，会不会是网络原因，电脑设置的原因。
	到了最后，一个服务器端开发过来换了一个账号登录一切正常。
	原来是后台对该账号做了限制。


问题、安全的权限验证机制。生成的token很短时间内token过期，
	方案1 登录成功的凭证token使用websoket 等技术推送到客户端
		缺点：用户页面没关闭就会一直保持登录状态；
	每次过期前，其他请求时由服务器端响应新的tokenresponse head 发回到客户端，由客户端更新token

	
问题、xhr获取response header 为null
	https://segmentfault.com/a/1190000009125333
	前端只能获取默认的头部，
Cache-Control、Content-Language、Content-Type、Expires
Last-Modified、Pragma
	要想获取到其他的(比如Authorization)需要在服务器端设置
	Access-Control-Expose-Headers : 'Authorization'


问题、elementui 表单验证，输入项小于最大值，大于最小值
	最大最小值是data函数返回对象上动态绑定的属性(由ajax获取)
	于是就需要自定义验证函数，
	但由于自定义验证函数在Vue对象的data函数中，所以自定义验证函数不能访问到data返回的对象
	的最大最小值。this也不会指向data对象。
	解决，将ajax获取到的最大最小值存放在dom上，使用dom操作来获取，

问题、Ajax Reponse 200 ok, but shows failed to load response data
	前端获取同一个接口过于频繁就会出现这个问题，
	更具体原因应该是服务器端没有返回正确的json数据

问题、vue只有npm run dev 和npm run build 怎样扩展出npm run test ;
	要区分开发环境   测试环境  生产环境  线上环境
	https://blog.csdn.net/web_youth/article/details/80052304
	
问题、vue生产环境与开发环境区分填坑
	开发环境与生产环境需要配置不同的服务器地址，
	在配置dev.env.js与prod.env.js时，  配置的全局变量    一定要   在单引号里用双引号包裹，如
		module.exports = merge(prodEnv, {
		  NODE_ENV: '"development"',
		  API_ROOT:'"/api"',
		  SOKET_ROOT:'"http://vue.by-998.com:6001"',
		  IMAGE_ROOT:'"http://image.dd788799.com"',
		})
	
问题、visibility: 由hidden变为visible之后，其hover动作第一次可能不会触发
	

问题、覆盖第三方组件裤样式如：elementUI 不成功
	  原因：style标签上的scope
	  解决：https://www.cnblogs.com/XHappyness/p/7686267.html
待解决疑问：为什么一定要app.css(全局css)才能有效，在vue文件中新增sytle标签不加scope为什么也不行呢？
	
问题、css文本处理
	https://blog.csdn.net/hjf_bluesky/article/details/48980177
	
开发笔记
问题、mouseover  mouseleavle  无效
	原因：在一个div上绑定鼠标事件，然后去界面上验证，
		发现界面不响应鼠标事件。居然是两个相似div(class列表相同)，事件绑错div了。
	解决：事件绑定到该绑定的div上

问题、vue-router路由切换(点击router-link超链接)或者初始化报错，
	Cannot read property '$createElement' of undefined
	解决、https://segmentfault.com/q/1010000011447867/a-1020000011507773
	创建组件时，components 变为 component  不要最后的 s 

问题、elementui el-form-item  表单项组件   **************
	没有加上当前scope的属性， 当前的scope内的scss语法对其不生效
	
	问题原因：
		待探索
	解决：
		当前vue文件中写一个不带scope的style标签
		直接操作 .el-form-item  的样式


问题、    **********
嵌套数组对象列表绑定，改变底层数组属性时，界面不能及时改变！
	解决1：将底层数组绑定的元素，封装为组件，将其引用到父页面中。
	界面的变化就可以通过子组件的逻辑来实现。
	解决2：使用jquery或原生js直接操作dom

问题、跨域    **********
	配置 config --> index.js 的 proxyTable 
https://www.cnblogs.com/wangyongcun/p/7665687.html
	正式上线时生成的静态文件，需要服务器跨域方案

问题、图片改名字，css背景图改为相应的名字后，界面无法显示图片，	

问题、多行文本溢出省略号
	http://www.css88.com/archives/5206
	
问题、开发环境覆盖iview样式有效，打包后无效，
	解决、要覆盖iview的样式可加上 !important
	
问题、通过地址栏变化，设置页面中相应的菜单项选中效果
	解决：监听路由变化
	watch:{
		'$route'(cur,old){
			this.isActive = cur.path;
		}
	}

问题、限制input type=number 的输入长度，注意type=number时，maxlength无效
	解决、<input type="number" oninput="this.value = this.value.slice(0,6)" title /> 
	
问题、在使用新特性声明函数时,如
{
	name:'new test',
	something(){
		console.log(this.name);
	}
}
在调试过程中， 断点运行到console.log处时，  this 为undefined， 给调试带来诸多不便;
解决、调试时，this为undefined，主观忽略掉。  要想看this.name的值，  使用console.log打印出来;



问题、webpack
	由于要跨域访问websoket，服务器限定域名，
	所以要把本地localhost:8080配置成指定的域名，
	然后webpack就报 Invalid Host header 错误
	
	解决、webpack.dev.conf.js中添加配置 disableHostCheck:true, 记得重启项目
	

问题、Chrome 自动填充用户名密码后输入框背景变黄色
	解决、
	input:-webkit-autofill,
	textarea:-webkit-autofill,
	select:-webkit-autofill {
	  -webkit-box-shadow: 0 0 0 1000px white inset;
	}

	
问题、window.onresize  vue 中无法改变元素样式

问题、钩子函数 updated 中  调用方法造成浏览器界面卡住无反应，

问题、vue watch 监听属性变化时，当属性为复杂对象时，能否监听属性的属性？
	可以监听：
	示例：
	data:{
		showChoiceTool:{
          show:false,
          value:'普通'
        }
	}
	.
	.
	.
	mounted() {
      this.$watch('showChoiceTool.value', function (newVal, oldVal) {
        console.log(newVal);
      });
	}

问题、js引入图片
	// let icon = params.menu.pic;
	// try{
	//   icon = require(`../assets/lottery-details/${params.name}.png`);
	// }catch(e){
	//   icon = require(`../assets/lottery-details/cqssc.png`);
	// }

问题、全站都用jsonp，但登录验证失败或者过期，走的是后台系统(并没有返回jsonp结果)，前端无法进入jsonp回调报错不能提示用户重新登录;
	

问题、华为荣耀8 自带浏览器打不开vue网页，未解决
	浏览器版本太低，加polyfill
	https://cdn.bootcss.com/babel-polyfill/6.23.0/polyfill.min.js

问题、手机浏览器地址栏将内容挤出屏幕   **********************
	解决，加载完成页面后隐藏地址栏
	https://blog.csdn.net/hbcui1984/article/details/8350107  (无效)
	向上滚动地址栏的高度的一半

问题、vue监听元素滚动事件
	better-scroll
	https://github.com/ustbhuangyi/better-scroll


问题、vue-router push  pop go 失效(页面不跳转，内容不切换)
	原因：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave、明显被定义后，
	一定要在方法内调用next() 函数
	解决：否则就会出现以上问题。

问题、页面显示问题，超出元素内容显示为省略号，但复制时可以复制全部内容;
	解决：

问题、父组件样式控制子组件(sass)
	1、使用 /deep/ 
		例如
		.wrapper{
			& /deep/ .way-row{
				
			}
		}


问题、文本操作 createTextRange(只对body input textarea 有效)

问题、文本复制
https://blog.csdn.net/qq1332479771/article/details/78007905


问题、git提交代码老是自动丢失。
	原因：团队中有人使用gitbush提交，
	有人使用webstorm的版本控制器提交，
	有人使用git命令行提交造成 警告：
	The file will have its original line endings in your working directory.
		warning: LF will be replaced by CRLF 
	解决、git commit之前使用 git add . (. 表示添加当目录所有文件);
问题、git commit 报 "Changes not staged for commit:"是怎么回事?
	解决、git commit之前使用 git add . (. 表示添加当目录所有文件);

		
问题、对象数组种的属性更新后，界面为及时更新
	this.wanRowData = Object.assign({}, this.wanRowData);


问题、computed(计算属性)只有getter，无setter，所以页面绑定了计算属性后，其实就是相当于单向绑定了。


问题、DNS预获取 dns-prefetch 提升页面载入速度  
	<meta http-equiv="x-dns-prefetch-control" content="on">
	https://www.cnblogs.com/lhm166/articles/6073787.html

问题、meta referrer   
	<meta name="referrer" content="always">
	http://www.freebuf.com/news/57497.html

问题、html 中ios 的图标问题 apple-touch-icon-precomposed
	https://www.baidu.com/link?url=jeKb5WssoCy6SI_VuwzTYBMojIyx539_qdFEB5NxxqCRgGU-ZGNfsjVtgFpCM9kp&wd=&eqid=e7742faf0000c710000000065b2740b8
