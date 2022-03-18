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

#### refs

> 组件标签可以通过 ```ref``` 来标识自己

- 通过ref获得的是 ```真实DOM``` 而不是 ```虚拟DOM```
- refs 常常和事件绑定一起使用

> 字符串形式的 ```ref``` (但是官方不推荐使用，有可能废弃 效率不高)

```jsx
class Demo extends React.Component{
    render() {
        return (
            <div className="wrapper">
                {/*字符串形式的ref*/}
                <input type="text" ref="input"/>
                <button onClick={this.showData}>button</button>
            </div>
        )
    }

    showData = () => {
        const {input} = this.refs
        console.log(input.value);
    }

}

ReactDOM.render(<Demo/>, document.getElementById(`app`))
```

> 回调函数形式的ref

- 内联绑定回调
- 类绑定回调

- 写在ref中的函数， ref自动调用，当前的dom节点作为参数传递给这个函数

```jsx
class Demo extends React.Component{
    render() {
        return (
            <div className="wrapper">
                {/*回调函数形式的ref*/}
                <input type="text" ref={currentElement => this.input = currentElement}/>
                {/*下面这一行代码和上面的代码等效*/}
                {/*<input type="text" ref={(currentElement) => {this.input = currentElement}}/>*/}
                <button onClick={this.showData}>button</button>
            </div>
        )
    }

    showData = () => {
        const {input}  = this
        console.log(input.value);
    }

}

ReactDOM.render(<Demo/>, document.getElementById(`app`))
```

- 回调的ref调用次数的问题

> 如果 ref 回调函数是以**内联函数**的方式定义的，在**更新**过程中它会被执行两次，第一次传入参数 null，然后第二次会传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 **class 的绑定函数**的方式可以避免上述问题，但是大多数情况下它是无关紧要的(所以在开发中用的多的是内联的写法)。
>> 上面 ```<input ref={currentElement => this.input = currentElement}>``` 就是内联函数
>> `更新`：是指当页面**再次**调用render函数的时候
>> render 第一次渲染的时候只传一次dom，不传null

> 而类绑定回调页面刷新时，也不会再次调用(只第一次渲染的时候调用)

```jsx
class Demo extends React.Component {
    state = {isBig: true}
    render() {
     return (
      <div className="wrapper">
          <input type="text" ref={currentElement =>{ this.input = currentElement;
              console.log(currentElement);
          } }/>
          <h2>The number is {this.state.isBig ? `big` : `small`}</h2>
          <button onClick={this.changeNumber}>button</button>
      {/*    点击按钮的时候会输出一个 null 和一个dom对象*/}
      </div>   
     )   
    }
    changeNumber = () => {
        this.setState({isBig: !this.state.isBig})
    }
}

ReactDOM.render(<Demo/>, document.getElementById(`app`))
```

> 通过 ```createRef``` 创建 ```ref```

- 通过 ```this.React.createRef()``` 创建一个用于存储ref的容器，但是他只能存储一个，后面放入的元素会把前面的元素的顶掉

```jsx
class Demo extends React.Component {
    input = React.createRef()
    //这里会成为一个对象 {current: dom节点} 所以取的时候需要 .current
    render() {
        return (
            <div className="wrap">
                <input type="text" ref={this.input}/>
                <button onClick={this.showValue}>button</button>
            </div>
        )
    }
    showValue = () => {
        // 通过 .current 取dom节点
        console.log(this.input.current.value);
    }
}

ReactDOM.render(<Demo/>, document.getElementById(`app`))
```

### 事件处理

- 通过 ```onXxx``` 指定事件处理函数
- - react使用的是自定义(合成)事件，而不是原生dom中的事件 (为了更换的兼容性)
- - react中的事件是通过事件委托(通过事件冒泡)的方式处理的(委托给组件最外层的元素)
- 通过 ```event.target``` 得到发生事件的dom元素对象

> 当发生事件的元素和要操作的元素相同是，使用 ```event.taget``` 而不使用 ```ref```, 因为要避免过度使用ref

```jsx
class Demo extends React.Component {
    render() {
        return (
            <div className="wrapper">
                <input type="text" onBlur={this.showValue}/>
            </div>
        )
    }
    showValue = (event) => {
        console.log(event.target.value);
    }
}

ReactDOM.render(<Demo/>, document.getElementById(`app`))
```

### 受控组件 和 非受控组件

#### 非受控组件

- 有需要输入的元素的组件，现用现取用，就是非受控组件

```jsx
class LogIn extends React.Component{
    state = {phoneNum: `123`, password: `456`}
    phone
    render() {
        return (
            <div className="wrapper">
                <form onSubmit={this.handleSubmit}>
                    <label>
                        phoneNumber:
                        <input type="text" ref={element => this.phoneNum = element}/>
                    </label>
                    <label>
                        password:
                        <input type="password" ref={element => this.password = element}/>
                    </label><br/>
                    <button>logIn</button>
                </form>
            </div>
        )
    }
    handleSubmit = (event) => {
        //阻止表单提交的默认事件
        event.preventDefault()
        if(this.phoneNum.value === this.state.phoneNum && this.password.value === this.state.password){
            alert(`success`)
        }else{
            alert(`error`)
        }
    }
}

ReactDOM.render(<LogIn/>, document.getElementById(`app`))
```

#### 受控组件

- 通过state维护组件(类似于Vue中的双向绑定)

```jsx
class LogIn extends React.Component {
    state = {
        dataBase: {
            phoneNum: `123`,
            password: `456`
        },
        phoneNum: ``,
        password: ``
    }

    render() {
        return (
            <div className="wrap">
                <form onSubmit={this.handleSubmit}>
                    <label>
                        phoneNum:
                        <input type="text" onChange={this.getPhoneNum}/>
                    </label><br/>
                    <label>
                        password:
                        <input type="password" onChange={this.getPassword}/>
                    </label><br/>
                    <button>submit</button>
                </form>
            </div>
        )
    }

    getPhoneNum = (event) => {
     this.setState({phoneNum: event.target.value})
    }

    getPassword = (event) => {
        this.setState({password: event.target.value})
    }

    handleSubmit = (event) => {
        event.preventDefault()
        if(this.state.phoneNum === this.state.dataBase.phoneNum && this.state.password === this.state.dataBase.password) {
            alert(`success`)
        }else{
            alert(`error`)
        }
    }
}

ReactDOM.render(<LogIn/>, document.getElementById(`app`))
```

### 高阶函数_函数柯里化 (这个就不是React知识)

- 高阶函数：函数的接收到的参数或返回值依然是一个函数
- 函数柯里化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式

> 函数柯里化的简单例子

```js
function sum(a) {
    return (b) => {
        return (c) => {
            return a+b+c
        }
    }
}

sum(1)(2)(3) //1+2+3
```

> 利用柯里化优化上面的登录的代码

如果有非常多的input(如注册页面,填一堆信息)， 那么上面的代码重复太多

```jsx
class LogIn extends React.Component {
    state = {
        dataBase: {
            phoneNum: `123`,
            password: `456`
        },
        phoneNum: ``,
        password: ``
    }

    render() {
        return (
            <div className="wrap">
                <form onSubmit={this.handleSubmit}>
                    <label>
                        phoneNum:
                        <input type="text" onChange={this.saveFormData(`phoneNum`)}/>
                    </label><br/>
                    <label>
                        password:
                        <input type="password" onChange={this.saveFormData(`password`)}/>
                    </label><br/>
                    <button>submit</button>
                </form>
            </div>
        )
    }

    saveFormData(dataType) {
        return (event) => {
            this.setState({[dataType]: event.target.value})
        }
    }

    handleSubmit = (event) => {
        event.preventDefault()
        if(this.state.phoneNum === this.state.dataBase.phoneNum && this.state.password === this.state.dataBase.password) {
            alert(`success`)
        }else{
            alert(`error`)
        }
    }
}

ReactDOM.render(<LogIn/>, document.getElementById(`app`))
```

### React 组件的生命周期

> 旧版本的生命周期

- 分为三部分
> 初始化组件
> 更新组件
> 卸载组件

![旧版本生命周期](./note-img/React生命周期(旧).png)

- shouldComponentUpdate 用于判断是否往下运行， 如果他的返回值是 ```false``` 那么就不会往下走，就调用不到render， 页面就不会改变，如果返回的是 ```true```， 那么就会继续往下调用 componentWillUpdate, 进而调用render改变页面，如果我们不写这个钩子，react默认返回true, 他就相当于控制页面是否更新的**阀门**
- 执行完 ```setState``` 就要进入 ```shouldComponentUpdate```

- 通过 ```forceUpdate()``` 能在不修改 ```state``` 的情况下，对页面发生更新 (绕过了shouldComponentUpdate)
- componentDidUpdate 可以接收三个参数(React自己传入), 第一个是**先前**传递的props， 第二个是 更新**之前**的 state，第三个是 ```getSnapshotBeforeUpdate``` 生命周期钩子的返回值

```jsx
class Demo1 extends React.Component {
    constructor(props) {
        super(props);
        console.log(`Demo1-constructor`);

        this.state = {isRed: true}
    }

    //挂载前调用
    componentWillMount() {
        console.log(`Demo1-componentWillMount`);
    }

    //首次渲染或者更新时调用
    render() {
        console.log(`Demo1-render`);
        return (
            <div className="wrapper">
                <h2 style={ {color: this.state.isRed ? `red` : `blue`} }>Color</h2>
                <button onClick={this.changeColor}>changeColor</button>
                <button onClick={this.deleteComp}>unmount</button>
                <button onClick={this.force}>不改state，强制更新页面</button>
            </div>
        )
    }

    changeColor = () => {
        let {isRed} = this.state
        this.setState({isRed: !isRed})
    }

    deleteComp = () => {
        ReactDOM.unmountComponentAtNode(document.getElementById(`app`))
    }

    force = () => {
        this.forceUpdate()
    }

    //挂载完成后调用
    componentDidMount() {
        console.log(`Demo1-componentDidMount`);
    }

    //控制页面是否更新的 "阀门"
    shouldComponentUpdate() {
        console.log(`Demo1-shouldComponentUpdate`);
        //必须返回布尔类型
        return true
    }

    //组件将要更新时调用
    componentWillUpdate() {
        console.log(`Demo1-componentWillUpdate`);
    }

    //组件更新完毕时调用
    componentDidUpdate() {
        console.log(`Demo1-componentDidUpdate`);
    }

    //卸载前调用
    componentWillUnmount = () => {
        console.log(`Demo1-componentWillUnmount`);
    }
}

ReactDOM.render(<Demo1/>, document.getElementById(`app`))
```

- 父组件的render触发子组件的 ```componentWillreceiveProps()``` 生命周期钩子

```jsx
class Parent extends React.Component {
    state  = {isRed: true}

    render() {
        return (
            <div className="wrapper">
                <h2>Parent</h2>
                <button onClick={this.changeColor}>changeColor</button>
                <Children isRed={this.state.isRed}/>
            </div>
        )
    }

    changeColor = () => {
        let {isRed} = this.state
        this.setState({isRed: !isRed})
    }

}

class Children extends React.Component {

    componentWillReceiveProps(newProps) {
        console.log(`children-componentWillReceiveProps`);
        console.log(newProps);
    }

    render() {
        return (
            <div className="wrapper">
                <h2 style={ {color: this.props.isRed ? `red` : `blue`} }>Children</h2>
            </div>
        )
    }
}

ReactDOM.render(<Parent />, document.getElementById(`app`))
```

> 新版本的生命周期

![新版本的生命周期](./note-img/React生命周期(新).png)

在React 17.x版本中， 有三个生命周期钩子被加上了弃用警告，要使用要加上```UNSAFE_``` 前缀

- componentWillMount --> ```UNSAFE_componentWillMount``` 下同
- componentWillUpdate
- componentWillReceiveProps

注意：这里的```UNSAFE``` 和安全性无关，只是React官方预测到之后使用异步渲染这三个钩子可能会出现bug， 和这三个钩子的滥用

增加了 ```getDerivedStateFromProps``` 和 ```getSnapshotBeforeUpdate```, 但是我们在开发中极少使用这两个钩子

- getDerivedStateFromProps -翻译-> 得到一个派生的state从Props中

这个钩子不是个给实例使用的，所以用的时候要加上 ```static```, 并且在是使用的时候返回一个**状态对象**(或者是null),它接收一个参数，就是组件传入的props，使得state中的值在人话时刻都取决于传入的props中的值

```jsx
class Demo extends React.Component {
    state = {count: 1}
    render() {
        return (
            <div className="wrapper">
                <h2>{this.state.count}</h2>
                <button onClick={this.increment}>add1</button>
            </div>
        )
    }
    increment = () => {
        this.setState({count: this.state.count + 1})
    }

    static getDerivedStateFromProps(props) {
        console.log(`getDerivedStateFromProps`);
        //这里就会把props 放入state中，但是state就改变不了了
        return props
    }
}

ReactDOM.render(<Demo count={100} />, document.getElementById(`app`))
```

- getSnapshotBeforeUpdate(prevProps, prevState) snapshop --> ```快照``` 的意思, 此生命周期钩子的返回任何返回值都将作为参数传递给 ```componentDidUpdate```

### DOM 的 diffing 算法

diffing 算法的最小单位是 标签(节点)， 是深层次的对比，逐层对比

#### React 和 Vue中 key 的原理和作用

- 经典面试题：
- - Vue和React中的key的作用
- - 为什么key中的值不建议使用index

> 虚拟dom中 key 的作用

key是虚拟dom对象的表示，在更新显示时， key起着至关重要的作用

![diff](./note-img/diff1.png)

![diff](./note-img/diff2.png)

> 使用index作为key值可能造成严重的效率问题

例如：下面的例子，如果将添加的元素放入数组的第一位，那么整个数组的index和key值都会改变，从而导致整个数组都对重新渲染

```jsx
class Demo extends React.Component {
    state = {
        letters: [`a`, `b`, `c`]
    }
    render() {
        return (
            <div className="wrapper">
                <h2 className="title">letters</h2>
                <ul>
                    {this.state.letters.map( (letter, index) => {
                        return <li key={index}>{letter}</li>
                    } )}
                </ul>
                <button onClick={this.addLetter}>addLetter</button>
            </div>
        )
    }

    addLetter = () => {
        console.log(1);
        this.setState({letters: [`d`, ...this.state.letters]})
    }
}

ReactDOM.render(<Demo/>, document.getElementById(`app`))
```
















