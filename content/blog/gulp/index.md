---

     title: gulp
     date: 2021-08-11T13:19:00.403Z
     description: gulp

---

> - 自动化任务执行工具
> - 完全基于流(传入以文件为单位,而不是普通的字节流)

- [x] [] 实现 react 组件库打包
- [x] 下划线转小驼峰

```js
"a-b".replace(/-(\w)/, function (...args) {
  return args[1].toUpperCase();
});
("aB");
```

## inistall

```shell
npm i gulp --save-dev
```

## 配置文件

> gulpfile.js

```js
const gulp = require('gulp')
gulp.task('任务名称',‘任务定义函数’)
```

> watch 监听文件变化 执行对应的 cb

```js
gulp.task("watch", () => {
  gulp.watch(glob, cb, op);
});
```

> watch 监听文件的变化执行特定的任务

```js
gulp.task("watch", () => {
  gulp.watch(glob, ["任务名称"]);
});
```

## 执行任务

> 如果不指定任务名称,则执行 `defautl` 任务

```shell
gulp [任务名称]
```

## 文件流

> gulp 中的流是文件流,而不是普通的字节流，处理的时候以文件为单位，每个文件即是一个一个的文件对象

```tsx
interface File {
  content: string;
  filename: string;
}
```

> src 用于获取可读流
>
> dest 用于获取可写流

```js
gulp.src('获取可读流').pipe(gulp.dest(‘输出目录’))
```

## 插件

> 通过 pipe 方法将文件对象导入到插件中

**处理 less 文件**

> gulp-less: 转换 less 文件
>
> gulp-clean-css: 压缩 css

```js
gulp.src("./**/*.less").pipe(less()).pipe(pipe.dest("输出目录"));
```

**处理 js 文件**

> gulp-concat: 合并 js 到插件
>
> gulp-uglify: 压缩
>
> gulp-rename: 修改文件对象的 filename 属性

```js
gulp
  .src("./**/*.js")
  .pipe(concat("新的文件名称"))
  .pipe(pipe.dest("输出目录"))
  .pipe(uglify())
  .pipe(rename("新的文件名称"))
  .pipe(pipe.dest("输出目录"));
```

**插件自动导入**

> gulp-load-plugins: 依次加载 pkg 中的 gulp 插件

```js
const $ = require("gulp-load-plugins");
```

> - 通过读取 pkg 的依赖对象,得到所有的 gulp 插件
> - 将 gulp 插件进行(require)动态导入

--- TODO 1:13 ---
