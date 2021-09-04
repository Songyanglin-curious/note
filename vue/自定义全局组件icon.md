# 自定义全局组件icon

#### 1、登录 iconfont官网[https://www.iconfont.cn](https://www.iconfont.cn/)找到图标并下载

#### 2、将其保存到

![image-20210904222341409](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210904222341409.png)

assets/iconfont（自己定义的）

#### 3、封装组件

- 可以通过type或者name入参来标识具体是哪个图标
- 要提供最常用的样式入参，一个是图标颜色color，另外一个是图标大小size
- 能响应事件
- 使用者能定制一些特殊样式如旋转角度rotation等

代码如下：

```javascript
<template>
  <!-- 有两个注意点：一是iconfont这个类名必须添加，二是通过$listeners将父作用域中的事件监听器导入进来，这样就可以正常相应点击事件 -->
    <i
    class="iconfont"
    :class="name"
    :style="{fontSize:`${size}px`,color}"
    :transform="`roate(${deg})`"
    v-on="$listeners"
    ></i>
</template>

<script>
export default {
    name: "Icon",
   props: {
    // 图标名称
    name: {
      type: String,
      default: ""
    },
    // 图标颜色
    color: {
      type: String,
      default: ""
    },
    // 图标大小
    size: {
      type: [String, Number],
      // type: Number,
      default: 100
    },
    // 图标旋转角度
    deg: {
      type: String,
      default: "0deg"
    }
  }

}
</script>

<style scoped>
/* // 这里加～是因为当你在一个资源路径最前面加了～时，webpack会把这个路径对应的文件视为一个module（模块）去解析
// 这样@对应的alias，也就是项目的src目录才会被正确识别，这步解析是webpack做的，和网上很多文章说的css-loader没有一毛钱关系
// 还有一种写法是@import url()，两种写法都可以，没有对错之分 */
@import "~@/assets/iconfont/iconfont.css";

.iconfont {
  cursor: pointer;
}

</style>
```

#### 4全局注册该组件

![image-20210904222848668](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210904222848668.png)

注册组件component中第一个参数是组件名称要用字符串，第二个参数是组件

#### 5使用图标组件

![image-20210904222940842](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210904222940842.png)

