# Nuxt

vue的一个框架，使用vue2。

### 项目结构：

### pages文件夹

整个nuxt项目的所有页面和路由，定义在这个文件夹下的`文件夹/文件.vue`都会自动的添加对应的路由。`xxx/yyy.vue`访问 host/xxx/yyy 即可

### components文件夹

和vue-cli创建的vue项目中的文件夹有差不多的作用，相当于公共组件，会被自动`import`到pages下的每一个vue文件中

### assets文件夹

不用编译的静态资源文件，css、img、font等

### static文件夹

映射项目根目录的文件夹（打包完成后的），favicon.ico存放的那个文件夹

### layouts文件夹

布局组件，里面提供`<Nuxt />`相当于插槽，可以嵌入pages组件

