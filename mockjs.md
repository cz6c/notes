
1. 安装

```js
cnpm i mockjs -S
```

2. src创建mock文件夹，再创建index.js

```js
import Mock from 'mockjs' // 引入mockjs


//配置请求延时
Mock.setup({
    timeout: 1000
})

//1.get请求模拟分页获取数据
//定义随机数据数组
const { List } = Mock.mock({
    'List|66': [{
        "id": "@increment()",
        "title": "@ctitle()",
        "content": "@cparagraph(5,15)",
        "img_url": "@image('100x100','#333','#333','png','cc')",
        "add_time": "@date(yyyy-MM-dd)"
    }]
})

//封装方法获取query请求参数
const getQuery = (url, name) => {
    const index = url.indexOf('?')
    if (index !== -1) {
        const queryStrArr = url.substr(index + 1).split('&')
        for (let i = 0; i < queryStrArr.length; i++) {
            const itemArr = queryStrArr[i].split('=')
            if (itemArr[0] === name) {
                return itemArr[1]
            }
        }
    }
    return null
}

//  模拟接口被参数拼接了所以这里写正则  /\/data\/index/
Mock.mock(/\/data\/index/, 'get', (options) => {
    //获取传递参数
    console.log(options);
    const pageindex = getQuery(options.url, 'pageindex')
    const pagesize = getQuery(options.url, 'pagesize')
        //截取数据的起始位置
    const start = (pageindex - 1) * pagesize
        //截取数据终点位置
    const end = pageindex * pagesize
        //计算总页数
    const totalPage = Math.ceil(List.length / pagesize)
        //对数据返回数据进行截取
    const newList = pageindex > totalPage ? [] : List.slice(start, end)
    return {
        status: 200,
        message: '获取成功',
        list: newList,
        total: List.length
    }
})

//2.post请求添加数据
Mock.mock('/add/index', 'post', (options) => {
    console.log(options);
    const body = JSON.parse(options.body)
    console.log(body);
    List.push(Mock.mock({
        "id": "@increment()",
        "title": body.username,
        "content": body.password,
        "img_url": "@image('100x100','#333','#333','png','cc')",
        "add_time": "@date(yyyy-MM-dd)"
    }))
    return {
        status: 200,
        message: '添加成功',
        list: List,
        total: List.length
    }
})
```

3. main.js引入

```js
require('@/mock')
```

4. 对应网络请求js中封装

```js
import { request } from '@/network/request'

export function getMock(params) {
    return request({
        url: '/data/index',
        params
    })
}
export function addMock(data) {
    return request({
        url: '/add/index',
        method: 'post',
        data
    })
}
```

5. 使用

```js
  created() {
    getMock({ pageindex: 6, pagesize: 10 }).then((res) => {
      console.log(res);
    });
  },
```