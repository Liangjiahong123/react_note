# ref获取DOM

- 在 `React` 开发模式中，通常情况不需要、也不建议直接操作 `DOM` 原生，但有一些特殊的情况确实需要获取`DOM` 进行某些操作
  - 管理焦点，文本选择或媒体播放
  - 触发强制动画
  - 集成第三方 `DOM` 库

## refs

- 首先在元素上绑定 `ref` 属性，在 `React` 中可以通过 `refs` 获取 `DOM`

```jsx
import { PureComponent } from 'react'
class App extends PureComponent {
  getNativeDOM() {
    console.log(this.refs.text);
  }
  render() {
    return (
      <div>
        <h2 ref="text">Hello React</h2>
        <button onClick={e => this.getNativeDOM()}>获取节点</button>
      </div>
    )
  }
}
```

## createRef

- 通过 `React` 提供的 `createRef` 函数提前创建一个 `ref`，然后绑定到元素上

```jsx
import { PureComponent, createRef } from 'react'

class App extends PureComponent {
  constructor() {
    super()
    this.textRef = createRef() // 提前创建ref
  }
  
  getNativeDOM() {
    // 通过其current属性获取
    console.log(this.textRef.current);
  }
  
  render() {
    return (
      <div>
        {/* 绑定到元素上 */}
        <h2 ref={this.textRef}>Hello React</h2>
        <button onClick={e => this.getNativeDOM()}>获取节点</button>
      </div>
    )
  }
}
```

- 如果对多个元素进行绑定，则最后的元素会覆盖前面的

```jsx
<h2 ref={this.textRef}>Hello React</h2>

<h3 ref={this.textRef}>Hello React</h3>
{/* this.textRef获取到的是h3 */}
```

## 回调函数

- 在元素的 `ref` 属性中传入回调函数，当元素被渲染后，回调函数被执行，并将元素传入

```jsx
import { PureComponent } from 'react'
class App extends PureComponent {
  getNativeDOM() {
    console.log(this.textEl);
  }
  render() {
    return (
      <div>
        <h2 ref={el => this.textEl = el}>Hello React</h2>
        <button onClick={e => this.getNativeDOM()}>获取节点</button>
      </div>
    )
  }
}
```

# 获取组件

- 通过 `createRef` 获取类组件实例

```jsx
class App extends PureComponent {
  constructor() {
    super()
    this.hwRef = createRef()
  }

  geComponent() {
    console.log(this.hwRef.current);
  }

  render() {
    return (
      <div>
        <HelloWorld ref={this.hwRef}/>
        <button onClick={e => this.geComponent()}>获取组件</button>
      </div>
    )
  }
}
```

- 通过高阶函数 `forwardRef` 获取函数式组件内的元素

```jsx
const HelloReact = forwardRef(function(props, ref) {
  return (
    <div>
      <h2 ref={ref}>HelloWorld</h2>
    </div>
  )
})
```

```jsx
class App extends PureComponent {
  constructor() {
    super()
    this.hrRef = createRef()
  }

  geComponent() {
    console.log(this.hrRef.current);  // 获取到函数式组件HelloReact内的h2元素
  }

  render() {
    return (
      <div>
        <HelloReact ref={this.hrRef}/>
        <button onClick={e => this.geComponent()}>获取组件</button>
      </div>
    )
  }
}

```