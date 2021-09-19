程

1、先布局，将登录注册页面画出

2、根据测试用例的要求，完成表单的验证

3、使用axios进行异步请求，完成登录和注册功能

4、进行后面的布局，先布局公共的框架（侧边导航栏，顶部，内容区）这部分抽出组件使用

5、进行总体路由的配置，完成各个页面的跳转、必要的重定向

6、进行路由守卫的设置，用cookie设置token令牌，进行权限验证。

7、进行后台功能的开发，主要是使用axios请求接口，返回数据，然后对数据进行处理，用到了一些element组件

## 登录注册

1、页面布局是基础的elementUI form组件（带验证规则的）进行校验，校验规则是用正则表达式书写，每一条校验规则用函数包裹抽出放置在自己建立在src下的utils文件下的validate.js，将其引入后使用，这样结构清晰，不凌乱。

### 使用axios进行接口请求

![image-20210919084021297](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919084021297.png)

![image-20210919084049147](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919084049147.png)

![image-20210919084850686](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919084850686.png)

在vueconfig.js中进行axios的一些配置进行**跨域**

[官方说明](https://cli.vuejs.org/zh/config/#devserver-proxy)

![image-20210919091515451](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919091515451.png)

![image-20210919091601521](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919091601521.png)

#### 跨域原理

由于浏览器同源策略（必须：协议、域名、端口号都一致，才能访问）于是很多时候不一致但又要请求资源，就需要进行跨域。

**代理解决跨域原理**：**化不同为相同**。

通过一些方法设置代理，在请求发送(接收)之前加入中间层，将不同的域名转换成相同的，就解决了跨域的问题。客户端发送请求时，不直接到服务器，而是先到代理的中间层。（套个壳让浏览器放行？）

![image-20210919092619296](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919092619296.png)

![image-20210919093408658](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919093408658.png)

![image-20210919093457118](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919093457118.png)

总结：反向代理跨域就是套个壳

#### 请求拦截器

```javascript
// 创建axios，赋值给service
const BASSURL = process.env.NODE_ENV === "production" ? "" : "/devApi";
const service = axios.create({
  baseURL: BASSURL,
  timeout: 5000,
});


// 添加请求拦截器
service.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么

    config.headers['Token'] = getToken();
    config.headers['UserName'] = getUsername();
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
service.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    let data = response.data;
    if(data.resCode !== 0){
      Message.error(data.message)
      // 请求错误
      return Promise.reject(data);
    }else{
      return response;
    }
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });

  export default service;
```

目前所用是在请求前在请求头加入Token和Username，对请求的错误进行弹窗报告。



目前使用的十分浅显，还需要继续阅读文档学习

## 后台页面处理和路由跳转

## 路由创建

1、定义路由的组件（即view下的文件）

2、定义路由（一般位于router/index)

![image-20210919135252334](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919135252334.png)

它将渲染到APP.vue下的router-view

![image-20210919135446899](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919135446899.png)

子路由

![image-20210919135726321](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919135726321.png)

子路由将渲染到父路由到组件中中的router-view

![image-20210919135941177](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919135941177.png)

路由重定向

![image-20210919140440533](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919140440533.png)

加载时/重定向会自动跳转到/login

### 路由守卫

[官方链接](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E5%89%8D%E7%BD%AE%E5%AE%88%E5%8D%AB)

![image-20210919151400793](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919151400793.png)

![image-20210919151417619](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919151417619.png)

```javascript
import router from "./index"
import store from  "@/store/index"
import {getToken,removeToken,removeUsername} from "@/utils/app"
// import store from "../store"

// 声明一个白名单
const whiteRouter = ['/login']

// 路由守卫
router.beforeEach((to,from,next) => {
    // console.log("to")
    // console.log(to)
    // console.log("from")
    // console.log(from)
    if(getToken()){
        // console.log("存在token")
        next();
        if(to.path == '/login'){
            // 登出的时候清空数据，安全起见
            removeToken();
            removeUsername();
            store.commit('app/SET_TOKEN', '')
            store.commit('app/SET_USERNAME', '')
            next();
        }else{
            // 1、获取用户角色
            // 2、动态分配路由权限
            next()
        }
    }else{
        console.log("不存在token")
        // next('/login') //由于路由指向，会出现死循环，解决方法白名单


        // 解决方法
        if(whiteRouter.indexOf(to.path) !== -1) {  // 存在
            next();  // to 
        }else{
            next('/login')  // 路由指向
        }
        /**
         * 1、直接进入index的时候，参数to被改变成了 "/index"，触发路由指向，就会跑beforeEach
         * 2、再一次 next 指向了login，再次发生路由指向，再跑beforeEach，参数的to被改变成了"/login"
         * 3、白名单判断存在，则直接执行next()，因为没有参数，所以不会再次beforeEach。
         */
    }

    // console.log(to) //to要去的页面即下一个页面
    // console.log(from) //from要离开的页面即上一个页面
    // console.log(next)
    // next() //没有参数默认为to
  })
```

![image-20210919152555096](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919152555096.png)

进入后台时候进行判断，有token则可以进入

在login登录时，将登陆成功服务器返回的数据中cookie存在token中，就可以正常跳转，每一次跳转都会触发这一全局前置守卫，当token失效时则无法进行跳转。

登出，就是将路由切换到/login即登录页面，此时将浏览器内部储存的token和程序内store中储存的清除掉。

如果不经登录直接进入后台则会被拦截。

## 组件

组件实例的作用域是孤立的

#### 父组件传值子组件用自定义属性

子组件接收父组件传入的值：子组件要显式用props声明预期的数据。

父组件所传数据更新则子组件的更新，但是子组件不能修改父组件传入的参数，这称为**单项数据流**。

#### 子组件传值父组件用自定义事件

![image-20210919161618370](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919161618370.png)

![image-20210919161659049](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919161659049.png)

![image-20210919161818952](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919161818952.png)

但是简单的数据，子组件不进行逻辑运算的数据可以用.sync更新到父组件

### 全局组件

![image-20210919154431009](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919154431009.png)

全局组件要在main.js中引入，并且注册，注册组件的前一个参数是组件名称使用时是"字符串类型"

```javascript
<icon :name="item.props.icon" size=16 color="#fff"></icon>
```

用的是组件名称icon

局部组件相差不多

![image-20210919154925987](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919154925987.png)

不过要在其父组件内部引用，注册。



## vuex

![image-20210919164341417](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919164341417.png)

![image-20210919164415580](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919164415580.png)

![image-20210919164445638](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210919164445638.png)

