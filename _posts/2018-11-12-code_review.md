---
author: tsingroo
date: 2018-11-12 14:00:50+08:00
layout: post
title: '关于代码审查的思考'
---

使用严格的规范和更高级的编程技巧来管理和降低复杂度，而不是增加复杂度。

以Google JavaScript Style Guide为基准。结合自己的实践整理出一份清单，并持续完善，供工程师们借鉴。
2018/11开始，每个月做到三条，并固化成习惯。每次提测前组长进行代码审查。
* 2018/11月,1-3条
* 2018/12月,4-6条
* 2019/1月,7-9条

----

### JS代码审查实践清单

#### 1.变量见名识意--代码自解释
一个好的变量名或者方法名就跟一个形象的人名一样，举个不太恰当的例子就是给人起的外号更能体现这个人的特色，而我们给变量的名字就是要达到这样的效果。最终要的结果就是你一看到这个变量就知道这是干什么的。

变量名能描述状态和属性，方法名能描述行为。这就是说方法尽量使用动宾类的写法，比如```getUserData()```这样的，一看就知道什么意思。

不要中英文混写，比如```getUserSKM(获取用户收款码)```，再比如```onlineZFZE(线上支付总额)```,```getKCXXInfo(获取库存信息)```。这写法真是让人崩溃。如果英文不好，花几分钟搜索一下,取一个合适的名字是非常有必要的。

#### 2.缩小变量作用域
如果变量只在局部范围内使用就不要声明成全局。如果变量只在类的成员函数内使用，那就不要声明成类成员。

#### 3.尽量使用const和let
能用const的用const声明，不行的话用let，尽量不要用var声明。
由此引申出一些使用map,reduce等函数式的编程特性，都推荐使用。

#### 4.状态使用枚举

有好多的代码中都是用了1,2,3这样的字面量，如果不写注释根本不知道这些字面量是什么意思，并且有时候看了代码也不知道是什么意思。而如果我们将这些变量使用一个枚举值存起来，比如下面的代码就比使用-1,0,1代码更能看出什么意思。
```JavaScript
const UserStatus = {
  LOGINED  : 1,
  UNLOGINED: 2,
  FORBBIDEN: 3
}
```

#### 5.表驱动法优化代码
代码中经常看到if-else-if，或者switch语句里面只有一个变量不一样，这个变量还是判断条件的情况。
```JavaScript
const userStatusCode = // get user status from other place
let currentUserStatusText = '';
if(userStatusCode == 1) {
  currentUserStatusText = 'LOGINED';
} else {
  if (userStatusCode == 2) {
    currentUserStatusText = 'LOGINED';
  } else {
    // ... some other shit logic
  }
}
```
如果遇到这样的我们用下面的代码是不是更合适一些?
```JavaScript
const userStatusCode = // get user status from other place
const userStatusText = ['', 'LOGINED', 'UNLOGINED', 'FORBBIDEN']
const currentUserStatusText = userStatusText[userStatusCode]
```
当然有些情况并不适用，需要自己取舍。
