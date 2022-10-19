---
description: 아래는 의존성 라이브러리를 통해 Web3j 를 추가하는 방법에 대해 작성하였습니다.
---

# 시작하기

## 설치하기

sym-web3j 를 사용하기 위해 프로젝트 형태에 맞게 아래와 같이 SDK 의존성을 추가합니다.

{% hint style="info" %}
현재 가장 최신 버전은 **0.0.2** 입니다.
{% endhint %}

### Maven Project

```
<dependencies>
    <dependency>
        <groupId>com.symverse.library</groupId>
        <artifactId>sym-web3j</artifactId>
        <version>X.X.X</version>
    </dependency>
</dependencies>
```

### gradle

```groovy
implementation 'com.symverse.library:symweb3j:X.X.X'
```

## 라이브러리 파일로 직접 추가하기

&#x20;sym-web3j 라이브러리를 파일로 직접 추가하는 경우는 직접 다운로드 및 프로젝트 내 추가해 주셔야합니다.

### 라이브러리 설치

&#x20;해당 설치 링크 - [<mark style="color:purple;">https://github.com/symverse-lab/sym-web3j-sample/releases/download/0.0.2/symweb3j-0.0.2.jar</mark>](https://github.com/symverse-lab/sym-web3j-sample/releases/download/0.0.2/symweb3j-0.0.2.jar)<mark style="color:purple;"></mark>

### 프로젝트에 추가하기&#x20;

1. &#x20;프로젝트 root 에서 `libs` 폴더 생성 및 다운로드 받은 라이브러리 파일을 추가해 줍니다.&#x20;
2. &#x20;의존성을 다음과 같이 추가해 줍니다.

### Maven Project

Maven 에서 아래와 같이 `systemPath` 에 맞게 다운로드 받은 라이브러리 경로를 설정해줍니다.

```xml
<dependency>    
  <groupId>com.symverse.library</groupId>    
  <artifactId>sym-web3j</artifactId>   
  <version>2.0</version>    
  <scope>system</scope> 
  <systemPath>${basedir}\libs\sym-web3j-0.0.2.jar</systemPath>  
</dependency>
```

### gradle&#x20;

```groovy
// sym-web3j required libraries
implementation fileTree(dir:'libs/', include:'*.jar')
```



## 샘플 코드

아래는 라이브러리를 이용한 샘플 코드 입니다.&#x20;

&#x20;샘플 코드 링크 - [<mark style="color:purple;">https://github.com/symverse-lab/sym-web3j-sample/tree/main/src/test/java/com/symverse/symweb3jsample</mark>](https://github.com/symverse-lab/sym-web3j-sample/tree/main/src/test/java/com/symverse/symweb3jsample)<mark style="color:purple;"></mark>
