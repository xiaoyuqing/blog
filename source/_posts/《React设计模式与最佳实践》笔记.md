---
title: 《React设计模式与最佳实践》笔记
date: 2019-03-21 17:07:54
categaries: React
tags: 
- web前端
- React
---


书里的demo都是15.3.2以下版本的，有些demo用最新的react 16.x版本会报错，安装包的时候记得改一下版本
 
### 第一章 React 基础
命令式编程描述代码如何工作，而声明式编程则表明想要实现什么目的
 
### 第二章 整理代码
展开属性操作符也是一项很重要的特性
{...props}
 
常见模式
1. 多行书写
2. 多个属性的书写
3. 条件语句
render-if包
4. 循环
map
5. 控制语句
jsx-control-statements
6. 次级渲染
拆分组件
 
ESLint
.eslintrc 文件
插件 eslint-plugin-react eslint-config-airbnb eslint@^2.9.0 eslint-pluginjsx-a11y@^1.2.0 eslint-plugin-import@^1.7.0 eslint-plugin-react@^5.0.1
 
函数式编程基础
纯粹函数是指它不产生副作用，也就是说它不会改变自身作用域以外的任何东西。
函数不会修改变量值，而是创建新的变量，赋新值后再返回变量。操作数据的这种方式称为不可变性。
柯里化
函数（和组件）可以结合产生新函数，从而提供更高级的功能与属性。
 
### 第三章 开发真正可复用的组件
1.props
2.state
解决函数绑定问题的最佳方案是在构造器内进行绑定操作，这样即使多次渲染组件，它也不会发生任何改变。
3.无状态函数式组件,没有state
this 在无状态函数式组件的执行过程中不指向组件本身。
无状态函数式组件只接收props（以及上下文）参数，并返回相应元素。
没有提供任何像componentDidMount 这样的生命周期钩子
setState 方法是异步的
propTypes，类型校验
 
可复用组件
react-docgen
 
可用的风格指南
react-storybook 包：@kadira/react-storybook-addon-info
 
### 第四章 组合一切
4.1 组件间的通信
props
children 是一个特殊的prop，拥有者组件可以将它传递给渲染方法内定义的组件。
 
4.2 容器组件与表现组件模式
容器组件：
- 更关心行为部分；
- 负责渲染对应的表现组件；
- 发起API 请求并操作数据；
- 定义事件处理器；
- 写作类的形式。
 
表现组件：
- 更关心视觉表现；
- 负责渲染HTML 标记（或其他组件）；
- 以props 的形式从父组件接收数据；
- 通常写作无状态函数式组件。
 
4.3 mixin
mixin 只能和createClass 工厂方法搭配使用
 
4.4 高阶组件
高阶组件其实就是函数，它接收组件作为参数，对组件进行增强后返回。
 
4.5 recompose
context 16.x有了很大的更改
 
4.6 函数子组件
```js
const Name = ({ children }) => children('World')
Name.propTypes = {
hildren: React.PropTypes.func.isRequired,
}
<Name>
{name => <div>Hello, {name}!</div>}
</Name>
```
 
### 第五章 恰当地获取数据
5.1 数据流
单向数据流
5.1.1 子组件与父组件的通信（回调函数）
每当子组件需要向父组件推送数据或者通知父组件发生了某个事件时，可以传递回调函数，同时将其余逻辑放在父组件中。
 
5.2 数据获取
用于获取数据的代码可以放在两个生命周期钩子中：componentWillMount 和component-DidMount。
 
5.3 react-refetch
 
### 第六章 为浏览器编写代码
6.1 表单
6.1.1 自由组件
6.1.2 受控组件
6.1.3 JSON schema
react-jsonschema-form
 
6.2 事件
http://reactpatterns.com/#event-switch
命名事件处理器函数的最流行做法就是事件名前加上handle 前缀（如handleKeyDown）。
如果需要为同一个组件创建新的事件处理器，无须创建新的方法并绑定，只要在switch 语句中增加一个事例即可。
React 实际做的是在根元素上添加单个事件处理器，由于事件冒泡机制，这个处理器会监听所有事件。当浏览器触发我们想要的事件时，React 会代表相应组件调用处理器。这个技巧称作事件代理，可以优化内存和速度。
 
6.3 ref
表单用得多
 
6.4 动画
react-addons-css-transition-group react-motion
 
### 第七章 美化组件
7.1 CSS in JavaScript
7.2 行内样式
7.3 Radium
7.4 CSS 模块
react-css-modules
!! 7.5 Styled Component
这个库的另一项绝佳特性是主题。将组件封装在ThemeProvider 组件中，可以为组件树注入主题属性，当要和其他组件共享一部分样式，剩下部分取决于当前选中主题时，创建UI 会变得非常方便。
 
### 第八章 服务端渲染的乐趣与益处
8.1 通用应用
同构应用就是指应用在服务端和客户端看起来一模一样。
 
8.2 使用服务端渲染的原因
8.2.1 SEO
8.2.2 通用代码库
8.2.3 性能更强
8.2.4 不要低估复杂度
 
8.5 Next.js
 
### 第九章 提升应用性能
9.1 一致性比较与key 属性，key要唯一的
 
9.2 优化手段
shouldComponentUpdate
它的用法很直观，创建组件类时继承React.PureComponent 来代替React.Component即可。
9.2.2 无状态函数式组件
 
9.3 常用解决方案
9.3.1 why-did-you-update 好像没有用了
9.3.2 在渲染方法中创建函数
9.3.3 props 常量
9.3.4 重构与良好设计
 
9.4 工具与库
immutable.js
 
9.4.2 性能监控工具
chrome-react-perf
react-perf-tool
 
9.4.3 Babel 插件
babel-plugin-transform-react-inline-elements
 
### 第十章 测试与调试
Jest
Mocha
TestUtils 和Enzyme
 
测试React 组件的方式一般有两种：
浅渲染；
将组件挂载到独立DOM中。
 
10.8 常用测试方案
10.8.1 测试高阶组件
10.8.2 页面对象模式
 
10.10 React 错误处理
react-component-errors
 
### 第十一章 需要避免的反模式
11.1 用prop 初始化状态
如果我们真的想要用属性值初始化组件，并且可以肯定这些值未来不会改变呢？
这种情况下，最好阐明这种做法的用意，并为属性起一个能清楚表达含义的名称
 
11.2 修改状态
首先，如果不通过setState 修改状态，则会出现两种糟糕的情况：
 状态改变不会触发组件重渲染；
 以后无论何时调用setState，之前修改的状态都会渲染到页面上。
 
11.3 将数组索引作为key
 
11.4 在DOM 元素上展开props 对象
 
### 第十二章 未来的行动
12.3 发布npm 包
npm publish
