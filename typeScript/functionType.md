## 函数声明
```angular2html
function sum(x: number, y: number): number {
    return x + y;
}
```

## 函数表达式
```angular2html
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

=> 表示函数的定义 左边是输入类型, 需要用括号括起来， 右边是输出类型

## 用接口定义函数形状
```angular2html
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

## 可选参数
可选参数必须出现在必须参数最后

## 剩余参数(rest参数)
```angular2html
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

## 重载
```angular2html
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```
上例中，我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。
