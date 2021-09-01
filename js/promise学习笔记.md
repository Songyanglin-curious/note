# promise学习笔记

本质上 Promise 是一个函数返回的对象，我们可以在它上面绑定回调函数，这样我们就不需要在一开始把回调函数作为参数传入这个函数了。

`Promise` 对象是由关键字 `new` 及其构造函数来创建的。该构造函数会把一个叫做“处理器函数”（executor function）的函数作为它的参数。这个“处理器函数”接受两个函数——`resolve` 和 `reject` ——作为其参数。当异步任务顺利完成且返回结果值时，会调用 `resolve` 函数；而当异步任务失败且返回失败原因（通常是一个错误对象）时，会调用`reject` 函数。

简单理解：resolve 是 promise 执行成功，相当于.then()

​					reject是 promise 执行失败，相当于.catch()

![image-20210901222159984](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210901222159984.png)

![image-20210901222229533](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210901222229533.png)

传参

![image-20210901222554013](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210901222554013.png)

![image-20210901222613262](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210901222613262.png)



```
 function promise1(status){
            return new Promise((resolve, reject) => {
                if(status) {
                    console.log('第1个promise成功');
                    resolve('第1个promise返回数据成功'); // axios
                }
                if(!status) {
                    console.log('第1个promise失败');
                    reject('第1个promise返回数据失败')
                }
            })
        }
        function promise2(status){
            return new Promise((resolve, reject) => {
                if(status) {
                    console.log('第2个promise成功');
                    resolve('第2个promise返回数据成功')
                }
                if(!status) {
                    console.log('第2个promise失败');
                    reject('第2个promise返回数据失败')
                }
            })
        }

        function promise3(status){
            return new Promise((resolve, reject) => {
                if(status) {
                    console.log('第3个promise成功');
                    resolve('第3个promise返回数据成功')
                }
                if(!status) {
                    console.log('第3个promise失败');
                    reject('第3个promise返回数据失败')
                }
            })
        }

        function promise4(status){
            return new Promise((resolve, reject) => {
                if(status) {
                    console.log('第4个promise成功');
                    resolve('第4个promise返回数据成功')
                }
                if(!status) {
                    console.log('第4个promise失败');
                    reject('第4个promise返回数据失败')
                }
            })
        }
        /* 
        链式调用
        */
        /* 
        这种链式调用：
        1、中间的执行失败即返回reject()，后面的都不执行，直接执行.catch()
        2、这种链式调用便于维护，可以在执行promise2()之后执行promise4()，
            而不受影响，在需要添加promise3()的时候能很容易添加。
        */
        
        // promise1(true).then( response => {
        //     console.log(response);//第1个
        //     // 返回参数，判断是否执行第二个
        //     return promise2(false)
        // })
        // .then(response => {
        //     console.log(response)//第2个
        //    return promise3(true)
        // })
        // .then(response => {
        //     console.log(response)//第3个
        // })
        // .catch(error => {
        //     console.log(error)
        // })


        /* 
        all()方法
        */
       /* 
       1、数组内的promise必须全部返回resolve()即成功才会执行.then()
        */
        // Promise.all([promise1(false),promise2(false),promise3(true),promise4(true),]).then(response => {
        //     console.log("全部调用成功")
        // })
        // .catch(error => {
        //     console.log("有些可能失败")
        // })
        // race数组，当在执行的时候，遇到的返回结果是resolve（成功），一直执行链式。
        // 最终的理解：rece，只要有一个是返回resolve，就代表成功，就会回调then。但是，如果第一个是返回reject，那么就失败了

        // race 的成功与否只与第一个promise有关，第一个promise返回resolve()即成功才会执行.then()，否则。catch()
        Promise.race([promise1(true), promise2(false), promise3(false)]).then(response => {
            console.log('成功');
        }).catch(error => {
            console.log('失败了');
        })

        // all race 都会将数组内的promise执行一遍

```

