---
description: Node와 Web 환경에서 SymVerse의 Transaction 작성과 SCT20/21/22 Method를 작성하는 기능을 제공합니다.
---

# sym-tools (javascript)

## 개요

Node 및 Web 환경에서 사용할 수 있는 SDK입니다. Node 환경과 Web 환경에서 사전에 준비해야하는 작업이 있으므로 아래 내용을 참고하여 주세요.

## 설치방법

`NPM` 설치 패키지 매니저를 통해 설치합니다.

```
npm install sym-tools
```

## 사용방법

### 1. Node 환경

Node 환경에서는 `dart-sdk`가 필요합니다. [Get the Dart SDK](https://dart.dev/get-dart)을 참고하여 `dart-sdk`를 설치한 후, dart 커맨드를 사용할 수 있도록 환경변수를 설정합니다.

```javascript
export PATH="$PATH:/path/to/dart-sdk/bin"
```

### 2. Web 환경

Web 환경에서는 `sym-tools.js`가 필요합니다. 아래 소스를 HTML 파일에 추가합니다.

```html
<script src="https://download.symverse.com/sym-tools-js/sym_tools.js"></script>
```

## 사용예제

```javascript
const symTools = require('sym-tools');

var result = symTools.transaction.composeSendTransaction(
  "0x00032dd1fa260e2a0002",
  "0xbd",
  "0x174876e800",
  "0x11e68",
  "0xaca4414a37d7a6dc43e5",
  "0x0",
  "0xd71501d48a000232c721024cdd0002880de0b6b3a7640000",
  "0x1",
  "0x0002537dc9a64d350002",
  "0x"
);

console.log(result);
```





