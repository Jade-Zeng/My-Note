##类型断言
 语法
1. 推荐使用
 ```angular2html
值 as 类型
```
2. 
```angular2html
<类型>值
```
###用途
将一个联合类型断言为其中一个类型

```angular2html
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```
需要注意的是，类型断言只能够「欺骗」TypeScript 编译器，无法避免运行时的错误，反而滥用类型断言可能会导致运行时错误

在进行类的类型断言的时候，要注意，接口是一个类型，不是一个真正的值，它在编译结果中会被删除，当然就无法使用instanceof来做运行时判断
```angular2html
interface ApiError extends Error {
    code: number;
}
interface HttpError extends Error {
    statusCode: number;
}

function isApiError(error: Error) {
    if (error instanceof ApiError) {
        return true;
    }
    return false;
}

// index.ts:9:26 - error TS2693: 'ApiError' only refers to a type, but is being used as a value here.
```
此时只能用类型断言来判断是否存在code属性，来判断传入的参数是不是 ApiError 了：
```angular2html
interface ApiError extends Error {
    code: number;
}
interface HttpError extends Error {
    statusCode: number;
}

function isApiError(error: Error) {
    if (typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}
```

讲函数返回值any断言成具体类型
```angular2html
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```
## 类型断言的限制

当 Animal 兼容 Cat 时，它们就可以互相进行类型断言了：
```angular2html
interface Animal {
    name: string;
}
interface Cat extends Animal {
    run(): void;
}
```
这样的设计其实也很容易就能理解：
* 允许 animal as Cat 是因为「父类可以被断言为子类」，这个前面已经学习过了
* 允许 cat as Animal 是因为既然子类拥有父类的属性和方法，那么被断言为父类，获取父类的属性、调用父类的方法，就不会有任何问题，故「子类可以被断言为父类」

总之，若 A 兼容 B，那么 A 能够被断言为 B，B 也能被断言为 A。
同理，若 B 兼容 A，那么 A 能够被断言为 B，B 也能被断言为 A。

所以这也可以换一种说法：
要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可，这也是为了在类型断言时的安全考虑，毕竟毫无根据的断言是非常危险的。

综上所述：

* 联合类型可以被断言为其中一个类型
* 父类可以被断言为子类
* 任何类型都可以被断言为 any
* any 可以被断言为任何类型
* 要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可
其实前四种情况都是最后一个的特例。
  
## 类型断言和类型转换
```angular2html
function toBoolean(something: any): boolean {
return something as boolean;
}

toBoolean(1);
// 返回值为 1
```

所以类型断言不是类型转换，它不会真的影响到变量的类型。

## 类型断言和类型声明
不能将父类的实例赋值给类型为子类的变量。
