## 关于网络请求的一些总结

### 1. request的封装

- **登录获权**``window.localStorage.setItem("token", res.access_token);``拿到服务器返回的token存到本地 
- **请求验权**``window.localStorage.getItem('token');``获取本地存的token加到请求头
- **退出清权**``window.localStorage.removeItem("token");``退出时清除本地存的token加到请求头

```js
//导入axios
import axios from 'axios'
//导入router,用做登录跳转
import router from '../router'

//封装网络请求模块
export function request(config) {
    //创建axios实例
    const instance = axios.create({
        baseURL: 'https://api.shop.eduwork.cn',//api请求头
        timeout: 5000
    })

    //axios拦截器

    //请求拦截
    instance.interceptors.request.use(config => {
        //用后端规定的方式把token加到请求头上，每次请求的时候携带到服务器验证
        const token = window.localStorage.getItem('token')
        if (token) {
            config.headers.Authorization = 'Bearer' + token
        }
        return config //处理完后要把拦截下来的config返回出去
    }, err => {
        //请求拦截失败
    })

    //响应拦截
    instance.interceptors.response.use(res => {
        //一般在这里过滤，返回res.data,后面就直接拿到了data数据
        return res.data ? res.data : res

    }, err => {
        //需要授权才可以访问的接口，统一跳转到login页面去登录
        if (err.response.status == 401) {
            router.push('/login')
        }

        //拿到所有错误提示对象里的所有key的第一个key的第一个错误信息对用户进行轻提示
        //err.response.data.errors[Object.keys(err.response.data.errors)[0]][0]
    })

    //发送真正的网络请求，相当于返回出去了一个promise函数
    return instance(config)
}
```

### 2.业务模块网络请求统一封装(增删改查)

- ``query``请求参数用``params``接收，``{key:value}``转换为``?key=value``，``key``的``value``值为空不转换，不影响最终的请求结果；如果key是一个变量时，直接传变量过来，直接拼接api``?'+key+'=xxx'``即可。**查***一般用来获取各种列表数据*
- ``body``请求参数用``data``接收，必须是个对象，即使只需要一条数据时。**增改***一般用来提交form表单数据*
- ``reset``请求参数用``id``接收。**删改***一般为对应数据的id*

```js
//导入第一步封装的request
import { request } from './request'

//获取列表
export function getSwipe(params) {
    return request({
        url: '/api/admin/slides'
        params
    })
}
//新增
export function addSwipe(data) {
    return request({
        url: '/api/admin/slides',
        method: 'POST',
        data
    })
}
//修改
export function editSwipe(id, data) {
    return request({
        url: '/api/admin/slides/' + id,
        method: 'PUT',
        data
    })
}
//删除
export function deleteSwipe(id) {
    return request({
        url: '/api/admin/slides/' + id,
        method: 'DELETE',
    })
}
//排序
export function seqSwipe(id, data) {
    return request({
        url: '/api/admin/slides/' + id + '/seq',
        method: 'PATCH',
        data
    })
}
//禁用和启用
export function isSwipe(id) {
    return request({
        url: '/api/admin/slides/' + id + '/status',
        method: 'PATCH'
    })
}
```

### 3.导入到vue组件中使用

```js
import { getSwipe,editSwipe,deleteSwipe,seqSwipe,isSwipe } from "network/swipe";
```