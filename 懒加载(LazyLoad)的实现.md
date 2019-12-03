​								**懒加载(LazyLoad)的实现**

懒加载是为了对页面性能进行优化,它不会让整个页面的资源一起加载,他会让以用户当浏览到相应的资源才会进行加载,当然这也会带来一些小问题,当用户网络不好的时候,会给浏览带来一些不好的体验

实现懒加载使用onscroll或者IntersectionObserver赖实现

我们来实现一些图片的懒加载:

一.使用onscroll来实现:

<img src="default.jpg" data-src="image1.jpg" />

<img src="default.jpg" data-src="image2.jpg" />

<img src="default.jpg" data-src="image3.jpg" /> 



`function lazyLoad(){`



​    `let imgs = document.querySelectorAll("img");`

​    `let clientHeight = document.documentElement.clientHeight;//可视窗口高度`

​    `let scrollHeight = document.documentElement.scrollTop || document.body.scrollTop;//滚动条滚动的高度`



​    `for( let i=0;i<imgs.length;i++ ) {`

​        `if( imgs[i].offsetTop < clientHeight+scrollHeight ) {`

​            `if( imgs[i].getAttribute("src") === "default.jpg" ) {`

​                `imgs[i].src = imgs[i].getAttribute("data-src")`

​            `}`

​        `}`

​    `}`



`}`



`window.addEventListener("scroll",lazyLoad,false);`

在这里会出现一个问题,就是性能问题,当滚动的时候,函数会高频的触发,你会想到什么了呢？没错那就是防抖和节流,防抖和节流的具体实现请看此系列的防抖和节流文件

此时只需要更改绑定函数即可:

window.addEventListener("scroll",throttle( lazyLoad,500 ),false)



二.使用JS新增的API IntersectionObserver：

此时实现就更加简便了：

​	let observer = new IntersectionObserver( (entries,observer)=>{

​		entries.forEach( (entry)=>{

​			if( entry.target.getAttribute("src") === "default.jpg" ) {

​					entry.target.src = entry.target.getAttribute("data-src"); 

​				}

​		} );

​	} );

​	document.querySelectorAll( "img" ).forEach( (img)=>{

​		observer.observer( img );

​	} );





