# [element ui 中的 resetFields() 报错'resetFields' of undefined](https://www.cnblogs.com/tentacion/p/11543169.html)

　　每次做各种form表单时，首先要注意的是初始化，但是刚开始若没有仔细看文档，则会自己写个方法将数据设置为空，但是这样就会出现一个问题，表单内存在各种验证，假如是一个弹框内有form表单，弹框出现就执行上述代码，可能会出现表单验证的错误提示仍然保留的情况。

element UI 官方文档提供了一个resetFields()的方法

```
this``.$refs[formName].resetFields()
```

## 不仅可以帮你初始化数据，还可以将验证提示消除！！！

但是在使用时踩了一些坑，

> 编辑和新增使用了同一个弹出框`<el-dialog><el-form></el-form></el-dialog>`
> 绑定了数据data里的commentForm对象
> 为了在新增弹出框清空表单, 使用了`this.$refs[formName].resetFields()`
> 每次第一次点击新增显示弹出框，都会报错
> "[Vue warn]: Error in event handler for "click": "TypeError: Cannot read property 'resetFields' of undefined""

### 问题原因： mouted加载table数据以后，隐藏的弹出框并没有编译渲染进dom里面。

### 所以`@click="dialogFormVisible = true;resetForm('dlgForm')"`click弹出的时候$refs并没有获取到dom元素导致 'resetFields' of undefined

### 解决方法：

1、($nextTick dom下一次更新之后)

```
            resetForm(formName) {
                this.$nextTick(()=>{
                    this.$refs[formName].resetFields();
                })                
            },
```

2、(如果是第一次就点击新增就没必要reset, 根据元素undefined判断)

```
                if (this.$refs[formName] !== undefined) {
                    this.$refs[formName].resetFields();
                }
```

### 注意事项：对DOM一系列的js操作最好都要放进Vue.nextTick()的回调函数中