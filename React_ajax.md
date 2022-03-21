# React ajax

## 配置代理，解决跨域问题

> 第一种方法：改变React里面的package.json， 通过中间件获取数据

- 后端服务器代码

```javascript
const express = require('express');

const app = express();

app.get(`/students`, (req, res) => {
    const student = [
        {'id': '1', stuName: 'qqq'},
        {'id': '2', stuName: 'www'},
        {'id': '3', stuName: 'eee'},
        {'id': '4', stuName: 'rrr'},
    ]
    res.send(student)
})

app.listen(5000, () => {})
```


前端脚手架跑在3000端口，直接请求会产生跨域问题

```tsx
import React from 'react';
import axios from 'axios';
import './App.less';

class App extends React.Component<any> {
  render() {
    return(
      <div className="wrapper">
        <button onClick={this.getStudentInfo}>getInfo</button>
      </div>
    )
  }

  getStudentInfo = () => {
    axios({
      url: `http://localhost:5000/students`,
      method: 'GET'
    })
      .then( res => {
        console.log(res.data);
      } , req => {})
      .catch( err => {
        console.log(err);
      })
  }
}

export default App;
```

在package.json中添加

```json
{
  "proxy": "http://localhost:5000"
}
```

将tsx请求的端口改为3000 即 ```url: `http://localhost:5000/students` ```

> 第二种在src中创建 setupProxy.js 要用commonjs写

```js
//setupProxy.js
const {createProxyMiddleware:proxy} = require('http-proxy-middleware');

module.exports = function (app) {
    app.use(
        proxy('/api', {
            target: 'http://localhost:5000',
            changeOrigin: true,
            pathRewrite: {'^/api': ''}
        })
    )
}
```

```tsx
import React from 'react';
import axios from 'axios';
import './App.less';

class App extends React.Component<any> {
  render() {
    return(
      <div className="wrapper">
        <button onClick={this.getStudentInfo}>getInfo</button>
      </div>
    )
  }

  getStudentInfo = () => {
    axios({
      url: `http://localhost:3000/api/students`,
      method: 'GET'
    })
      .then( res => {
        console.log(res.data);
      } , req => {})
      .catch( err => {
        console.log(err);
      })
  }
}

export default App;
```

## 搜索github用户的功能

```tsx
import React from 'react';
import Search from './components/Search';
import List from './components/List'
import './App.less';
import axios from "axios";

class App extends React.Component<any> {
  state = {usersArr: []}
  render() {
    return(
      <div className="App">
        <div className="wrapper">
          <Search changeUsersArr={this.changeUsersArr}/>
          <List usersArr={this.state.usersArr}/>
        </div>
      </div>
    )
  }

  /**
   * 搜索后更新用户列表
   * @param keyword 用户输入的关键词
   * */
  changeUsersArr = async (keyword:string) => {
    console.log(keyword);
    let res = await axios({
      url: `https://api.github.com/search/users`,
      method: `get`,
      params: {
        q: keyword
      }
    })
    console.log(res.data);
    this.setState({usersArr: res.data.items})
  }
}

export default App;
```

```tsx
import React, {Component} from 'react';
import './index.less'

class Search extends Component<any> {

  input:any = React.createRef()
  render() {
    let {changeUsersArr} = this.props

    return (
      <div className="search">
        <div className="search_wrapper">
          <input type="text"
                 placeholder="Please enter the name of a user"
                 ref={currentElement => this.input = currentElement}/> &nbsp; &nbsp; &nbsp;
          <div className="btn" onClick={() => changeUsersArr(this.input.value)}>Search</div>
        </div>
      </div>
    );
  }

  search = async () => {

  }
}

export default Search;
```

```tsx
import React, {Component} from 'react';
import './index.less'

class List extends Component<any> {
  render() {
    let {usersArr} = this.props
    return (
      <ul className="list">
        {
          usersArr.map( (item:any) => {
            return (
              <li>
                <img src={item.avatar_url} alt=""/>
                <span>{item.login}</span>
              </li>
            )
          })
        }
      </ul>
    );
  }
}

export default List;
```