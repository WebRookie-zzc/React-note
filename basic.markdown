# React

```html
<!--index.html-->
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>React</title>
</head>
<body>

    <div id="app"></div>
<script type="text/javascript" src="react.development.js"></script>
<script type="text/javascript" src="react-dom.development.js"></script>
<script type="text/javascript" src="babel.js"></script>
    <script src="./index.js" type="text/babel"></script>
</body>
</html>
```

```jsx
/*index.js*/
let vDom = <h2>React</h2>

ReactDOM.render(vDom, document.getElementById(`app`))
```

## jsx 语法规则

- 用jsx定义虚拟DOM时， 不用写引号 ```let h2 = <h2>二级标题</h2>```
- 标签中使用js要用 ```{}```
- 类名的指定不能用 ```class``` ， 要用 ```className```
- 内联样式要用 ```style= {{key:value}}``` 的形式去写， 并且使用小驼峰
- 只能有一个根标签
- 标签必须闭合 单标签也必须闭合 ```<input type="text" />``` 或者写成  ```<input type="text"></input>```
- 标签首字母
- - 若小写字母开头，则转换为同名标签
- - 若大写字母开头，则转换为 ```组件```

```jsx
//index.js
let vDom = (
    <div className={className1}>
        <h2 style={{color: `#c200c2`, fontSize: `50px`}}>React</h2>
        <input type="text"/>
    </div>
)

let className1 = `wrapper`

ReactDOM.render(vDom, document.getElementById(`app`))
```

> react 能遍历对象但是不能遍历数组

```jsx
let data1 = [1,2,3]
let data2 = {num1: 1, num2: 2, num3: 3}

let element1 = (<h2>{data1}</h2>)
let element2 = (<h2>{data2}</h2>) //这里会报错
ReactDOM.render(element2, document.getElementById(`app`))
```

> 注意： ```{}``` 中只能写 js 表达式不能写 js 语句

**表达式和语句的区别**

- js 表达式会产生一个值，可以放在任何一个需要值的地方
- js 语句 控制代码走向的是语句 例如 ```if(){} for(){} switch(){case: xxx} ```

> 通过map遍历数组，将数组变为带有标签的数组

```jsx
let numArr = [1,2,3]

let vDom = (
    <div className='wrapper'>
        <h3>numbers</h3>
        <ul>
            {
                numArr.map((number, index) => {
                    return <li key={index}>{number}</li>
                })
            }
        </ul>
    </div>
)

ReactDOM.render(vDom, document.getElementById(`app`))
```

















