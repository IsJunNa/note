## 数据类型

### 字面量类型 '   '

```typescript
// 只能为'nan'
let sex:'nan'

// 只能为'nan'或'nv'
let sex: 'nan' | 'nv'

// 只能为布尔类型或字符串
let sex: boolean | string
```



### 任意类型 any（不建议使用）

```typescript
// 显示any,可以为任意类型 
let name: any

// 不指定类型为隐式any,可以为任意类型 
let name
```



### 未知类型 unknown

```typescript
// 可以为任意类型，但是不能将给其他类型赋值
let name: unknown
name = true
```



### 类型断言<>或as

```typescript
let sex: unknown
let name: string
name = <string>sex
name = sex as string
```



### 空值 void

```typescript
// 表示没有返回值
function fn(): void {}
```



### 没有值never

```typescript
// 表示没有值，常用于报错处理
function fn(): never {
    throw new Error('报错了')
}
```



### 对象object

```typescript
// 限制对象中只能包含string类型的变量name 
let obj: { name: string }

// 对象中必须包含string类型的变量name,变量sex可有可无
let obj: { name: string,sex?: number }

// 变量中必须string类型的变量name，其他随意
let obj: { name: string, [propName: string]: any }
```



### 数组类型 [  ]

```typescript
// 语法一
let arr: string[]

// 语法二
let arr: Array<number>
```



### 元组 [  ]

```typescript
// 固定长度的数组
let arr:[string,number]
```



### 枚举 enum

```typescript
enum Gender { nan = 0, nv = 1 }
let sex1 = Gender.nan
```



### 类型别名 type

```typescript
type myType = 1|2|3|4|5
let nub = myType
```



## 配置文件

在ts根目录创建 tsconfig.json 文件用于配置TS编译器

```typescript
{
    // 指定哪些ts文件需要被编译
   "include": ["../TS类型/"],
       
    // 指定哪些ts文件不需要被编译
   "exclude": ["./TS类型/*"],
       
    // 指定具体哪些ts文件需要被编译
    "files": ["01_helloTs.ts"],
        
    // 编译器选项
    "compilerOptions": {
        // 指定ts被编译的es的版本
        "target": "ES6",
        // 指定要使用的模块化规范
        "module": "ES6",
        // 指定项目中要使用的库
        "lib": [],
        // 指定编译后文件所在的目录
        "outDir": "./dist",
        // 将代码合并为一个文件
        "outFile": "./dist/app.js",
        // 是否对js文件进行编译
        "allowJs": false,
        // 检查js代码是否符合语法规范
        "checkJs": false,
        // 是否移除注释
        "removeComments": false,
        // 是否生成编译后的文件
        "noEmit": false,
        // 当有错误代码时不生成编译后的文件
        "noEmitOnError": false
        // 编译后的js文件是否使用严格模式
        "alwaysStrict": false,
        // 是否允许隐式any类型
        "noImplicitAny": false,
        // 是否允许不明确this的使用
        "noImplicitThis": false,
        // 是否开启严格检查空值
        "strictNullChecks": false
        //严格检查总开关
        "strict": false,
    }
    
}
```



## 面向对象

### 属性

```typescript
// 创建对象
class Person {
    // 实例属性,需要创建对象后才能访问
    name: string
    // 静态属性,无需创建对象就可以访问
    static age: number
    // 只读属性,只能读取不能修改
    readonly sex:number
}

// 初始化对象
const per = new Person()
```



### 方法

```typescript
class dog {
    name: string
    age: number
    // 会在对象创建时调用
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
	// 构造方法
    eat() {
        console.log(this.age + '岁的' + this.name + '在吃饭')
        // this表示当前创造的那个对象
        console.log(this)
    }
}

const dog = new dog('小黑', 3)
```



### 继承

```typescript
// 定义动物类
class animal {
    name: string
    age: number
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
    sayHello() {
        console.log(this.name + '在叫')
    }
}
// 定义狗狗类继承动物类
class dog extends animal {
    eat() {
        console.log(this.name + '在吃骨头')
    }
    // 方法重写
    sayHello() {
        console.log(this.name + '汪汪汪')
    }
}
// 定义猫猫类继承动物类
class cat extends animal {
    run() {
        console.log(this.name + '在跑步')
    }
}
// 初始化狗狗类
const d = new dog('旺财', 5)
d.sayHello()
d.eat()
// 初始化猫猫类
const c = new cat('咪咪', 2)
c.sayHello()
c.run()
```



### super

super表示父类,如果在子类中写了constructor构造函数，此时必须对父类的constructor构造函数进行调用

```typescript
class Animal {
    name: string
    constructor(name: string) {
        this.name = name
    }
    sayHello() {
        console.log(this.name + '在叫')
    }
}
class Dog extends Animal {
    age: number
    constructor(name: string, age: number) {
        // 如果在子类中写了构造函数，此时必须对父类的构造函数进行调用
        super(name)
        this.age = age
    }
    sayHello(): void {
        // super表示当前类的父类
        super.sayHello()
        console.log('汪汪汪')
    }
}
```



### 抽象类

```typescript
// 抽象类,不能用来创建对象,专门用于被继承的类
abstract class Animal {
    name: string
    constructor(name: string) {
        this.name = name
    }
    // 抽象方法,只能定义在抽象类中，子类必须对抽象方法进行重写
    abstract sayHello(): void
}
```



### 接口

1.接口用来定义一个类的结构,用于对类的限制
2.接口中的属性和方法都不能有实际的值，只能定义对象的结构
3.与抽象方法相比，接口中都是抽象方法，而抽象类中可以有抽象方法和普通方法

```typescript
interface MyInterface {
    name: string
    age: number
    sayHello(): void
}
class MyClass implements MyInterface {
    name: string
    age: number
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
    sayHello(): void {
        console.log(this.name + '说Hello')
    }
}
```



### 封装

```typescript
class Person {
    // 公共的,可以在任意位置修改,默认值
    public name: string
    // 受保护的属性,只能在内部和子类中访问
    protected gender:string
    // 私有的,只能在内部访问
    private age: number
    
    // 定义get和set方法可以修改和访问private中的属性
    // get和set语法一
    getAge() {
        return this.age
    }
    setAge(age: number) {
        this.age = age
    }
    // get和set语法二
    get Age() {
        return this.age
    }
    set Age(age: number) {
        this.age = age
    }
}
```



### 泛型

```typescript
// 语法一
function fn1<T>(num: T): T {
    return num
}
fn1(1)
fn1('1')
// 语法二
function fn2<T, K>(n1: T, n2: K): K {
    console.log(n1)
    return n2
}
fn2(1, '2')
```

