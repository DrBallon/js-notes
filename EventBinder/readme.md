# 功能

一个原生js写的给dom元素绑定事件的小轮子

通过选择器获取匹配到元素并绑定提供的事件

```javascript
//使用流程
//1.创建EventNode实例，并设置css选择器
let en = new EventNode(css选择器)
//2.设置需要绑定的事件,支持多个，多种类
en.addEvent('click',func1)
en.addEvent('click',func2)
en.addEvent('mouseover',mouseover1)
en.addEvent('mouseover',mouseover2)
//支持链式调用
en
.addEvent('click',func1)
.addEvent('click',func2)
.addEvent('mouseover',mouseover1)
.addEvent('mouseover',mouseover2)

//3.（可选）设置元素的索引。比如只绑定给元素中的第1,2个
en.addIndexes([0,1])
//也支持链式，可以跟2写在一起

//4.创建EventNodeBinder实例。需要传入EventNode实例数组
let eb = new EventNodeBinder([en])
//或者创建之后调用方法添加
let eb = new EventNodeBinder()
eb.addNode(en)
//支持多个EventNode实例
let eb = new EventNodeBinder([en,en2,en3,en4,en5])

//5.调用实例的attachEvents方法绑定事件
eb.attachEvents(dom树根节点)

/*
	目前支持的功能：
		1.所有匹配的元素绑定单个、多个、单种类或者多个种类的事件
		2.在1的基础上，允许通过设置元素的索引。
	目前不支持的功能：
		1.元素的事件解绑
		2.不同的事件设置不同的索引（比如给0,1号的元素绑定click的同时给2,3号的元素绑定mouseover事件
	可能出现的问题
		1.给EventNodeBinder实例添加多个 选择器相同的EventNode实例 可能会冲突
*/
```

​	

# 例子

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .span1,
      .span2,
      .span3,
      .span4 {
        display: inline-block;
        width: 100px;
        height: 100px;
        background-color: skyblue;
      }
      .span2 {
        background-color: aqua;
      }
      .span3 {
        background-color: blue;
      }
      .span4 {
        background-color: black;
      }
    </style>
  </head>
  <body>
    <span class="span1"></span>
    <span class="span1"></span>
    <span class="span1"></span>
    <span class="span1"></span>
    <br />
    <span class="span2"></span>
    <span class="span2"></span>
    <span class="span2"></span>
    <span class="span2"></span>
    <br />
    <span class="span3"></span>
    <span class="span3"></span>
    <span class="span3"></span>
    <span class="span3"></span>
  </body>
</html>
<script src="./EventBinder.js"></script>

<script>
  let body = document.querySelector('body')

  //例子1
  function click1() {
    console.log('所有的有.span1类名的元素绑定了点击事件')
  }
  let en = new EventNode('.span1')
  let eb = new EventNodeBinder([en])
  en.addEvent('click', click1)
  eb.attachEvents(body)

  //例子2
  function click2() {
    console.log('有.span2类名的元素中的第一个和第四个绑定了点击事件')
  }
  let en2 = new EventNode('.span2')
  en2.addEvent('click', click2).addIndexes([0, 3])
  let eb2 = new EventNodeBinder([en2])
  eb2.attachEvents(body)
  //例子3
  function click3() {
    console.log('有.span3类名的元素绑定了点击事件')
  }
  function mouseover1() {
    console.log('有.span3类名的元素绑定了mouseover事件')
  }
  let en3 = new EventNode('.span3')
  en3.addEvent('click', click3).addEvent('mouseover', mouseover1)
  let eb3 = new EventNodeBinder([en3])
  eb3.attachEvents(body)
</script>

```
