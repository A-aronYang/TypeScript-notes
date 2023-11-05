## 联合类型（Union Type） 
[https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)<br />**TypeScript的类型系统允许我们使用多种运算符，从现有类型中构建新类型。 **

- 联合类型是由**两个或者多个其他类型**组成的类型
- 表示可以是**这些类型中的任何一个值**
- 联合类型中的每一个类型被称之为联合成员（union's _members_）
## 使用联合类型
**传入给一个联合类型的值是非常简单的：只要保证是联合类型中的某一个类型的值即可 **

- 但是我们获取的这个值可能是联合类型中的任何一种类型
- 比如我们拿到的值可能是 string 或者 number，由于无法确定具体类型，我们就不能对其调用 string 上的一些方法

![image.png](https://cdn.nlark.com/yuque/0/2023/png/2384107/1697901677144-a16de331-d8b1-4e4b-852c-006c1bfb5c62.png#averageHue=%232b2f38&clientId=ub86086ec-8da8-4&from=paste&height=190&id=u7df021ad&originHeight=238&originWidth=738&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34637&status=done&style=none&taskId=u1c586e16-4c25-4af4-a91c-181850e37d2&title=&width=590.4)<br />**那么我们怎么处理这样的问题呢？ **

- 我们需要使用缩小（Narrow）联合（后续我们还会专门讲解缩小相关的功能） 
- TypeScript 可以根据我们缩小的代码结构，推断出更加具体的类型
```javascript
function printID(id: number | string) {
  console.log("您的ID:", id)

  // 类型缩小
  if (typeof id === "string") {
    console.log(id.length)
  } else {
    console.log(id)
  }
}

printID("abc")
printID(123)
```
