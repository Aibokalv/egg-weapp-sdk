# egg-weapp-sdk

[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![David deps][david-image]][david-url]
[![Known Vulnerabilities][snyk-image]][snyk-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/egg-weapp-sdk.svg?style=flat-square
[npm-url]: https://npmjs.org/package/egg-weapp-sdk
[travis-image]: https://img.shields.io/travis/eggjs/egg-weapp-sdk.svg?style=flat-square
[travis-url]: https://travis-ci.org/eggjs/egg-weapp-sdk
[codecov-image]: https://img.shields.io/codecov/c/github/eggjs/egg-weapp-sdk.svg?style=flat-square
[codecov-url]: https://codecov.io/github/eggjs/egg-weapp-sdk?branch=master
[david-image]: https://img.shields.io/david/eggjs/egg-weapp-sdk.svg?style=flat-square
[david-url]: https://david-dm.org/eggjs/egg-weapp-sdk
[snyk-image]: https://snyk.io/test/npm/egg-weapp-sdk/badge.svg?style=flat-square
[snyk-url]: https://snyk.io/test/npm/egg-weapp-sdk
[download-image]: https://img.shields.io/npm/dm/egg-weapp-sdk.svg?style=flat-square
[download-url]: https://npmjs.org/package/egg-weapp-sdk

<!--
Description here.
-->

## 依赖说明

- egg-redis

- egg-session-redis

### 依赖的 egg 版本

egg-weapp-sdk 版本 | egg 1.x
--- | ---
1.x | 😁
0.x | ❌

### 依赖的插件
<!--

如果有依赖其它插件，请在这里特别说明。如

- security
- multipart

-->

## 开启插件

```js
// config/plugin.js
exports.redis = {
  enable: true,
  package: 'egg-redis',
};

exports.sessionRedis = {
  enable: true,
  package: 'egg-session-redis',
};

exports.weappSDK = {
  enable: true,
  package: 'egg-weapp-sdk',
};

```


## 使用场景

- Why and What: 独立管理微信小程序用户会话，校验身份。利用redis储存会话。

- How: 具体的示例代码:

含两种方法:

1. 登陆:  loginService.login()

2. 校验用户:  loginService.check()

```js
// app/controller/weapp.js
module.exports = app => {
  class WeappController extends app.Controller {
    * login() {
      const { ctx, app } = this;
      const loginService = app.weapp.LoginService.create(ctx.request, ctx.response);
      yield loginService.login()
        .then(data => {
          ctx.body = data;
        });
    }

    * user() {
      const { ctx, app } = this;
      const loginService = app.weapp.LoginService.create(ctx.request, ctx.response);
      yield loginService.check()
        .then(data => {
          ctx.body = {
            code: 0,
            message: 'ok',
            data: {
              userInfo: data.userInfo,
            },
          };
        });
    }
  }
  return WeappController;
};
```

## 详细配置

请到 [config/config.default.js](config/config.default.js) 查看详细配置项说明。
```js
// {app_root}/config/config.default.js

module.exports = appInfo => {
  const config = {};

  config.redis = {
    client: {
      host: '127.0.0.1',
      port: '6379',
      password: '',
      db: '0',
    },
  };

  config.sessionRedis = {
    name: '', // single redis does not need to config name
  };

  config.weappSDK = {
    appId: 'wx8ce5cc812ef169a9',
    appSecret: '352a0d3e58f4549cceb7e5b8f5a17847',
  };

  return config;
};

```

## 单元测试

<!-- 描述如何在单元测试中使用此插件，例如 schedule 如何触发。无则省略。-->

## 提问交流

请到 [egg issues](https://github.com/eggjs/egg/issues) 异步交流。

## License

[MIT](LICENSE)
