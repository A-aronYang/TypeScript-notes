在前面，我们通过在类型注解中编写 对象类型 和 联合类型，但是当我们想要多次在其他地方使用时，就要编写多次<br />这时我们可以使用 type 给不同的数据类型起一个别名方便书写使用
```javascript
// 类型别名: type
type MyNumber = number
const age: MyNumber = 18

// 给ID的类型起一个别名
type IDType = number | string

function printID(id: IDType) {
  console.log(id)
}


// 打印坐标
type PointType = { x: number, y: number, z?: number }
function printCoordinate(point: PointType) {
  console.log(point.x, point.y, point.z)
}

```
### <br />
