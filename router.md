# react-router5

```shell
npm install react-router-dom@5

npm install react-router-dom@6
```

## react-router-dom

给web开发人员使用的库

> 内置组件

- ``` <BrowserRouter> ```
- ```<HashRouter>```
- ```<Route>```
- ```<Redirect>```
- ```<Link>```
- ```<NavLink>```
- ```<Switch>```

> 其他

- history 对象
- match 对象
- withRouter 函数

### BrowserRouter 和 HashRouter

这两个组件用于包裹 Link 组件，和 Route 组件 前者是history模式，后者是hash\(#\)模式

**注意** : Link 和 Router 要用同一个 BrowserRouter 包裹， 所以我们一般去包裹 App组件

### Link

Link 组件必须被BrowserRouter或者HashRouter包裹

### Route

用于展示组件

### NavLink

他有Link 的功能， 他比Link多了可以用于选中高亮

处于激活状态下，会被加上active(默认)类名

这里可以通过 activeName 来改变

### Switch

Switch 用于 包裹 Route , 匹配到路径以后就不会再往西匹配

```tsx
//index.tsx
import React from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter} from 'react-router-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    {/*注意这里包裹了一层BrowserRouter*/}
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById('root'))
```

```tsx
//App.tsx
import React from 'react';
import {NavLink, Route} from 'react-router-dom';
import Home from './components/Home';
import About from './components/About'
import './App.less';

class App extends React.Component<any> {
  render() {
    return (
      <div className="wrapper">
        <h2>react-router-dom</h2>

        <NavLink activeClassName="active" to="/home" className="home">Home</NavLink>
        <NavLink activeClassName="active" to="/about" className="about">About</NavLink>

        <Switch>
          <Route path="/home" component = {Home} />
          <Route path="/about" component = {About} />
        </Switch>
      </div>
    )
  }
}

export default App;
```

> 路由组件要放在Pages文件夹下

路由组件会收到带有 ```history``` ```location``` ```match``` 的props

### 解决样式丢失的问题

如果请求的路径不存在，会返回index.html

路由路径是多级的时候，而且在刷新后样式会丢失(第一次是正常的)

- 将 ```<link rel="stylesheet" href="./css/bootstapt.css">``` 换为 ```<link rel="stylesheet" href="/css/bootstapt.css">``` (即：相对路径换为绝对路径) 或 ```<link rel="stylesheet" href="%PUBLIC_URL%/css/bootstapt.css">```
- 将 BrowserRouter 换为 HashRouter