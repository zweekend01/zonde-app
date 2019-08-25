# znode-app

基于 NodeJS 实现的博客网站

## 1. 项目的功能

此项目是对 CNode 社区的模拟，能实现浏览文章、登录、评论、发布文章等基本功能，后端的 api 均自行设计

## 2. Client Side Render

### 2.1 Client Side

#### 2.1.1 技术选型

前端技术选型包括：

- webpack
- typescript
- react, react-router4, mobx, mobx-react, antd
- axios

#### 2.1.2 工程架构

> (1) editorconfig

editorconfig 可以对空白行、缩进等编码格式进行格式化，这种格式化与编程语言无关，有助于团队协作，配置步骤如下：

- IDE 安装 editorconfig 插件（默认下载 editorconfig npm package），该插件会在工作目录调用 editorconfig npm package
- 在工程目录下，添加 .editorconfig 文件，对全局的 editorconfig 进行相关配置

```conf
  root = true

  [*]
  charset = utf-8
  indent_style = space
  indent_size = 2
  end_of_line = lf
  insert_final_newline= true
  trim_trailing_whitespace = true
```

> (2) tslint

tslint 是 typescript 编程格式的校验工具，有助于团队的编程格式统一，配置步骤如下：

- IDE 安装 tslint 插件，该插件会在工作目录调用 tslint 相关的 npm package
- 安装相关的 npm package：

```cmd
  $ npm i -D tslint tslint-react typescript
```

- 在工程目录下，添加 tslint.json 文件，对全局的 tslint 进行相关配置，在 client/ 目录下添加 tslint.json 文件，对 client/ 目录的 tslint 进行相关配置

```json
{
  "rules": {
    "arrow-return-shorthand": true,
    "callable-types": true,
    "class-name": true,
    "comment-format": [
      true,
      "check-space"
    ],
    "curly": true,
    "deprecation": {
      "severity": "warn"
    },
    "eofline": true,
    "forin": true,
    "import-blacklist": [
      true,
      "rxjs/Rx"
    ],
    "import-spacing": true,
    "indent": [
      true,
      "spaces"
    ],
    "interface-over-type-literal": true,
    "label-position": true,
    "max-line-length": [
      true,
      140
    ],
    "member-access": false,
    "member-ordering": [
      true,
      {
        "order": [
          "static-field",
          "instance-field",
          "static-method",
          "instance-method"
        ]
      }
    ],
    "no-arg": true,
    "no-bitwise": true,
    "no-console": [
      true,
      "debug",
      "info",
      "time",
      "timeEnd",
      "trace"
    ],
    "no-construct": true,
    "no-debugger": true,
    "no-duplicate-super": true,
    "no-empty": false,
    "no-empty-interface": true,
    "no-eval": true,
    "no-inferrable-types": [
      true,
      "ignore-params"
    ],
    "no-misused-new": true,
    "no-non-null-assertion": true,
    "no-redundant-jsdoc": true,
    "no-shadowed-variable": true,
    "no-string-literal": false,
    "no-string-throw": true,
    "no-switch-case-fall-through": true,
    "no-trailing-whitespace": true,
    "no-unnecessary-initializer": true,
    "no-unused-expression": true,
    "no-use-before-declare": true,
    "no-var-keyword": true,
    "object-literal-sort-keys": false,
    "one-line": [
      true,
      "check-open-brace",
      "check-catch",
      "check-else",
      "check-whitespace"
    ],
    "prefer-const": true,
    "quotemark": [
      true,
      "single"
    ],
    "radix": true,
    "semicolon": [
      true,
      "always"
    ],
    "triple-equals": [
      true,
      "allow-null-check"
    ],
    "typedef-whitespace": [
      true,
      {
        "call-signature": "nospace",
        "index-signature": "nospace",
        "parameter": "nospace",
        "property-declaration": "nospace",
        "variable-declaration": "nospace"
      }
    ],
    "unified-signatures": true,
    "variable-name": false,
    "whitespace": [
      true,
      "check-branch",
      "check-decl",
      "check-operator",
      "check-separator",
      "check-type"
    ],
    "no-output-on-prefix": true,
    "use-input-property-decorator": true,
    "use-output-property-decorator": true,
    "use-host-property-decorator": true,
    "no-input-rename": true,
    "no-output-rename": true,
    "use-life-cycle-interface": true,
    "use-pipe-transform-interface": true,
    "component-class-suffix": true,
    "directive-class-suffix": true
  }
}
```

```json
{
  "extends": ["../tslint.json", "tslint-react"],
  "rules": {
    "quotemark": false
  }
}
```

> (3) git

避免团队成员上传不符合规范的代码，配置步骤如下：

- 安装相关 npm package

```cmd
  $ npm i -D husky
```

- 在 package.json 文件中，添加 npm script 和 husky.hooks 如下，在代码提交时用 eslint 检测代码规范：

```json
{
  "scripts": {
    "lint": "tslint --ext .ts --ext .tsx client/"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint"
    }
  }
}
```

- 在工程目录下添加 .gitignore 文件

> (4) webpack

开发环境和生产环境有部分配置重叠，配置步骤如下:

- 安装相关 npm package：

```cmd
  $ npm i -D \
    tslint-loader \
    ts-loader \
    raw-loader \
    url-loader \
    style-loader \
    typings-for-css-modules-loader \
    postcss-loader \
    postcss-import \
    postcss-preset-env \
    cssnano \
    autoprefixer \
    postcss-flexbugs-fixes \
    less-loader \
    less \
    html-webpack-plugin \
    webpack-dev-server \
    webpack \
    webpack-cli \
    cross-env \
    rimraf

  $ npm i -S react-hot-loader @types/react-hot-loader
```

- 配置 webpack.config.client.js、webpack.config.index.js、tsconfig.json、postcss.config.js 文件

```javascript
// build/webpack.config.client.js => 开发环境和生产环境共用的配置
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'production',
  resolve: {
    extensions: ['.ts', '.tsx'],
    alias: {
      '@': path.join(__dirname, '../client')
    }
  },
  entry: {
    index: path.join(__dirname, 'client/index.tsx')
  },
  output: {
    path: path.join(__dirname, '../public'),
    publicPath: '/public/',
    filename: '[name].[hash].js'
  },
  module: {
    rules: [
      {
        enforce: 'pre',
        test: /\.(ts|tsx)$/,
        use: 'tslint-loader',
        exclude: [path.join(__dirname, '../node_modules')]
      },
      {
        test: /\.(ts|tsx)$/,
        use: 'ts-loader',
        exclude: [path.join(__dirname, '../node_modules')]
      },
      {
        test: /\.txt$/,
        use: 'raw-loader'
      },
      {
        test: /\.(png|jpg|jpeg|gif|svg|eot|ttf|woff|woff2)$/,
        use: [{ loader: 'url-loader', options: { limie: 8192 } }]
      },
      {
        test: /\.less$/,
        use: [
          'style-loader',
          {
            loader: 'typings-for-css-modules-loader',
            options: {
              modules: true,
              namedExport: true,
              camelCase: true,
              minimize: true,
              localIdentName: '[local]_[hash:base64:5]'
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss'
            }
          },
          {
            loader: 'less-loader',
            options: {
              javascriptEnabled: true
            }
          }
        ],
        exclude: [path.join(__dirname, '../node_modules')]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      minify: { removeAttributeQuotes: true },
      template: path.join(__dirname, '../client/index.html'),
      filename: 'index.html'
    })
  ]
};
```

```javascript
// build/webpack.config.index.js => 开发环境和生产环境不同的配置
const path = require('path');
const webpack = require('webpack');
const clientConfig = require('./webpack.config.client');

const isDev = process.env.NODE_ENV === 'development';

if (isDev) {
  clientConfig.mode = 'development';
  clientConfig.plugins.push(new webpack.HotModuleReplacementPlugin());
  clientConfig.devtool = '#cheap-module-eval-source-map';
  clientConfig.devServer = {
    host: '0.0.0.0',
    port: '8888',
    contentBase: path.join(__dirname, '../public'),
    publicPath: '/public',
    historyApiFallback: { index: '/public/index.html' },
    overlay: { errors: true },
    hot: true
  };
} else {
}

module.exports = clientConfig;
```

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["dom", "esnext"],
    "jsx": "react",
    "sourceMap": true,
    "strict": true
  },
  "include": ["./client/**/*"]
}
```

```javascript
// .postcss.config.js
module.exports = {
  plugins: {
    'postcss-import': {},
    'postcss-preset-env': {},
    cssnano: {},
    autoprefixer: {
      browsers: [
        '>1%',
        'last 4 versions',
        'Firefox ESR',
        'not ie < 9' // React doesn't support IE8 anyway
      ],
      flexbox: 'no-2009'
    },
    'postcss-flexbugs-fixes': {}
  }
};
```

- 在 package.json 文件中添加 npm scripts

```json
{
  "dev:build": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.index.js",
  "clear": "rimraf public",
  "pro:build": "npm run clear && cross-env NODE_ENV=production webpack --config build/webpack.config.index.js"
}
```

#### 2.1.3 项目架构

```cmd
  - client
    - assets        => 静态资源
    - components    => 项目的通用组件，包括布局组件和高阶组件等
      - hoc
      - layout
    - config        => 项目的一些配置
    - pages         => 页面组件
    - services      => http请求
    - stores        => 状态管理
    - styles        => 项目通用的样式，以及全局的样式兼容性设置
    - utils         => 工具库
    - App.less
    - App.tsx
    - app.d.ts
    - index.html
    - index.tsx
    - Router.tsx
    - tslint.json
```

#### 2.1.4 业务开发

- 安装相关 npm packages:

```cmd
  $ npm i -S
    react \
    @types/react \
    react-dom \
    @types/react-dom \
    react-css-modules \
    @types/react-css-modules \
    antd \
    ts-import-plugin \
    axios \
    react-router-dom \
    @types/react-router-dom \
    mobx \
    mobx-react \
    await-to-js \
    joi-browser
```

> (1) 配置全局的样式

解决 html 元素在各浏览器中兼容性的问题、实现基于 rem 的弹性布局

> (2) 解决 antd 组件库按需加载以及样式打包的问题

配置 webpack.config.client.js 如下：

```javascript
const tsImportPluginFactory = require('ts-import-plugin');

module.exports = {
  modules: {
    rules: [
      // 解决按需加载
      {
        test: /\.(tsx|ts)$/,
        loader: 'ts-loader',
        options: {
          transpileOnly: true,
          getCustomTransformers: () => ({
            before: [ tsImportPluginFactory( /** options */) ]
          }),
          compilerOptions: {
            module: 'es2015'
          }
        },
        exclude: /node_modules/
      },
      // 解决样式冲突
      {
        test: /\.css$/,
        use: [
          'style-loader',
          {
            loader: 'typings-for-css-modules-loader',
            options: {
              importLoaders: 1
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss'
            }
          }
        ],
        include: /node_modules/
      }
    ]
  }
};
```

> (3) 基于 axios 封装 http 请求

请求 loading 的开启与关闭、请求成功的统一提示、请求失败的统一提示

#### 2.1.5 性能优化

#### 2.1.6 安全防御

#### 2.1.7 单元测试

#### 2.1.8 打包上线

### 2.2 Server Side

#### 2.2.1 技术选型

后端技术选型包括：

- typescript
- express
- mysql

#### 2.2.2 工程架构

> (1) editconfig

> (2) eslint

> (3) git

> (4) nodemon

- 安装相关 npm package:

```cmd
  $ npm i nodemon -D
```

- 配置 nodemon.json 文件

```json
{
  "restartable": "rs",
  "ignore": [
    ".git",
    "node_modules/**/node_modules",
    "tslint.json",
    "build",
    "client",
    "public"
  ],
  "env": {
    "NODE_ENV": "development"
  },
  "verbose": true,
  "ext": "ts"
}
```

- 在 package.json 文件中添加 npm scripts

```json
{
  "scripts": {
    "dev:start": "cross-env NODE_ENV=development node server/bin/www.ts",
    "dev:monstart": "nodemon server/bin/www.ts",
    "pro:start": "cross-env NODE_ENV=production node server/bin/www.ts",
    "pro:monstart": "nodemon server/bin/www.ts"
  }
}
```

#### 2.2.3 项目架构

```cmd
  - server
    - api
    - apidoc
    - bin
      - www.js
    - config
    - controllers
    - middlewares
    - models
    - public
    - routers
    - sql
    - utils
    - views
    - app.js
```

#### 2.2.4 业务开发

#### 2.2.5 单元测试

#### 2.2.6 性能优化

#### 2.2.7 部署上线
