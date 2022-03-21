# React 脚手架

```shell
npm install create-react-app -g

create-react-app project_name

# 或者使用typescript
creat-react-app project_name --template-typescript

#暴露配置文件，以修改webpack配置(例如配置less等)(此过程不可逆！！！)
num eject

#运行项目
npm start
```

> 简单的示例代码

- 入口文件

```tsx
//index.tsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
ReactDOM.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>,
    document.getElementById('root')
);

```

- App为放置所有组件的大组件

```tsx
//App.tsx
import React from 'react';
import './App.css';
import Hello from './Hello/Hello'
import Wellcome from "./Wellcome/Wellcome";

class App extends React.Component {
    render() {
        return (
            <div className="App">
                <Hello />
                <Wellcome />
            </div>
        )
    }
}

export default App;
```

```tsx
//Hello.tsx
import "./Hello.less"

import React, {Component} from 'react';

class Hello extends Component {
    render() {
        return (
            <div className="hello">
                <h2 className="title">
                    Hello,React
                </h2>
            </div>
        );
    }
}
export default Hello;
```

> 通过给props传入函数的方式进行 子传父的通信

- nanoid 库可以生成全球唯一的id

### 看一个 todoList 的简单的案例

```tsx
//App.tsx
import React from 'react';
import Header from './Header'
import List from './List'
import Footer from './Footer'
import './App.less';

class App extends React.Component<any> {
  state = {todos: [
      {id: `001`, thing: `吃饭`, done: true},
      {id: `002`, thing: `睡觉`, done: true},
      {id: `003`, thing: `打豆豆`, done: false}
    ]}

  render() {
    const { todos } = this.state;
    return (
      <div className="App">
        <div className="wrapper">
          <Header addTodo = {this.addTodo} todos={todos}/>
          <List todos={todos} changeTodo={this.changeTodo} deleteTodo={this.deleteTodo}/>
          <Footer todos={todos} checkAll={this.checkAll} deleteDoneTodo={this.deleteDoneTodo}/>
        </div>
      </div>
    )
  }

  /**
   * 添加state中的todo对象
   * @param todoObj 要添加的todo对象
   * */
  addTodo = (todoObj:any) => {
    this.setState({todos: todoObj})
  }

  /**
   * 勾选的时候改变state中的状态
   * @param id 点击的item的id
   * @param event checked 的事件对象
   * */
  changeTodo = (id: string, event: any) => {
    console.log(`changeTodo`);
    let {todos} = this.state
    let newTodos = todos.map( (todoObj:any) => {
      if(todoObj.id === id) return {...todoObj, done: event.target.checked}
      return todoObj
    })
    this.setState({todos:newTodos})
  }

  /**
   * 删除todo里面的内容
   * @param id 要删除todo的id
   * */
  deleteTodo = (id:string) => {
    console.log(`deleteTodo`);
    let {todos} = this.state
    let newTodo = todos.filter(todoObj => {
      return todoObj.id !== id
    })
    this.setState({todos: newTodo})
  }

  /**
   * 全选或全取消
   * @param event 事件对象
   * */
  checkAll = ( event:any ) => {
    let {todos} = this.state
    let newTodos = todos.map( (todoObj) => {
      return {...todoObj, done: event.target.checked }
    })
    this.setState({todos:newTodos})
  }

  /**
   * 删除全部已经做过的todo
   * */
  deleteDoneTodo = () => {
    console.log(`deleteDoneTodo`);
    let {todos} = this.state
    let newTodos = todos.filter(todoObj => {
      return !todoObj.done;
    })
    console.log(newTodos);
    this.setState({todos: newTodos})
  }
}

export default App;
```

```tsx
//Header/index.tsx
import React, {Component} from 'react';
import {nanoid} from "nanoid";
import './index.less'

class Header extends Component<any> {
  render() {
    return (
      <div className="header">
        <input type="text" placeholder="请输入要写入的TODO" onKeyUp={this.handleKeyup}/>
      </div>
    );
  }

  handleKeyup = (event:any) => {
    let {addTodo, todos} = this.props
    if (event.key !== `Enter`) return
    //不能传入空
    if(event.target.value.trim() === '') return
    let newTodos = [{id: nanoid(), thing: event.target.value, done: false}, ...todos]
    addTodo(newTodos)
    event.target.value = ''
  }
}

export default Header;
```

```tsx
//List/index.tsx
import React, {Component} from 'react';
import Item from '../Item'
import './index.less'

class List extends Component<any> {
  render() {
    let {todos, changeTodo, deleteTodo} = this.props;
    return (
      <ul className="list">
        {
          todos.map( (item:any) => {
            return <Item key={item.id} id={item.id} thing={item.thing} done={item.done} changeTodo = {changeTodo} deleteTodo={deleteTodo}/>
          })
        }
      </ul>
    );
  }
}

export default List;
```

```tsx
//Item/index.tsx
import React, {Component} from 'react';
import './index.less'

//规定传入的prop的数据类型
interface Props {
  thing: string,
  done: boolean,
  changeTodo: any,
  id: string,
  deleteTodo: any
}

class Item extends Component<Props> {
  render() {
    const {id, thing, done, changeTodo, deleteTodo} = this.props
    return (
      <li className={`item`}>
        <input type="checkbox"
               checked={done}
               onChange={(event) => changeTodo(id, event)} />
        <span>{thing}</span>
        <button onClick={ () => deleteTodo(id) }>删除</button>
      </li>
    );
  }
}

export default Item;
```