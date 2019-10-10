# 安装npm

安装 node.js后就 可以使用 npm命令了

# 安装cnpm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

# 安装vue-cli

-g ：全局安装参数

```
cnpm install -g @vue/cli
```

# 安装webpack

```
cnpm install -g  webpack
```





# 创建项目

使用vue提供的ui界面建立项目（真的是巨慢！！）

```
vue ui
```

使用命令创建项目

```
vue create projectname
```

## 手动选择组件（未完成）

```
? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
>(*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 ( ) Router
 ( ) Vuex
 ( ) CSS Pre-processors
 (*) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing   
```





# 项目结构说明

```
public  -- 打包后，需要部署到生产环境的内容
src -- 开发的源码
```



# idea环境配置 

安装 vue.js 插件 

# 启动项目

```
npm run serve
```

# 安装iview组件库

```
npm install iview --save
```







# vue介绍

一个vue应用，由一个vue实例、可选组件（组件可嵌套、可复用）树组成。



一个组件，就是一个可复用的vue实例

## 组件

props 定义组件拥有的属性，