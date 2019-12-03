​											**虚拟DOM和Diff算法:**

**虚拟DOM(createElement)：**

​	虚拟DOM就是使用树形结构对象去实现真实的DOM。

在React中创建一个DOM节点,DOM树是使用React.createElement( type,props,children );

例如实现一个简单的虚拟DOM,实质上就是创建一个对象：

`function CreateElement(type,props,children){`

​	`return new Element(type,props,children);`

`}`

`class Element{`

​		`constructor(tpe,props,children){`

​			`this.type=type;`

​			`this.props = props;`

​			`this.children = children;`

​	`}`

`}`

例如：

`<div id="aa">  <span>222222</span> </div>`

在React.createElement中是这样去解释使用的:

React.createElement( "div",{id:"aa"},[  React.createElement("span",null,222222)  ] );

所以实质上实现和使用虚拟DOM对象并不是一件难事,最重要的是使用对象去模拟真正的DOM节点



**渲染虚拟DOM(render)**:

​		在React中只要我们在render这个生命周期中写上对应的需要渲染的标签,那样React就会自动取帮我们渲染,

这样我们很容易忽视这个生命周期,下面我来简单实现一下这个生命周期:

​	`function render(objDOM) {`

​			`//获取元素类型,并创建相应的元素`

​			`let element = document.createElement( objDOM.type );`



​			`//向element中加入相应的属性`

​			`for( let key in objDOM.props ){`

​				`setAttr( objDOM,key,objDOM.props[key] );//需要考虑不同的属性`

​			`}`

​		`//最后是遍历子节点,如果子元素是元素节点则递归调用render,如果是文本节点则直接创建`

​		`objDOM.children.forEach( ( dom )=>{` 

​				`if( dom instanceof Element ) { dom = render(dom) }`

​				`else { dom = document.createTextNode(dom) }` 

​				`//最后把dom加到element中`

​				`element.appendChild( dom );`

​		 `} )`

​		`//最后返回这个节点`

​		`return element;`

`}`

//给某个标签加上相应的属性需要对不同类型的标签进行处理

`function setAttr(target,key,value){`

​	`switch(key){`

​		`case "value":`

​			`if( target.tagName.toLowerCase==="input" || target.tagName.toLowerCase==="textarea" ) {`

​				`target.value=value;//当然这里也可以使用setAttribute也是可以的,但是标准来说target是需要target.value这样设置的`

​			`}else {`

​					`target.setAttribute(key,value):`

​				`}`

​			`break;`

​		`case "style":`

​			`//直接赋值行内样式`

​			`target.style.cssText = value;`

​			`break;`

​	`default :`

​		`target.setAttribute(key,value);`

​		`break;`

​	`}`

`}`



`//最后一步就是渲染节点到页面上(某个节点)`

`function renderDOM( target,el ){`

​	`target.appendChild(el);`

`}`

*上面的这样三个函数就完成的页面的渲染节点*,至此虚拟DOM就基本完成了





**Diff算法：**

​	传统的diff算法时间复杂度为 O(n^3),我们可以试想一下,如果两颗非常大的虚拟DOM Tree进行对比的话所要花费的时间得多长,而且效率得多低,同时在React Fiber架构出来之前,是进行同步比较得,也就是说比较过程是不可以打断得,必需比较完成之后才会渲染完成,但是React采取一种非常聪明得办法,使时间复杂度降低到O(n),因为它忽略了跨层得比较,只会进行同层得比较,因为需要跨层得业务使非常少得,在React中所谓得Diff算法,实质上就是比较新旧虚拟DOM Tree得区别,将新的去替换旧的DOM节点,然后重新渲染页面.

实现：

`function diff(oldTree, newTree) {`     

`// 声明变量patches用来存放补丁的对象`   

  `let patches = {};`   

  `// 第一次比较应该是树的第0个索引`  

   `let index = 0;`   

  `// 递归树 比较后的结果放到补丁里`    

 `walk(oldTree, newTree, index, patches);      return patches;`

 `}`



**`function** walk(oldNode, newNode, index, patches) {`
`// 每个元素都有一个补丁`
`let current = [];`

  `**if** (!newNode) { // rule1`
`current.push({ type: 'REMOVE', index });`

  `} **else** **if** (isString(oldNode) && isString(newNode)) {`
`// 判断文本是否一致`

  `**if** (oldNode !== newNode) {`
`current.push({ type: 'TEXT', text: newNode });`
`}`

  `} **else** **if** (oldNode.type === newNode.type) {`

 `// 比较属性是否有更改`
`let attr = diffAttr(oldNode.props, newNode.props);`

`**if** (Object.keys(attr).length > 0) {`
`current.push({ type: 'ATTR', attr });`
`}`

`// 如果有子节点，遍历子节点`
`diffChildren(oldNode.children, newNode.children, patches);`
`} **else** {    // 说明节点被替换了`

`cu`rrent.push({ type: 'REPLACE', newNode});
}

  `// 当前元素确实有补丁存在`
`**if** (current.length) {`
`// 将元素和补丁对应起来，放到大补丁包中`
`patches[index] = current;`
`}`

`}`

**`function** isString(obj) {return typeof obj === 'string';}``

`**function** diffAttr(oldAttrs, newAttrs) {`
`let patch = {};`
`// 判断老的属性中和新的属性的关系`
`**for** (let key **in** oldAttrs) {`
`**if** (oldAttrs[key] !== newAttrs[key]) {`

   `patch[key] = newAttrs[key]; // 有可能还是undefined`
`}`
`}`

`**or** (let key **in** newAttrs) {`
`// 老节点没有新节点的属性`
`**if** (!oldAttrs.hasOwnProperty(key)) {`

 `patch[key] = newAttrs[key];`
`}`
`}`
`return patch;`
`}`

// 所有都基于一个序号来实现
let num = 0;

**`function** diffChildren(oldChildren, newChildren, patches) {`
`// 比较老的第一个和新的第一个`

  `oldChildren.forEach((child, index) => {`
`walk(child, newChildren[index], ++num, patches);`
`});`
`}`



在React中,最为令人津津乐道的是虚拟DOM和Diff完美的结合,他会正确的帮我计算出Virtual DOM中真正变化的部分,并且只会渲染更改的那部分,并不会重新渲染整个页面,从而保证的高效渲染和性能



**Diff策略：**

1. Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计。

2. 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构。

3. 对于同一层级的一组子节点，它们可以通过唯一 id 进行区分。

   

   基于以上三个前提策略，React 分别对 tree diff、component diff 以及 element diff 进行算法优化，事实也证明这三个前提策略是合理且准确的，它保证了整体界面构建的性能



**Tree diff:**

Tree diff遵循策略一,React对树的对比进行优化,即进行分层比较,两棵树中只会对同一层次进行比较.

例如:

![1575039246151](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575039246151.png)



只会在相同颜色的进行比较,如果进行同层比较的时候，发现比较的元素不存在或者改变,它就会进行删除该节点及其子元素,然后增加相应的节点,并不需要比较它的子节点,这样只会对树进行一次的比较就完成整颗树的遍历,这样就会优化其性能

或许你会有个疑问,如果DOM节点跨层移动操作了呢？React diff会怎么样了呢?

例如：

![1575039552126](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575039552126.png)

由于React只会进行同层比较，并不会进行跨层比较,所以只有创建和删除操作,所以当遍历到D的时候发现D多了个子节点,就会创建A=》创建B=》创建C=》删除A(原先的A节点)



**Component Diff:**

​	React是由组件构建而成的,可以所React应用所有都是组件组合而成的.

​	**如果是同一类型的组件,就会按照刚才的策略Tree diff进行比较**

​	**如果不是则会判断为dirty component，从而替换整个组件下的所有节点子节点**

例如:

![1575040394832](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575040394832.png)

当Component D改变成Component G，即使它们结构相似,但是如果React一旦判断不是同类型的组件,就会删除ComponentD，重新创建Component G ，虽然当两个 component 是不同类型但结构相似时，React diff 会影响性能，但正如 React 官方博客所言：不同类型的 component 是很少存在相似 DOM tree 的机会，因此这种极端因素很难在实现开发过程中造成重大影响的。



**Element Diff:**

​	在节点处于同一级的时候,React diff提供了三种操作的方法**INSERT_MARKUP**（插入）、**MOVE_EXISTING**（移动）和 **REMOVE_NODE**（删除）。

**INSERT_MARKUP**:新的 component 类型不在老集合里， 即是全新的节点，需要对新节点执行插入操作。

**MOVE_EXISTING**:在老集合有新 component 类型，且 element 是可更新的类型，generateComponentChildren 已调用 receiveComponent，这种情况下 prevChild=nextChild，就需要做移动操作，可以复用以前的 DOM 节点

**REMOVE_NODE**，老 component 类型，在新集合里也有，但对应的 element 不同则不能直接复用和更新，需要执行删除操作，或者老 component 不在新集合里的，也需要执行删除操作。

例如:

![1575080443811](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575080443811.png)

在新旧集合对比当中,老集合的A和新集合的B对比,当发现不一样,就会创建B，插入新集合中,并删除老集合中的A，

依次类推,创建A，D，C，删除B,C,D

不知道你有没有发现,实质上它们只是移动了位置而已,并不是更改了节点,这会导致低效的创建和删除,但是React去非常的聪明,制定出一个策略,就是可以往同层的节点中添加key值，那样就只会触发节点的移动,并不会作出创建删除这种低效的操作，虽然只是小小的改动,但是性能就会出现翻天覆地的变化

例如：

![1575080770605](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575080770605.png)

通过key值发现都是相同的节点，这样就只需要移动A,C就可以了,并不需要创建和删除操作

再例如:

![1575080936632](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575080936632.png)



通过key值，发现有新增的节点,有失去的节点,那样就会创建E节点,移除D节点,移动A,B节点

## 总结

- React 通过制定大胆的 diff 策略，将 O(n3) 复杂度的问题转换成 O(n) 复杂度的问题；
- React 通过**分层求异**的策略，对 tree diff 进行算法优化；
- React 通过**相同类生成相似树形结构，不同类生成不同树形结构**的策略，对 component diff 进行算法优化；
- React 通过**设置唯一 key**的策略，对 element diff 进行算法优化；
- 建议，在开发组件时，保持稳定的 DOM 结构会有助于性能的提升；
- 建议，在开发过程中，尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，在一定程度上会影响 React 的渲染性能。



