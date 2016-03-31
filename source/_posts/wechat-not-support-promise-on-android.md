title: wechat not support promise on android
date: 2016-03-31 11:30:59
categories: [javascript]
tags: [javascript, wechat]
---

![](/images/s44.jpg)

# wechat not support promise on android 

 wechat not support promise in low version android , but some IOS system can do it .

 if you debug get nothing, but the program run not correct,

 you can care about that.

 i catched this bug used four hours....

 if you use some es6 ability , you need some polyfill tool handle this .

 ```
  # for example :
  npm install es6-promise --save

  # use 
  var Promise = require('es6-promise').Promise 
 ```