### 字面量类型
除了前面我们所讲过的类型之外，也可以使用字面量类型（literal types）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1697904191805-7a7e1195-bf46-4a72-9844-2d1b59e309a5.png#averageHue=%232b2e37&clientId=u124f5e85-28dd-4&from=paste&height=70&id=udb34fe37&originHeight=87&originWidth=973&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19994&status=done&style=none&taskId=u711e672e-9df0-4286-8f35-83892a3bbb9&title=&width=778.4)
```typescript
// 1."Hello World" 也可作为一种类型, 叫做字面量类型
let message: "Hello World" = "Hello World"
// message = 'coder' // 报错：Type '"coder"' is not assignable to type '"Hello World"'
// 2. 123 也可作为一种类型，叫做字面量类型
let num: 123 = 123
```
默认情况下这么做是没有太大的意义的，但是如果将多个类型联合在一起，就变得有意义了<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1678593690570-702db62d-d988-4819-ae0f-1f27d2041d72.png#averageHue=%232a2f38&clientId=u627d6801-ad18-4&from=paste&height=192&id=ucfbfd8ba&originHeight=240&originWidth=725&originalType=binary&ratio=1.125&rotation=0&showTitle=false&size=53508&status=done&style=none&taskId=u115a087c-57c8-48dc-ba05-910a5c82324&title=&width=580)
```typescript
// 3.字面量类型的意义, 就是必须结合联合类型
type Alignment = 'left' | 'right' | 'center'
let align: Alignment = 'left'// ok
align = 'right'// ok
align = 'center' // ok
// align = 'bottom' // 报错 Type '"bottom"' is not assignable to type
```
上述代码中，我们就限制了 align 变量的取值范围为 left、right、 center，如果赋其他值就会报错

### **字面量推理**
我们来看下面的代码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1678593717992-1ef7302a-244c-438a-acf0-b08157401b1a.png#averageHue=%23292d36&clientId=u627d6801-ad18-4&from=paste&height=263&id=u4f43f203&originHeight=329&originWidth=791&originalType=binary&ratio=1.125&rotation=0&showTitle=false&size=52766&status=done&style=none&taskId=u3152a59e-4fca-4b31-892c-0df67bad61f&title=&width=632.8)<br />这是因为我们的对象在进行字面量推理的时候，info 的推导出的类型其实是`{url: string, method: string}`，所以我们没办法将一个`string` 赋值给一个 字面量类型，`info.method`就会报错<br />下面有三个解决方法
```javascript
// 栗子: 封装请求方法
type MethodType = "get" | "post"
function request(url: string, method: MethodType) {
}

request("http://codercba.com/api/aaa", "post")

// TS细节
// const info = {
//   url: "xxxx",
//   method: "post"
// }
// 下面的做法是错误: info.method获取的是string类型
// request(info.url, info.method)

// 解决方案一: info.method 使用as进行类型断言
// request(info.url, info.method as "post")

// 解决方案二: 直接让info对象类型是一个字面量类型
// const info2: { url: string, method: "post" } = {
//   url: "xxxx",
//   method: "post"
// }
// 解决方法三: 将info推导为字面量类型
const info2 = {
  url: "xxxx",
  method: "post"
} as const // 字面量推理
// const断言告诉编译器为表达式推断出它能推断出的最窄或最特定的类型
// xxxx 本身就是一个 string
request(info2.url, info2.method)

export {}


```
