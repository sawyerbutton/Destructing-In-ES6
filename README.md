# destructuring -In-ES6

## 理解ES6中的解构

> 解构是ES6中的一种新语法，以更便捷的方式从数组和对象中获取值

## 数组解构能做什么

> 不使用解构语法从数组或对象中获取多个值可能会非常麻烦

```javascript
const items = ['car', 'bike', 'plane'];

const car = items[0];
const bike = items[1];
const plane = items[2];

console.log(car); // 'car'
console.log(bike); // 'bike'
console.log(plane); // 'plane'
```

> 同样的场景使用解构语法

```javascript
const items = ['car', 'bike', 'plane'];
const [car, bike, plane] = items;

console.log(car); // 'car'
console.log(bike); // 'bike'
console.log(plane); // 'plane'
```

> 从本质上来说，解构会自动将数组或对象相对应索引`(数组对应index,对象对应key)`的值分配给变量

### 使用数组解构的小技巧

> 解构可以跳过某些元素，比如通过

```javascript
const items = ['car', 'bike', 'plane'];
const [,,plane] = items;

console.log(plane); // 'plane'
```

> 解构也可以作用于嵌套数组，比如

```javascript
const items = ['car', ['bike', 'plane'], ['boat']];
const [car, [bike, plane], [boat]] = items;

console.log(car); // 'car'
console.log(bike); // 'bike'
console.log(plane); // 'plane'
console.log(boat); // 'boat'
```

> 解构也可以和剩余语法一起使用(rest syntax)

## 对象解构能做什么

> 从对象中抽取数据本身并不复杂，但是解构在此依旧有一席之地

> 对象的解构将对象的属性分配给变量，只是可以指定希望分配的属性和希望被分配的变量，用代码说话

```javascript
const car = {
  brand: 'ferrari',
  type: 'superCar',
  horsepower: 600,
  wheels: 4
};

const { brand: brandName, type: carType } = car;

console.log(brandName); // ferrari
console.log(carType); // superCar
```

> 值得注意的是，如何不介意变量的名称和对象的属性名一致，可以使用解构的简化语法(当然这有可能造成一定的混淆)

```javascript
const car = {
  brand: 'ferrari',
  type: 'superCar',
  horsepower: 600,
  wheels: 4
};

const { brand, type } = car;

console.log(brand); // ferrari
console.log(type); // superCar
```

> 和数组类似，解构也可以作用于嵌套形式的对象

```javascript
const car = {
  brand: 'ferrari',
  type: 'superCar',
  engine: { 
    horsepower: 600,
    liters: 4,
    fuel: 'gas'
  },
  wheels: 4
};

const { engine: { horsepower } } = car;
// engine对应car对象中的engine属性，engine属性的value是一个对象
console.log(horsepower); // 600
```

## 解构语法的用例

> 除了上述的用法之外，解构也可以用来帮助向方法传递参数

> 抛弃以特定顺序传入特定参数的做法，而是直接传入一个对象而使用解构语法来将对象中所需的属性暴露给方法体

```javascript
//  carFunction允许接受一个对象并在传入时解构以获得所需的参数
const carFunction = ({ brand, engine: { horsepower, liters }}) => {
  return `${brand} with engine of ${horsepower} horsepower and ${liters} liters`
}

const car = {
  brand: 'ferrari',
  type: 'superCar',
  engine: {
    horsepower: 600,
    liters: 4,
    fuel: 'gas'
  },
  wheels: 4
};

console.log(carFunction(car)); // ferrari with engine of 600 horsepower and 4 liters
```

> 除了在方法传递参数时可以使用解构外，解构同样也可以应用于处理方法返回多个结果的情况

```javascript
// 返回结果为数组时
const getCars = () => {
  return [
    {
      brand: 'ferrari',
      type: 'superCar',
      engine: {
        horsepower: 600,
        liters: 4,
        fuel: 'gas'
      },
      wheels: 4
    },
    {
      brand: 'porsche',
      type: 'sportsCar',
      engine: {
        horsepower: 455,
        liters: 6,
        fuel: 'gas'
      },
      wheels: 4
    },
  ];
}

const [ferrari, porsche] = getCars()
console.log(ferrari, porsche);
```

```javascript
// 返回结果为对象时
const getCar = () => {
  return {
    brand: 'ferrari',
    type: 'superCar',
    engine: {
      horsepower: 600,
      liters: 4,
      fuel: 'gas'
    },
    wheels: 4
  }
}

const {brand, type} = getCar()
console.log(brand, type);
// ferrari, superCar
```

> 在引入CommonJS模块时，解构可以避免命名混乱的情况

```javascript
const { session } = require('passport');
session();

// instead of
const passport = require('passport');
passport.session();
```

```typescript
import { myFunction } from 'myModule';
myFunction();

// instead of
import * as x from 'myModule';
myModule.myFunction();
```

## 小心

> 如果解构赋值的值不匹配，返回值为undefined

```javascript
let point = {
  x: 1
};
let { x: a, y: b } = point;
console.log(a); // 1
console.log(b); // undefined
```

> 如果在解构时省略var，let或const将会抛出一个错误，因为代码块不能解构赋值

```javascript
let point = {
  x: 1
};
{ x: a } = point; // throws error
```

> 需要将代码块用括号包起来解决这个问题,当然不推荐这种写法

```javascript
let point = {
  x: 1
};
({ x: a } = point);
console.log(a); // 1
```