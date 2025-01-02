# vue2

## 目录结构

```
├── public                              // 静态资源
|   ├── favicon.ico                     // 标签页图标
|   ├── index.html                      // 入口html
|   └── libs                            // 无需编译插件目录
|       └── jquery@1.0.0
|           └── index.min.js
├── src                                 // 源码目录
|   ├── api                             // 接口
|   |    └── user.js                    // 与接口一致：/api/user/list
|   ├── assets                          // 静态资源
|   |    ├── css                        // 所有css、scss 文件
|   |    |   └── index.scss             // 样式入口文件
|   |    ├── fonts                      // 字体和iconfont 文件
|   |    └── images                     // 图片
|   ├── components                      // 组件
|   |   └──co-button                    // 按钮组件目录
|   |      ├── index.vue                // 按钮组件
|   |      └── index.md                 // 按钮组件说明文档
|   ├── layout                          // 布局
|   |   ├── index.vue                   // 布局组件
|   |   └── components                  // 布局子组件
|   |       ├── footer.vue              // 底部组件
|   |       └── header.vue              // 头部组件
|   ├── router                          // 路由目录
|   |   ├── index.js                    // 路由主文件
|   |   └── user.js                     // 用户路由
|   ├── store                           // 全局状态管理
|   |   ├── index.js                    // 全局状态管理主文件
|   |   └── modules                     // 全局状态管理子模块
|   |       └── user.js                 // 用户模块
|   ├── utils                           // 工具函数
|   |   └── axios.js
|   |   └── .js
|   ├── views                           // 页面
|   |   └── user                        // 与接口一致：/api/user/list
|   |       ├── list                    // 用户列表目录，路由: /user/list
|   |       |   ├── index.vue           // 用户列表页面  页面超300行时拆分
|   |       |   └── components          // 用户列表子组件
|   |       |       ├── user-list-header.vue
|   |       |       └── user-list-table.vue
|   |       ├── update                  // 用户编辑目录 路由：/user/update
|   |       |   └── index.vue           // 用户编辑页面
|   |       |   └── components          // 用户列表子组件
|   |       |       ├── user-update-header.vue
|   |       |       ├── user-update-form.vue
|   |       |       └── user-update-footer.vue
|   |       └── info                    // 用户详情目录 路由：/user/info
|   |           ├── index.vue           // 用户详情页面
|   |           └── components          // 用户列表子组件
|   |               ├── user-info-header.vue
|   |               ├── user-info-form.vue
|   |               └── user-info-footer.vue
|   ├── App.vue                         // 入口页面
|   └── main.js                         // 入口文件
└── README.md                           // 说明文档
```

## api 目录

> 方法名与接口一致

src/api/user.js

```
export const userList = ({ params, data, headers, id })=> {
    return axios({
        method: 'get',
        url: `/user/list/${id}`,
        params,
        data,
        headers
    })
}

// reset api
export const userDelete = ({ params, data, headers, id })=> {
    return axios.get({
        method: 'delete',
        url: `/user`,
        params,
        data,
        headers
    })
}
```

## assets/css/index.scss 目录

> index.scss 包含所有 css、scss 文件

```
@import "~normalize.css/normalize.css";  // 引入 node_modules下的css文件
@import "../fonts/iconfont/index.css";
@import "var.scss"
```

## router 目录

> router/user.js

```
import Layout from '@/layout/index.vue'
export default [
  {
    path: '/user',
    component: Layout,
    children: [
      {
        path: 'list',
        name: 'userList',
        component: () => import(/* webpackChunkName: "userList" */ '../views/user/list/index.vue'),
        meta: {
          title: '用户列表'
        }
      }
    ]
  }
]

```

> router/index.js

```
import user from './user.js'
const router = new VueRouter({
  routes: [...user]
})

```

## store 目录

> store/modules/user.js
```
const state = {
  token: '',
}
const getters = {
  token: (state) => state.token,
}
const mutations = {
  SET_TOKEN: (state, token) => {
    state.token = token
  },
}
const actions = {
  setToken({ commit }, token) {
    setToken(token)
    commit('SET_TOKEN', token)
  },
}

export default {
  namespaced: true,
  state,
  mutations,
  actions,
  getters
}
```

> store/index.js

```
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)
const store = new Vuex.Store({
  modules: {
    user
  }
})

export default store

```

## views/user/list/index.vue
 
> 每个页面有唯一的顶层class名如：user-list  
> 命名规则：与vue文件名一致   
> 其余class 都在顶层class下  
> class遵循[BEM命名](../css/BEM命名.md)
```
.user-list{
    /** css 遵循 BEM */
    .card{
        &-header{}
        &-main{
            &__title{
              &--red{}
            }
            &__content{}
        }
        &-footer{}
    }
}
```

> 组件引入的变量名首字母大写驼峰命名  
> 组件注册和使用使用都以驼峰方式使用
```

```
import UserListHeader from './components/user-list-header'
components: {
    UserListHeader
}
```
<template>
  <div class="user-list">
    <UserListHeader  />
    <div class="card">
        <div class="card-header"></div>
        <div class="card-main">
            <div class="card-main__title card-main__title--red"></div>
            <div class="card-main__content">{{userToken}}</div>
        </div>
        <div class="card-footer"></div>
    </div
  </div>
</template>
<script>
import { mapGetters } from 'vuex'
import UserListHeader from './components/user-list-header'
import {userList } from '@/api/user.js'
export default {
    name: 'UserList',
    components: {
        UserListHeader
    },
     computed: {
      ...mapGetters({
        userToken: 'user/token'
      })
    },
    data(){
        return {
            userParams:{
                pageNum: 1,
                pageSize: 10,
                keyword: ''
            },
            userData: {
                list:[],
                total: 0
            }
        }
    },
    created(){
        this.userListService()
    },
    methods:{
        async userListService(){
          try {
              const data = await userList(this.userParams)
          } catch (error) {
          }
        },
        setUserToken(token){
          this.$store.dispatch('user/setToken', token)
        }
    }
}
</script>
<style lang="scss" scoped>
.user-list{
    /** css 遵循 BEM */
    .card{
        &-header{}
        &-main{
            &__title{
              &--red{}
            }
            &__content{}
        }
        &-footer{}
    }
}
</style>

```

## 代码行数要求

```
代码行数推荐300行以下，最好不超过500行，禁止超过1000行。
```
