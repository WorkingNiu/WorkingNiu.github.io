# 微信小程序跳转总结


工作中总结的小程序跳转方法总结

如有不足欢迎补充

<!--more-->

## 1 利用小程序提供的 API 跳转
```javascript
// 保留当前页面，跳转到应用内的某个页面，使用wx.navigateBack可以返回到原页面。
// 注意：调用 navigateTo 跳转时，调用该方法的页面会被加入堆栈，但是 redirectTo 
wx.navigateTo({
  url: 'page/home/home?user_id=111'
})
```