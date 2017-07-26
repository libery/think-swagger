# think-swagger

thinkjs 是一个nodejs的集成环境，通过简单的指令生成项目目录，集成了大量开箱即用的插件，适合nodejs环境下的快速开发，thinkjs3.0即将发布，本项目为thinkjs3.0提供swagger的支持，本项目包含三个子项目和一个示例：

| 名称 | 说明 |  
| --- | --- |  
| [think-swagger-parser](https://github.com/libery/think-swagger-parser) | swagger文档解析模块 |  
| [think-swagger-router](https://github.com/libery/think-swagger-router) | 路由模块 | 
| [think-swagger-controller](https://github.com/libery/think-swagger-controller) | 控制器模块 | 
| [think-swagger-demo](https://github.com/libery/think-swagger-demo) | 示例程序 | 

通过三个middleware模块让thinkjs支持swagger，基于swagger的yaml文件实现路由和接口实现的映射，可以快速开发RESTful的接口，要实现该功能，在新建thinkjs3.0项目后，需要修改src/config/middleware.js和config.js引入并启用以上3个middleware，并配置swagger文档路径和thinkjs controller路径：

1. 安装3个模块

```sh
npm i think-swagger-parser --save
npm i think-swagger-router --save
npm i think-swagger-controller --save
```
2. 修改 `src/config/middleware.js` 文件  

```js
const path = require('path');
const swaggerParser = require('think-swagger-parser');
const swaggerRouter = require('think-swagger-router');
const swaggerController = require('think-swagger-controller');
const isDev = think.env === 'development';

module.exports = [
  {
    handle: 'meta',
    options: {
      logRequest: isDev,
      sendResponseTime: isDev
    }
  },
  {
    handle: 'resource',
    enable: isDev,
    options: {
      root: path.join(think.ROOT_PATH, 'www'),
      publicPath: /^\/(static|favicon\.ico)/
    }
  },
  {
    handle: 'trace',
    enable: !think.isCli,
    options: {
      debug: isDev
    }
  },
  {
    handle: 'payload',
    options: {}
  },
  {
    handle: swaggerParser,
    options: {
      debug: isDev
    }
  },
  {
    handle: 'router',
    options: {}
  },
  {
    handle: swaggerRouter,
    options: {
      debug: isDev
    }
  },
  'logic',
  {
    handle: swaggerController,
    options: {
      debug: isDev
    }
  }
  // 'controller'
];
```
3. 修改 `src/config/config.js`

```js
module.exports = {
  api_doc: './api/swagger.yaml',
  controller_dir: './app/controller'
};
```

