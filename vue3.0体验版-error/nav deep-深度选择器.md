# /deep/ 深度选择器

在vue中，我们为了避免父组件的样式影响到子组件的样式，会在style中加<style scoped>，这样父组件中如果有跟子组件相同的class名称或者使用选择器的时候，就不会影响到子组件的样式。

当你想在父组件修改子组件的样式，就需要使用/deep/

在vue3.0体验版后台项目导航菜单

```
li.el-submenu {
    &.is-active.is-opened /deep/ .el-submenu__title { background-color: $mainColorR !important; }
}
```

如果不加 /deep/ 将不会修改子组件样式