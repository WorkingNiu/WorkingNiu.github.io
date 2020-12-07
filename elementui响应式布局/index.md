# Element-UI响应式布局


公司用的elementui，有一部分响应式的需求

自我总结，欢迎指正

<!--more-->

去看了下bootstrap的响应式用法，

才发现原来element-ui以及其他好几个vue的ui库都做过响应式的布局的组件，

话不多说直接上代码

```html
<el-row :gutter="10">
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple"></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple"></div></el-col>
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple-light"></div></el-col>
</el-row>
```
其实就是做了每行总共24个栅格，在不同尺寸的页面上如何分配宽度比例：

|名称      | 尺寸       |
|---------|-----------| 
| xs     | <768px        
| sm     | ≥768px         
| md     | ≥992px
| lg     | ≥1200px
| xl     | ≥1920px

比如这里直接给xs赋值4，他的宽度在xs（<768px，手机）就是4/24。

除了直接给赋值数字，也可以给对象如：{span:18,offset:3}。

span即是仅赋值数值时的默认参数位，为宽度。

offset为从左边的偏移量，也是1/24为单位。

###示例

立刻搞定之前想要的效果：宽屏页面时内容仅仅占页面宽50%居中显示。窄屏幕时占70%，手机时占100%。

```html
 <el-row :gutter="10">
    <el-col :xs="{span:24,offset:0}" :sm="{span:16,offset:4}" :md="{span:12,offset:6}">
    </el-col>
 </el-row>
```

gutter就是各col之间的间距。

这种方法其实使用的还是@media。但是确实方便配置多了。
