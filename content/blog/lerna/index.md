---

     title: lerna
     date: 2021-08-29T12:43:54.816Z
     description: lerna

---

## 基础命令

- lerna link : 只做软连接 ,bootstrap 会重新安装依赖

1. 添加依赖到 package.json 中
2. 执行 lerna link

- lerna create [pkg] --registry http://xxx : 设置 pkg 包 将要发布 到 指定的 registry(publicConfig.registry)

- lerna run xx: 执行所有 pkg 的 xx 命令(npm 命令)

```shell
lerna run test
```

- lerna exec -- xx: 在所有 pkg 的 执行 xx shell 命令

```shell
# 在特定包下面执行 jest命令
lerna exec --scope lerna -- jest
```

- lerna add pkg: 在所有 pkg 中安装指定的包

```shell
# 安装 yargs 到 cli
lerna add yargs packages/cli
```

- lerna diff : 查看变更的文件

## 调试源码

- 入口文件: lerna/cli.js
- 使用 vscode 进行调试: vscode/launch.json

1. args 设置命令运行的参数
2. program 设置运行的程序

## yargs

- https://www.npmjs.com/package/yargs
- command 的格式

1. positional 表示命令中占位的参数
2. options 表示 命令 即 --

```js
module.exports = {
  command: "init <name>",
  describe: "",
  builder(yargs) {
    yargs
      // init name
      .positional("name", {
        type: "string",
        describe: "pkg name",
      })
      // init name --registry='xxx'
      .options({
        registry: {
          group: "", //分组
          describe: "",
          type: "string",
        },
      });
  },
  handler(args) {
    // 命令的执行逻辑
    const { name, registry } = args;
  },
};
```

## GET

- git clone xxx --depth = 1 : 只下载第一层
- 零配置 npm 私服: https://www.npmjs.com/package/verdaccio

```shell
# 执行命令得到私服的地址
verdaccio
```

- npm set-script: 通过 npm 添加 script 脚本

```shell
# 在当前 package.json 的script中 添加一个脚本 prepare 对应的脚本为 husky install
npm set-script prepare 'husky install'
```

- jest 配置

```js
decribe("x", () => {
  it("y", () => {});
});
```

```js
module.exports = {
  testMatch: ["**/__test__/**"], // 测试文件目录
};
```

- eslint 约束代码质量，prettier 对代码风格进行约束
- fs.writeJson: 可直接写入对象到 json 文件

```js
const fs = require("fs-extra");
fs.writeJson(
  path,
  "package.json",
  {
    name: "root",
    private: true,
    // package.json文件内容
  },
  {
    spaces: 2, // 添加两个缩进
  }
);
```

- fs.mkdirp(path,目录名称): 是 可以递归创建目录

```js
// 可递归创建目录 会先创建 src目录再创建 init目录
fs.mkdirp(path.resolve(__dirname, "src/init"));
```

- 利用 Promise.resolve 替代 new Promise

```js
const read = Promise.resolve();
function delay() {
  return new Promise(function (resolve, reject) {
    setTimeout(() => {
      resolve();
    }, 2000);
  });
}
read.then(async (val) => {
  const data = await delay();
  console.log(data);
});
```

- pify: 将 callback style 变为 promise 风格

1. https://www.npmjs.com/package/pify

- init-package-json: 使用交互式的方式初始化 package.json 文件

1. https://www.npmjs.com/package/init-package-json

```js
const fs = require("fs-extra");
const initPkgJson = require("pify")(require("init-package-json"));
const path = require("path");
const initFile = path.resolve(process.env.HOME, ".npm-init");
fs.mkdirpSync(path.resolve(__dirname, "packages/lib"));
initPkgJson(path.resolve(__dirname, "packages/lib"), initFile);
```

- yarn link & npm link 都可以将 pkg 添加到全局
- resolve-cwd: 根据当前 cwd 解析 指定的目录

1. https://www.npmjs.com/package/resolve-cwd

```js
const resolveCwd = require("resolve-cwd");

console.log(__dirname);
//=> '/Users/sindresorhus/rainbow'

console.log(process.cwd());
//=> '/Users/sindresorhus/unicorn'

console.log(resolveCwd("./foo"));
//=> '/Users/sindresorhus/unicorn/foo.js'
```

## TODO

- eslint 插件
- import-local

1. https://www.npmjs.com/package/import-local
