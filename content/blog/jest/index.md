---

     title: jest
     date: 2021-10-10T14:12:39.741Z
     description: jest

---

- http://www.zhufengpeixun.com/strong/html/90.react-test.html

- 一组相关的测试称为测试套件

## 基本的测试框架原型

- 使用 describe 对测试用例(`由it定义`)进行分组.

```js
describe("分组描述信息", () => {
  it("测试用例描述信息", () => {
    // 存放断言函数的函数
  });
});
```

## jest

### 安装

```shell
npm i jest -D
```

### 基本使用

- expect 进行断言
- it 或者 test 定义 case
- describe 对 case 进行分组

```js
describe("分组描述信息", () => {
  it("测试用例描述信息", () => {
    // 存放断言函数的函数
  });
});
```

- 查看测试覆盖率,会自动输出测试的报告文件.

```shell
jest --coverage
```

- 输出 jest 配置文件

```shell
jest --init
```

- 配置 babel 支持 ts 和 esm

1. 添加 babel 配置
2. 添加 ts 配置
3. 添加 jest 配置

- 监听

```shell
jest --watchAll
```

### matchers 匹配器

- 官方文档: https://jestjs.io/docs/expect

- toBe: 比较基本类型
- toEqual: 比较对象
- toBeNull: 期望一个值是 null
- toBeUndefined: 期望一个值是 undefined
- toContain: 期望一个数组或者字符串包含某个函数
- not: 取反
- toMatch: 期望匹配某个正则

### 测试 DOM

- 基于 jsdom

1. until.tsx

```ts
function remove(node) {
  node.parentNode.removeChild(node);
}
```

2. case

```ts

```

### 测试异步请求

- jest 默认不会`等待异步代码完成`可以通过以下的方式来测试异步代码

1. done: 适用于测试回调风格的代码

```tsx
test("测试cb", (done) => {
  // 异步结束后 调用 done
});
```

2. 使用 async/await

```ts
test("promise", async () => {
  const result = await somePromise();
  expect(result).toEqual({
    code: 1,
  });
});

test("promise", () => {
  expect(somePromise()).resolves.toMatch({
    code: 1,
  });
});

test("promise", (done) => {
  somePromise().then((val) => {
    expect(result).toEqual({
      code: 1,
    });
    done();
  });
});
```

### mock

- 模拟函数的调用

```ts
// 测试 exec 中是否调用了 callback
import { exec } from "../exec";
it("xx", () => {
  const callback = jest.fn();
  callback.mockReturnValueOnce("xx"); // 模拟回调函数的返回值
  exec(callback);
  expect(callback).toBeCalled(); // 判断一个 callback 是否被调用
  expect(callback).toBeCalledTimes(2); // 判断一个 callback 的调用次数
  expect(callback).toBeCalledWith("111"); // 判断一个 callback 被调用时传入的参数
  console.log(callback.mock);
});
```

- 测试某个类的实例化

1. 返回在 callback.mock 对象上的 `instance` 属性上

## TDD

- 先写测试用例,在写对应的功能实现,

### enzyme

> https://enzymejs.github.io/enzyme/docs/api/ReactWrapper/simulate.html

```shell
npm i enzyme @types/enzyme
```

```tsx
it("test message-form render element", () => {
  const wrapper = mount(<MessageForm />);
  const form = wrapper.find(`form`);
  const input = wrapper.find(`input`);
  const button = wrapper.find(`button`);
  expect(form).toHaveLength(1);
  expect(input).toHaveLength(1);
  expect(button).toHaveLength(1);
});

it("test message-form submit input value by button", () => {
  const addMessage = jest.fn();
  const wrapper = mount(<MessageForm addMessage={addMessage} />);
  const input = wrapper.find(`input`);
  const button = wrapper.find(`button`);
  input.simulate("change", {
    target: {
      value: 1,
    },
  });
  button.simulate("click");
  expect(addMessage).toBeCalledWith(1);
  wrapper.instance(); // 获取类组件的实例
  const childComp = wrapper.find(组件); // 直接获取 子组件
});
```

## BDD

-  行为驱动开发

1. 定义用户故事(站在用户的角度，去描述用户的行为)

2. 根据用户故事写测试用例

## UI 测试

- 让测试用例跑在浏览器端

- puppeteer 集成 jest

```shell
npm i jest-puppeteer jest puppeteer -D
```

## GET

- [x] node 提供的断言函数: `console.assert(condition,msg) `, 浏览器端也可以使用.
- [x] @babel/preset-env: `targets.node:current`表示将高版本的 js 语法 转换成当前 node 版本支持的版本
- [x] jsdom 符合 web 标准的，可以跑在 node 端的 `dom 实现 `,在 jest 中通过` jestEnvironment`指定
- [ ] mock 调用是什么意思
- [x] el.nodeName: 获取 dom 节点的 tagname 和 tagName 行为一致
- [x] expect().toBeTruthy():  判断一个值是不是 truthy
- [x] wrapper.instance() : 获取类组件的实例
- [x] wrapper.find(组件) :  直接获取子组件

1. 注意当 props 更新的时候,需要手动再次获取  子组件的实  例

- [] expect.any(String) : 表示任意的字符串
- [] 什么是白盒测试和黑盒测试

  > 白盒测试：`已知产品的内部工作过程`，可以通过测试证明每种`内部操作是否符合设计规格要求`，所有内部成分是否以经过检查。 软件的黑盒测试意味着测试要在软件的接口处进行。 这种方法是把测试对象看做一个黑盒子，测试人员完全不考虑程序内部的逻辑结构和内部特性，`只依据程序的需求规格说明书，检查程序的功能是否符合它的功能说明。`

- [x] 测试类组件可以直接使用 wrapper.state 获取组件的状态

- [x] 如何测试 redux 和 router

1. 测试使用 redux 的组件,需要使用 `Provider` 进行包裹，在挂载

```tsx
imp Provider
imp store
const wrapper = mount(<Provider store={store}><App></Provider>)
```

2. 测试 reducer (reducer 是纯函数，非常容易测试)

```tsx
// 测试初始化状态
expect(count1(undefined, { type: "@@REDUX/init" })).toEqual({});
```

3. 测试仓库(注意测试用的是统一仓库，可以通过 `beforeEach` 在每次测试之前重置 仓库)

- [x] toMatchObject 和 toEqual 有什么区别

1. toMatchObject 匹配的是对象的属性,如果属性名称相同值不同 则不匹配

2. toEqual 对象属性完全相等
