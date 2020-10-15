# 本电子文档介绍

智慧校园PC2.0重构总结

#### 技术栈介绍

- vue-cli：webpack v2.6.1 + vue v2.5.7 + vue-router v2.0.3 + vuex v2.0.0
- iview v3.4.2：webpack 和 vue 版本不能低于以上版本
- element-ui v2.10.0：tree 组件用到elmUI
- axios v0.19.0：项目中 ajax 基于 axios 二次封装
- less v3.9.0：less.js 最新版本，对应 less-loader v5.0.0。2.0升级了 less 版本，解决了因版本太低导致 less 函数不能使用问题
- echarts v4.2.1：图表组件
- cropper.js v1.5.4：图片裁剪
- vue-clipboard2 v0.2.1：复制组件
- vue-grid-layout v2.3.4：栅格化布局组件，类似 win10 应用菜单

#### 组件库介绍
1. 基于iview 3.0二次封装
2. 内部封装install方法，可以通过vue.use使用，全局挂载
3. 公共组件库(base)使用 git submodule(git子模块) 引入项目。优点：
   1. base同时也是单独的git项目，拥有自己的版本tag
   2. 新增或编辑组件可以在base项目，也可以在引入的项目中完成，分布式提交
   3. 依托于git，减少其他包管理工具引入


[git submodule相关文档](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)       
[git submodule使用问题](https://docs.qq.com/doc/DR0VaUGFJcWlGSGRG) 
