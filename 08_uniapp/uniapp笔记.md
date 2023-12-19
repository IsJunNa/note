## 	标签

### view标签

#### 作用

替代div



### text标签

#### 作用

替代span

#### 属性

selectable：是否可选，属性值是一个布尔值，默认false

space：显示连续空格 属性值三个：ensp，emsp，nbsp 分别是 ：中文字符空格一半大小，中文字符空格大，根据字体设置的空格大小

decode：是否解码，属性值是一个布尔值，默认false



### image标签

#### 作用

替代img标签，相比有默认宽高为320x240





## 页面顶部

### 给页面单独设置

在**pages.json**中的**pages**项找到要设置的页面中的**style**属性

pages.json页面 > pages项 > style项

```js
"style": {
			"navigationBarTitleText": "首页", // 标题栏的标题内容
			"navigationBarBackgroundColor": "green", //标题栏的背景颜色
			"navigationBarTextStyle": "white" //标题栏的的文字颜色（只支持白色和黑色）
			"enablePullDownRefresh": true //是否开启下拉刷新
		}
```



### 给所有页面配置

在**pages.json**中的**globalStyle**项中设置

pages.json页面 > globalStyle项

```js
 //全局的页面窗体
"globalStyle": {
				  "navigationBarTextStyle": "white",
				  "navigationBarTitleText": "我是默认标题",
				  "navigationBarBackgroundColor": "pink",
				  "backgroundColor": "#F8F8F8"
			  }
```



### 在组件页面中配置

```vue
<script>
methods: {
	onLoad() {
        //页面标题
		uni.setNavigationBarTitle({title: 'js操作的title'})
        //页面背景颜色，字体颜色（字体颜色只能#ffffff和#000000）
		uni.setNavigationBarColor({backgroundColor: "#007aff",frontColor: "#ffffff"})
        //顶部显示加载圆圈
		uni.showNavigationBarLoading()
        //隐藏加载圆圈
		uni.hideNavigationBarLoading()
    }
}
</script>
```





## 页面底部

### Pages中设置

```javascript
"tabBar": {
    //字体颜色
	"color": "#333",
    //底部列表
	"list": [{
        	//文字内容
			"text": "主页",
        	//页面路径
			"pagePath": "pages/index/index",
        	//图标路径
			"iconPath": "static/021_主页.png",
        	//选中时的图标路径
			"selectedIconPath": "static/021_主页 (1).png"
		}
	]
}
```



### 组件中设置

```javascript
onLoad() {
    //设置颜色样式
    uni.setTabBarStyle({
		color: 'pink',
		backgroundColor: '#007aff'
	})
    //设置tabBar内容
    uni.setTabBarItem({
		index: 4,
		text: '黑色'
	})
	//隐藏tabBar
	uni.hideTabBar()
	//右上角消息提示框(可以携带文字)
	uni.setTabBarBadge({
		index: 2,
		text: '3'
	})
	//右上角红点(不可以携带文字)
	uni.showTabBarRedDot({
		index: 1
	})
	//隐藏右上角消息提示框
	uni.hideTabBarRedDot({
		index:1
	})
}
```



### inconfont图标

#### 作用

设置页面的字体图标

#### 步骤

1.将 iconfont.ttf 文件放入项目的静态文件夹中

2.在pages.json文件中的 tabBar 对象中添加 iconfontSrc 配置，值为iconfont.ttf的路径

3.在list中选择要配置的页面添加 iconfont 对象，并配置具体的值

4.配置text属性时,在原来的引入名称 （&#xe674；）上去掉  **&#x**和**；**并在前面添加 **\u** 才能使用

#### 语法

```json
"tabBar": {
    //底部导航,一个对象为一个
	"list": [
		{
            //文件路径
			"pagePath": "pages/green/green",
            //底部导航名称
			"text": "绿色",
            //配置iconfont
			"iconfont": {
                //未选中时的图标样式(引入&#xe674; 然后去掉&#x号和;号并在前面添加\u才能正常使用)
				"text": "\ue674"
                //选中时的图标样式
                  "selectedText": "\ue674"
                //未选中时的图标颜色
                  "color": "pink",
                //选中时的图标颜色
				 "selectedColor": "pink"
                //图标大小
                "fontSize": "30px"
			}
		}
	],
	"iconfontSrc": "static/font/iconfont.ttf"
}
```







## 组件

### 创建组件

1.在项目页面创建一个components文件夹

2.在components文件夹下创建组件

3.在需要使用组件的地方放入与组件名相同标签



### props（父传子）

#### 概念

用于**父组件向子组件**之间的通信，可以让**父组件向子组件**之间实现数据的传输

#### 传递数据

组件中

```vue
<template>
	<view>
		<h1>{{title1}}</h1>
		<h3>{{title2}}</h3>
	</view>
</template>

<script>
	export default {
		name: "MyHeader",
		props: ["title1", "title2"],
	}
</script>
```

使用组件处

```vue
<!-- 也可以使用 :title 的形式将data中的数据放入页面 -->
<MyHeader title1="首页" title2="Jun"></MyHeader>
```

#### 限制传递的数据

组件中

```vue
<template>
	<view>
		<view class="box">
			<h1>{{title1}}</h1>
			<hr color="blue">
			<h3>{{title2}}</h3>
		</view>
	</view>
</template>

<script>
	export default {
		name: "MyHeader",
		props: {
			title1: {
				type: String, //数据类型
				default: "默认标题" //如果未传值则使用该值
			},
			title2: {
				type: String,
				default: "默认副标题"
			}
		}
</script>
```

使用组件处

title1没有传值所以他的参数为默认的 : "默认标题"

title2有传值所以他的参数为data中绑定的 : "新副标题"

```vue
<template>
	<view class="content">
		<MyHeader :title2=title2></MyHeader>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title2:"新副标题"
			}
		}
	}
</script>
```

#### 注意

```javascript
// 如果要传递的数据的类型是数组或对象类型则default要写成函数的形式(类似于data)
list: {
	type: Array,
	default () {
		return [1, 2, 3]
	}
},
user: {
	type: Object,
	default () {
		return {
			name: "张三",
			age: 18
		}
	}
}
```

```vue
<MyHeader :list="[5,6,7]" :user="{name:'李四',age:22}"></MyHeader>
```



### emit（子传父）

#### 概念

用于**子组件向父组件**之间的通信，可以让**子组件向父组件**之间实现数据的传输

本质就是一个可以传递数据的自定义事件



#### 步骤

1.在子组件中使用 $emit 方法（在vm身上）并传入两个参数，第一个参数为事件名，第二个参数为传递的数据

2.在父组件中的methods对象使用该方法，格式为  事件名（）

3.获得数据，调用该自定义方法后，该方法的e就是该数据值



#### 语法

子组件

```vue
<script>
	methods: {
		jun(e) {
			this.$emit("事件名", e.detail.value)
		}
	}
</script>
```

父组件

```vue
</template>
	<MyFooter @MyEmit="MyEmit"></MyFooter>
</template>

<script>
	methods: {
		MyEmit(e) {
			console.log(e)
		}
	}
</script>
```



#### 注意

1.定义的emit只能在其组件标签上使用

2.组件标签上使用原生事件无法使用，因为会默认当作自定义事件来处理所以需要在原生事件后加上 **.native** 才能正常使用

```vue
<MyFooter :test="test" @MyEmit="MyEmit" @click.native="zzp"></MyFooter>
```



### .sync 修饰符

#### 概念

当子组件和父组件之间要传递实时数据时，可以用 .sync 修饰符



#### 步骤

1.在父组件中按原来的props语法，在传送的变量名后加 .sync

2.在子组件中使用props配置项接收父组件传递的数据

3.在methods配置项中创建一个更新事件

4.在更新事件中配置 this.$emit('update:data', test )

其中data是父组件传递数据用的变量名

test 是传递回父组件的数据

其他为固定语法



#### 语法

父组件

```vue
<template>
	<view>
        <!-- 原; props传递数据方法 -->
        <view :data="title"></view>
        <!-- 现; 双向传送数据方法 -->
		<view :data.sync="title"></view>
	</view>
</template>

<script>
	data:{
        //要传递的数据
		title:"我是要传递的数据"
	}
</script>
```

子组件

```vue
<template>
	<view>
		<view @click="Jun">{{title}}</view>
	</view>
</template>

<script>
	export default {
        // 通过props接收父组件传递的数据
		props:["data"],
		methods:{
			Jun(){
            	 //data是父组件传递的数据时使用的数据名
				this.$emit('update:data',"我是传递回父组件的数据")
			}
		}
	}
</script>
```





## 生命周期

### 概念

从vue实例产生开始到vue实例被销毁这段时间所经历的过程



### mounted

实例创建完成后的钩子，此时可以通过vm访问到data中的数据和methods中的方法



### created

虚拟DOM转换为真实DOM后的钩子，此时可以开启定时器，发送网络请求，订阅消息，绑定自定义事件等初始化操作



### APP.vue函数

```javascript
// 第一次加载时调用
onLaunch: function() {
	console.log('App Launch')
},
// 离开页面时调用
onShow: function() {
	console.log('App Show')
},
// 重新加载时调用
onHide: function() {
	console.log('App Hide')
}
```



### onLoad

页面载入时执行，其形参e为一个对象，对象内是get方法后穿过来的键值对

```javascript
onLoad(e) {
    console.log(e)
}

// e输出的内容为路径上传递的参数(键值对)
{name: 'jun'}
```





## 路由跳转

### uni.navigateTo

#### 使用

给想要跳转的盒子添加一个点击事件，在事件中配置 uni.navigateTo 配置对象（url为跳转地址）

#### 特点

跳转后可以返回，不能打开导航页面

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="goform"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			goform() {
                //配置 uni.navigateTo
				uni.navigateTo({
					url: "/pages/form/form"
				})
			}
		}
	}
</script>
```



### uni.redirectTo

#### 使用

给想要跳转的盒子添加一个点击事件，在事件中配置 uni.redirectTo配置对象（url为跳转地址）

#### 特点

跳转后不可以返回，不能打开导航页面

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="goform"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			goform() {
                //配置 uni.redirectTo
				uni.redirectTo({
					url: "/pages/form/form"
				})
			}
		}
	}
</script>
```



### uni.reLaunch

#### 使用

给想要跳转的盒子添加一个点击事件，在事件中配置 uni.reLaunch 配置对象（url为跳转地址）

#### 特点

跳转后不可以返回，可以打开导航页面,可以携带参数

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="goform"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			goform() {
                //配置 uni.reLaunch,可以携带参数
				uni.reLaunch({
					url: "/pages/index/index?name='jun'&password='123456'"
				})
			}
		}
	}
</script>
```



### uni.switchTab

#### 使用

给想要跳转的盒子添加一个点击事件，在事件中配置 uni.switchTab 配置对象（url为跳转地址）

#### 特点

跳转后不可以返回，可以打开导航页面,不可以携带参数

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="goform"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			goform() {
                //配置 uni.switchTab,不可以携带参数
				uni.switchTab({
					url: "/pages/index/index	"
				})
			}
		}
	}
</script>
```



### uni.navigateBack()

#### 使用

给想要跳转的盒子添加一个点击事件，在事件中配置 uni.navigateBack()

#### 特点

可以返回上一页

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="goform"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			goform() {
                //配置 uni.navigateBack()
				uni.navigateBack()
			}
		}
	}
</script>
```





## 交互反馈

### uni.showToast

#### 效果

弹出提示框

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="clickalert"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			clickalert() {
				uni.showToast({
                    //提示内容
					title: "点击失败",
                    //提示图标（默认为勾）
                    icon: "error",
                    //自定义提示图标
                    image: "/static/logo.png",
                    //弹窗时禁止点击其他位置
                    mask:true,
                    //消失时间
                    duration: 4000，
                    //接口调用成功的回调
                    success:res=> {},
                   	//接口调用失败的回调
                   	fail:err=> {},
                   	//接口调用成功失败的都执行回调
                    complete() {}
				})
			}
		}
	}
</script>
```



### uni.hideToast()

#### 效果

立即隐藏uni.showToast调用的弹框

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="clickalert"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			clickalert() {
				uni.hideToast()
			}
		}
	}
</script>
```



### uni.showLoading

#### 效果

显示一个加载的弹框

#### 语法

```vue

<script>
	export default {
		onLoad() {
			uni.showLoading({
				title: '数据加载中'
			})
		}
	}
</script>

```



### uni.hideLoading

#### 效果

隐藏 加载弹框（uni.showLoading）

#### 语法

```vue

<script>
	export default {
		onLoad() {
			uni.hideLoading()
		}
	}
</script>
```



### uni.showModal

#### 效果

弹出消息提示框

#### 语法

```vue
<template>
	<view>
		<view class="box" @click="show"></view>
	</view>
</template>

<script>
	export default {
		methods: {
			show() {
				uni.showModal({
                    //提示框标题
					title: '提示框',
                    //提示框内容
					content: '继续进入相关页面',
                    //是否显示取消按钮
					showCancel: true,
                    //取消按钮内容
					cancelText: '否',
                    //取消按钮颜色
					cancelColor: 'red',
                    //是否显示输入框
					editable: true,
                    //输入框提示文字
                    placeholderText: '我是提示文字',
                    //接口调用成功的回调
                    success:res=> {},
                   	//接口调用失败的回调
                   	fail:err=> {},
                   	//接口调用成功失败的都执行回调
                    complete() {}
				})
			}
		}
	}
</script>
```



### uni.showActionSheet

#### 效果

弹出一个底部选择框

#### 语法

```vue
<script>	
	uni.showActionSheet({
		itemList: ['A', 'B', 'C'],
		success: (res)=> {
			console.log('选中了第' + (res.tapIndex + 1) + '个按钮');
		}
	})
</script>
```





## 网络请求

#### 语法

```javascript
uni.request({
    //请求路径
	url: 'https://api.thecatapi.com/v1/images/search',
    //？号后的参数
	data: {limit: 2},
    //响应方法
    method:'GET'
    //响应的最长时间
	timeout: 1000,
    //成功的回调
	success:()=>{},
    //失败的回调
	fail:()=>{},
    //成功失败都会执行的回调
    complete:()=>{}
})
```





## 本地存储

### uni.setStorage

#### 区别

同步存储数据

#### 语法

```javascript
uni.setStorage({
    //键
	key: 'name',
    //值
	data: '朱俊鹏',
    //成功回调
	success: (res) => {
		console.log(res)
	}
})
```



### uni.setStorageSync

#### 区别

异步存储数据

#### 语法

```javascript
uni.setStorageSync('性别', '男')
```



### uni.getStorageSync

#### 区别

异步获得本地存储数据

#### 语法

```javascript
let sex = uni.getStorageSync('性别')
console.log(sex)
```



### uni.removeStorageSync

#### 区别

异步删除指定的本地存储数据

#### 语法

```java
uni.removeStorageSync('性别')
```



### uni.clearStorage()

#### 区别

删除所有的本地存储数据

#### 语法

```javascript
uni.clearStorage()
```