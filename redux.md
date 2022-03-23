# redux

- 类似于Vuex用于状态管理
- redux 不是react独有的，他不是react开发的，vue和angular中也可以使用

## 原理

![redux原理图](./note-img/redux.png)

### redux三个核心概念

> action

- 动作对象
- 包含两个属性
- - type 标识属性，字符串类型， 唯一，必要属性
- - data 数据， 可选属性

> reducer

- 用于初始化和加工状态
- 加工时，根据action和久的state，来产生一个新的state
- 它是一个纯函数

> store

- 将state action reducer 联系在一起的对象

- 通过 ```store.getState()``` 获取state值
- 通过 ```store.dispath(action对象)``` 来调用reducer里面的函数，但是调用以后，页面没有重新render
- 

