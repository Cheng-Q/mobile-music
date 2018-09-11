
### 运行

```
 git clone git@github.com:godlikedeveloper/mobile-music.git

 npm install

 npm run dev

```
### 项目介绍:blush:

> 网易云音乐接口+vue全家桶开发一款移动端音乐webApp

> 项目还在develop中，感兴趣想要参与的小伙伴可以私我

效果图：

![骨架图](http://u-to-world.com:8081/wp-content/uploads/2018/09/QQ截图20180907173310.png "骨架图")

![首页](http://u-to-world.com:8081/wp-content/uploads/2018/09/Screenshot_2018-09-07-17-30-43-526_com.tencent.mo_.png "首页")

![登录后侧边栏](http://u-to-world.com:8081/wp-content/uploads/2018/09/Screenshot_2018-09-07-17-31-00-822_com.tencent.mo_.png "登录后侧边栏")

![每日推荐](http://u-to-world.com:8081/wp-content/uploads/2018/09/Screenshot_2018-09-07-17-31-41-811_com.tencent.mo_.png "每日推荐")

![歌单页](http://u-to-world.com:8081/wp-content/uploads/2018/09/Screenshot_2018-09-07-17-32-03-492_com.tencent.mo_.png "歌单页")

![小播放器](http://u-to-world.com:8081/wp-content/uploads/2018/09/Screenshot_2018-09-07-17-32-20-307_com.tencent.mo_.png "小播放器")

![大播放器](http://u-to-world.com:8081/wp-content/uploads/2018/09/Screenshot_2018-09-07-17-32-27-759_com.tencent.mo_.png "大播放器")

### 详细信息

> <a href='http://u-to-world.com:8080/static/index.html#/' style="text-decoration: underline;">测试地址</a>



### 开发总结


#### 项目结构
 
 vue-cli搭建

 新增目录如下：
 
   ```
     ---src 
     ------api        // 放置api的目录
     ---------base.js // 放置axios的一些配置，接口域名地址，以及公共参数配置，与后台约定跨域的配置，全局loading配置等
     ---------urls.js // 放置接口url 
     ---------api.js  // 放置封装的promise请求
     ------base       // 放置一些基础组件 
     ------common  
     ---------js      // 公共js 
     ---------sass    // 公共样式 
  ```

#### 类库使用

 * fastclick解决移动端300ms延迟

 * vux 快速构建一些常规页面

 * vue-lazyLoad 对图片进行懒加载处理

 * better-scroll 轮播图

 * NeteaseCloudMusicApi  wy音乐接口，node封装转发，部署在自己服务器上



 #### 路由按需加载

   ```
    const view = (path, name) => () => import(`@/components/${path}${name}`)// 路由按需加载
    //这边用的是vue异步组件的方式实现路由的按需加载
    new Vue({
      // ...
      components: {
        'my-component': () => import('./my-async-component')
      }
    })

   ```
  * 路由加载时用了transition动画组件添加了一个切换动画
  * 注意如果你希望在 Vue Router 的路由组件中使用上述语法的话，你必须使用 Vue Router 2.4.0+ 版本。

#### 播放器组件

大小播放器分别写了`MiniPlayer.vue`和`NormalPlayer.vue`两个组件，因为想要职责单一，就没有放在一起

* 隐藏显示 通过vuex进行管理

* 动画 
  
   1. 头部下坠和底部的上浮

     
      ```
       <transition name="example">

      </transition>

      /*css 样式*/
      // 给 transition下第一个元素显示或隐藏时添加的样式
       //这两个类名都是定义开始到结束的持续时间 方式 以及延迟
      .example-enter-active{
        transition:all 0.4s linear  对所有属性执行0.4s的动画 匀速
      }
      .example-leave-active{
        transition:all 0.4s linear  对所有属性执行0.4s的动画 匀速
      }
      // 进入过度的开始状态 触发时机 元素被插入前 插入后下一帧移除
      .example-enter{


      }
      // 离开过度的结束状态 触发时机 example-leave下一帧  动画过度完成被移除
      .example-leave-to{


      }
     
       可以使用碟中谍6中的halo跳伞来理解

       .example-enter-active就是从飞机上离开到开伞的时间

       .example-enter 下坠前在飞机上的最后一刻

       .example-enter-to  开始下坠，具备加速度的那一刻 

       .example-leave-active 开伞到着陆的时间

       .example-leave 开伞命令发出时

       .example-leave-to 伞开下一刻
      ```
     
   2. 播放器的cd的位移及缩放

       先计算出小播放器图片离最终大播放器cd的x,y轴上的距离

       使用 `create-keyframe-animation` 进行一个`css3`动画状态的注册

       再利用transition的动画方法钩子

       在`enter`时`run`动画,`afterEnter`时清除动画 `leave`同理

   3. 播放器的旋转

       定义一个旋转的`css`动画，在一个`class`中进行调用，在`play`的状态下给它`addClss`,`pause`时加上`animation-play-state: paused`


 #### audio的使用

  使用`html5`的 `audio`结合`vuex`来进行播放器功能的实现，包括进度条，播放，暂停，上一曲，下一曲，播放模式等

 #### 布局

   * 绝大多数使用了flex  webpack中配置低版本安卓，ios加前缀

   * 考虑到fixed元素的移动端问题，在这种场景下，使用100%高度+absolute方案更适合

   * 使用媒体查询，兼容一下某些样式在768px以上的样式变形

   * 使用rem 在vue实例的`mounted`的钩子里注册`resize`和`onload`监听，进行最外层rem基准的计算

   * 使用骨架屏进行加载资源白屏时填充，待优化至完全的主页面服务端渲染




### 感谢:blush:

  * vue

  * vuex

  * vue-router

  * vux

  * vue-lazyLoad

  * NeteaseCloudMusicApi

