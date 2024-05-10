# TypeScript基础

1. ts转换完代码后会去掉类型注释。
2. ts类型报错默认不会阻断代码转换，可以使用`noEmitOnError`阻断代码输出。
3. Typescript默认会转化为ES3代码，可以使用 `target`指定输出版本。
4.  [`tsconfig.json`](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) 中的 `"strict: true"` 配置项，可以一次性开启全部严格性设置。
5. 启用 [noImplicitAny](https://www.typescriptlang.org/tsconfig#noImplicitAny) 配置项，在遇到被隐式推断为 `any` 类型的变量时就会抛出一个错误。
6. 启用[strictNullChecks](https://www.typescriptlang.org/tsconfig#strictNullChecks) 配置项，让`null` 和 `undefined` 属于单独的类型。



## 基本类型

- string、number、boolean

  类型名称String、Number和Boolean（以大写字母开头）是合法的，但指的是代码中很少出现的一些特殊内置类型。对于类型，始终使用string、number和boolean。

  ~~~ts
  let name: string = 'BS'
  let age: number = 22
  let flag: boolean = true
  ~~~

- Arrays

  ~~~ts
  let arr: Array = []
  let arr1: []  = []
  let arr2: string[] = []
  let arr3: Array<string> = []
  ~~~

- any

  可以表示任意类型。

  当一个值的类型为any时，您可以访问它的任何属性。

  当没有指定类型，并且TypeScript无法从上下文中推断出它时，编译器通常会默认为any。使用编译器标志`noImplicitAny`将任何隐式any标记为错误。

- Functions

  - 函数参数

    ~~~ts
    function greet(name:string){
        console.log("Hello," + name.toUpperCase()+"!!")
    }
    ~~~

  - 函数返回值

    ~~~ts
    function getFavoriteNumber(): number {
      return 26;
    }
    ~~~

    