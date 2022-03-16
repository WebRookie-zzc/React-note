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

## 组件

### 函数式组件

- 组件的函数名要大写
- 传入 ```ReactDOM``` 中的组件标签要自闭合

```jsx
function MyComponent() {
    let arr = [`a`, `b`, `c`]
    console.log(this) //这里里面的this是undefined， 因为在React编译的时候用的是严格模式
    return (
        <ul>
            {
                arr.map((letter, index) => {
                    return <li key={index}>{letter}</li>
                })
            }
        </ul>
    )
}

ReactDOM.render(<MyComponent/>, document.getElementById(`app`))
```

### 类式组件

- 组件类要继承 ```React.Component```
- 要有 ```render``` 函数并且有返回值
- render 中的 this 是该类的实例对象(组件实例对象 / 组件对象)


### 组件的三大核心属性 (只有类式组件才有)

- state
- ref
- props(函数也可以有，因为函数可以传递参数)

#### state

- 自定义函数要用箭头函数，因为普通函数的this为undefined，拿不到创建的组件实例对象
- 更改state的值要用 ```this.setState({key: value})```

```jsx
class Mycomponent extends React.Component {

    state = {counter: 0}


    render() {
        return (
            <div className="counter">
                <h2>{this.state.counter}</h2>
                {/*这里的C要大写*/}
                <button onClick={this.increaseCounter}>btn</button>
            </div>
        )
    }

    //这里要用箭头函数，因为普通函数的this是undefined
    increaseCounter = () => {
        //改变state要通过setState改变
        this.setState({counter: this.state.counter + 1})
    }
}

ReactDOM.render(<Mycomponent/>, document.getElementById(`app`))
```

#### props

- 通过向组件标签中添加属性来传递props数据
- - 传递数据可以用解构
- - 传递数据可以使用 ```{}``` 也可以用 ```“”```
- 通过 ```this.props``` 来获取传递的数据
- props 中的属性是只读的
- 通过 ```组件.propType``` 来限制传递的数据 或 使用 ```static``` 写在组件类的内部
- 通过 ```组件.defaultProps``` 来规定props的默认值 或 使用 ```static``` 写在组件类的内部

```jsx
class Mycomponent extends React.Component {
    //通过 ```组件.propType``` 来限制传递的数据
    //这个需要导入包才能用, 因为15.x已经弃用了
    static propType = {
        name: PropTypes.string.isRequired, //字符串 必须传递的参数
        age: PropTypes.number,
        speak: PropTypes.func //func 表示函数类型的
    }

//通过 ```组件.defaultProps``` 来规定props的默认值
    static defaultProps = {
        name: `none`,
        age: 123
    }


    state = {counter: 0}


    render() {
        //这里就可以接收到传过来的数据
        let {name, age, phoneNum, message} = this.props
        return (
            <div className="counter">
                <ul>
                    <li>{name}</li>
                    <li>{age}</li>
                    <li>{phoneNum}</li>
                    <li>{message}</li>
                </ul>
            </div>
        )
    }
}
//这里是写在类外面的props数据限制
// //通过 ```组件.propType``` 来限制传递的数据
// //这个需要导入包才能用
// Mycomponent.propType = {
//     name: PropTypes.string.isRequired, //字符串 必须传递的参数
//     age: PropTypes.number,
//     speak: PropTypes.func //func 表示函数类型的
// }
//
// //通过 ```组件.defaultProps``` 来规定props的默认值
// Mycomponent.defaultProps = {
//     name: `none`,
//     age: 123
// }

const info = {name: `redLeaf`, age: 11}

//这里可以用解构 传递数据可以用{} 也可以用“” 但是“”只能传递number类型的数据
ReactDOM.render(<Mycomponent {...info} phoneNum={123} message="hello"/>, document.getElementById(`app`))
```

> 类式组件构造器的作用

- 通过给 ```this.state``` 赋值来初始化内部的state
- 为事件处理函数绑定实例

所以类中的构造器完全可以省略


















