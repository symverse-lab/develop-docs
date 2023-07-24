---
description: "아래는 의존성 관리를 통해 \bSDK를 추가하는 방법에 대해 작성하였습니다."
---

# 시작하기

## 설치하기

sym-web3js 를 사용하기 위해 프로젝트 형태에 맞게 아래와 같이 SDK 의존성을 추가합니다.

```bash
npm i sym-web3js
or
yarn add sym-web3js
```

### 사용하기

```javascript
import SymWeb3 from "sym-web3js"

console.log(SymWeb3)  // { utils: ...} 

const symjs = SymWeb3();
console.log(symjs); // {network: ..., utils: ..., signer: ...} 
```

### 브라우저 사용방식

NodeJs 환경이 아닌 Web 브라우저 환경일 경우 아래 링크를 통해 스크립트를 직접 추가합니다.

* &#x20;다운로드 링크: [https://github.com/symverse-lab/sym-web3js/releases](https://github.com/symverse-lab/sym-web3js/releases)
*   코드 추가

    <pre class="language-html"><code class="lang-html"><strong>&#x3C;script src='./sym-web3.browser.js'>&#x3C;/script>
    </strong></code></pre>

