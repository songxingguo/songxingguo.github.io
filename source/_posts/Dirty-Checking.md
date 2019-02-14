title: 脏检查机制
author: songxingguo
tags:
  - AngularJs
categories: []
date: 2019-02-14 11:29:00
---
## 脏检查

> 对脏数据的检查就是脏检查，比较UI和后台的数据是否一致!

## $watch 对象

```js
watch = {
    name:'',      //当前的watch 对象 观测的数据名
    getNewValue:function($scope){ //得到新值
        ...
        return newValue;
        },
    listener:function(newValue,oldValue){  // 当数据发生改变时需要执行的操作
        ...
    }
}
```
每当我们将数据绑定到 UI 上，angular 就会向你的 watchList 上插入一个 $watch。

```html
<span>{{user}}</span>
<span>{{password}}</span>
```
<!-- more -->

## 双向数据绑定

> 界面的操作能实事反应到数据，数据的更改也能在界面呈现。

界面到数据的更改，是由 UI 事件，ajax请求，或者timeout 等回调操作,而数据到界面的呈现则是由脏检查来做。

只有当触发UI事件，ajax请求或者 timeout 延迟，才会触发脏检查。

原生实现

```html
<body>
    <button ng-click="increase">increase</button>
    <button ng-click="decrease">decrease</button>
    <span ng-bind="data"></span>
    <script src="app.js"></script>
</body>
```
```js
window.onload = function() {
    'use strict';

    var scope = {
        increase: function() {
            this.data++;
        },
        decrease: function decrease() {
            this.data--;
        },
        data: 0
    }

    function bind() {
        var list = document.querySelectorAll('[ng-click]');
        for (var i = 0, l = list.length; i < l; i++) {
            list[i].onclick = (function(index) {
                return function() {
                    var func = this.getAttribute('ng-click');
                    scope[func](scope);
                    apply();
                }
            })(i);
        }
    }

    // apply
    function apply() {
        var list = document.querySelectorAll('[ng-bind]');
        for (var i = 0, l = list.length; i < l; i++) {
            var bindData = list[i].getAttribute('ng-bind');
            list[i].innerHTML = scope[bindData];
        }
    }

    bind();
    apply();
}
```
通过 apply() 来反映到界面上。

## 脏检查总结

- 一次脏检查就是调用一次 $apply() 或者 $digest(),将数据中最新的值呈现在界面上。
- 而每次 UI 事件变更，ajax 还有 timeout 都会触发 $apply()。

## 写在最后

[AngularJS 脏检查深入分析](https://www.cnblogs.com/likeFlyingFish/p/6183630.html)

[理解angularJS中的脏检测机制](http://www.webnpm.com/60.html)