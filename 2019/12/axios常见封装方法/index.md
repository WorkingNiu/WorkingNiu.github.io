# Axios常见封装方法


参考了几个开源项目，分享一套比较常见的axios二次封装使用方法。

源码参考 [本人Github](https://github.com/WorkingNiu/vueskeleton)。

示例中使用了restful风格的API，话不多说直接上代码，如有错误欢迎指正。

<!--more-->
## request.js

封装axios的工具类，可以放在utils包中，包括前置钩子、后置钩子。这里的baseURL是服务器地址，如http://localhost:8080。

{{< highlight javascript  >}}
import axios from 'axios'

// 创建axios实例
const service = axios.create({
  baseURL: process.env.BASE_API, // api的base_url
  timeout: 15000 // 请求超时时间
})

// request拦截器
service.interceptors.request.use(config => {
  /**
   * 写请求验证，一般在这里写入token,如下可以带上名为XXX的xxxxxxxxxxx...token字符串
   * config.headers['xxx'] = xxxxxxxxxxxxxxxxxxxxxxxxxxxx
   */
  return config
}, error => {

})

// respone拦截器
service.interceptors.response.use(
  response => {
    const res = response.data
    /**
     * code不是200则是请求失败
     */
    if (response.status !== 200) {
      /**
       * 写后端定义的错误码对应需要触发的逻辑。。。
       * 例如报500.。。弹个服务器错误提示框什么的
       */
      return Promise.reject('error')
    } else {
      //正常时返回，这里我只要响应数据
      return response.data
    }
  },
  error => {
    return Promise.reject(error)
  }
)

export default service

{{< / highlight >}}

## test.js{#test}

实际生产中一般对应一组业务请求，例如商品的增删改查，所有请求放在一个js文件里方便管理。

{{< highlight javascript >}}
import request from '@/utils/request'
//restful API示例
const url = "/test"

//查询全部数据
export function findall() {
  return request({
    url: url,
    method: 'get',
  })
}

//根据ID查询数据
export function findByid(id) {
  return request({
    url: url,
    method: 'get',
    params: {
      id
    },
  })
}

//新增数据
export function insert(data) {
  return request({
    url: url,
    method: 'post',
    data: data
  })
}

//修改数据
export function update(id,data) {
  return request({
    url: url+`/${id}`,
    method: 'put',
    data: data
  })
}

//删除数据
export function deleteByid(id) {
  return request({
    url: url+`/${id}`,
    method: 'delete',
  })
}
{{< / highlight >}}

## login.vue

实际使用:按需要引入上面的[test.js](#test)文件中的方法,如下演示了使用update方法。

{{< highlight vue >}}

<template>
  <div>
    login页
  </div>
</template>
<script>
    import {findall,findByid,insert,update} from '../../api/test';
    export default {
        data() {
            return {
                student: {
                    id: 1,
                    name: 'lzq',
                    money: 999.999
                }
            };
        },
        mounted() {
            update(this.student.id,this.student).then(Response => {
                console.log(Response)
            }).catch(err=>{
                console.log(err)
            })
        },
        methods: {
        }
    };
</script>

{{< / highlight >}}
