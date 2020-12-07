# less简单入门


个人使用中一点总计，如有错误欢迎指出

<!--more-->

## 1 变量     

我们常常在 CSS 中 看到同一个值重复多次，这样难易于代码维护。

理想状态，应是下面这样：

```javascript
const bgColor="skyblue";
$(".post-content").css("background-color",bgColor);
$("#wrap").css("background-color",bgColor);
$(".arctive").css("background-color",bgColor);
```
只要我们修改 bgColor这一个变量， 整个页面的背景颜色都会随之改变。

而 Less 中的变量十分强大，可化万物，值得一提的是，其变量是常量 ，所以只能定义一次，不能重复使用。

### 值变量
```css
  /* Less */
      @color: #999;
      @bgColor: skyblue;//不要添加引号
      @width: 50%;
      #wrap {
        color: @color;
        width: @width;
      }
    
      /* 生成后的 CSS */
      #wrap {
        color: #999;
        width: 50%;
      }
```
以 @ 开头 定义变量，并且使用时 直接 键入 @名称。

在平时工作中，我们就可以把 常用的变量 封装到一个文件中，这样利于代码组织维护。

```css
 @lightPrimaryColor: #c5cae9;
 @textPrimaryColor: #fff;
 @accentColor: rgb(99, 137, 185);
 @primaryTextColor: #646464;
 @secondaryTextColor: #000;
 @dividerColor: #b6b6b6;
 @borderColor: #dadada;
```

### 选择器变量

让 选择器 变成 动态

```css
/* Less */
      @mySelector: #wrap;
      @Wrap: wrap;
      @{mySelector}{ //变量名 必须使用大括号包裹
        color: #999;
        width: 50%;
      }
      .@{Wrap}{
        color:#ccc;
      }
      #@{Wrap}{
        color:#666;
      }
    
      /* 生成的 CSS */
      #wrap{
        color: #999;
        width: 50%;
      }
      .wrap{
        color:#ccc;
      }
      #wrap{
        color:#666;
      }
```

### 属性变量

可减少代码书写量

```css
 /* Less */
      @borderStyle: border-style;
      @Soild:solid;
      #wrap{
        @{borderStyle}: @Soild;//变量名 必须使用大括号包裹
      }
    
      /* 生成的 CSS */
      #wrap{
        border-style:solid;
      }
```

### url变量

项目结构改变时，修改其变量即可。

```css
/* Less */
      @images: "../img";//需要加引号
      body {
        background: url("@{images}/dog.png");//变量名 必须使用大括号包裹
      }
    
      /* 生成的 CSS */
      body {
        background: url("../img/dog.png");
      }
```

### 声明变量

有点类似于 下面的 混合方法

```javascript
 - 结构: @name: { 属性: 值 ;};
 - 使用：@name();
```
```css
 /* Less */
      @background: {background:red;};
      #main{
          @background();
      }
      @Rules:{
          width: 200px;
          height: 200px;
          border: solid 1px red;
      };
      #con{
        @Rules();
      }
    
      /* 生成的 CSS */
      #main{
        background:red;
      }
      #con{
        width: 200px;
        height: 200px;
        border: solid 1px red;
      }
```

### 变量运算

不得不提的是，Less 的变量运算完全超出我的期望，十分强大。

```javascript
 - 加减法时 以第一个数据的单位为基准
 - 乘除法时 注意单位一定要统一
```

```css
  /* Less */
      @width:300px;
      @color:#222;
      #wrap{
        width:@width-20;
        height:@width-20*5;
        margin:(@width-20)*5;
        color:@color*2;
        background-color:@color + #111;
      }
    
      /* 生成的 CSS */
      #wrap{
        width:280px;
        height:200px;
        margin:1400px;
        color:#444;
        background-color:#333;
      }
```

### 变量作用域

一句话理解就是：就近原则，不要跟我提闭包。

```css
 /* Less */
      @var: @a;
      @a: 100%;
      #wrap {
        width: @var;
        @a: 9%;
      }
    
      /* 生成的 CSS */
      #wrap {
        width: 9%;
      }
```
用变量去定义变量

```css
/* Less */
      @fnord:  "I am fnord.";
      @var:    "fnord";
      #wrap::after{
        content: @@var; //将@var替换为其值 content:@fnord;
      } 
      /* 生成的 CSS */
      #wrap::after{ 
        content: "I am fnord.";
      }
```

## 2 嵌套

& ：代表的上一层选择器的名字，此例便是header。

### & 的妙用

```css
/* Less */
      #header{
        &:after{
          content:"Less is more!";
        }
        .title{
          font-weight:bold;
        }
        &_content{//理解方式：直接把 & 替换成 #header
          margin:20px;
        }
      }
      /* 生成的 CSS */
      #header::after{
        content:"Less is more!";
      }
      #header .title{ //嵌套了
        font-weight:bold;
      }
      #header_content{//没有嵌套！
          margin:20px;
      }
```

### 媒体查询
在以往的工作中，我们使用 媒体查询，都要把一个元素 分开写
```css
 #wrap{
        width:500px;
      }
      @media screen and (max-width:768px){
        #wrap{
          width:100px;
        }
      }
```
Less 提供了一个十分便捷的方式

```css
  /* Less */
      #main{
          //something...
    
          @media screen{
              @media (max-width:768px){
                width:100px;
              }
          }
          @media tv {
            width:2000px;
          }
      }
      /* 生成的 CSS */
      @media screen and (maxwidth:768px){
        #main{
            width:100px; 
        }
      }
      @media tv{
        #main{
          width:2000px;
        }
      }
```

唯一的缺点就是 每一个元素都会编译出自己 @media 声明，并不会合并。

### 实战技巧
可以借助 Less 在元素中，去定义自己的私有样式。
```css
 /* Less */
      #main{
        // something..
        &.show{
          display:block;
        }
      }
      .show{
        display:none;
      }
```
```css
    const main = document.getElementById("main");
      main.classList.add("show");
```

结果：

```css
#main.show{
        display:block;
      }
      .show{
        display:none; //会被覆盖。
      }
```

## 3 混合方法
### 无参数方法
方法犹如 声明的集合，使用时 直接键入名称即可。
```css
/* Less */
      .card { // 等价于 .card()
          background: #f6f6f6;
          -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
          box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
      }
      #wrap{
        .card;//等价于.card();
      }
      /* 生成的 CSS */
      #wrap{
        background: #f6f6f6;
        -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
        box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
      }
```

其中 .card 与 .card() 是等价的。

个人建议，为了避免 代码混淆，应写成 :

```css
 .card(){
        //something...
      }
      #wrap{
        .card();
      }
```

要点：

  `.` 与 `#` 皆可作为 方法前缀。
  
  方法后写不写 `()` 看个人习惯。
  
  ### 默认参数方法
  Less 可以使用默认参数，如果 没有传参数，那么将使用默认参数。
  
  @arguments 犹如 JS 中的 arguments 指代的是 全部参数。
  
  传的参数中 必须带着单位。
  
  ```css
   .border(@a:10px,@b:50px,@c:30px,@color:#000){
          border:solid 1px @color;
          box-shadow: @arguments;//指代的是 全部参数
      }
      #main{
          .border(0px,5px,30px,red);//必须带着单位
      }
      #wrap{
          .border(0px);
      }
      #content{
        .border;//等价于 .border()
      }
    
      /* 生成的 CSS */
      #main{
          border:solid 1px red;
          box-shadow:0px,5px,30px,red;
      }
      #wrap{
          border:solid 1px #000;
          box-shadow: 0px 50px 30px #000;
      }
      #content{
          border:solid 1px #000;
          box-shadow: 10px 50px 30px #000;
      }
```
### 方法的匹配模式
与 面向对象中的多态 很相似
```css
    /* Less */
      .triangle(top,@width:20px,@color:#000){
          border-color:transparent  transparent @color transparent ;
      }
      .triangle(right,@width:20px,@color:#000){
          border-color:transparent @color transparent  transparent ;
      }
    
      .triangle(bottom,@width:20px,@color:#000){
          border-color:@color transparent  transparent  transparent ;
      }
      .triangle(left,@width:20px,@color:#000){
          border-color:transparent  transparent  transparent @color;
      }
      .triangle(@_,@width:20px,@color:#000){
          border-style: solid;
          border-width: @width;
      }
      #main{
          .triangle(left, 50px, #999)
      }
      /* 生成的 CSS */
      #main{
        border-color:transparent  transparent  transparent #999;
        border-style: solid;
        border-width: 50px;
      }
```

  - 第一个参数 `left` 要会找到方法中匹配程度最高的，如果匹配程度相同，将全部选择，并存在着样式覆盖替换。

  - 如果匹配的参数 是变量，则将会匹配，如 `@_` 。
  
  ### 方法的命名空间
  让方法更加规范
  
  ```css
   /* Less */
      #card(){
          background: #723232;
          .d(@w:300px){
              width: @w;
              
              #a(@h:300px){
                  height: @h;//可以使用上一层传进来的方法
                  width: @w;
              }
          }
      }
      #wrap{
          #card > .d > #a(100px); // 父元素不能加 括号
      }
      #main{
          #card .d();
      }
      #con{
          //不得单独使用命名空间的方法
          //.d() 如果前面没有引入命名空间 #card ，将会报错
          
          #card; // 等价于 #card();
          .d(20px); //必须先引入 #card
      }
      /* 生成的 CSS */
      #wrap{
        height:100px;
        width:300px;
      }
      #main{
        width:300px;
      }
      #con{
        width:20px;
      }
```

  - 在 CSS 中`>` 选择器，选择的是 儿子元素，就是 必须与父元素 有直接血源的元素。
  - 在引入命令空间时，如使用 `>` 选择器，父元素不能加 括号。
  - 不得单独使用命名空间的方法 必须先引入命名空间，才能使用 其中方法。
  - 子方法 可以使用上一层传进来的方法
  
  ### 方法的条件筛选
  Less 没有 if else，可是它有 when
  ```css
 /* Less */
    #card{
        
        // and 运算符 ，相当于 与运算 &&，必须条件全部符合才会执行
        .border(@width,@color,@style) when (@width>100px) and(@color=#999){
            border:@style @color @width;
        }
    
        // not 运算符，相当于 非运算 !，条件为 不符合才会执行
        .background(@color) when not (@color>=#222){
            background:@color;
        }
    
        // , 逗号分隔符：相当于 或运算 ||，只要有一个符合条件就会执行
        .font(@size:20px) when (@size>50px) , (@size<100px){
            font-size: @size;
        }
    }
    #main{
        #card>.border(200px,#999,solid);
        #card .background(#111);
        #card > .font(40px);
    }
    /* 生成后的 CSS */
    #main{
      border:solid #999 200px;
      background:#111;
      font-size:40px;
    }
```
要点
 - 比较运算有： > >= = =< <。
  - = 代表的是等于
  - 除去关键字 true 以外的值都被视为 false：

### 数量不定的参数
如果你希望你的方法接受数量不定的参数，你可以使用... ，犹如 ES6 的扩展运算符。
```css
    /* Less */
      .boxShadow(...){
          box-shadow: @arguments;
      }
      .textShadow(@a,...){
          text-shadow: @arguments;
      }
      #main{
          .boxShadow(1px,4px,30px,red);
          .textShadow(1px,4px,30px,red);
      }
    
      /* 生成后的 CSS */
      #main{
        box-shadow: 1px 4px 30px red;
        text-shadow: 1px 4px 30px red;
      }
```

### 方法使用important！
使用方法 非常简单，在方法名后 加上关键字即可。
```css
    /* Less */
      .border{
          border: solid 1px red;
          margin: 50px;
      }
      #main{
          .border() !important;
      }
      /* 生成后的 CSS */
      #main {
          border: solid 1px red !important;
          margin: 50px !important;
      }
```
### 循环方法
Less 并没有提供 for 循环功能，但这也难不倒 聪明的程序员，使用递归去实现。

下面是官网中的一个 Demo，模拟了生成栅格系统。

```css
/* Less */
      .generate-columns(4);
    
      .generate-columns(@n, @i: 1) when (@i =< @n) {
        .column-@{i} {
          width: (@i * 100% / @n);
        }
        .generate-columns(@n, (@i + 1));
      }
      /* 生成后的 CSS */
      .column-1 {
        width: 25%;
      }
      .column-2 {
        width: 50%;
      }
      .column-3 {
        width: 75%;
      }
      .column-4 {
        width: 100%;
      }
```
