---
description: https://beta-pro.ant.design/docs/request-cn
---

# 网络请求

基础使用

```jsx
import { useRequest } from 'umi';
export default () => {
  const { data, error, loading } = useRequest(() => {
    return services.getUserList('/api/test');
  });
  if (loading) {
    return <div>loading...</div>;
  }
  if (error) {
    return <div>{error.message}</div>;
  }
  return <div>{data.name}</div>;
};
```

{% hint style="warning" %}
问题： 若返回数据不包含data 怎么办？
{% endhint %}

拦截器使用

包括请求拦截器和相应拦截器

```jsx
const authHeaderInterceptor = (url: string, options: RequestOptionsInit) => {
  const authHeader = {
    Cookie: 'xxx',
    ...options.headers,// 注意说明
  };
  return {
    url: `${url}`,
    options: { options, interceptors: true, headers: authHeader },
  };
};
const responseInterceptor = async (response: Response, options: RequestOptionsInit) => {
  const data = await response.json();
  if (Object.keys(data).length > 0 && data.errno === undefined) {
    return { data: { ...data }, errno: 0, errmsg: 'OK' };
  }
  if (data.errno !== undefined && data.data === undefined) {
    return { errno: data.errno, errmsg: data.errmsg, data: { ...data } };
  }
  return data;
};

export const request: RequestConfig = {
  errorConfig: {
    adaptor: (resData) => ({
      ...resData,
      success: resData.errno === 0,
      errorMessage: resData.errmsg,
    }),
  },
  // 统一错误处理
  errorHandler: (error) => {
    message.error(error.message);
  }, 
  requestInterceptors: [authHeaderInterceptor],
  responseInterceptors: [responseInterceptor],
};

```

{% hint style="warning" %}
如何在一个项目中使用多个interceptor?
{% endhint %}

```jsx
#  使用 extend 并设置,{global:false}
import { extend } from 'umi';
const nRequest = extend({
  prefix: '/api/v1',
  timeout: 1000,
  headers: {
    'Content-Type': 'multipart/form-data',
  },
});
nRequest.interceptors.request.use(
  (url, options) => {
    return {
      url: `${url}&interceptors=yes`,
      options: { ...options, interceptors: true },
    };
  },
  { global: false }
); // second paramet defaults { global: true }
```

参考文档

umi-request - [https://github.com/umijs/umi-request](https://github.com/umijs/umi-request)

