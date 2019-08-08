# Typing-speed

<br><br>



# 项目概览



### :wind_chime:项目描述

本项目是基于 [vue框架](https://cn.vuejs.org/v2/guide/installation.html#Vue-Devtools) 和 [element-ui](https://element.eleme.cn/#/zh-CN/component/menu) 组件库开发的打字测速模块，用于进行打字速度的测试。本模块提供三个指标：打字时长、打字速度（字/每分钟）、正确率。



### :movie_camera:项目展示

![typing-speed](C:\Users\Invoker\Desktop\typing-speed.gif)

### :surfer:项目测试

1. 下载项目

   ```
   git clone https://github.com/yinvoke/typing-speed.git
   ```

2. 安装依赖

   ```
   npm i element-ui -S
   npm install
   ```

3. 运行项目

   ```
   npm run serve
   ```

   



### :sweat_drops:作为组件使用

1. 下载项目中的`typingSpeed.vue`

2. 注册组件

   ```js
   export default {
     name: 'app',
     components: {
       typingSpeed
     }
   }
   ```

3. 使用

   ```css
   <typingSpeed/>
   ```



### :fire:作为路由使用

1. 下载项目中的`typingSpeed.vue`

2. 注册路由

   ```js
   const router = new VueRouter({
     routes: [
       {
         path: '/typingSpeed',
         component: typingSpeed,
       }
     ]
   })
   
   export default {
     name: 'app',
     router
   }
   ```

3. 使用

   ```
   <div>
   	<router-view></router-view>
   	<router-link to="/typingSpeed">
   </div>
   ```



