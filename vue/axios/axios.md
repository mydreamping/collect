axios拦截器

目的：统一处理所有http请求和响应

如果是封装一个axios请求函数的话，我们首先是要引入依赖

import axios from ‘axios’;

import qs from 'qs';

axios.defaults.timeout = 10000; .//响应时间

axios.defaultts.baseURL = 'xxx'; //设置为统一的域名

请求拦截：

```
axios.interceptors.request.use(
  config => {
  	//请求前先执行这部分的内容，一般都在这里加上统一的请求头或者统一加上token
    if (store.state.token) { // 判断是否存在token，如果存在的话，则每个http header都加上token
      config.headers.Authorization = `token ${store.state.token}`;
    }
    return config;
  },
  err => {
    return Promise.reject(err);
  });
```



响应拦截：一般用于处理登录过期，网络报错的统一处理

```
axios.interceptors.response.use(
    response => {
        Vue.$vux.loading.hide();
        if (response.data.code == 0) {
            return response;
        } else if (response.data.code == -1000) {
            Vue.$vux.toast.show({
                text: "登录过期，请重新登录",
                type: 'text',
                onHide() {
                	//清除完全session
                    sessionStorage.clear()
                    下面是两种跳转方式
                    // window.location.href = "/"
                    //需要将router文件引入才能这样跳转使用
                    router.replace({
                        path: 'index',
                        query: {status:true}
                    })
                }
            });
        } else {

            return response;
        }
    },
    error => {
    	//一般用于网络出错处理，这里我使用的是vux的弹框，可根据引入的UI组件不同而设置不同的弹框
        console.log(error)
        if (error.message.includes('timeout')) {
            Vue.$vux.toast.show({
                text: "网络出错，请刷新重试",
                type: "text"
            });
            return Promise.reject(error);
        }
        return Promise.reject(error)
    }
);
```

