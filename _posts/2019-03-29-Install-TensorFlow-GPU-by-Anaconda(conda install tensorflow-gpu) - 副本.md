---
layout:     post
title:      前端模板规范总结
subtitle:   
date:       
author:   
header-img: img/art-Anaconda-TensorFlow.jpg
catalog: true
tags:
   
---

# 前端模板规范总结
## 什么是前端模块化？
###### 将一个复杂的程序依据一定的规则封装成几个块，然后在组合在一块

##模块化规则的好处？
1. 避免命名冲突
2. 说是按需加载
3. 解决引入多个`<script></script>`发送请求太多,出现无法响应问题

##模块化规范的方法
1.Common.js 
使用`module.exports={}`，页面引用模块用`require`（同步，运行时加载）

2.AMD语法，用require.js来实现（异步）
用法:
在页面引用
```
<script src='/require.js' async='async' defer data-main='/main.js'>
</script>
```
引用main.js页面为主js入口（如果引用的模块js和mian.js是同一个文件名，不需要写路径，直接js名字即可）
main.js:
```
//配置
require.config({
    urlArgs: "version = 1.1.5",//定义版本号，防止子模块js缓存
    shim: {
        'jquery':{
            deps:['','']//依赖的模块名
            exports:''//不规范模块的别名
        }
     },//不规范模块引用方法
     paths: {
       'jquery':'jquery.min'//规范的define处理的模块，也可以引用外部链接,http:
        }
});


//（如果引用的模块js和mian.js是同一个文件名，不需要写路径，直接js名字即可,例如:vue.js直接vue,function()里的参数是命名的参数，引用哪个模块就要用这个参数来调用）

require(['vue', 'component',  'api'], function (Vue,cmp,  api) {
  new Vue({
        el: '#demo',
        data: {
            msg: '我的练习的demo',
            title: '测试',
            yscolor: '#000',
            bgcolor: '#fff',
            show: 0,
            backshow: true,
      },
      components: {
          navhead: cmp.navhead//需要和引用的模块的命对上，然后参数名.模块里的方法或者属性或者对象
      },

    })
});

```
在component.js中:
```
//依赖于api模块
define(['api'], function (api) {
    var navhead = {
        props: [],
     data: function () {
            return {
                title:'',
                }
        },
        created() {
          },
 template: '',

        methods: {
             getlist() {
                var that = this;
                //这里引用api模块的方法
                api.customizemenus({
                    menuName: "navigation"
                }).then(res => {
                    if (res.Code == 200) {
                       
                    }
                }).catch(error => {
                    console.log(error)
                })
            },
         }
    }
    //抛出这个对象
    return {
        navhead
    }
}) 

```
3.ES6模块化方法，静态编译
用export{},页面用import来引用模块




