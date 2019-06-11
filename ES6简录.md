# ES6简录

## 1. Babel 转码器

### 1.1 介绍

 	babel 是ES6的一个转码器,可以将ES6转为ES5,从而使代码可以在浏览器或者其他环境运行

### 1.2 使用

1. 在项目根目录下新建  .babelrc 文件

2.  .babelrc 文件内容基本格式

   ```javascript
   {
     "presets":[],
     "plugins":[]
   }
   ```

   presets:设定转码规则,可安装如下插件

   ```javascript
   //最新转码规则
   $ npm install --save-dev babel-preset-latest
   //react转码规则
   $ npm install --save-dev babel-preset-react
   //选装一个
   $ npm install --save-dev babel-preset-stage-0
   $ npm install --save-dev babel-preset-stage-1
   $ npm install --save-dev babel-preset-stage-2
   $ npm install --save-dev babel-preset-stage-3
   ```

   加入 .babelrc

   ```javascript
   {
     "presets":[
       "latest",
       "react",
       "stage-2"
     ],
     "plugins":[]
   }
   ```

3. 命令行转码 babel-cli

   安装 babel-cli 在项目里

   ```javascript
   $ npm install --save-dev babel-cli
   ```

   修改 package.json 文件

   ```javascript
   {
     //...
     "devDependencies":{
       "babel-cli": "^6.0.0"
     },
     "scripts":{
   	"build": "babel src -d lib"
     }
   }
   ```

   转码的时候执行以下命令

   ```javascript
   $ npm run build
   ```

4.  babel-node

   这是随babel-cli一起安装的工具,可以提供一个支持ES6的REPL环境,可以直接运行ES6代码

   用法,修改 packge.json 文件:

   ```javascript
   {
     "scripts": {
       "script-name": "babel-node script.js"
     }
   }

   ```

5. 其他工具

   babel-register\babel-core\babel-polyfill

### 1.3 与其他工具的配合

与ESLink 配合使用

```
$ npm install --save-dev eslint babel-eslint
```

然后在根目录下添加一个配置文件  .eslintrc  ,在其中加入:

```javascript
{
  "parser":"babel-eslint",
    "rules":{
      ...
    }
}
```

接着在package.json 加入

```javascript
{
  "name":"my-module",
   "script":{
     "lint":"eslint my-files.js"
   },
   "devDependencies":{
     "babel-eslint":"...",
     "eslint":"..."
   }
}
```

