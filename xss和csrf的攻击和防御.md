**xss**

xss叫跨站脚本攻击,实质上也就是代码注入,引起的原因是:浏览器和服务器没有对用户输入进行过滤,导致恶意代码的执行。

xss分类：反射型,储存型,dom型。

xss防范:

1.对用户的输入进行转义过滤(客户端和服务端都需要)

2.在http的cookie首部上的set-cookie对象上设置http-only属性，禁止javascript脚本读取cookie

3.在必要的时候可以使用验证码



**csrf**:

csrf叫跨站伪造请求,

csrf实例:A需要在银行进行一次转账,黑客B用广告引诱用户点击,即向银行发送一个转账的请求,这个请求会依据后台的session验证是否合法,在大多数的情况下会失败,但是如果A刚刚进行了一笔转账,黑客B伪造的请求携带cookie给了服务端,而这时的session还没有过期,那么就悲剧了,黑客B的转账就会成功,这个就是csrf的一个简单的实例



csrf防御策略：

1.验证Http Referer字段:

在http首部字段中有个referfer属性,它的值是一个请求的源地址,就像上面那个例子中Referer的值是黑客伪造的地址,而不是A(银行的url)的地址,所以处理的话验证referer的值和A(银行的url)的地址是否一样就可以,但是有个缺点是Referer的字段是由浏览器提供,安全对于这种的话不能依赖于第三方(浏览器).



2.在http请求中添加token并进行验证：

在http请求中加入一个随机生成的token=tokenValue得值,而服务端则建立一个拦截器获取请求的token值,与存在于session中token进行对比验证,从而知道token是否合法,缺点是在get的请求方式下会显示在url中,并且每个请求都需要在请求加上token,这样的话太麻烦了.



3.在http头的自定义属性进行:

它是一次性给XMLHttpRequest这个构造函数添加scrfToken的，但凡通过ajax请求的都会拥有token值,

