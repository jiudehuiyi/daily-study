三栏布局实质上是两栏固定,一栏自适应:



第一种:float方法,先加载定宽的两侧,在加载主内容

HTML结构:

`<div class="container">`

    <div class="left"></div>
    <div class='right'></div>
    <div class='main'></div>
</div>

CSS样式表:

<style>


    .left {
        width: 100px;
        height: 100px;
        background-color: red;
        float: left;
    }
    .right {
        width: 200px;
        height: 100px;
        background-color: green;
        float: right;
    }
    .main {
        height: 100px;
        background-color: blue;
        margin-left: 120px;
        margin-right: 220px;
    }



</style>

第二种:float+BFC，先加载定宽的两侧,在加载主内容，主要利用BFC特性实现

HTML结构：

<div class="container">
        <div class="left"></div>
        <div class="right"></div>
        <div class="main"></div>
    </div>

CSS结构:

<style>


   .left {
       width: 100px;
       height: 100px;
       background-color: red;
       float: left;
       margin-right: 20px;
   }
   .right {
       width: 200px;
       height: 100px;
       background-color: green;
       float: right;
       margin-left: 20px;
   }
   .main {
       height: 100px;
       background-color: blue;
       overflow: hidden;
   }



</style>



第三种:双飞翼布局

HTML代码：

  <div class="content">
       <div class="main"></div>
   </div>
   <div class="left"></div>
   <div class="right"></div>
CSS代码：

<style>


  .content {
      width: 100%;
      float: left;
  }
  .main {
      margin-left: 120px;
      margin-right: 220px;
      height: 100px;
      background-color: red;
  }
  .left {
      height: 100px;
      background-color: green;
      float: left;
      width: 100px;
      margin-left: -100%;
  }
  .right {
      height: 100px;
      background-color: blue;
      float: right;
      width: 200px;
      margin-left: -200px;
  }

</style>

第四种:圣杯模式:

HTML

<div class="container">
    <div class="main"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>

 <div class="container">
    <div class="main"></div>
    <div class="left"></div>
    <div class="right"></div>
  </div>

CSS:

<style>


.container {
    margin-left: 120px;
    margin-right: 220px;
}
.main{
    float: left;
    width: 100%;
    height: 100px;
    background-color: red;
}
.left {
    float: left;
    width: 100px;
    height: 100px;
    background-color: green;
    margin-left: -100%;
    position: relative;
    left: -120px;
}
.right {
    float: right;
    width: 200px;
    height: 100px;
    background-color: blue;
    margin-left: -200px;
    position: relative;
    right: -220px;
} 

</style>

第五种:flex布局:

HTML:

 <div class="container">
    <div class="main"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>

CSS:

<style>


.container{
    display: flex;
}
.main {
    height: 100px;
    background-color: red;
    flex-grow: 1;
}
.left {
    height: 100px;
    background-color: green;
    flex: 0 1 100px;
    order: -1;
    margin-right: 20px;
}
.right {
    height: 100px;
    background-color: blue;
    flex:0 1 200px;
    margin-left: 20px;
}

第六种:position

<div class="container">
    <div class="main"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>

CSS:

.container {

​        position: relative;

​    }

​    .main {

​        height: 100px;

​        background-color: red;

​        margin: 0 120px;

​    }

​    .left {

​        position: absolute;

​        left: 0px;

​        top: 0px;

​        width: 100px;

​        height: 100px;

​        background-color: blue;

​    }

​    .right {

​        position: absolute;

​        width: 100px;

​        height: 100px;

​        background-color: green;

​        right: 0px;

​        top: 0px;

​    }