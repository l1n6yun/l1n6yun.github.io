title: Vue - 创建项目
author: l1n6yun
tags: 
 - Vue
 - 前端开发
 - SPA
categories:
date: 2019-01-25 22:38:00
---
> Vue 提供了一个官方的 CLI，为单页面应用 (SPA) 快速搭建繁杂的脚手架。它为现代前端工作流提供了开箱即用的构建设置。只需要几分钟的时间就可以运行起来并带有热重载、保存时 `lint` 校验，以及生产环境可用的构建版本

## 安装

```sh
# 需要管理员权限才能执行这些操作
npm install -g @vue/cli
# OR
yarn global add @vue/cli

# 验证是否安装成功
vue --version
```

## 创建项目

```sh
vue create hello-world

# 选择预设
Vue CLI v4.5.15
? Please pick a preset:
> Default ([Vue 2] babel, eslint)
  Default (Vue 3) ([Vue 3] babel, eslint)
  Manually select features
```

## 运行项目

```sh
 cd hello-word
 npm run serve
```

启动成功

```sh
 DONE  Compiled successfully in 2244ms                         下午10:20:26

  App running at:
  - Local:   http://localhost:8080/
  - Network: http://192.168.0.104:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```