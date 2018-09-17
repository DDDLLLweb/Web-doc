### 

### withRouter
withRouter 是一个高阶组件，把 match，location，history 三个对象注入到组件的 props 中。这是一个非常实用的函数，下面以四个小例子阐述它的用法
1. redux与connect结合    
 Route 是组件，路由表是不断变化的。在项目中使用了 redux 来管理数据，当数据没有变化时，组件也就不会重新渲染。假设在组件中某个 Route 组件并未被渲染，数据也未发生变化，即便当前页面的链接发生了变化，也不会有任何的路由匹配该链接。因为这时候 Route 组件还是未被渲染！如何知道链接变化了呢？这时候就需要 withRouter 了
 ```js
import { withRouter } from 'react-router-dom'
export default withRouter(connect(mapStateToProps, mapDispatchToProps)(Component))
```
2. 页面的跳转    
withRouter 封装的组件中的 props 包含 history，通过 history 对象来控制页面的跳转。history 对象有 push，replace 与 go 等方法，调用这些方式实现页面的跳转。
```js
class Comoponent extends React.Component {
  handleClick () {
    this.props.history.push('/article')
  }
}
export default withRouter(Component)

```
3. 获取路由中的参数   
组件 props 的 match 对象里包含了路由中的参数。
```js
class ArticleDetail extends React.Component {
  state = {
    id: null
  }
  componentDidMount () {
    const { id } = this.props.match
    this.setState({ id })
  }
}

```