# 插件测试2  <!-- {docsify-ignore-all} -->
### 事件的基本概念
> 事件 是指在文档或者浏览器中发生的一些 特定交互瞬间，比如打开某一个网页，浏览器加载完成后会触发 load 事件，当鼠标悬浮于某一个元素上时会触发 hover 事件，当鼠标点击某一个元素时会触发 click 事件等等。 

事件处理 就是当事件被触发后，浏览器响应这个事件的行为，而这个行为所对应的代码即为 事件处理程序。
<!-- [link1](/demo/ ':ignore title') -->
[example.com](https://example.com/ ':crossorgin')

### 事件操作：监听与移除监听
监听事件
浏览器会根据一些事件作出相对应的 事件处理，事件处理的前提是需要 监听事件，监听事件的方法主要有以下三种：

HTML 内联属性
即在 HTML 元素里直接填写与事件相关的属性，属性值为事件。

```html
<button onclick="console.log('You clicked me!');"></button>
```
使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑
 
这种方式将 HTML 代码与 JavaScript 代码耦合在一起，不利于代码的维护，所以应该尽量避免使用这样的方式。

DOM 属性绑定
通过直接设置某个 DOM 节点的属性来指定事件和事件处理程序

```javascript
const btn = document.getElementById("btn");
btn.onclick = function(e) {
    console.log("You clicked me!");
};
```

 
首先获得 btn 这个对象，通过给这个对象添加 onclick 属性的方式来监听 click 事件，这个属性值对应的就是 事件处理程序。这段程序也被称作 DOM 0 级 事件处理程序。

DOM0 是最早期的事件模型，所有现代浏览器都支持，它只支持冒泡不支持捕获。

事件监听函数

```javascript
const btn = document.getElementById("btn");
btn.addEventListener("click", () => {
    console.log("You clicked me!");
}, false);
 
```

上面的示例表示先获得表示节点的 btn 对象，然后在这个对象上面添加了一个事件监听器，当监听到 click 事件发生时，则调用回调函数，即在控制台输出 "You clicked me!"，这段程序也被称作 DOM 2 级 事件处理程序。

移除监听事件
在为某个元素绑定了一个事件后，如果想接触绑定，则需要用到 removeEventListener 方法。
```javascript
const handler = function() {
    // handler logic
}
const btn = document.getElementById("btn");

btn.addEventListener("click", handler);
btn.removeEventListener("click", handler);
```

 
需要注意的是，绑定事件的回调函数不能是匿名函数，必须是一个已经被声明的函数，因为解除事件绑定时需要传递这个回调函数的引用。

## 事件触发过程
事件流 描述了页面接收事件的 顺序。事件流包含三个过程，分别是 捕获阶段、目标阶段 和 冒泡阶段。

首先发生的是事件 捕获阶段，为 截获事件 提供了机会，然后是 目标接收事件，最后是 冒泡阶段。

<!-- ![logo](./_media/event.png) -->

捕获阶段
当我们对 DOM 元素进行操作时，比如 鼠标点击、悬浮 等，就会有一个事件传输到这个 DOM 元素，这个事件从 Window 开始，依次经过 docuemnt、html、body，再不断经过子节点直到到达 目标元素，从 Window 到达目标 元素父节点 的过程称为 捕获阶段 ，注意此时还未到达目标节点。

addEventListener(eventName, handler, useCapture) 函数。第三个参数是 useCapture，代表是否在 捕获阶段 进行事件处理。

如果是 true，在捕获阶段进行事件处理，如果是 false， 则在冒泡阶段进行事件处理。 默认 是 false。

目标阶段
捕获阶段 结束时，事件到达了 目标节点 的 父节点，最终到达 目标节点，并在目标节点上 触发了这个事件，这就是 目标阶段。

事件触发的目标节点为最底层的节点。

冒泡阶段
当事件到达 目标节点 之后，就会沿着 原路返回，这个过程有点类似水泡从水底浮出水面的过程，所以称这个过程为 冒泡阶段。

addEventListener的第三个参数同上。

<details>
<summary>自我评价</summary>

- abc
- bac
  - abc
  
</details>

## 事件委托
我们可以利用 事件冒泡 机制这一特性来提高页面性能，事件委托 便事件冒泡是最典型的应用之一。

JavaScript 中，事件委托 表示给元素的父级或者祖级，甚至页面，由他们来绑定事件，然后利用事件冒泡的基本原理，通过 事件目标 对象 进行检测 ，然后执行相关操作。
```javascript
<ul id="list">
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
  ......
  <li>item n</li>
</ul>

// 给父层元素绑定事件
document.getElementById('list').addEventListener('click', function (e) {
  // 兼容性处理
  let event = e || window.event;
  let target = event.target || event.srcElement;
  // 判断是否匹配目标元素
  if (target.nodeName.toLocaleLowerCase() === 'li') {
    console.log('the content is: ', target.innerHTML);
  }
});
```

 
上列代码中，列表项的点击事件均委托给了父元素 <ul id="list"></ul>。

target 元素则是在 #list 元素之下具体被点击的元素，然后通过判断 target 的一些属性（比如：nodeName，id 等等）可以更精确地匹配到某一类 #list li 元素之上。

为什么需要事件委托
减少事件绑定。上面的例子中，也可以分别给每个列表项绑定事件，但利用事件委托的方式不仅省去了一一绑定的麻烦，也提升了网页的性能，因为每绑定一个事件便会 增加内存使用。

可以动态监听绑定。上面的例子中，我们对若干个列表项进行了事件监听，当删除一个列表项时不需要单独删除这个列表项所绑定的事件，而增加一个列表项时也不需要单独为新增项绑定事件。

## 阻止事件流
事件委托是事件冒泡的一个应用，但有时候我们并不希望事件冒泡。
```javascript
document.addEventListener("click", function(e) {
   console.log("ele-click");
   e.stopPropagation(); // 阻止事件冒泡
}, false);
```
 
阻止冒泡与捕获的方式：

ie
event.cancelBubble = true
event.returnValue = false
w3c
e.stopPropagation()
e.preventDefault()
event 对象
Event 对象 代表事件的 状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。

当一个事件被触发的时候，就会创建一个事件对象。