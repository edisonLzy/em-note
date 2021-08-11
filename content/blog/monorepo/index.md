---

     title: monorepo
     date: 2021-08-11T13:11:26.602Z

---

## lerna

> lerna 通常用于发包

- [x] 初始化 monorepo 项目 `lerna init`
- [x] 添加包 `lerna create [packagename]`
- [x] 安装依赖模块并建立 子包之间的引用关系 `learn bootstrap --npm-client yarn --use-workspaces`
- [x] 删除所有 nodule_module `lerna clean`

## yarn

> 管理依赖

- [x] 查看当前项目含有哪些包 `yarn workspace info`
- [x] 添加依赖到 root `yarn add chalk --ingnore-workspace-root-check`

> 添加到 root 到依赖所有的包都可以使用，默认不允许向 root 添加依赖所以需要添加 `--ingnore-workspace-root-check`用来忽略根目录检查

- [x] 添加依赖到指定的包 `yarn workspace [packagename] add [依赖名称] `

- [x] `yarn install` 安装依赖模块并建立 子包之间的引用关系
- [x] 删除所有 nodule_module `yarn workspaces run clean`
- [x] 查看缓存目录`yarn cache dir`
- [x] 清除本地缓存`yarn cache clean`
