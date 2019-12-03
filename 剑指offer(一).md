											**剑指Offer(一)**

**1.**

*题目描述: 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。*

 **代码实现:**

`function Find(target,array){`

​    `//如果二维数组中的某一个元素存在target,那么久直接返回true,如果每个元素都不存在则再最后返回false`

​    `for( let i=0;i<array.length;i++ ) {`

​        `if( array[i].indexOf(target) !== -1 ) {`

​            `return true;`

​        `}`

​    `}`

​    `return false;`

`}`

**2.**

*题目描述：请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。*

代码实现：

`function replaceSpace(str) {`

​    `return str.replace(/\s/g,"%20");`

`}`

**3.**

*题目描述:输入一个链表，按链表从尾到头的顺序返回一个ArrayList。*

代码实现：

`function printListFromTailToHead(head) {`



​    `let result = [];`

​    `let replace = head;//储存一个头指针,为了不改变原先的head指向的头指针`

​    `while(replace) {`



​        `result.push( replace.val );`

​        `replace = replace.next;`



​    `}`

​    `return result.reverse();`

`}` 

**4.**

*题目描述:用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。*

代码实现：

`var stack1=[],stack2=[];`
`function push(node)`
`{`
    `stack1.push(node);`
`}`
`function pop()`
`{`
    `if(stack2.length==0){`
        `if(stack1.length==0){`
            `return null;`
        `}else{`
            `var len = stack1.length;`
            `for(var i=0;i<len;i++){`
                `stack2.push(stack1.pop());`
            `}`
            `return stack2.pop();`
        `}`
    `}else{`
        `return stack2.pop();`
    `}`
`}`

**5.**

题目描述:

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

代码实现：

`function minNumberInRotateArray(rotateArray) {`

​    `if( rotateArray.length === 0 ) return 0;`

​    `rotateArray.sort(  (a,b)=>{ return a-b });`

​    `return rotateArray[0]`

`}`

**6.**

题目描述:

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）

代码实现：

`function Fibonacci(n){`

​    `if(n===0) return 0;`



​    `let prev = 0;`

​    `let next = 1;`

​    `for(let i=2;i<n;i++) {`

​        `next = prev+next;`

​        `prev = next - prev;`

​    `}`

​    `return next;`

`}`

**7**.

*题目描述:一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。*

//这个题通过观察发现,实质上是符合斐波那契数列的,

`function jumpFloor(n){`

​    `if(n<2) return 1;`

``     

​    `let arr = [1,2];`

​    `for(let i=2;i<n;i++){`

​        `arr[i] = arr[i]+arr[i-2];`

​    `}`

​    `return arr[n-1];`

`}`

**8**.

题目描述:一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法

代码实现：

`function jumpFloorII(number)`
`{`
    `// write code here`
    `if(number<1){`
        `return null;`
    `}`
    `if(number === 1) {`
        `return 1;`
    `}`
    `let value = 1;`
    `for(let i=1;i<number;i++){`
           `value *= 2;`
    `}`
   `return value;`
`}`

**9.**

题目描述:输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

代码实现：

`function reOrderArray(array){`

​    `let jArray = [],oArray=[];`

​    `for( let i=0;i<array.length;i++ ){`

​        `if( array[i]%2===0 ) {`

​            `oArray.push( array[i] );`

​        `}else{`

​            `jArray.push( array[i] );`

​        `}`

​    `}`

​    `return Array.prototype.concat.call( jArray,oArray );`

`}`

**10**

题目描述：输入一个链表，输出该链表中倒数第k个结点。

代码实现：

`function FindKthToTail(head,k) {`

​    `let node = head,arr=[];`

​    `while(node !== null) {`

​        `arr.push(node);`

​        `node = node.next;`

​    `}`

​    `return arr[arr.length-k];`

`}`

**11.**

题目描述:

输入一个链表，反转链表后，输出新链表的表头。

代码实现:

`function reverserList(head) {`



​    `let pHead = head,arr=[];`



​    `while( pHead !== null ) {`

​        `arr.push( pHead.val );`

​        `pHead = pHead.next;`

​    `}`



​    `pHead = head;`

​    `while( pHead!== null ) {`

​        `pHead.val = arr.pop();`

​        `pHead = pHead.next;`

​    `}`

​    `return pHead;`



`}`



**12.**

题目描述：

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

代码实现:

`function Merge(head1,head2) {`



​    `if(head1 === null) { return head2 };`

​    `if( head2 === null ) { return head1 };`

​    `let list = {};`

​    `if( head1.val > head2.val ) {`

​        `list = head2;`

​        `list.next = Merge(head1,head2.next);`

​    `}else {`

​        `list=head1;`

​        `list.next = Merge(head2,head1.next);`

​    `}`

​    `return list;`

`}`

**13**

题目描述:

操作给定的二叉树，将其变换为源二叉树的镜像。

![1575341678917](C:\Users\旧的回忆\AppData\Roaming\Typora\typora-user-images\1575341678917.png)

代码实现：

`function Mirror(root){`

​    `if(root==null) { return null; }`

​    `let temp;`

​    `temp = root.left;`

​    `root.left = root.right;`

​    `root.right = root.left;`

​    `Mirror(root.left);`

​    `Mirror(root.right);`

`}`

**14**

题目描述：

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

代码实现：

`function IsPopOrder(pushV, popV)`
`{`
    `// write code here`
    `let stack = [];`
    `let index = 0;`
    `for(let i=0;i<pushV.length;i++){`
        `stack.push( pushV[i] );`
        `while( stack.length && stack[stack.length-1] === popV[index] ) {`
            `stack.pop();`
            `index++;`
        `}`
    `}`
    `return stack.length == 0;`
`}`

**15**.

题目描述:

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

代码实现：

`function PrintFromTopToBottom(root)`
`{`
    `// write code here`
    `let arr = [];`
    `let data = [];`
    `if(root){`
        `arr.push(root);`
    `}`
    `while(arr.length != 0){`
        `let node = arr.shift();`
        `if(node.left){`
            `arr.push(node.left)`
        `}`
        `if(node.right){`
            `arr.push(node.right);`
        `}`
        `data.push(node.val);`
    `}`
    `return data;`

`}`

**16.**

题目描述:

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

代码实现：

`function Clone(pHead)`

`{`



​    `if(pHead==null) { return null; }`



​    `let head = new RandomListNode(pHead.label);`

​    `head.random = pHead.random;`

​    `head.next = Clone(pHead.next);`

​    `return head;`

`}`

**17**.

题目描述:

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

代码实现：

`function MoreThanHalfNum_Solution(numbers)`
`{`
    `// write code here`
    `numbers.sort( (a,b)=>a-b );`
    `let result = numbers[ Math.floor( numbers.length/2 ) ];`
    `let count = 0;`
    `for(let i=0;i<numbers.length;i++){`
        `if( numbers[i] === result ){`
            `count++;`
        `}`
    `}`
    `if( count > numbers.length/2 ){`
        `return result;`
    `}else {`
        `return 0;`
    `}`
`}`

**19**.

题目描述：

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

代码实现：

`function GetLeastNumbers_Solution(input, k)`

`{`

​    `// write code here`

​    `if( k>input.length ){`

​        `return [];`

​    `}`

​    `input.sort( (a,b)=>a-b );`

``    

​    `return input.slice(0,k);`

`}`