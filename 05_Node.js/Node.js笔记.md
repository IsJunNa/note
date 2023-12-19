## 计算机组成

### CPU

中央处理器，整个计算机控制与运算的中心

### 内存

存储数据的介质，可以存放大量的0和1的数据，读写速度快，断电会丢失数据

### 硬盘

与内存作用相似，但是读写速度慢，断电不会丢失数据

### 显卡

用来处理视频信号，当有视频需要处理时会将信号传递给显卡，显卡处理完毕再传递给显示器并进行显示

### 主板  

是一块大的集成电路板，用来装载CPU，内存，硬盘，显卡



## CMD常用命令

### 切换盘符

**命令名：**C: 或 D:

### 切换工作目录

**命令名：**cd

### 查看目录文件

**命令名：**dir

### 清屏

**命令名：**cls



## Buffer

### 创建

**方法一：alloc**

速度比allocUnsafe会慢，但是不会保留旧数据

```javascript
let buf = Buffer.alloc(10);
```

**方法二：allocUnsafe**

与alloc相比会有旧数据，但是速度比alloc快因为无需清零

```javascript
let buf = Buffer.allocUnsafe(10)
```

**方法三：from**

可以将数组或字符串转为Buffer

```javascript
let buf = Buffer.from('hello')
```



### Buffer与字符串的转换

**Buffer转换成字符串**

```javascript
let buf = Buffer.from([105,108,111,118,101,121,111,117])
console.log(buf.toString())
```

**字符串单个元素转换成二进制**

```javascript
let buf = Buffer.from('hello')
console.log(buf[0].toString(2))
```

**Buffer的修改**

```javascript
let buf = Buffer.from（'hello'）
buf[0]=95
console.log(buf.toString())
```



### 溢出与中文

**溢出：**buffer最高位为8位，超出部分会舍弃

**中文：**一个utf-8的中文汉字在buffer中占三个字节



## fs文件写入

###  概念

fs全称File System，叫做文件系统，作用是与硬盘进行交互，例如文件的创建，删除，重命名，移动或文件内容的写入，读取



### 文件写入

```javascript
// 导入fs模块
const fs = require('fs') 
// 写入文件（）
//函数内参数 文件名，写入的内容，选项设置（可选），写入回调
fs.writeFile('./jun.txt','朱俊鹏',err=>{
    //写入成功err为null，写入失败为错误对象
    console.log(err)
})
```



### 同步写入与异步写入

**同步**：从上至下执行代码 

```javascript
// 导入fs模块
const fs = require('fs') 
// 同步写入
//函数内参数 文件名，写入的内容，选项设置（可选）
fs.writeFileSync('./jun.txt','朱俊鹏')
```

**异步：**不会等待写入完成后再执行后续代码，会将回调函数放入任务队列（io线程）中等待主线程代码全部执行完毕再执行

```javascript
// 导入fs模块
const fs = require('fs') 
// 异步写入
//函数内参数 文件名，写入的内容，选项设置（可选），写入回调
fs.writeFile('./jun.txt','朱俊鹏',err=>{
    //写入成功err为null，写入失败为错误对象
    console.log(err)
})
```



### 追加写入

**异步追加**

```javascript
fs.appendFile('./jun.txt','我是异步追加的文字'，err=>{
	//写入成功err为null，写入失败为错误对象
	console.log(err)       
})
```

**同步追加**

```javascript
fs.appendFileSync('jun.txt', ',我是同步追加的文字')
```

**writeFile实现追加写入**

```javascript
fs.writeFile('./jun.txt','朱俊鹏',{flag:'a'},err=>{
    //写入成功err为null，写入失败为错误对象
    console.log(err)
})
```



### 流式写入

**优点**：程序打开一个文件会消耗资源，流式写入可以减少打开关闭资源的次数，流式写入适合大文件的写入或者频繁写入的场景，而writeFile适合写入频率较低的场景

```javascript
// 导入fs模块
const fs = require('fs')

// 创建写入流对象
const ws = fs.createWriteStream('静夜思.txt')

// 写入内容
ws.write('床前明月光\r\n')
ws.write('疑是地上霜\r\n')
ws.write('举头望明月\r\n')
ws.write('低头思故乡\r\n')

// 关闭写入流通道（可选项，不加也没事）
ws.close()
```



## fs文件读取

### 同步读取与异步读取

**异步读取**

```javascript
// 导入fs模块
const fs = require('fs')

// 异步读取
// 参数一：读取的文件的路径	参数二：回调函数err为错误信息data为读取到的内容，是Buffer格式，需要转换成中文
fs.readFile('fs流式写入.txt', (err, data) => {
    console.log(err)
    console.log(data.toString())
})
```

**同步读取**

```javascript
// 导入fs模块
const fs = require('fs')

// 同步读取
// 参数一：读取的文件的路径
//需要用一个变量接收到并打印才能看到，默认也是Buffer格式，需要转换成utf-8中文编码
let data = fs.readFileSync('fs流式写入.txt')
console.log(data.toString())
```



### 流式读取

**优点：**能够提高读取效率

```javascript
// 引入fs模块
const fs = require('fs')

// 创建rs读取对象
const rs = fs.createReadStream('fs流式写入.txt')

// 绑定data事件 chunk为读取的内容
rs.on('data', chunk => {
    console.log(chunk.length)
})

// end事件(可选项)
rs.on('end', () => {
    console.log('读取完成')
})
```



### 文件的移动和重命名

**异步**

```javascript
// 引入fs模块
const fs = require('fs')

// 参数一:原路径 参数二:新路径 参数三:回调函数（err为错误对象）
fs.rename('jun.txt', 'Jun.txt', err => {
    if (err) {
        console.log('重命名失败')
        return
    }
    console.log('重命名成功')
})
```

**同步**

```javascript
// 引入fs模块
const fs = require('fs')

// 参数一:原路径 参数二:新路径
fs.renameSync('jun.txt', 'jun.txt')
```



### 文件的删除

**unlink方法

```javascript
fs.unlink('jun.txt', err => {

	console.log(err)

})
```

**rm方法**

```javascript
fs.rm('jun.txt', err => {

	console.log(err)

})
```



## fs文件夹操作

### 文件夹的创建

**单个创建**

```javascript
// 参数一:创建的路径	参数二：回调函数（错误对象）
fs.mkdir('./demo文件夹', err => {
    if (err) {
        console.log('创建失败')
        return
    }
    console.log('创建成功')
})
```

**递归创建**

```javascript
// 参数一:创建的路径	参数二:配置对象，需配置recursive参数		参数二：回调函数（错误对象）
fs.mkdir('./a/b/c',{recursive:true}, err => {
    if (err) {
        console.log('创建失败')
        return
    }
    console.log('创建成功')
})
```



### 文件夹的读取

```javascript
// 参数一:要读取文件夹的路径	参数二：回调函数（err为错误对象，data为读取到的内容）
fs.readdir('../day1', (err,data) => {
    if (err) {
        console.log('读取文件夹失败')
        return
    }
    console.log('读取文件夹成功')
    console.log(data)
})
```



### 文件夹的删除

**单个文件夹的删除**

```javascript
// 参数一:要删除文件夹的路径	参数二：回调函数（err为错误对象）
fs.rmdir('./html', err => {
    if (err) {
        console.log('删除文件夹失败')
        return
    }
    console.log('删除文件夹成功')
})
```

**递归删除**

```javascript
// 参数一:要删除文件夹的路径	参数二:配置对象，需配置recursive参数		参数二：回调函数（错误对象）
fs.rm('./a',{recursive:true}, err => {
    if (err) {
        console.log('删除文件夹失败')
        return
    }
    console.log('删除文件夹成功')
})
```



### 查看资源相关信息

```javascript
// 参数一:要查看的文件的资源路径	参数二：回调函数（err为错误对象，data为该文件资源状态）
fs.stat('./fs流式写入.txt', (err, data) => {
    if (err) {
        console.log('查看失败')
        return
    }
    console.log('查看成功')
    console.log(data.isDirectory())
})
```

**data的两个方法**

**data.isFile()**：判断是否为文件，结果返回一个布尔值

**data.isDirectory()**：判断是否为文件夹，结果返回一个布尔值



## HTTP协议

### 概念

超文本传输协议， 对浏览器和服务器之间的通信进行约束   



### HTTP请求报文

**请求行：**由请求方法，URL（协议名，主机名，端口号，路径，查询字符串），HTTP版本号组成

**请求头：**由一些键值对组成

**空行：**

**请求体：**请求内容



### HTTP响应报文

**响应行：**由HTTP版本号，响应状态码（1信息响应，2成功响应，3服务，4客户端错误响应，5服务端错误响应），响应状态的描述组成

**响应头：**记录与服务器相关的内容和与响应体相关的内容

**空行：**

**响应体：**响应体的内容格式是非常灵活的。常见的响应体格式有HTML，CSS ，JavaScript，图片，视频，JSON字符串



### IP

**概念：**一个数字标识，本质是一个32Bit的二进制数字

**作用：**用来标识网络中的设备，实现设备间的通信

**分类：**局域网（192.168.0.0或172.16.0.0或10.0.0.0），本机回环IP（127.0.0.1），公域网



### 端口

 应用程序的数字标识，一台现代计算机有65536个端口（0-65535）

一个应用程序可以使用一个或多个端口，主要作用实现不同主机应用程序之间的通信



### 创建HTTP服务端

```javascript
// 引入http模块
const http = require('http')

// 创建服务对象
// request:对请求报文的一个封装对象 response：对响应报文的一个封装对象
const sever = http.createServer((request,response) => {
    // 当服务接受到HTTP请求时执行函数体内容
    response.setHeader('content-type', 'text/html;charset=utf-8')	// 响应体中有中文需要配置该行代码
    response.end('你好')	// end方法设置响应体
})

// 监听端口，启动服务
sever.listen(9000, () => {
    console.log('服务启动成功')
})
```



### 获取请求报文

```javascript
// 引入http模块
const http = require('http')

// 创建服务对象
// request:对请求报文的一个封装对象 response：对响应报文的一个封装对象
const sever = http.createServer((request, response) => {
    // 请求头的获取
    console.log(request.method) //获取请求方法
    console.log(request.url) //获取请求url
    console.log(request.httpVersion) //获取HTTP协议版本号
    console.log(request.headers) //获取请求头
	
	const server = http.createServer((request, response) => {
    // 请求体的获取
    let body = ''
    request.on('data', chunk => {
        body += chunk
    })
    request.on('end', () => {
        console.log(body)
        response.end('Hello')
    })
})

// 监听端口，启动服务
sever.listen(9000, () => {
    console.log('服务启动成功')
})
```



### 获得url路径

**方式一**

```javascript
// 导入http模块
const http = require('http')
// 导入url模块
const url = require('url')

// 创建服务对象
const server = http.createServer((request, response) => {
    // 解析requst.url
    // 第二个参数传true时query变成一个对象
    let res = url.parse(request.url,true)
    console.log(res)
    response.end('url')
})

// 监听端口，启用服务
server.listen(9000, () => {
    console.log('服务启动成功')
})
```

**方式二**

```javascript
const http = require('http')

const server = http.createServer((request, response) => {
    // 实例化URL对象
    let url = new URL('http://127.0.0.1:9000')
    console.log(url)
    response.end('Hello')
})

server.listen(9000, () => {
    console.log('服务启动成功')
})
```



### 设置HTTP响应报文

```javascript
const http = require('http')
const server = http.createServer((request, response) => {
    // 设置相应状态码
    response.statusCode = 200
    // 响应状态描述
    response.statusMessage = 'Hello'
    // 设置响应头
    response.setHeader('content-type', 'text/html;charset=utf-8')
    // 设置多个同名响应头
    response.setHeader('content-type', ['a', 'b', 'c'])
    // 设置响应体
    response.write('Hello')
    // 设置响应体(必须有且一个)
    response.end('Hello')
})
server.listen(9000, () => {
    console.log('服务启动成功')
})
```



## 模块化

###   概念

**概念：**将一个复杂的程序文件依据一定的规则拆分成多个文件的过程称之为模块化，其中拆分出的每一个文件就是一个模块，模块内的数据是私有的，不过模块内部数据可以暴露给其他模块使用

**优点：**1.防止命名冲突 2.高复用性 3.高维护性

 

### 模块暴露

**模块文件**

```javascript
// 声明一个函数
function chouyan() {
    console.log('抽烟')
}
function hejiu() {
    console.log('喝酒')
}

// 暴露数据
module.exports = {
    chouyan,
    hejiu
}
```

**主文件**

```javascript
//自己创建的模块需要用相对路径引入，且./和../不能省略
//文件的后缀可以省略，也可以导入JSON文件，同名无后缀的JSON文件和JS文件优先引入JS文件
const yuqian = require('./11.模块化2.js')

yuqian.hejiu()
```

**注意事项**

1.自己创建的模块，导入时路径填写相对路径，且不能省略 ./ 和 ../

2.JS和JSON文件导入时可以不用写后缀

3.导入其他类型文件会当做JS文件处理

4.如果导入的文件是一个文件夹，会检测该文件夹下的package.json文件中的main属性对应的值，没有值则报错，没有package.json文件则导入index.js或index.json，没有则报错

5.导入node.js内置模块时，直接require模块的名字就可以



## 包管理工具

### 概念

**包**是源码的集合，**包管理工具**是管理包的应用软件，可以对包进行下载安装，更新，删除，上传等操作，常用的包管理工具有npm,yarn,cnpm等



### 初始化包

**指令**

```javascript
npm init //自定义初始化，每一项的值都可以设置
npm init -y //直接初始化，所有参数的都是默认的
```

**参数**

```javascript
package name:包的名字 //不能使用中文和大写，默认使用文件夹名
version: 版本号 //必须以x.x.x定义
description:描述 
entry point: 入口文件
test command:测试命令
git repository:仓库地址    
keywords:关键字                                                                                                 author:作者
license:开源证书
```



### 搜索包

**指令搜索**

```javascript
npm s  //s后跟需要搜索的内容
```

**网站搜索**

```javascript
www.npmjs.com // 网址
```



### 开发依赖和生产依赖

**开发环境：**程序员专门用来写代码的环境，一般是指程序员的电脑

**生产环境：**项目代码正式运行的环境，一般是指正式的服务器电脑

```javascript
// 生产依赖命令(默认)
npm i -S 包名 
npm i -save 包名

// 开发依赖命令
npm i -D 包名
npm i -save-dev 包名
```



### 全局安装

```javascript
// 命令
npm i -g 包名

// 说明
//1.全局安装的命令不受工作目录的位置影响
//2.不需要通过require引入
//3.全局安装包的位置可以通过 npm root -g 查看
```



### 安装指定版本包和删除包

```javascript
// 安装指定版本包命令
npm i  包名@版本号

// 删除包命令
npm r 包名
```



### 设置别名

**设置**

在package.json配置文件中的scripts对象中设置，为键值对形式

```javascript
{
// "name": "day3",
// "version": "1.0.0",
// "description": "",
// "main": "index.js",
//  "scripts": {
//    "test": "echo \"Error: no test specified\" && exit 1",
     "别名"："内容"
//  },
//  "author": "",
//  "license": "ISC"
}
```

**使用**

```javascript
// 命令
run 别名
// 别名为start 不用加run就可以执行
```



### 修改淘宝镜像

```javascript
//全局安装nrm
npm i -g nrm

//更改镜像为淘宝镜像
nrm use taobao

//查看支持的镜像
nrm ls

//查看当前使用的镜像(registry = "https://registry.npmmirror.com/"表示当前为淘宝镜像)
npm config list
```





## express框架

### 语法

```javascript
// 下载express包
npm i express

// 导入express模块
const express = require('express')

// 创建应用对象
const app = express()

// 创建路由
app.get('/home', (req, res) => {
    res.end('Hello')
})

// 监听端口,启动服务
app.listen(9000, () => {
    console.log('服务启动成功');
})


```



### 路由

**概念：**路由确定了应用程序如何响应客户端对特定端口的请求

**组成：**请求方法，路径，回调函数

```javascript
//app.请求方法（路径,回调函数）

//请求方法  all:匹配所有的方法   

//路径 *:匹配所有的路径（常用与匹配404）

//回调函数 req 和 res 两个形参
```



### 请求报文参数

```javascript
// 请求路径
req.path

// 请求参数
req.query

//客户端ip地址
req.ip

//获取请求头
req.get('host')

```



### 路由参数

```javascript
app.get('/:id.html', (req, res) => {
    //获取浏览器中输入的id值
    console.log(req.params.id)
    res.end('Hello')
})
```



### 响应参数

```javascript
// 响应状态码
res.status(404)

// 响应头
res.set('响应头')

// 响应体(会自动添加中文字符集设置)
res.send('你好')

// 跳转响应
res.redirect('www.xxx.com')

// 下载响应
res.download('./index.txt')

// JSON响应
res.json()

// 响应文件内容
res.sendFile('./index.txt')
```



### 中间件

本质是一个回调函数，中间件函数可以像路由回调一样访问请求对象，响应对象

```javascript
// 引入express模块
const express = require('express')
// 引入fs模块
const fs = require('fs')
// 初始化express模块
const app = express()

// 创建中间件函数
function a(req, res, next) {
    let { url, ip } = req
    fs.appendFileSync('text.txt', `${url},${ip}`)
    // 使后续方法都能使用中间件函数
    next();
}

// 使用中间件函数
app.use(a)	

// 创建路由规则
app.get('/home', (req, res) => {
    res.send('主页')
})

app.get('/admin', (req, res) => {
    res.send('后台')
})

app.listen(9000, () => {
    console.log('服务启动成功')
})
```



### EJS模版引擎

**概念：**用于分离用户界面和业务数据的一种技术（分离HTML和服务端JS）

**原生js用法**

```javascript
// 导入ejs模块
const ejs = require('ejs')

// 导入fs模块
const fs = require('fs')

const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧', '白龙马']

// 将HTML页面分离出去并用fs读取
let html = fs.readFileSync('./西游.html').toString()

// 使用ejs模块
let result = ejs.render(html, { xiyou: xiyou })

console.log(result)
```



**express框架用法**

```javascript
const express = require('express')
const ejs = require('ejs')
const path = require('path')
const app = express()

// 设置模版引擎
app.set('view engine', 'ejs')

// 设置模版文件引擎存放位置(具有模版语法的内容的文件)
app.set('views', path.resolve(__dirname, './view'))

// 创建路由
app.get('/home', (req, res) => {
    let data = 'Hello'
    // res.render(模板文件名,数据)
    res.render('home', { data })
})

// 监听端口
app.listen(3000, () => {
    console.log('服务启动成功')
})
```



**generator工具**

```javascript
// 全局安装generator工具
npm install -g express-generator

// 安装
express -e 所要安装到的文件夹名称
```



## mongoDB

### 概念

是一个基于分布式文件文件存储的数据库

数据库：是一个数据仓库，数据库服务下可以创建很多数据库，数据库中可以存放很多集合

集合：集合类似JS中的数组，在集合中可以存放很多文档

文档：文档是数据库中最小的单位，类似于JS中的对象



### 常用命令

```javascript
// 显示所有的数据库
show dbs

// 切换到指定数据库下，不存在则自动创建
use 数据库名

// 显示当前所使用的数据库
db

// 删除当前数据库
db.dropDatabase()

// 查看当前数据库下所有的集合
show collections

// 删除集合
db.集合名.drop()

// 添加集合
 db.createCollection('集合名')

// 集合重命名
db.原集合名.renameCollection('新集合名')

// 插入文档
db.集合名.insert({文档对象})

// 查看集合下的文档(括号中可以添加查询条件)
db.集合名.find()

// 修改文档（会覆盖）
db.集合名.update({要修改的文档的内容},{被修改的文档的内容})

// 修改文档（不会覆盖）
db.集合名.update({要修改的文档的内容},{$set:{被修改的文档的内容}})

// 删除文档
db.集合名.remove({要删除的文档的内容})
```



### mongoose

```javascript
// 导入mongoose
const mongoose = require('mongoose')

// 连接mongoose服务
mongoose.connect('mongodb://127.1.0.1:27017/bilibili')

// 连接成功的回调
mongoose.connection.once('open', () => {
    console.log('连接成功')
})

// 连接失败的回调
mongoose.connection.once('error', () => {
    console.log('连接失败')
})

// 连接关闭的回调
mongoose.connection.once('close', () => {
    console.log('连接关闭')
})

```



### mongoose添加文档

```javascript
// 连接成功的回调
mongoose.connection.once('open', () => {
    console.log('连接成功')
    // 设置文档中的属性和属性值的类型
    let BookSchema = new mongoose.Schema({
        name: String,
        author: String,
        price: Number
    })

    // 创建模型对象
    let BookModel = mongoose.model('books', BookSchema)

    // 添加
    BookModel.create({
        name: '书名',
        author: '作者',
        price: 11
    }).then((err, data) => {
        if (err) {
            console.log(err)
            return
        }
        console.log(data)
        // 关闭连接
        mongoose.disconnect()
    })
})
```



### mongoose字段类型

```javascript
// 字符串
String

// 数字
Number

// 布尔值
Boolean
 
// 数组
Array 或 []

// 日期
Date

// Buffer对象
Buffer 

// 任意类型（Mixed）
mongoose.Schema.Types.Mixed

// 对象id（ObjectId）
mongoose.Schema.Types.ObjectId

// 高精度数字（Decimal128）
mongoose.Schema.Types.Decimal128
```



### 字段值验证

```javascript
let BookSchema = new mongoose.Schema({
    name: {
        type：String, 
       	// 不能为空
        required:true,
        // 默认值
        default：'我是默认值',
        // 枚举(选择的值必须是枚举值当中的)
        emun:[内容1,内容2,内容3]
    	// 唯一值
    	unique:true
    },
    author: String,
    price: Number
})
```



### 删除文档

```javascript
// 单个删除 
BookModel.deleteOne({ _id: "65116b2fd62a17b7d452e5d7" }).then((data) => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

// 批量删除
BookModel.deleteMany({ "name": "红楼梦" }).then((data) => {
    console.log(data)
}).catch(err => {
    console.log(err)
})
```



### 更新文档

```javascript
// 单个文档更新
BookModel.updateOne({ "name": "西游记" }, { "name": "红楼梦" }).then((data) => {
    console.log(data)
}).catch(err => {
    console.log(err)
})
// 多个文档更新
BookModel.updateMany({ "name": "西游记" }, { "name": "红楼梦" }).then((data) => {
    console.log(data)
}).catch(err => {
    console.log(err)
})
```



### 读取文档

```javascript
// 读取单条
BookModel.findOne({ "name": "红楼梦" }).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

// ID读取
BookModel.findById("6512777a449269b8f7808e0c").then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

// 批量获取
BookModel.find().then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})
```



条件查询

**格式**

```javascript
// 运算符
    BookModel.find({ "price": { $lt: 10 } }).then(data => {
        console.log(data)
    }).catch(err => {
        console.log(err)
    })

// 逻辑运算
BookModel.find({ $and: [{ "price": { $lt: 100 } }, { "price": { $gt: 10 } }] }).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

// 正则表达式
BookModel.find({ "name": new RegExp("红") }).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})
```

**语法**

```javascript
// 大于>
$gt
// 小于>
$lt
// 大于等于>=
$gte
// 小于等于>=
$lte
// 不等于！==
$ne

// 逻辑或
$or
// 逻辑与
$and

// 正则表达式
BookModel.find({ "name":/"红"/)
BookModel.find({ "name": new RegExp("红") })
```



### 个性化读取

```javascript
// 返回指定字段
BookModel.find().select({ "name": 1, _id: 0 }).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

// 数据排序
BookModel.find().sort({ price: 1 }).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

//数据截断
BookModel.find().sort({ price: 1 }).limit(3).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})

//指定数据开始到指定数据截断
BookModel.find().sort({ price: 1 }).skip(3).limit(3).then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
})
```



### lowdb工具

```javascript
const low = require('lowdb')
const FileSync = require('lowdb/adapters/FileSync')
const { post } = require('../routes')
const adapter = new FileSync('db.json')
const db = low(adapter)

// 初始化数据
db.defaults({ posts: [], user: {} }).write()

// 写入数据
db.get('posts').push({ id: 1, title: '内容' }).write()

// 获取数据 
console.log(db.get('posts').value())

// 删除数据
db.get('posts').remove({ id: 3 }).write()

// 更新数据
db.get('posts').find({ id: 1 }).assign({ title: '标题' }).write()
```



## 接口

### 概念

接口是前后端通信的桥梁，简单理解就是一个接口就是服务中的一个路由规则 ，根据请求响应结果；接口的作用是实现前后端通信



### JsonServer

```javascript
// 1. 全局安装 json-server
npm i -g json-server

// 2. 创建 JSON 文件（db.json），编写基本结构
{
	"song": [
		{ "id": 1, "name": "干杯", "singer": "五月天" },
		{ "id": 2, "name": "当", "singer": "动力火车" },
		{ "id": 3, "name": "不能说的秘密", "singer": "周杰伦" }
	]
}

// 3. 以 JSON 文件所在文件夹作为工作目录 ，执行如下命令

json-server --watch db.json
```



## 会话控制

### 介绍 

所谓会话控制就是对会话进行控制，HTTP 是一种无状态的协议，它没有办法区分多次的请求是否来自于同一个客户端， 无法区分用户 而产品中又大量存在的这样的需求，所以我们需要通过 会话控制 来解决该问题 常见的会话控制技术有三种： cookie，session，token



### cookie



















