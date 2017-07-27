# think-swagger

ThinkJS 是一个 Node.js 框架，自带脚手架通过简单的指令生成项目目录，集成了大量开箱即用的插件，适合 Node.js 环境下的快速开发。本项目为 ThinkJS 3.x 提供 swagger 的支持，本项目包含三个子项目和一个示例：

| 名称 | 说明 |  
| --- | --- |  
| [think-swagger-parser](https://github.com/libery/think-swagger-parser) | swagger文档解析模块 |  
| [think-swagger-router](https://github.com/libery/think-swagger-router) | 路由模块 | 
| [think-swagger-controller](https://github.com/libery/think-swagger-controller) | 控制器模块 | 
| [think-swagger-demo](https://github.com/libery/think-swagger-demo) | 示例程序 | 

通过三个 middleware 模块让 ThinkJS 支持 swagger，基于 swagger 的 yam l文件实现路由和接口实现的映射，可以快速开发 RESTful 的接口。

## 安装

要实现该功能，在新建 ThinkJS 3.x 项目后，需要修改`src/config/middleware.js`和`config.js`引入并启用以上3个 middleware，并配置 swagger 文档路径和 ThinkJS controller 路径：

1. 安装 3 个模块

    ```bash
    npm i think-swagger-parser think-swagger-router think-swagger-controller --save
    ```
    
2. 修改`src/config/middleware.js`文件  

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
    
3. 修改`src/config/config.js`文件：

    ```js
    module.exports = {
      api_doc: './api/swagger.yaml',
      controller_dir: './app/controller'
    };
    ```

