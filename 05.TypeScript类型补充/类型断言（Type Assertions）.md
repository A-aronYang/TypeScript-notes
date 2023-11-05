有时候TypeScript无法获取具体的类型信息，这个我们需要使用类型断言（Type Assertions）。
## 类型断言as
我们可以使用关键字 as 进行类型断言<br />比如我们通过 `document.getElementById`获取`img`元素，TypeScript 只知道该函数会返回 `Element` ，但并不知道它具体的类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1697902469257-f2fcb346-a924-412a-8465-75f6b18816f5.png#averageHue=%232b2f38&clientId=ue46bf976-f071-4&from=paste&height=132&id=u265edfc6&originHeight=165&originWidth=605&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21470&status=done&style=none&taskId=u4e7c14fe-f981-43d6-8bf8-d2daa485bf3&title=&width=484)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1697902484147-df4be003-c77b-4a8d-b542-7d4642432468.png#averageHue=%232b2e37&clientId=ue46bf976-f071-4&from=paste&height=135&id=ub727a2d9&originHeight=169&originWidth=678&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21295&status=done&style=none&taskId=ucd35bbf8-2af8-4b58-a894-e0f1f0195cf&title=&width=542.4)<br />当我们确定一个元素的类型时，可以使用类型断言<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1686997459520-a009d2a1-8406-4df9-8f96-af58f9297d0b.png#averageHue=%23292e37&clientId=u350bbf54-38d0-4&from=paste&height=86&id=uc96a0aa6&originHeight=108&originWidth=636&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11570&status=done&style=none&taskId=uebab1a1a-0592-4cf0-b636-dd8abfb7ceb&title=&width=508.8)<br />类型断言的规则：断言只能断言成更加具体的类型, 或者 不太具体(any/unknown) 的类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1686997549104-66f05f32-f774-4450-af05-ce768c05f84a.png#averageHue=%23292e37&clientId=u350bbf54-38d0-4&from=paste&height=62&id=u4bfb699a&originHeight=78&originWidth=453&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7153&status=done&style=none&taskId=u25c1d777-8226-4555-8839-1e7d0b35dc9&title=&width=362.4)![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1686997573341-f468c83f-03fd-4d85-b681-287da528d0a4.png#averageHue=%232a3039&clientId=u350bbf54-38d0-4&from=paste&height=89&id=u81fa93f1&originHeight=111&originWidth=496&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13674&status=done&style=none&taskId=ua2ffae03-c074-483e-854f-cab1e5f20bf&title=&width=396.8)
## 非空类型断言!
**当我们编写下面的代码时，在执行ts的编译阶段会报错： **<br />这是因为传入的 message 有可能是为 undefined 的，这个时候是不能执行方法的<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/2384107/1666272911263-2b318f7a-4785-48f4-9948-a51ee6ae1580.png#averageHue=%232b313b&clientId=u4c8db111-70bf-4&errorMessage=unknown%20error&from=paste&height=164&id=u90fd3cb0&originHeight=205&originWidth=641&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94156&status=error&style=none&taskId=u609c8320-052c-471d-8b2c-6448da9ef02&title=&width=512.8)<br />为了解决这个问题，可以使用 if 语句进行非空判断，**如果我们可以肯定传入的参数是有值的，这个时候我们可以使用非空类型断言**<br />非空断言使用的是`!`，表示可以确定某个标识符是一定有值的，可以**跳过ts在编译阶段对它的检测**<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/2384107/1666272918884-749000ef-bcaa-494e-849a-1b62c52db265.png#averageHue=%232d343f&clientId=u4c8db111-70bf-4&errorMessage=unknown%20error&from=paste&height=107&id=u21c14b76&originHeight=134&originWidth=675&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79752&status=error&style=none&taskId=ub6f91141-49bb-4377-9f9e-619ba83bd78&title=&width=540)
```typescript
// 定义接口
interface IPerson {
  name: string
  age: number
  friend?: {
    name: string
  }
}

const info: IPerson = {
  name: "why",
  age: 18
}

// 属性赋值:
// 解决方案一: 类型缩小
if (info.friend) {
  info.friend.name = "kobe"
}

// 解决方案二: 非空类型断言(有点危险, 只有确保friend一定有值的情况, 才能使用)
info.friend!.name = "james"

```
## 可选链的使用
可选连`?.`是在ES2020（ES11）中新增的特性，通常在定义属性和获取值的时候使用

- 当对象的属性不存在的时会发生短路，返回undefined
- 当对象的属性存在时才会继续执行，例如 info.friend?.age
```typescript
interface IPerson {
  name: string
  age: number
  friend?: {
    name: string
  }
}

const info: IPerson = {
  name: "why",
  age: 18
}

// 访问属性: 可选链: ?.
console.log(info.friend?.name)
```
## !!操作符
`!!`操作符（双非运算符）可以将一个其他类型的元素转换成 boolean 类型，类似于`Boolean(变量)` 这种函数转换的方式<br />[逻辑非（!） - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_NOT#%E5%8F%8C%E9%9D%9E%E8%BF%90%E7%AE%97%E7%AC%A6%EF%BC%88!!%EF%BC%89)
```typescript
const message = "Hello World"
// 1.以前的方式转成boolean类型
// const flag = Boolean(message)
// console.log(flag)
// 2.或使用 !! 操作的方式转成boolean类型
const flag = !!message
console.log(flag) // true
export {}
```
## ??操作符
`??`操作符是 ES11 新增的空值合并运算符，用于判断一个值是否为 null 或 undefined。<br />当左侧的操作数为 [null](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/null) 或者 [undefined](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined) 时，返回其右侧操作数，否则返回左侧操作数。
```typescript
let message: string | null = 'Hello World'
// 1.以前的方式：使用三元运算符判空赋默认值(判断null、undefined、''、false)
const content1 = message ? message: "你好啊, 李银河1"
// 2.或使用 || 操作符判空赋默认值(判断null、undefined、''、false)
const content2 = message || "你好啊, 李银河2"
// 3.或使用 ?? 操作符判空赋默认值(只判断 null 或 undefined)
const content3 = message ?? "你好啊, 李银河3"
console.log(content1, content2, content3)
export{}
```

