**什么是类型缩小呢？ **

- 类型缩小的英文是 Type Narrowing（也有人翻译成类型收窄）
- 我们可以通过类似于 typeof padding === "number" 的判断语句，来改变TypeScript的执行路径 
- 在给定的执行路径中，我们可以缩小比声明时更小的类型，这个过程称之为 缩小（ Narrowing ）
- 而我们编写的 typeof padding === "number 可以称之为 类型保护（type guards） 

**常见的类型保护有如下几种： **

- typeof 
- 平等缩小（比如===、!==，switch） 
- instanceof 
- in 
- 等等...
### typeof
typeof 运算符可以用来检查值的类型，在 TypeScript 中，可以通过检查值的类型来缩小不同分支中的类型，这种检查值的类型称为类型保护： 
```typescript
type IDType = number | string
function printID(id: IDType) {
  // 1.用typeof实现类型缩小, 将联合的id类型，缩小为string类型
  if (typeof id === 'string') {
    console.log(id.toUpperCase())
  } else {
    console.log(id)
  }
}
```
### 平等缩小
我们可以使用switch或者相等的一些运算符来表达相等性（比如===, !==, ==, and != ），从而实现类型缩小
```typescript
type Direction = "left" | "right" | "top" | "bottom"
function printDirection(direction: Direction) {
  // 1.if判断-缩小类型
  if (direction === 'left') {
    console.log(direction)
  } else if (direction === 'right'){
    console.log(direction)
  }
  // 2.switch判断-缩小类型
  switch (direction) {
    case 'top':
      console.log(direction)
      break;
    default:
      console.log(direction)
      break
  }
}
```
### instanceof
instanceof 运算符用来检查一个值是否是另一个值的“实例”：
```typescript
class Student {
  studying() {}
}
class Teacher {
  teaching() {}
}
function work(p: Student | Teacher) {
  if (p instanceof Student) {
    p.studying()
  } else {
    p.teaching()
  }
}
const stu = new Student()
work(stu)
```
### in
in运算符用于确定对象是否具有指定名称的属性<br />如果指定的属性在指定的对象或其原型链中，则 in 运算符返回 true
```typescript
type Fish = {
  swimming: () => void
}
type Dog = {
  running: () => void
}
function walk(animal: Fish | Dog) {
  // 判断 swimming 是否是 animal对象中的属性
  if ('swimming' in animal) {
    animal.swimming()
  } else {
    animal.running()
  }
}
const fish: Fish = {
  swimming() {
    console.log("swimming")
  }
}
walk(fish)
```
```typescript
// 1.typeof: 使用的最多
function printID(id: number | string) {
  if (typeof id === "string") {
    console.log(id.length, id.split(" "))
  } else {
    console.log(id)
  }
}


// 2.===/!==: 方向的类型判断
type Direction = "left" | "right" | "up" | "down"
function switchDirection(direction: Direction) {
  if (direction === "left") {
    console.log("左:", "角色向左移动")
  } else if (direction === "right") {
    console.log("右:", "角色向右移动")
  } else if (direction === "up") {
    console.log("上:", "角色向上移动")
  } else if (direction === "down") {
    console.log("下:", "角色向下移动")
  }
}


// 3. instanceof: 传入一个日期, 打印日期
function printDate(date: string | Date) {
  if (date instanceof Date) {
    console.log(date.getTime())
  } else {
    console.log(date)
  }

  // if (typeof date === "string") {
  //   console.log(date)
  // } else {
  //   console.log(date.getTime())
  // }
}


// 4.in: 判断是否有某一个属性
interface ISwim {
  swim: () => void
}

interface IRun {
  run: () => void
}

function move(animal: ISwim | IRun) {
  if ("swim" in animal) {
    animal.swim()
  } else if ("run" in animal) {
    animal.run()
  }
}

const fish: ISwim = {
swim: function() {}
}

const dog: IRun = {
run: function() {}
}

move(fish)
move(dog)

```
