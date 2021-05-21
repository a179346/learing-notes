# Node 12 重大更新

## 1. async stack

```js
async function foo(x) {
  await bar(x);
}

async function bar(x) {
  await x;
  throw new Error("Let's have a look...");
}

foo(1).catch(e => console.log(e.stack));
```
Node 12 前:
```
Error: Let's have a look...
    at bar (<anonymous>:7:9)
```
Node 12 後:
```
Error: Let's have a look...
    at bar (<anonymous>)
    at async foo (<anonymous>)
```

## 2. Worker Threads

現在使用 Worker Threads 不需要 flag 了

https://medium.com/hybrid-maker/%E8%A9%A6%E7%8E%A9%E4%B8%80%E4%B8%8B-nodejs-10-5-0-worker-threads-a6ac7c6dfb8a

## 3. 啟動時間優化
## 4. ES6 Module 支持
可使用 import, export

當 Node >= v13，選擇下面兩種其一：
- 將 ES modules 檔案的副檔名存為 `.mjs`
- 加入 `{ "type": "module" }` 在 package.json 檔案中

# 其他功能支援

## 1. Promise.allSettled (v12.10.0)

會等到所有執行完再傳每個的結果

```js
Promise.allSettled([
  Promise.resolve(33),
  new Promise(resolve => setTimeout(() => resolve(66), 0)),
  99,
  Promise.reject(new Error('an error'))
])
.then(values => console.log(values));

// [
//   {status: "fulfilled", value: 33},
//   {status: "fulfilled", value: 66},
//   {status: "fulfilled", value: 99},
//   {status: "rejected",  reason: Error: an error}
// ]
```

## 2. nullish coalescing operator (??) (v14.5.0)
類似 || 但 ?? 只針對 null, undefined回傳右邊的值 其他回傳左邊
```js
let val;
// expected val: "default string"
val = null ?? 'default string';

// expected val: 0
val = 0 ?? 42;
// expected val: 42
val = 0 || 42
```

## 3. optional chaining operator (?.) (v14.17.0)
```js
let val;
// Node 14.17.0 前
val = req.body && req.body.data && req.body.data.name;
// Node 14.17.0 後，可以這樣寫
val = req.body?.data?.name;
```

## 4. public/private instance fields (v12.4.0)
```js
class ClassWithInstanceField {
  publicInstanceField = 'public instance field'
  #privateInstanceField = 'private instance field'
  // static
  static publicStaticField = 'public static field'
  static #privateStaticFiled = 'private static field'
}
```

## 5. private class methods (v14.17.0)
```js
class ClassWithPrivateMethod {
  #privateMethod() {
    return 'private method'
  }
  static #privateStaticMethod() {
    return 'private static method'
  }
}
```


# 其他資訊 

## Promise在Node各版本支援
| name                 | description                                     |          |
| -------------------- | ----------------------------------------------- | -------- |
| `Promise.allSettled` | does not short-circuit                          | v12.10.0 |
| `Promise.all`        | short-circuits when an input value is rejected  | v0.12.18 |
| `Promise.race`       | short-circuits when an input value is settled   | v0.12.18 |
| `Promise.any`        | short-circuits when an input value is fulfilled | v15.14.0 |

## Logical Assignment (v15.14.0)

### 1. `||=`
> `x ||= y` equals to `x || (x = y)`
```js
const a = { duration: 50, title: '' };

a.duration ||= 10;
console.log(a.duration);
// expected output: 50

a.title ||= 'title is empty.';
console.log(a.title);
// expected output: "title is empty"
```
### 2. `&&=`
> `x &&= y` equals to `x && (x = y)`
```js
let a = 1;
let b = 0;

a &&= 2;
console.log(a);
// expected output: 2

b &&= 2;
console.log(b);
// expected output: 0
```
### 3. `??=`
> `x ??= y` equals to `x ?? (x = y)`
```js
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25
```
