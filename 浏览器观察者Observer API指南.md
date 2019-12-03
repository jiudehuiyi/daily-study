							**浏览器观察者Observer API**

现代浏览器为了优化性能,出现新了API，下面要介绍的是四种观察者

Intersection Observer:交叉观察者,作用为判断一个元素是否再一个视窗内,

Mutation Observer 变动观察者,作用为监控DOM节点的变动,

resize Observer  视图观察者,作用为监控窗口的变化

performance Observer 性能观察者,作用为监控性能

tip:这些观察者的兼容性都是有问题的,但是我们可以都可以找到它们相对于的polyfill(能兼容到IE8+)

**一. Intersection Observer：**

一般的浏览器是采取onscroll来进行对某个元素可见性的监控,并且它是同步的,会导致大量的轮询,并且不断的重绘和回流,导致性能浪费,而IntersectionObserver则采取异步处理的方式,消除昂贵的DOM和css样式的查询,来进行性能的优化。

基本使用:

1.创建观察者

2.定义回调事件

3.定义要观察的对象



创建观察者:

let observer = new IntersectionObserver( handler,options );
//定义回调函数

function handler( entries,observer ){

​	entries.forEach( (entry)=>{

​		 // entry.boundingClientRect     

​		 // entry.intersectionRatio   目标元素的可见比例,全部可见为1，全部不可见为0

 	   // entry.intersectionRect   

  	 // entry.isIntersecting   

​	   // entry.rootBounds      根元素位置

​	  // entry.target    DOM节点 

​	 // entry.time  时间戳

​	} )

}

//定义参数对象options

let options={

​		root:DOM,//根节点

​		rootMargin:"xxxpx",使用类似于设置CSS边距的语法来指定根边距

​		threshold:[number],阈值，可以为数组。`[0.3]`意味着，当目标元素在根元素指定的元素内可见30%时，调用处理函数

}

定义要观察的目标

observer.observer( document.querySelector("#target") );

此外observer实例还有两个额外的方法:observer.unobserver(停止观察),observer.disconnect(终止对多有目标观察)



实例：

1.1 图片赖加载:

​	一般我们可以window.onscroll对图片来加载,并且可以使用节流的方法进行优化,但是IntersectionObserver提供的是一种异步并且更加简便性能更高的方法：

<img src="placeholder.png" data-src="img-1.jpg">
<img src="placeholder.png" data-src="img-2.jpg">
<img src="placeholder.png" data-src="img-3.jpg">

实现：
let observer = new Observer( (entries,observer)=>{

​	entries.forEach( (entry)=>{

​		entry.target.src = entry.target.getAttribute("data-src");

​		observer.unobserver(entry.target);		

​	} );

} );

document.querySelectorAll("img").forEach( (img)=>{

​	observer.observer( img );

} ) 

1.2 对动画或者视频执行和暂停

<video src="" control> </video>
let video = document.querySelector("video");

let isPause = false;

let observer = new IntersectionObserver( (entries,observer)=>{

​	entries.forEach( (entry)=>{

​		if( entry.intersectionRadio !== 1 && !isPause ) {

​			video.pause();

​			isPause = true;

​		}else {

​			video.play();

​			isPause=false;

​		}

​	} );

},{ threshold:1 } );

observer.observer( video );

**二. MutationObserver**

​	mutationObserver是用来监听DOM节点的变化,并且是异步(微任务)进行的,

使用步骤：

创建观察者:

​	let observer = new MutationObserver( callback );

​	function callback(mutations,observer){

​		mutations.forEach( (mutation)=>{

​				console.log( mutation );

​		} )

​	}

上述的mutation对象包括以下属性

![1574778278089](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1574778278089.png)

定义观察的对象:

observer.observer( dom,options );

//dom是需要监听的节点,

//options对象设置如下：

![1574778346834](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1574778346834.png)



使用实例:监听文本框

let domTarget = document.querySelector("dom-target");

let observer = new MutationObserver( (mutations,observer)=>{

​		//在此处进行文本修改的时候进行处理,例如加上一个提示框之类的	

} );

observer.observer( domTarget,{

​		characterData:true

} );

**三. ResizeObserver:**

resizeObserver可以监听页面的尺寸变化,例如将页面扩大和缩小进行监听,因为这可以代替window.onscroll，它可以提高性能,

创建观察者:

let observer = new ResizeObserver( (entries)=>{

​		entries.forEach( (entry)=>{

​			console.log( entry );//详细信息可以打印出来看以下

​		} );

} );

//观察的节点

observer.**observe**( document.body );

**四.PerformanceObserver:**

它用来替代原先的监控性能的api,提高监控性能,

//创建观察者

let observer = new PerformanceObserver( callback );



const callback = (list, observer) => {

​    const entries = list.getEntries();

​    entries.forEach((entry) => {

​     console.log("Name:" + entry.name + “, Type: “ + entry.entryType + “, Start: “ + entry.startTime + “, Duration: “ + entry.duration + “\n”); });

 }

 

//定义观察者

observer.observe({entryTypes: ["entryTypes"]});