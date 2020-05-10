---
layout: post
title: Vue自定义指令
description: 介绍vue框架中的自定义指令directive
tag: Vue、directive、自定义指令
category: 技术、总结
---
vue提供了很多默认的内置指令，如v-if，v-for等。但是常常在实际的开发中，这些并不能完全的满足我们的需求。例如页面上有个搜索框，当我进入页面时希望搜索框能够自动获取焦点，这时候便需要对dom进行操作，基础指令已经无法满足了。

那我们如何用自定义指令来实现这个功能呢？看一段简短的代码：

```javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  inserted: function(el){
    el.focus();
  }
})

new Vue({
  // ...
})
```

除了上述方法可以注册全局自定义指令之外，如果想注册一个局部指令，我们也可以在组件的选项中进行定义：

```javascript
directives: {
  focus: {
    inserted: function(el){
      el.focus()
    }
  }
}
```

注册好组件之后，我们就可以在模板中使用自定义组件了：

```markup
<input v-focus>
```

## 钩子函数

**bind**：可选。指令第一次绑定到元素时调用，只会被调用一次。

**inserted**：可选。被绑定元素插入父节点时调用（仅保证父节点存在，但不一定已被插入文档中）。

**update**：可选。所在组件的VNode更新时调用，但是可能发生在其子VNode更新之前。指令的值可能发生变化，也可能没有。可以通过对边更新前后的值来进行判断。

**componentUpdated**：可选。指令所在组件的VNode及其子VNode全部更新后调用。

**unbind**：可选。指令与元素解绑时调用，只会被调用一次。

## 钩子函数参数

**el**：指令所绑定的元素，可以用来直接操作DOM。

**binding**：一个对象，包含以下属性：

--name：指令名，不包''v-''前缀。

--value：指令的绑定值，例如：v-demo="2"，绑定值为2。

--oldValue：指令绑定的前一个值，仅在update和componentUpdated钩子中可用。无论值是否改变都可用。

--expression：字符串形式的指令表达式。例如：v-demo="1+1"，表达式为"1+1"。

--arg：传给指令的参数，可选。例如：v-demo:foo，参数为"foo"。

--modifiers：一个包含修饰符的对象。例如：v-demo.foo.bar，修饰符对象为{ foo: true, bar: true }。

**vnode**：Vue编译生成的虚拟节点。

**oldVnode**：上一个虚拟节点，仅在update和componentUpdated钩子中可用。

注意：除了el之外，其他参数都应该是只读的，切勿进行修改。

### 动态指令参数

传给指令的参数可以是动态的。例如：v-demo:[foo]，foo是一个可变的参数。

### 写个Demo来看看

```markup
<div v-demo:[foo].stop="200">example...</div>
```

```javascript
Vue.directive('demo', {
  bind: function(el, binding, vnode){
    el.style.position = 'fixed';
    var s = (binding.arg == 'left' ? 'left' : 'top');
    el.style[s] = binding.value + 'px';
  }
})

new Vue({
  el: "",
  data: function() {
    return {
      foo: 'left'
    }
  }
})
```

上面这个例子，是想实现这个元素的固定定位，按照top还是left。

## 其他技巧

### 函数简写

如果需要在bind和update时触发相同的行为，而不关心其他的钩子。可以这样简写指令：

```javascript
Vue.directive('demo', function(el, binding){
  // ...
})
```

### 对象字面量

如果指令需要传多个值，可以传入一个JavaScript对象字面量。指令函数能够接受所有合法的JavaScript表达式。

```markup
<div v-demo="{ color: 'black', text: 'hello world' }"></div>
```

```javascript
Vue.directive('demo', function(el, binding){
 	var color = binding.value.color; // 'black'
  var text = binding.value.text; // 'hello world'
})
```

