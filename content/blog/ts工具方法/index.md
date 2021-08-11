#  ts 工具方法

```tsx
// 获取 array 每一项的类型
type GetArrayItemType<T> = T extends Array<infer P>?P:never
```

```ts
// 删除对象中的某个属性
type DeleteProperty<T,D extends keyof T> = {
  [K in keyof T as Exclude<K,D>]:T[K]
}
```

```tsx
// 获取对象 key的联合类型
export type GetKeys<T> = T extends Record<infer P, any> ? P:any;
```

```tsx
// 获取pormise的值类型
type ResolvePromise<T> = T extends () => Promise<infer P> ? P:never
type IInfo =ResolvePromise<typeof fetchTodayVisitApi>
```

