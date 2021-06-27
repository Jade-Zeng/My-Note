#数组类型

## 「类型 + 方括号」表示法
```
let fibonacci: number[] = [1, 1, 2, 3, 5]
```

## 数组泛型
我们也可以使用数组泛型（Array Generic） Array<elemType> 来表示数组：

```angular2html
let fibonacci: Array<number> = [1, 1, 2, 3, 5]；
```
 
## 用接口表示数组

```angular2html
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];

```
NumberArray 表示：只要索引的类型是数字时，那么值的类型必须是数字。

## 类数组
arguments 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：
```angular2html
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
```

## any 在数组中的应用
```angular2html
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```
