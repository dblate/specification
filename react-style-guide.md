<h1>React 代码规范</h1>

<h3>组件</h3>

* [强制]组件名使用 PascalCase 的形式，且组件名应与文件夹名一致
* [强制]使用 onXxx 的形式作为 props 中用于回调的属性名
	* 区分 props 中回调和非回调属性，可以方便的了解组件与其他组件的通信方式
* [强制]高阶组件使用 camelCase 命名
	* [建议]使用 withXxx 或 xxxable 的形式作为高阶组件的名称
		* 高阶组件是对组件使用模式上的抽象，使用上面的表达方式能比较清晰的表达此高阶组件的用途
* [强制]使用 ES Class 的形式声明组件，而不使用 React.createClass
	* react 在 15.5.0 版本中已经废弃 React.createClass 函数
    * 使用 class Modal extends React.component 的形式
* [强制]所有组件都需要声明该组件中所有的 propTypes
	* props 是组件对外接受的参数，列出所有的 props 可以让调用者对组件有一定的了解
	* 设置参数类型检查使得组件更加健壮
* [强制]所有非 required 的 props 都要设置 defaultProps
	* 设置默认值可以省去代码中对 props 属性是否为空的判断
	* defaultProps 对组件来说是简易文档般的存在

<h3>jsx</h3>
<p>jsx 中的规范主要是一些代码风格上的统一<p>

* [建议]没有子元素的标签使用自闭合的语法。最后的反斜杠与前面有一个空格

```javascript
// bad
<Hello></Hello>

// good
<Hello />
```

* [建议]对于有多个 props 属性，需要换行的组件，每个 props 一行，最后的闭合标签单独一行

```javascript
// 无子元素时
<ImageUploader
    src="http://baikebcs.bdimg.com/default/baike.png"
    onChange={this.handleChange}
/>
	
// 有子元素时
<Modal
    maskClosable
    onCancel={this.handleCancel}
    onOk={this.handleOk}
>
    say hello to everyone
</Modal>
```

* [建议]jsx 的层级控制在 3 层以内
	* 太长的 jsx 不容易看清结构
	* 对于嵌套较深的 jsx，如果里面再前套表达式（如三目运算符）就更不容易理解了
	* 可以通过定义变量的形式来减少 jsx 层级

```javascript
// bad
render() {
    const src = this.state.src;
    return (
        <div className="image-uploader">
            <Spin
                spinning
                tip="loading"
            >
                {src ? (
                    <ImagePanel
                        src = {src}
                        onDelete={this.handleDelete}
                    />
                ) : (
                    <WorkPanel onChange={this.handleChange} />
                )}
            </Spin>
        </div>
    );
}

// good
render() {
    const str = this.state.src;
    let panel = null;
    if (src) {
        panel = (
            <ImagePanel
                src={src}
                onDelete={this.handleDelete}
            >
        );
    } else {
        panel = (
            <WorkPanel onChange={this.handleChange} />
        );
    }
    
    return (
        <div className="image-uploader">
            <Spin
                spinning
                tip="loading"
            >
                {panel}
            </Spin>
        </div>
    );
}
```
