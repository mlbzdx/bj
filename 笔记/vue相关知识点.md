> 我的第一个Vue应用（未完，待补充）

# 1.main.js （入口模块）

```main.js
// 入口模块
import "@/mock";
import Vue from "vue";
import App from "./App";
import "@/styles/global.less";
import router from "@/router";
import { showMessage } from "@/utils";
Vue.prototype.$showMessage = showMessage;

new Vue({
  render : (h) => h(App),
  router
}).$mount("#app");
```

# 2.App.vue （根组件模块）

```App.vue
<template>
  <div class="test-container">
    <Layout>
        <template v-slot:left>
            <SiteAside/>
        </template>
        <template>
              <RouterView />
        </template>
        <!-- <template v-slot:right>
            <div>右侧区域</div>
        </template> -->
    </Layout>
  </div>
</template>

<script>
import Layout from "@/components/Layout";
import SiteAside from "@/components/SiteAside";
export default {
    components : {
        Layout,
        SiteAside
    }
}
</script>

<style>
.test-container{
    width:100%;
    height:100vh;
    border: 2px solid;
    margin: 0 auto;
    white-space: nowrap;
}
.left {
  width: 200px;
  height: 100%;
}
.main {
  width: 100%;
  height: 100%;
}
/* .right {
  width: 150px;
  height: 100%;
} */
</style>
```

# 3.api组件 （网络数据请求）

## （1）banner.js （获取请求）

```banner.js
import request from "./request";
export async function getBanner(){     
   return await request.get("/api/banner")
}
```

## （2）request.js (创建请求实例与拦截)

```request.js
import axios from "axios";
import showMessage from "@/utils/showMessage";

const instance =  axios.create();
instance.interceptors.response.use((resp) => {
    if(resp.data.code===0){
        return resp.data.data;
    }else{
        showMessage({
            content:resp.data.msg,
            type : "error"
        })
        return null;
    }       
})
export default instance;
```

# 4.components (通用组件)

## （1）Avatar (头像组件)

```index.vue
<template>
  <div class="avatar-container">
    <img class="avatar-container"
    :src="url" 
    :style="{
      width : size + 'px',
      height : size + 'px'
    }" alt="">
  </div>
</template>

<script>
export default {
  props : {
    url : {
      type : String,
      required : true
    },
    size : {
      type : Number,
      default : 150
    }
  }
}
</script>

<style lang="less" scoped>
  .avatar-container{
    img{
      display: block;
      border-radius: 50%;
      object-fit: cover;

    }
  }
</style>
```

## （2）Empty (空白页组件)

```index.vue
<template>
    <div class="empty-container">
        <Icon  type="empty"></Icon>
        <div class="center">{{ text }}</div>
    </div>
</template>

<script>
import Icon from "../Icon";
export default {
    components : {
        Icon
    },
    props : {
        text : {
            type : String,
            default : "无数据"
        }
    }

}
</script>

<style lang="less" scoped>
@import "~@/styles/mixin.less";
@import "~@/styles/var.less";
.empty-container{
    .self-center();
    text-align: center;
    color: @gray;
}
.icon-container{
    font-size: 72px;
}
</style>
```

## （3）Icon (图标组件)

```index.vue
<template>
  <i class="iconfont icon-container" :class="fontClass"></i>
</template>

<script>
const classMap = {
  home: "iconzhuye",
  success: "iconzhengque",
  error: "iconcuowu",
  close: "iconguanbi",
  warn: "iconjinggao",
  info: "iconxinxi",
  blog: "iconblog",
  code: "iconcode",
  about: "iconset_about_hov",
  weixin: "iconweixin",
  mail: "iconemail",
  github: "icongithub",
  qq: "iconsign_qq",
  arrowUp: "iconiconfonticonfonti2copy",
  arrowDown: "iconiconfonticonfonti2",
  empty: "iconempty",
  chat: "iconliuyan",
};
export const types = Object.keys(classMap);
export default {
  props: {
    type: {
      type: String,
      required: true,
    },
  },
  computed: {
    // 图标类样式
    fontClass() {
      return classMap[this.type];
    },
  },
};
</script>

<style scoped>
@import "//at.alicdn.com/t/font_2164449_nalfgtq7il.css";
.iconfont {
  color: inherit;
  font-size: inherit;
}
</style>

```

## （4）ImageLoader (图片加载显示组件)

```index.vue
<template>
    <div class="image-loader-container">
      <img v-if="!everytihsingover" :src="placeholder" class="placeholder">
      <img alt="正在加载图片……"
      @load="overLoader($event)"
      :src="src"  
      :style="{
        opacity : isvisible,
        transition : `${duration}ms ease-in-out`
      }">
    </div>
</template>

<script>
export default {
  props : {
    src : {
      type : String,
      required : true
    },
    placeholder : {
      type : String,
      required : true
    },
    duration : {
      type : Number,
      default : 500
    }
  },
  data(){
    return {
      isOpacity : true,
      everytihsingover : false
    }
  },
  computed : {
    isvisible(){
      return this.isOpacity ? 0 : 1;
    }
  },
  methods : {
    overLoader(){ 
      this.isOpacity = false;
      setTimeout(() => {
        this.everytihsingover = true;
        this.$emit("load");
      },this.duration)
    }
  }
}
</script>

<style lang="less" scoped>
@import "~@/styles/mixin.less";
  .image-loader-container{
    width: 100%;
    height: 100%;
    position: relative;
    overflow : hidden;
    & img{
      .self-fill();
      object-fit: cover;
    }
    & .placeholder{
      filter: blur(2vw);
    }
  }
</style>
```

## （5）Layout （布局组件）

```index.vue
<template>
  <div class="layout-container">
    <header class="left">
        <slot name="left"></slot>
    </header>
    <main class="main" >
        <slot></slot>
    </main>
    <footer class="right">
        <slot name="right"></slot>
    </footer>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>
.layout-container{
    width: 100%;
    height: 100%;
    display: flex;
    .left,.right{
        flex: 0 0 auto;
        overflow: hidden;
    }
    .main{
        flex: 1 1 auto;
        overflow: hidden;
    }
}

</style>
```

## （6）Pager (翻页组件)

```index.vue
<template>
  <!-- 只有总页数大于1时才显示 -->
  <div class="pager-container" v-if="pageNumber > 1">
    <a @click="handleClick(1)" :class="{ disabled: current === 1 }">
      |&lt;&lt;
    </a>
    <a @click="handleClick(current - 1)" :class="{ disabled: current === 1 }">
      &lt;&lt;
    </a>
    <a
      @click="handleClick(n)"
      v-for="(n, i) in numbers"
      :key="i"
      :class="{ active: n === current }"
    >
      {{ n }}
    </a>

    <a
      @click="handleClick(current + 1)"
      :class="{ disabled: current === pageNumber }"
    >
      &gt;&gt;
    </a>
    <a
      @click="handleClick(pageNumber)"
      :class="{ disabled: current === pageNumber }"
    >
      &gt;&gt;|
    </a>
  </div>
</template>

<style lang="less" scoped>
@import "~@/styles/var.less";
.pager-container {
  display: flex;
  justify-content: center;
  margin: 20px 0;
  a {
    color: @primary;
    margin: 0 6px;
    cursor: pointer;
    &.disabled {
      color: @lightWords;
      cursor: not-allowed;
    }
    &.active {
      color: @words;
      font-weight: bold;
      cursor: text;
    }
  }
}
</style>

<script>
export default {
  props: {
    current: {
      type: Number,
      default: 1,
    },
    total: {
      type: Number,
      default: 0,
    },
    limit: {
      type: Number,
      default: 10,
    },
    visibleNumber: {
      type: Number,
      default: 10,
    },
  },
  computed: {
    // 总页数
    pageNumber() {
      return Math.ceil(this.total / this.limit);
    },
    // 得到显示的最小数字
    visibleMin() {
      let min = this.current - Math.floor(this.visibleNumber / 2);
      if (min < 1) {
        min = 1;
      }
      return min;
    },
    visibleMax() {
      let max = this.visibleMin + this.visibleNumber - 1;
      if (max > this.pageNumber) {
        max = this.pageNumber;
      }
      return max;
    },
    numbers() {
      let nums = [];
      for (let i = this.visibleMin; i <= this.visibleMax; i++) {
        nums.push(i);
      }
      return nums;
    },
  },
  methods: {
    handleClick(newPage) {
      if (newPage < 1) {
        newPage = 1;
      }
      if (newPage > this.pageNumber) {
        newPage = this.pageNumber;
      }
      if (newPage === this.current) {
        return;
      }
      // 抛出一个事件
      this.$emit("pageChange", newPage);
    },
  },
};
</script>

```

## （7）SiteAside (侧边栏组件)

### 1）inde.vue

```index.vue
<template>
  <div class="site-aside-container">
    <Avatar url="http://mdrs.yuanjin.tech/FgMwAPYq17So9nwVH44ltDHo7u3c"/>
    <h1 class="title">侯东泽的小窝</h1>
    <Menu />
    <Contact />
    <p class="footer">
      黑ICP备17001719号
    </p>
  </div>
</template>

<script>
import "@/styles/global.less";
import Contact from "./Contact";
import Menu from "./Menu";
import Avatar from "@/components/Avatar";
export default {
    components : {
        Avatar,
        Menu,
        Contact,
    }
}
</script>

<style lang="less" scoped>
@import "~@/styles/var.less";
    .site-aside-container{
      padding: 20px 0;
      background: @dark;
      width: 100%;
      height: 100%;
      overflow-x: hidden;
      overflow-y: auto;
      .avatar-container{
        display: flex;
        justify-content: center;
      }
      .footer {
        text-align: center;
        font-size: 12px;
        }
      .title {
         font-size: 1.2em;
         color: #fff;
         text-align: center;
         padding-top: 10px;
        }
    }
</style>
```

### 2) Contact (联系方式组件)

```index.vue
<template>
    <ul class="contact-container">
        <li>
            <a href="">
                <Icon class="icon"   type="github"/>
                <span>github</span>
            </a>
        </li>
        <li>
            <a href="">
                <Icon class="icon mail"   type="mail"/>
                <span>邮箱：hdz1768321@163.com</span>
            </a>
        </li>
        <li>
            <a href="">
                <Icon class="icon "   type="qq"/>
                <span>QQ</span>
                <div class="pop">
                    <img src="../../../images/qq.png" alt="">
                </div>
            </a>
        </li>
        <li>
            <a href="">
                <Icon class="icon weixin"   type="weixin"/>
                <span>微信</span>
                <div class="pop">
                    <img src="../../../images/qq.png" alt="">
                </div>
            </a>
        </li>
    </ul>
</template>

<script>
import Icon from "@/components/Icon";
export default {
    components : {
        Icon
    },
}
</script>

<style lang="less" scoped>
@import "~@/styles/var.less";
    .contact-container{
        color : @gray;
        list-style: none;
        padding: 20px;
        @itemHeight : 30px ;
        li{
            height: @itemHeight;
            line-height: @itemHeight;
            text-align: center;
            margin: 14px 0;
            white-space: nowrap;
            a {
                display:flex;
                cursor: pointer;
                font-size: 14px;
                position: relative;
                span{
                    overflow-x: hidden;
                }
                .icon{
                    text-align: center;
                    font-size: 26px;
                    width: 36px;
                    &.weixin {
                    font-size: 31px;
                    text-indent:-1px;
                     }
                    &.mail{
                       margin: 0 5px;
                    }
                }
                &:hover .pop{
                    transform: scaleY(1);
                }
                .pop{
                    display: flex;
                    align-items: center;
                    transform: scaleY(0);
                    transition: 0.3s;
                    transform-origin: center bottom;
                    background: #fff;
                    position: absolute;
                    left: 0;
                    border-radius: 5px;
                    bottom : @itemHeight+5px;
                    &::after{
                        content: '';
                        display: block;
                        position: absolute;
                        width: 10px;
                        height: 10px;
                        background: #fff;
                        left: 50%;
                        bottom: 0;
                        transform: translate(-50%,50%) rotate(45deg);
                    }
                    img{
                        width: 150px;
                        height: 150px;
                        object-fit: cover;
                    }
                }

            }
        }
    }
</style>
```

### 3) Menu （导航菜单组件）

```index.vue
<template>
<ul class="menu-container">
    <li v-for="item in items"
        :key="item.link">
        <RouterLink 
        :exact="item.isExact" 
        :to="{name : item.name}"
        active-class="selected"
        exact-active-class="">
            <Icon :type="item.type" class="icon"/>
            <span>{{ item.title }}</span>
        </RouterLink>
    </li>
</ul>
</template>

<script>
import Icon from "@/components/Icon";
export default {
    components : {Icon},
    data(){
        return {
            items : [
                {
                    name : 'Home',
                    title : '首页',
                    type : "home",
                    isExact : true
                },
                {
                    name : 'Blog',
                    title : '文章',
                    type : "blog",
                    isExact : false
                },
                {
                    name : 'About',
                    title : '关于我',
                    type : "about",
                    isExact : true
                },
                {
                    name : 'Code',
                    title : '项目&效果',
                    type : "code",
                    isExact : true
                },
                {
                    name : 'Message',
                    title : '留言板',
                    type : "chat",
                    isExact : true
                },
            ]
        }
    },
    methods : {
        isSelcted(item){
            let path = item.path.toLowerCase();
            let curPathname = location.pathname.toLowerCase();
            if(item.startWith){
                return curPathname.startsWith(path);
            }else{
                return curPathname === path;
            }
            
        }
    }
}
</script>

<style lang="less" scoped>
@import "~@/styles/var.less";
.menu-container{
    color : @gray;
    margin: 24px 0;
    @itemHeight : 30px;
    li{
        list-style: none;
        a{
            &.selected{
            background: darken(@words, 3%);
        }
            display: flex;
            padding: 0 50px;
            align-items: center;
            height: 45px;
            
            .icon{
            // font-size: 24px;
            width: 24px;
            }
            &:hover{
                color: #fff;
            }
        }
    }
}
</style>
```

# 5.mock (模拟数据组件)

## (1)index.js

```index.js
import "./banner";
import "./blog";
import Mock from "mockjs";
Mock.setup({
    timeout : "500-1000"
})
```

## (2)banner.js （模拟首页数据）

```banner.js
import Mock from "mockjs";
export default Mock.mock("/api/banner","get",{
    "code" : 0,//错误码
    "msg" : "我也不知道发生了什么错误",//错误消息
    "data" : [
        {
          id: "1",
          midImg: "http://mdrs.yuanjin.tech/img/20201031141507.jpg",
          bigImg: "http://mdrs.yuanjin.tech/img/20201031141350.jpg",
          title: "凛冬将至",
          description: "人唯有恐惧的时候方能勇敢",
        },
        {
          id: "2",
          midImg: "http://mdrs.yuanjin.tech/img/20201031205550.jpg",
          bigImg: "http://mdrs.yuanjin.tech/img/20201031205551.jpg",
          title: "血火同源",
          description: "如果我回头，一切都完了",
        },
        {
          id: "3",
          midImg: "http://mdrs.yuanjin.tech/img/20201031204401.jpg",
          bigImg: "http://mdrs.yuanjin.tech/img/20201031204403.jpg",
          title: "听我怒吼",
          description: "兰尼斯特有债必偿",
        },
      ]
      //错误的具体内容
})
```

# 6.router (路由组件)

## (1) index.js

```index.js
import Vue from "vue";
import VueRouter from "vue-router";
Vue.use(VueRouter);
import routes from "./routes";

const router = new VueRouter({
  routes,
  mode:"history"
});
export default router;
```



## (2) routes.js

```routes.js
import Home from "@/views/Home";
import About from "@/views/About";
import Blog from "@/views/Blog";
import Project from "@/views/Project";
import Message from "@/views/Message";


export default [
    {name :"Home", path:'/',component:Home},
    {name :"About",path:'/about',component:About},
    {name :"Blog",path:'/blog',component:Blog},
    {name :"Message",path:'/message',component:Message},
    {name :"Code",path:'/code',component:Project}
  ]
```

# 7. styles (样式组件)

## (1) global.less (全局样式)

```global.less
@import "./var.less";
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html {
  color: @words;
  font-family: "-apple-system", "BlinkMacSystemFont", "Roboto", "Helvetica Neue",
    "微软雅黑", sans-serif;
  overflow: hidden;
}
body {
  overflow: hidden;
  margin: 0;
}

a {
  color: inherit;
  text-decoration: none;
  &:hover {
    color: @primary;
  }
}

```



## (2) mixin.less (混合样式)

```mixin.less
// 提供各种混合样式

.self-center(@pos: absolute) {
  position: @pos;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.self-fill(@pos: absolute) {
  position: @pos;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

```

## (3) var.less (颜色样式) 

```var.less
// less变量
@danger: #cc3600; // 危险、错误
@primary: #6b9eee; // 主色调、链接
@words: #373737; // 大部分文字、深色文字
@lightWords: #999; // 少部分文字、浅色文字
@warn: #dc6a12; // 警告
@success: #7ebf50; // 成功
@gray: #b4b8bc; // 灰色
@dark: #202020; // 深色

```

# 8.utils (弹窗组件)

## （1）Index.js

```index.js
export  {default as showMessage} from "./showMessage";
export  {default as getComponentRootDom} from "./getComponentRootDom";
```



## （2）getComponentRootDom.js （获取组件根元素）

```getComponentRootDom.js
import Vue from "vue";
/**
 * @export getComponentRootDom 该函数返回对应组件渲染的根Dom元素
 * @param {*} comb 传入组件
 * @param {*} props 传入组件属性 
 */
export default function getComponentRootDom(comp,props){
    const vm = new Vue({
      render : (h) => h(comp,{props})
    });
    vm.$mount();
    return vm.$el;
  }
```



## （3）showMessage.js （弹窗函数）

```showMessage.js
import getComponentRootDom from "./getComponentRootDom";
import styles from "./showMessage.module.less";
import Icon from "@/components/Icon";
/** 
 * @export showMessage 弹出消息，显示在容器中央
 * @param {String} content 弹出消息的内容
 * @param {String} type    消息类型：info error success warn
 * @param {Number} duration 消息显示的时间
 * @param {HTMLElement} container 消息所显示的容器；如果不传则显示到页面中央
 * @param {Function} callback 弹出消息完成后所需的后续操作
 */
export default function(options = {}){
    const content = options.content || "";
    const type = options.type|| "info";
    const duration = options.duration || 2000;
    const container = options.container || document.body
    // 获取Icon组件的根元素
    const IconDom = getComponentRootDom(Icon,{type})
    // 创建消息元素
    const div = document.createElement('div');
    div.innerHTML = `<span class="${styles.icon}">${IconDom.outerHTML}</span><div>${content}</div>`;
    // 设置样式
    
    div.className = `${styles.message} ${styles[`message-${type}`]}`;
    // 将div加入到容器中
    if(getComputedStyle(container).position === "static"){
        container.style.position ="relative";
    }
    container.appendChild(div);
    // 浏览器强行渲染
    div.clientHeight;
    // 回归到正常位置
    div.style.opacity = 1;
    div.style.transform = "translate(-50%,-50%) "
    // 等一段时间消失
    setTimeout(() => {
        div.style.opacity = 0;
        div.style.transform = "translate(-50%,-50%) translateY(25px) " 
        div.addEventListener("transitionend",() => {
            div.remove();
            // 运行回调函数
            options.callback && options.callback()
        },{once:true})
    },duration)
}
```

## （4）showMessage.module.less

```showMessage.module.less
@import "~@/styles/mixin.less";
@import "~@/styles/var.less";
.message{
    .self-center(absolute);
    z-index: 999;
    box-shadow:-2px 2px 5px rgba(0,0,0,0.5) ;
    border-radius: 5px;
    padding: 15px 30px;
    line-height: 2;
    display: flex;
    align-items: center;
    color: #fff;
    transform: translate(-50%,-50%) translateY(15PX);
    opacity: 0;
    transition: 0.4s;
    white-space: nowrap;
    &-info{
        background: @primary;
    }
    &-error{
        background: @warn;
    }
    &-success{
        background: @success;
    }
    &-danger{
        background: @danger;
    }
}
.icon{
    font-size: 20px;
    margin-right: 7px;
}
```

# 9.views (内容页组件)

## (1)About (关于我页面)

## (2) Blog （文章页）

## (3) Home （首页）

```index.js
<template>
  <div ref="container" class="home-container">
    <h1>{{ banners.length }}</h1>
    <ul>
      <li v-for="item in banners" :key="item.id">
        <img :src="item.midImg" alt="">
        <h2>{{ item.title }}</h2>
        <h3>{{ item.description }}</h3>
      </li>
    </ul>
  </div>
</template>

<script>
import {getBanner} from "@/api/banner";

export default {
    data(){
        return {
            banners : [],
        }
    },
    async created(){
      this.banners = await getBanner();
    }
}
</script>

<style>

</style>
```

## (4) Message （评论页）

## (5) Project （项目页）