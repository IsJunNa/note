# 基础语法



## 初识Vue

### 介绍

Vue是一种动态构建用户界面的渐进式JavaScript框架

### 特点

遵循**MVVM**模式（M：模型-data，V：视图-Vue模板，VM：视图模型-Vue实例对象）

编码简洁, 体积小, 运行效率高, 适合移动/PC 端开发

### 使用

第一步   引入Vue.js库

第二部   准备一个root容器（里面的代码仍符合HTML语法规范，只不过混入了一些特殊的Vue语法，容器里的代码被成为**Vue模板**）

第三步   实例化Vue对象并传入配置对象（一个实例化对应一个容器，一个容器也只对应一个实例化，也就是**一对一**）

### 配置对象

**el：**指定当前Vue实例为哪个容器服务，值通常为一个css选择器（也可以用DOM来获取容器对象）

写法一：`Vue({ el:#root })`

写法二：`Vue({ })     v.$mount(#root)`

**data：**一般为一个对象，数据需要存储该对象中

对象式：`const vm = Vue({ data:{age:18} })`

函数式：`Vue({data:function(){return{age:18} } })`

**methods：**为一个对象，该对象里放入与事件绑定的函数

**computed：**为一个对象，该对象里放的是计算属性

## 模板语法

### 插值语法

语法：: `{{xxx}}` ，xxxx 会作为表达式来被解析

**功能:** 用于**解析标签体**内容，也就是两个标签之间的内容

概念：是一种用双大括号包裹 表达式 的写法

表达式：一个表达式可以生成一个值，放在任何一个容器中，例如a，a+b，Jun（），undefind等

### 指令语法

语法：`<a v-bind:href:'url'> </a>` ，url会作为 js 表达式被解析

功能: 解析**标签的属性**、**解析标签体**、**绑定事件**

概念**：**以 v-开为开头的写法，在属性前加上指令语法



## 数据绑定

### 单向的数据绑定

概念：是一种**单向**的数据绑定，数据只能从data流向页面

语法：**v-bind：** 简写为   **：**

### 双向的数据绑定

概念：是一种**双向**的数据绑定，数据不只能从data流向页面还能从页面流回data，但是**只能运用在表单类元素上**

语法：**v-model：value**   简写    **v-model**



## 数据代理

### 概念

通过vm对象来代理data对象中属性的操作（读/写）

### 优点

更加方便的操作data中的数据

### 原理

```javascript
let obj = {x:100}
let obj2 = {y:200}
Object.defineProperty(obj2,'x',{
    get(){
    	return obj.x
    },
    set(value){
        obj.x = value
    }
})
```

​     

通过Object.defineProperty()方法，把data对象中所有属性添加到vm上

为每一个添加到vm上的属性，都指定一个getter/setter

在getter/setter内部去操作（读/写）data中对应的属性



## 事件处理

### 语法

基本写法：**v-on:事件类型="事件名"**   

简写：   **@事件类型="事件名"**

### 参数

参数一：$event，在绑定的函数名后用（）添加 ，如果没有添加第二个参数则默认带有不用添加

参数二：自己想要传入的参数，添加后想使用event则要在（）中传入$event



## 事件修饰符

### .prevent

语法：@click.prevent

作用：阻止事件默认行为，等同于e.preventdefault（常用）

### .stop

语法：@click.stop

作用：阻止事件冒泡（常用）

### .once

语法：@click.once

作用：事件只触发一次（常用）

### .capture

语法：@click.capture

作用：使用事件的捕获模式

### .self

语法：@click.self

作用：只有event.target是当前操作的元素时才触发事件

### .passive

语法：@click.passive

作用：事件的默认行为立即执行，无需等待事件回调执行完毕



## 键盘事件

### 语法

`@keyup.键盘事件名称`

当键盘按下 "键盘事件名称" 时才触发该事件

### 常用键盘事件名称

回车 ：enter

删除 ： delete

退出 ： esc

空格 ： space

换行 ： tab (特殊，必须配合keydown去使用)

上 ： up

下 ：down

左 ： left

右 ： right

### 其他键盘事件名称

Vue未提供别名的按键，可以使用按键原始的key值去绑定

例如console.log(e.target.key)来获取该键盘按键的名称，但注意要转为小写，且多个单词之间用  -  隔开（短横线命名）

### 特殊用法按键

特殊按键：ctrl、alt、shift、meta

配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发

配合keydown使用：正常触发事件



## 计算属性

### 概念

定义：要用的属性不存在，要通过已有的属性计算得来

原理：底层借助了Objcet.defineproperty方法提供的getter和setter

优点：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便

### 语法

```javascript
//完整写法
Vue({
	computed：{
        Jun：{
            //初次读取时，所依赖的数据发生改变时会调用get方法
            get(){
                return 该计算属性
            },
            //当该计算属性被修改时调用set方法
            set(value){
                //set方法中需要为修改时属性赋值
            }
        }
    }
})

//简写(再没有set方法时才能使用，整个函数相当于完整写法中的get函数)
Vue({
    computed:{
        Jun(){
            return //该计算属性
        }
    }
})
```

### 注意

初次读取时get函数会执行一次，当依赖的数据发生改变时会被再次调用

计算属性最终会出现在vm上，直接读取使用即可

如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变



## 监视属性

### 语法

```javascript
//方法一
new Vue({
    watch:{
        被监视的属性名:{
            //当该被监视的属性发生修改时调用handler函数中的代码
            handler(newValue, oldValue) {
                //newValue是修改后的值，oldValue是修改前的值
                发生修改时执行的语句
            },
            immediate：true, //为true时初始化时就执行一次handler函数中的代码
            deep:true//开启深度监视
        }
    }
})

//方法二
vm.$watch('被监视的属性名',{
    //当该被监视的属性发生修改时调用函数中的代码
    handler(newValue, oldValue) {
    	//newValue是修改后的值，oldValue是修改前的值
    	发生修改时执行的语句
    },
    immediate：true, //为true时初始化时就执行一次handler函数中的代码
    deep:true//开启深度监视
})
```

### 深度检测

Vue中的watch默认不监测对象内部值的改变，但是配置deep属性为true可以监测对象内部值的改变

### 简写形式

当watch中只有一个handler函数时可以开启简写模式

```javascript
//方法一
new Vue({
    watch:{
        被监视的属性名(newValue, oldValue){
            //newValue是修改后的值，oldValue是修改前的值
    		发生修改时执行的语句
        }
    }
})

//方法二
vm.$watch('被监视的属性名',function(newValue, oldValue){
    //newValue是修改后的值，oldValue是修改前的值
    发生修改时执行的语句
})
```



## 绑定样式

### class样式

语法：**:class="xxx"**（xxx可以是字符串、对象、数组）

字符串写法适用于：类名不确定，要动态获取

对象写法适用于：要绑定多个样式，个数不确定，名字也不确定

数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用

### style样式

语法：**:style="{fontSize: xxx}"**（其中xxx是动态值）



## 条件渲染

### v-show

**作用：**可以隐藏元素，但是还占据原本所在位置，需要频繁显示隐藏时推荐使用

```html
语法
<div v-show="true"></div>
```

注意：值可以为变量，布尔值或条件表达式，但是最终都要为布尔值

### v-if

**作用：**可以把元素从页面移除，不占据原来位置，需要频繁显示隐藏时不推荐使用

```html
语法
<div v-if="x==1">1</div>
<div v-else-if="x==2">2</div>
<div v-else>3</div>
```

注意：如果v-if后还有其他表达式则他们之间不能添加其他标签，连在一起的为一组，并且v-if可以搭配template使用



## 列表渲染

### v-for

```html
<ul>
    <li v-for="p in persons">{{p}}</li>
</ul>
```

persons为要循环遍历的对象，需要在data中提前准备好

p为persons中的一个对象

注意：1.循环次数为persons的长度			

2.v-for不仅可以遍历数组对象，也可以遍历单独的一个对象或者字符串或遍历指定次数

### ：key

使用v-for时必须要携带一个key值

```html
<ul>
    <li v-for="(p,index) in persons" :key="index">{{p}}</li>
</ul>
```

key可以是该p对象自己携带的id值，也可以为其索引



## 数据监测

### 对象监测 

对象中后追加的属性，Vue默认不做响应式处理

如需给后添加的数据做响应式，需要使用set( )方法

```js
let data = {
    name: 'Jun',
    sex: '男',
    age: 20
}
const obs = new Observer(data)
let vm = {}
vm._data = data = obs
function Observer(obj) {
    const keys = Object.keys(obj)
    keys.forEach(k => {
        Object.defineProperty(this, k, {
            get() {
                return obj[k]
            },
            set(val) {
                console.log(1);
                obj[k] = val
            }
        })
    })
}
```

### 数组监测

数组中的数据如需要更新，需要调用原生的数组处理事件进行更新

如push，pop，shift，unshift，splice，sort，reverse这些数组原生方法或set( )方法

```vue
<div id="root">
    <button @click="update">点击修改数组第一个值为开车</button>
    <h1>姓名:{{student.name}}</h1>
    爱好:<ul>
        <li v-for="(h,index) in student.hobby" :key="index">{{h}}</li>
    </ul>
</div>

<script>
	const vm = new Vue({
      el: '#root',
      data() {
          return {
              student: {
                  name: '张三',
                  hobby: ['抽烟', '喝酒', '烫头']
              }
          }
      },
      methods: {
          update() {
              this.student.hobby.splice(0, 1, '开车')
          }
      },
  })
</script>
```

### set( )方法

用于给对象或数组添加新内容，但是不能用于vm身上或vm的根数据对象添加属性d

```vue
<div id="root">
    <h2>姓名:{{student.name}}</h2>
    <h2 v-if="student.sex">性别:{{student.sex}}</h2>
    <h2>年龄:{{student.age.rAge}}</h2>
    <button @click="addSex">点我给学生追加性别</button>
</div>

<script>
    const vm = new Vue({
        el: '#root',
        data() {
            return {
                student: {
                    name: '张三',
                    age: {
                        rAge: 40,
                        sAge: 18
                    }
                }
            }
        },
        methods: {
            addSex() {
                // 方法一
                this.$set(this.student, 'sex', '男')
                // 方法二
                Vue.set(this.student, 'sex', '女')
            }
        },
    })
</script>
```



## 表单收集

### 语法

```html
<!-- 不收集空格 -->
账户:<input type="text" v-model.trim="userInfo.username">
<!-- 将收集数据转换为number类型，一般搭配表单的number类型使用 -->
密码:<input type="number" v-model.number="userInfo.password">
<!-- 失去焦点再收集 -->
其他信息:<textarea v-model.lazy="userInfo.other"></textarea>
```



## 过滤器

### 语法

```vue
<div id="root">
    <!-- 计算属性实现 -->
    <h1>现在的时间是{{fmtTime}}</h1>
    <!-- methods实现 -->
    <h1>现在的时间是{{mTime()}}</h1>
    <!-- 过滤器实现(传参) -->
    <h1>现在的时间是{{time | timeFormater('YYYY年MM月DD日 HH:mm:ss')}}</h1>
    <!-- 过滤器实现(不传参) -->
    <h1>现在的时间是{{time | timeFormater}}</h1>
    <!-- 过滤器实现(多个过滤器串联) -->
    <h1>现在的时间是{{time | timeFormater | slice}}</h1>
    <!-- 过滤器(标签内使用) -->
    <h1 :x="msg | slice">标签内使用过滤器</h1>
</div>

<script>
    new Vue({
        el: '#root',
        data() {
            return {
                time: Date.now(),
                msg: '123456789'
            }
        },
        // methods
        methods: {
            mTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        // 计算属性
        computed: {
            fmtTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        // 局部过滤器
        filters: {
            timeFormater(val, str = '') {
                return dayjs(val).format(str)
            },
            slice(val) {
                return val.slice(0, 4)
            }
        }
    })

    // 全局过滤器
    Vue.filter('timeFormater', (val, str = '') => {
        return dayjs(val).format(str)
    })
</script>
```



## 内置指令

### v-text

```vue
<div id="root">
    <h1 v-text="text">我会被覆盖</h1>
</div>

<script>
    new Vue({
        el: '#root',
        data() {
            return {
                text: 'Jun'
            }
        },
    })
</script>
```

### v-html

```vue
<div id="root">
    <!-- v-html指令 -->
    <h1 v-html="text">我会被覆盖</h1>
</div>

<script>
    new Vue({
        el: '#root',
        data() {
            return {
                text: '<a href="#">Jun</a>'
            }
        },
    })
</script>
```

### v-cloak

配合css样式防止未经解析的Vue模板加载在页面，当Vue框架未解析出来之前v-cloak会作为属性保留在标签上，这时可以搭配css做一些操作，当vue解析出来时v-cloak属性会删掉

```vue
<style>
  [v-cloak] {
     display: none;
  }
</style>

<div id="root" v-cloak>{{name}}</div>

<script>
	new Vue({
         el: '#root',
         data() {
             return {
                 name:'Jun'
             }
         },
     })
</script>
```

### v-once

v-once所在的节点会在初次动态渲染之后就视为静态内容了

以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能

```vue
<div id="root">
    <h1 v-once>初始化的n值是{{n}}</h1>
    <h1>当前n值是{{n}}</h1>
    <button @click="n++">点我n++</button>
</div>
<script>
    new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        },
    })
</script>
```

### v-pre

跳过所在节点的编译过程，可利用他跳过没有使用指令语法，插值语法的节点会加快编译

```vue
<div id="root">
    <h1>{{t1}}</h1>
    <h1 v-pre>{{t2}}</h1>
</div>

<script>
    new Vue({
        el: '#root',
        data() {
            return {
                t1: '我会被解析',
                t2: '我不会被解析了'
            }
        },
    })
</script>
```



## 自定义指令

### 语法

```vue
<div id="root">
    <h1>当前的n值是<span v-text="n"></span></h1>
    <h1>放大10倍后的n值是<span v-big="n"></span></h1>
    <button @click="n++">点我n++</button>
    <br>
    <input type="text" v-Fbind:value="n">
</div>
<script>
    new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        },
        // 自定义指令的配置项
        directives: {
		   // 写法一
            //指令与元素成功绑定时会被调用
		   //指令所在的模板重新解析时也会被重新调用  
            big(element, binding) {
                element.innerText = binding.value * 10
            },
            // 写法二
            Fbind: {
                // 指令与元素成功绑定时(一上来)
                bind(element, binding) {
                    element.value = binding.value
                },
                // 指令所在元素被插入页面时
                inserted(element, binding) {
                    element.focus()
                },
                // 指令所在模板被重新解析时
                update(element, binding) {
                    element.value = binding.value
                    element.focus()
                }
            }
        }
    })
    // 写法三(全局自定义指令)
    Vue.directive('Fbind', {
        bind(element, binding) {
            element.value = binding.value
        },
        // 指令所在元素被插入页面时
        inserted(element, binding) {
            element.focus()
        },
        // 指令所在模板被重新解析时
        update(element, binding) {
            element.value = binding.value
            element.focus()
        }
    })
</script>
```



## 生命周期

也叫生命周期回调函数，生命周期函数，生命周期钩子，是vue在关键时候帮我们调用的一些特殊名称的函数，生命周期中的this。指向是vm或组件实例对象

### 挂载流程

```js
const vm = new Vue({
    el: '#root',
    // 在数据代理还未完成前执行,此时data中的数据还不存在
    beforeCreate() {
        console.log('数据代理前')
        console.log(this)
    },
    // 数据代理完成后执行，此时可以通过vm拿到data中的数据
    created() {
        console.log('数据代理完成')
        console.log(this)
    },
    // 此时虚拟DOM已经创建完成但还未放入页面(页面还是呈现未编译的效果),此时所有对DOM的操作都是无效的
    beforeMount() {
        console.log('挂载前')
        console.log(this)
    },
    // 此时页面呈现的都是vue编译后的DOM(此时可以做一些ajax请求,消息订阅),至此初始化过程结束
    mounted() {
        console.log('挂载完成')
        console.log(this)
    }
})
```

### 更新流程

```html
<div id="root">
    <h1>n的值是{{n}}</h1>
    <button @click="n++">点我n++</button>
</div>
<script>
    new Vue({
        el: '#root',
        data() {
            return {
                n: 1
            }
        },
        // 此时数据是新的，但页面还是旧的
        beforeUpdate() {
            console.log('更新完成前' + this.n)
            debugger
        },
        // 此时页面已经更新完毕,数据与页面保持同步
        updated() {
            console.log('更新完成' + this.n)
            debugger
        },
    })
</script>
```

### 销毁流程

```js
new Vue({
    el: '#root',
    // 销毁之前,此时vm处于可用状态,但是对数据的更改不会再触发更新,马上要执行销毁过程(一般在此阶段关闭定时器,取消消息订阅等)
    beforeDestroy() {
        console.log('销毁完成前')
        debugger
    },
    // 此时vm销毁完成(一般不执行操作)
    destroyed() {
        console.log('销毁完成')
        debugger
    }
})
```





# 组件化

## 非单文件组件

### 概念

组件：用来实现局部（特定）功能效果的代码集合，可以复用编码，提高运行效率

模块化：当页面中的js都以模块来编写，那么这个应用就是一个模块化应用

组件化：当页面中的功能都是多组件的方式来编写，那么这个应用就是一个组件化应用

非单文件组件：一个组件中包含多个组件，组件不需要el配置项,因为所有组件最终都要被一个vm管理

### 创建步骤

```vue
<div id="root">
    <!-- 第三步 使用组件 -->
    <student></student>
    <hr>
    <school></school>
</div>
<script>


    /* 第一步 创建组件 */
    const school = Vue.extend({
        name: 'school',
        template: '<div><h1>学校:{{schoolName}}</h1><h1>地址:{{address}}</h1></div>',
        data() {
            return {
                schoolName: '尚硅谷',
                address: '北京'
            }
        }
    })
    const student = Vue.extend({
        name: 'student',
        template: '<div><h1>姓名:{{studentName}}</h1><h1>年龄:{{age}}</h1></div>',
        data() {
            return {
                studentName: '张三',
                age: 18
            }
        },
    })
    new Vue({
        el: '#root',
        /* 第二步 注册组件(局部注册) */
        components: { school, student }
    })

    /* 第二步 注册组件(全局注册) */
    Vue.component('school', school)
</script>
```

### 组件命名规范

只有一个单词：写法一：首字母小写，如school	写法二：首字母大写，如School

多个单词：写法一：横线相隔，如my-school	写法二：驼峰式，如MyShool（只能要在脚手架上）

### 注意

1.组件的本质就是一个构造函数，是由Vue.extend生成的

2.我们只需要写<school></school>使用组件，Vue解析时就会帮我们创建school组件的实例对象

3.每次调用Vue.extend返回的都是一个全新的VC

4.组件配置中：data，methods，watch，computed中的函数的this均是VueComponent实例对象，new vue时则是Vue实例对象(vm)



## 脚手架结构

### 项目结构

```js
// git的忽略文件
.gitignore 
// babel的控制文件(ES6转ES5的控制文件)
babel.config.js
// 包版本控制文件
package-lock.json
// 包的配置文件
package.json
// 项目介绍文件
README.md
// 项目根目录
src
	// 该文件是整个项目的入口文件
	main.js
	// 所有组件的父级
	App.vue
// 项目的页面
public
	// 网站的页签图标
	favicon.ico
	// 整个应用的界面
	index.html
```



## ref属性

### 作用

用来给元素或子组件注册引用信息(id的替代者)

用在html标签上是获得真实的DOM元素，应用在组件标签上是组件实例对象 (VC)

### 语法

```html
<h1 ref="msg">你好</h1>
<MySchool ref="vc"></MySchool>

<!-- 此处获取的是h1的真实DOM元素 -->
console.log(this.$refs.msg)
<!-- 此处获取的是MySchool的组件实例对象(VC) -->
console.log(this.$refs
```

 

## props

### 作用

让组件接收外部传过来的数据，有三种接收方式，props是只读的，vue会检测对props的修改，如果进行了修改就会发出警告，若确实需要修改需将props的数据复制在data中再修改data中的数据

### 父组件语法

```vue
<template>
  <div>
    <MyStudent name="张三" age="18"></MyStudent> <!-- 数据类型不匹配,会报错 -->
    <MyStudent name="李四" :age="20"></MyStudent>
  </div>
</template>
```

### 子组件语法

```vue
<template>
    <div>
        <h1>姓名:{{ name }}</h1>
        <h1>年龄:{{ age + 1 }}</h1>
    </div>
</template>

<script>
export default {
    /* 方式一:简单接收 */
    props: ['name', 'age'],

    /* 方式二:限制类型接收 */
    props: {
        name: String,
        age: Number
    },

    /* 方式三:限制接收 */
    props: {
        name: {
            // 类型
            type: String,
            // 是否必填
            required: true,
            // 默认值
            default: '默认名字'
        }
    }
}
</script>
```

### 修改props

接收到的props不允许更改

```vue
<template>
    <div>
        <h1>姓名:{{ name }}</h1>
        <!-- 2.页面不直接使用props的数据,而是使用data中的数据 -->
        <h1>年龄:{{ Myage + 1 }}</h1>
        <!-- 3.修改直接修改data中的数据 --> 
        <button @click="Myage++">点我age++</button>
    </div>
</template>

<script>
export default {
    name: 'MyStudent',
    props: ['name', 'age'],
    data() {
        return {
           	// 1.在data中定义一个属性,属性值等于传过来的props
            Myage: this.age
        }
    }
}
</script>
```



## mixin

### 概念

mixin,混合也叫混入，功能把多个组件共用的配置提取成一个混入对象

### 局部使用

#### 1.配置混合内容

```js
export const mixin = {
    methods: {
        showName() {
            alert(this.name)
        }
    }
}
```

#### 2.使用混合的组件

```vue
<script>
// 1.局部引入混合
import { mixin } from '../mixin'
export default {
    name: 'MyStudent',
    data() {
        return {
            name: '张三',
            age: 20
        }
    },
    // 2.注册混合
    mixins: [mixin]
}
</script>
```

### 全局混合

1.配置混合内容

```js
export const mixin = {
    methods: {
        showName() {
            alert(this.name)
        }
    }
}
```

2.在mian.js中使用混合

```js
import Vue from 'vue'
import App from './App.vue'
// 引入混合对象
import mixin from './mixin'
// 注册混合
Vue.mixin(mixin)

new Vue({
  render: h => h(App),
}).$mount('#app')
```



## 插件

### 概念

#### 功能

用于增强Vue

#### 本质

包含install方法的一个对象，install的第一个参数是Vue，第二个参数是插件使用者传递的数据

### 语法

1.在src下创建plugins.js文件用于定义插件内容

```js
// 定义插件
export default {
    install(vue) {
        console.log('我是一个插件')
        console.log(vue)
    }
}
```

2.在main.js中使用插件

```js
import Vue from 'vue'
import App from './App.vue'

// 1.引入插件
import plugins from '@/plugins'
// 2.使用插件
Vue.use(plugins)

new Vue({
  render: h => h(App),
}).$mount('#app')
```



## 自定义事件

### 概念

是一种组件间的通信方式，适用于子组件给父组件传递数据

### 绑定

#### 方式一

父组件

```vue
<template>
    <div>
        <!-- 1.为传递数据的子组件绑定一个自定义事件 ( @自定义事件名="调用的函数" ) -->
        <MyStudent @Jun="demo"></MyStudent>
    </div>
</template>

<script>
import MyStudent from './components/MyStudent.vue';
export default {
    name: 'App',
    components: { MyStudent },
    methods: {
        // 2.定义自定义事件中的内容
        demo(name) {
            console.log('子组件传递过来的数据是' + name)
        }
    }
}
</script>
```

子组件

```vue
<script>
export default {
    mounted: {
        // 3.在子组件中调用自定义事件 ( this.$emit(父组件中定义的自定义事件名, 子组件给父组件传递的数据) )
        this.$emit('Jun', '张三')
    }
}
</script>
```

#### 方式二

父组件

```vue
<template>
    <div>
        <!-- 1.为传递数据的子组件绑定一个自定义事件 ( @自定义事件名="调用的函数" ) -->
        <!-- 2.为子组件定义一个ref属性 -->
        <MyStudent @Jun="demo" ref="student"></MyStudent>
    </div>
</template>

<script>
import MyStudent from './components/MyStudent.vue';
export default {
    mounted() {
        // 3.在父组件中调用子组件的自定义事件
        this.$refs.student.$emit('Jun', this.$refs.student.name)
    }
}
</script>
```

### 解绑

```js
// 解绑一个自定义事件
this.$off('自定义事件名')
// 解绑多个自定义事件
this.$off(['自定义事件名1', '自定义事件名2'])
// 解绑所有自定义事件
this.$off()
```



## 全局事件总线

### 概念

一种组件间的通信方式，适用于任意组件间通信

### 语法

1.在main.js中安装全局事件总线

```js
new Vue({
  // 1.安装全局事件总线
  beforeCreate() {
    Vue.prototype.$bus = this
  }  
})
```

2.在接受数据的组件上绑定自定义事件

```js
mounted() {
   	// 通过全局事件总线，在其身上绑定一个自定义事件
    this.$bus.$on('Hello', data => {
        console.log('我是school组件，我接受了数据:' + data)
    })
}
```

3.在传递数据的组件上调用绑定在全局事件总线的自定义事件

```js
mounted() {
    // 通过调用绑定在全局事件总线上的自定义事件调用自定义事件来传递数据
     this.$bus.$emit('Hello', this.name)
}
```

4.记得在组件销毁时接触绑定在全局事件总线上的自定义事件

```js
 beforeDestroy() {
     this.$bus.$off('Hello')
 }
```



## 消息订阅与发布

### 概念

一种组件间的通信方式，适用于任意组件之间进行通信，使用需要引入第三方库

### 语法

1.安装 pubsub 消息订阅库

```js
npm i pubsub-js
```

2.传递数据的组件处

```js
// 1.引入pubsub组件
import pubsub from 'pubsub-js'

// 2.传递数据(消息名,传递的数据)
mounted() {
      pubsub.publish('hello', this.name)
}
```

3.接收数据的组件处

```js
// 1.引入pubsub组件
import pubsub from 'pubsub-js'

// 2.接收数据(回调的第一个参数是消息名称,第二个才是传递过来的数据)
mounted() {
    this.pubId = pubsub.subscribe('hello', (name, data) => {
        console.log('School组件接收到了一个参数' + data)
    })
},
```

4.销毁时取消订阅消息

```js
// 参数是接收消息时的那个对象,在接收数据时需要用一个变量保存,与取消定时器一样
beforeDestroy() {
    pubsub.unsubscribe(this.pubId)
}
```



## nextTick

### 作用

在下一次DOM更新结束后再执行其指定的回调函数体

当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数执行可以使用该api

### 语法

```js
this.$nextTick(()=>{})
```



## 动画

### 动画效果

```vue
<template>
    <div id="root">
        <!-- 1.给需要加动画的标签用transition标签包裹,并设置name属性 -->
        <!-- appear表示第一次进入页面时动画就会执行 -->
        <transition name="Jun" appear>
            <h1 v-show="flag">你好</h1>
        </transition>
    </div>
</template>

<style lang="less" scoped>
#root {
	/* 2.标签来到页面执行的 */
    .Jun-enter-active {
        animation: 3s Jun linear;
    }
	/* 3.标签离开页面执行的 */
    .Jun-leave-active {
        animation: 3s Jun linear reverse;
    }
    
    @keyframes Jun {
        from {
            transform: translateX(-100%);
        }
        to {
            transform: translateX(0%);
        }
    }
}
</style>
```



### 过渡效果

```vue
<template>
    <div id="root">
  	    <!-- 1.给需要加动画的标签用transition标签包裹,并设置name属性 -->
        <!-- appear表示第一次进入页面时动画就会执行 -->
        <transition name="Jun" :appear="true">
            <h1 v-show="flag">你好</h1>
        </transition>
    </div>
</template>

<style lang="less" scoped>
#root {
    h1 {
        background-color: pink;
        transition: 2s;
    }
	/* 标签来到页面时的起点 */
    .Jun-enter {
        transform: translateX(-100%);
    }
	
    /* 标签来到页面时的终点 */
    .Jun-enter-to {
        transform: translateX(0%);
    }
	
    /* 标签离开页面时的起点 */
    .Jun-leave {
        transform: translateX(0%);
    }
	
    /* 标签离开页面时的终点 */
    .Jun-leave-to {
        transform: translateX(-100%);
    }
}
</style>
```

### 多个元素过渡

一个transition标签只能包裹一个元素，如果有多个则需要使用transition-group，并且被包裹的元素需要一个key值

```vue
<transition-group name="Jun">
    <h1 v-show="flag" key="1">你好</h1>
    <h1 v-show="flag" key="2">你好</h1>
</transition-group>
```

### animate动画库

1.安装animate动画库

```js
npm i animate.css
```

2.引入animate动画库样式

```js
import 'animate.css'
```

3.使用animate动画库

```vue
<!-- name为声名该标签要使用animate动画库样式 -->
<!-- enter-active-class为进入页面的样式 -->
<!-- leave-active-class为离开页面的样式 -->
<transition name="animate__animated animate__bounce" enter-active-class="animate__fadeInLeftBig"
    leave-active-class="animate__bounceOutDown" :appear="true">
    <h1 v-show="flag">你好</h1>
</transition>
```



## 跨域

### 概念

同源策略规定协议名，主机名，端口号必须保持一致，否则就会跨域

### 解决方式

#### 1.cors

由后端在响应头配置相关参数，再发送请求

#### 2.jsonp

前端与后端都要进行相关配置，并且只能解决get请求的跨域

#### 3.代理服务器

一个中间服务器，代理服务器与前端服务器一致，负责将数据转发给后端（服务器与服务器之间不存在跨域问题）

### 配置代理服务器

#### 方式一

1.在vue.config.js中配置代理服务器

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  //配置代理服务器(值为请求数据的服务器地址)
  devServer: {
    proxy: 'http://localhost:5000'
  }
})
```

2.在本机中发送请求

```js
axios({
    // 请求地址为该项目的服务器地址
    url: 'http://localhost:8080/students'
}).then(res => console.log(res.data), err => console.log(err.message))
```

#### 方式二

1.在vue.config.js中配置代理服务器

```js
module.exports = defineConfig({
  devServer: {
    proxy: {
      // /api路径下的请求才会进入代理服务器
      '/api': {
        	// 请求数据的服务器地址
        	target: 'http://localhost:5000',
        	// 将请求路径的/api部分转换成空字符串
        	pathRewrite: { '^/api': '' },
        	// 用于支持websocket(默认true)
        	ws: true,
        	// 是否保持跟请求数据的服务器一样的地址(默认true)
        	changeOrigin: true
      	}
    }
  }
})
```

2.在本机中发送请求

```js
axios({
    // 请求地址为该项目的服务器地址
    url: 'http://localhost:8080/api/students'
}).then(res => console.log(res.data), err => console.log(err.message))
```

 

## 插槽

### 默认插槽

#### 父组件

```vue
<template>
  <div id="root">
    <Category :title="'美食'">
      	<!-- 1.在组件标签中传入内容 -->
        <img>
    </Category>
  </div>
</template>
```

#### 子组件

```vue
<template>
    <div>
        <!-- 2.用slot标签表示传入内容在此处展示 -->
        <slot></slot>
    </div>
</template>
```

### 具名插槽

#### 父组件

```vue
<template>
  <div id="root">
    <Category :title="'美食'">
      <!-- 1.在组件标签传入内容同时并定义一个slot属性,值为子组件的name属性的值 -->
      <img slot="header">
      <a href="http://baidu.com" slot="footer">更多美食</a>
    </Category>
  </div>
</template>
```

#### 子组件

```vue
<template>
    <div>
        <!-- 2.在slot标签处定义name属性,在父组件标签传入的值就会在对应name属性的slot标签中 -->
        <slot name="header"></slot>
        <slot name="footer"></slot>
    </div>
</template>
```

### 作用域插槽

数据在子组件中，但根据数据生成的结构需要父组件来决定

#### 父组件

```vue
<template>
  <div id="root">
    <Category :title="'游戏'">
      	<!-- 1.传入的结构需要使用template包裹,传入的数据通过scope属性接受(传递过来的数据是一个对象形式) -->
      	<template scope="data">
        	<ul>
          		<li v-for="(item, index) in data.games" :key="index">{{ item }}</li>
        	</ul>
      	</template>
    </Category>
  </div>
</template>
```

#### 子组件

```vue
<template>
    <div id="box">
		<!-- 2.将数据通过slot标签传入父组件中 -->
        <slot :games="games"></slot>
    </div>
</template>
```



## vuex

### 概念

专门在vue中实现集中式状态（数据）管理的一个vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件通信

当多个组件依赖同一个数据或不同组件需要修改同一数据可以使用vuex

### 搭建环境

第一步：在src下创建store文件夹和index.js

第二步：在index.js中引入vue,vuex并使用vuex

```js
import Vue from 'vue'
import vuex from 'vuex'
Vue.use(vuex)
```

第三步:在index.js创建并暴露store

```js

/* 用于响应组件中的动作 */
const actions = {}
/* 用于操作数据 */
const mutations = {}
/* 用于存储数据 */
const state = {}

// 创建并暴露store
export default new vuex.Store({
    actions, mutations, state
})
```

第四步：在main.js中引入store

```js
import store from './store/index'
```

第五步：将store添加进配置项

```js
new Vue({
  render: h => h(App),
  // 第四步:将store添加进配置项
  store
}).$mount('#app')
```

### 语法

#### main.js中

```js
// 第一步:引入vue,vuex并使用vuex
import Vue from 'vue'
import vuex from 'vuex'
Vue.use(vuex)

/* 用于响应组件中的动作 */
const actions = {
    jiAdd(context, val) {
        if (context.state.sum % 2) context.commit('JIADD', val)
    }
}
/* 用于操作数据 */
const mutations = {
    ADD(state, val) {
        state.sum += val
    },
    JIADD(state, val) {
        state.sum += val
    }
}
/* 用于存储数据 */
const state = {
    sum: 0
}

// 第二步:创建并暴露store
export default new vuex.Store({
    actions, mutations, state
})
```

#### 组件中

```vue
<script>
export default {
    name: 'Count',
    data() {
        return {
            n: 1
        }
    },
    methods: {
        add() {
            // 无处理逻辑就调用store的commit方法直接走mutations
            this.$store.commit('ADD', this.n)
        },
        jiAdd() {
            // 有处理逻辑就调用store的dispatch方法走actions,在actions中完成处理逻辑再走commit方法
            this.$store.dispatch('jiAdd', this.n)
        }
    }
}
</script>
```

### getters

概念：当state中的数据需要加工后再使用时，可以使用getters加工（类似于计算属性）

#### 语法

```vue
<!-- 1.在store中配置处理逻辑 -->
const getters = {
    bigSum(state) {
        return state.sum * 10
    }
}

<!-- 2.在页面使用 -->
<template>
	<h1>当前的值放大十倍为{{ $store.getters.bigSum }}</h1>
</template>
```

### mapState

#### 步骤

1.引入mapState

```js
import { mapState,mapGetters } from 'vuex'
```

2.在计算属性使用mapState

```js
computed: {
    // mapState生成计算属性(对象写法)
    ...mapState({ sum: 'sum', name: 'name', address: 'address' }) 
    // mapState生成计算属性(数组写法)
    ...mapState(['sum', 'name', 'address']),
    ...mapGetters(['bigSum'])
}
```

3.使用变量

```html
<h1>当前的值为{{ sum }}</h1>
<h1>当前的值放大十倍为{{ bigSum }}</h1>
<h1>{{ name }}在{{ address }}</h1>
```



### mapMutations

#### 步骤

1.引入mapMutations

```
import { mapMutations,mapActions } from 'vuex'
```

2.在页面调用方法

```html
<button @click="ADD(n)">+</button>
<button @click="NOADD(n)">-</button>
<button @click="jiAdd(n)">当前值为奇数再加</button> 
<button @click="timeAdd(n)">等一等再加</button>
```

3.在methods中定义mapMutations

```js
methods: {
    // mapMutations对象写法
    ...mapMutations({ add: 'ADD', noAdd: 'NOADD' }),
    // mapMutations数组写法
    ...mapMutations(['ADD', 'NOADD']),
    // mapActions对象写法
    ...mapActions({ jiAdd: 'jiAdd', timeAdd: 'timeAdd' }),
    // mapActions数组写法
    ...mapActions(['jiAdd', 'timeAdd'])
},
```

### 模块化

#### 作用

让代码更好维护，让多种数据分类更明确

#### 语法

在store中

```js
// Count组件
const Count = {
    /* 开启命名空间 */
    namespaced: true,
    /* 用于响应组件中的动作 */
    actions: {},
    /* 用于操作数据 */
    mutations: { },
    /* 用于存储数据 */
    state: {},
    /* 用于将state中的数据进行加工 */
    getters: {}
}

// 第二步:创建并暴露store
export default new vuex.Store({
    modules: {
        /* 暴露的组件名 */
        Count
    }
})
```

组件中

```js
export default {
    methods: {
        ...mapMutations('Count',['ADD', 'NOADD']),
        ...mapActions('Count',['jiAdd','timeAdd'])
    },
    computed: {
        // (模块名,[变量名,变量名])
        // 这样可以直接通过变量名直接访问数据
        ...mapState('Count',['sum', 'name', 'address']),
        ...mapGetters('Count',['bigSum'])
    }
}
```



## 路由

### 概念

vue的一个插件库，专门用来实现SPA应用（单页面应用）

### 使用

第一步：安装vue-router

```js
npm i vue-router
```

第二步：在main.js中引入并使用

```js
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

第三步：在src下创建router文件夹和index.js并配置路由规则

```js
// 1.引入vue-router
import VueRouter from 'vue-router'

// 2.引入配置的组件
import about from '@/components/about'
import home from '@/components/home'

// 3.配置路由规则
export default new VueRouter({
    routes: [
        {	
            // 当应用的路径为path的值时,展示对应的component组件
            path: '/about',
            component: about
        },
        {
            path: '/home',
            component: home
        }
    ]
})
```

第四步：实现路由切换

```html
<!-- active-class可配置高亮样式 -->
<router-link active-class="active" to="/about">About</router-link>
<router-link active-class="active" to="/home">Home</router-link>
```

第五步：设置路由出口

```html
<router-view></router-view>
```

第六步：在main.js引入路由规则

```js
// 1.引入路由
import router from '@/router'

new Vue({
  render: h => h(App),
  // 2.添加路由规则配置项
  router
}).$mount('#app')
```

### 嵌套路由

第一步：在router中的index.js中配置路由规则

```js
import news from '@/pages/news'
import message from '@/pages/message'

{
    path: '/home',
    component: home,
    children: [
        {
            // 此处的路径不要加/
            path: 'news',
            component: news
        },
        {
            path: 'message',
            component: message
        }
    ]
}
```

第二步：在组件中配置路由

```html
<div>
     <h2>我是Home的内容</h2>
     <!-- 此处的路径需要加上父级路径 -->
     <router-link to="/Home/news">News</router-link>
     <router-link to="/Home/message">Message</router-link>
     <router-view></router-view>
</div>
```

### query传参

#### 传参

```html
<!-- 普通写法 -->
<router-link :to="'/home/message/Detail?id=' + item.id + '&title=' + item.title">
    {{ item.title }}
</router-link>

<!-- 对象写法 -->
<router-link :to="{
    path: '/home/message/Detail',
    query: {
        id: item.id,
        title: item.title
    }
}">{{ item.title }}</router-link>
```

#### 接收

```html
<li>消息id:{{ $route.query.id }}</li>
<li>消息标题:{{ $route.query.title }}</li>
```

### 命名路由

可以简化路由的跳转

#### index.js中

```js
{
    name: 'Detail',
    path: 'Detail',
    component: Detail,
}
```

#### 跳转处

```html
<!-- 普通写法 -->
<router-link :to="{name:'Detail'}">
    {{ item.title }}
</router-link>

<!-- 对象写法 -->
<router-link :to="{
    name: 'Detail',
    query: {
        id: item.id,
        title: item.title
    }
}">{{ item.title }}</router-link>
```

### params传参

路由携带params参数时,若使用to的对象写法，则不能使用path配置项，必须使用name配置

第一步：在index.js中声明接受params参数

```js
{
    path: 'message',
    component: message,
    children: [
        {
            path: 'Detail/:id/:title',
            component: Detail,
        }
    ]
}
```

第二步：传递参数

```html
<router-link :to="`/home/message/Detail/${item.id}/${item.title}`">
    {{ item.title }}
</router-link>
```

第三步：使用参数

```html
<li>消息id:{{ $route.params.id }}</li>
<li>消息标题:{{ $route.params.title }}</li>
```

### props

#### 作用

让路由组件更方便的收到参数

```js
{
    path: 'message',
    component: message,
    children: [
        {
            path: 'Detail/:id/:title',
            component: Detail,
            // 第一种写法:对象写法,该对象中的所有key-value最终都会通过props传给组件
            rops: {a: 'a1',b: 'b2'}
            // 第二种写法:布尔值写法,布尔值为true则把路由收到的所有params参数通过props传给组件
            props: true
            // 第三种写法:函数写法,该函数返回的对象中每一组key-value都会通过props传给组件
            props($route){
        		return{
        			id:$route.query.id,
        			name:$route.query.name
        		}
       		}
        }
    ]
}
```

### 编程式路由导航

#### 作用

不借助<router-link>实现路由跳转，让路由跳转更加灵活

#### 语法

```js
// 可回退
this.$router.push({
    path: `/home/message/Detail/${item.id}/${item.title}`
})
//不可回退
this.$router.replace({
    path: `/home/message/Detail/${item.id}/${item.title}`
})
// 后退到上一次
this.$router.back()
// 前进到上一次
this.$router.forward()
// 前进后退到指定次数
this.$router.go(2)
this.$router.go(-2)
```

### 缓存路由组件

#### 作用

让不展示的路由组件保持挂载，不被销毁

#### 语法

```html
<!-- include指具体包含哪个组件不被销毁 -->
<keep-alive include="news">
    <router-view></router-view>
</keep-alive>

<!-- 缓存多个路由组件 -->
<keep-alive include="['news','message']">
    <router-view></router-view>
</keep-alive>
```

### 新生命周期

路由中独有的两个生命周期钩子

```js
// 组件激活时触发
activated() {
    console.log('news组件激活了')
},
// 组件失活时触发
deactivated() {
    console.log('news组件失活了')
}
```

### 路由守卫

#### 全局前置路由守卫

```js
router.beforeEach((to, from, next) => {
    if (to.meta.isAuth) {
        if (localStorage.getItem('Jun') === 'Jun') next()
        else alert('暂无权限')
    } else next()
})
```

#### 全局后置路由守卫

```js
router.afterEach((to, from) => {
    console.log(to, from)
})
```

#### 独享前置路由守卫

```js
beforeEnter(to, from, next) {
    if (localStorage.getItem('Jun') === 'Jun') next()
    else alert('暂无权限')
}
```

#### 组件内路由守卫

```js
// 通过路由规则,进入组件时调用 
beforeRouteEnter(to, from, next) {
    console.log(1)
    next()
},
// 通过路由规则,离开组件时调用 
beforeRouteLeave(to, from, next) {
    console.log(2)
    next()
}
```
