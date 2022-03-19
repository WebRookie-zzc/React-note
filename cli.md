# React 脚手架

```shell
npm install create-react-app -g

create-react-app project_name

# 或者使用typescript
creat-react-app project_name --template-typescript

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

