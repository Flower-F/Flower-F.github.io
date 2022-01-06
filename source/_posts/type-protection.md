---
title: Typescript 类型保护
date: 2021-12-09 17:11:33
tags: [typescript]
---
# typeof 类型保护

`typeof` 类型保护用于判断变量是哪种原始类型。

```ts
function fn(param: string | number) {
    if (typeof param === 'string') {
        console.log(param + 1);
    }
    if (typeof param === 'number') {
        console.log(param + 1);
    }
}
// 原本是联合类型，由于应用了 typeof，后面作用域的 param 类型就确定了。
fn('1'); // 11
fn(1); // 1
```

同理还有 instance 保护。

# 自定义类型保护 is

`typeof` 和 `instanceof` 类型保护能够满足一般场景，对于一些更加特殊的，可以通过自定义类型保护来对应类型，比如我们自己定义一个请求参数的接口类型。

```ts
interface RequestParams {
  url: string;
  onSuccess?: () => void;
  onFailed?: () => void;
}

function isValidRequestParams(request: any): request is RequestParams {
  return request && request.url;
}

let request;
// 检测客户端发送过来的参数
if (isValidRequestParams(requst)) {
  request.url;
}
```

这里面通过判断，我们需要手动告诉编译器通过 `isValidRequestParams` 的判断以后则 `request` 就是 `RequestParams` 类型的参数，编译器通过类型谓词 `parameterName is Type` 得知，`isValidRequestParams` 返回了 `true` 则 `request` 就是 `RequestParams` 类型。

参考资料：
https://tsejx.github.io/typescript-guidebook/syntax/advanced/type-guards