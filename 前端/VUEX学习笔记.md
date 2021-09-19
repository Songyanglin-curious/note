# VUEX学习笔记

npm install vuex --save



**state** 

储存初始化数据  获取this.$store.state.xxxxx 

![image-20210906122322754](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906122322754.png)

**getters** 

对State 里面的数据二次处理（对数据进行过滤类似filter的作用）比如State返回的为一个对象，我们想取对象中一个键的值用这个方法

this.$store.getters.xxx

![image-20210906125858801](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906125858801.png)

**mutations** 

对数据进行计算的方法全部写在里面（类似computed） 在页面中触发时使用
 this.$store.commit('mutationName')触发Mutations方法改变state的值

![image-20210906130032522](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906130032522.png)

**actions** **（异步）**

action的功能和mutation是类似的，都是去变更store里的state，不过action和mutation有两点不同：

1、action主要处理的是异步的操作，mutation必须同步执行，而action就不受这样的限制，也就是说action中我们既可以处理同步（视图触发Action，Action再触发Mutation），也可以处理异步的操作



2、action改变状态，最后是通过提交mutation

this.$store.dispatch(actionName)



3、角色定位基于流程顺序，二者扮演不同的角色。

Mutation：专注于修改State，理论上是修改State的唯一途径。

Action：业务代码、异步请求。

![image-20210906150803234](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906150803234.png)

![image-20210906150833593](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906150833593.png)

**modules** 

模块化Vuex

简单的模块划分只是将state划分，而getters，mutations，action，并未划分。

名名空间

​	将各个state，getters，mutations，action之类的提出来，暴露出去的时候要加上namespaced: true，



![image-20210906161354657](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906161354657.png)

命名空间完成结果如下图

![image-20210906161628329](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210906161628329.png)

以模块名加方法名调用