---
description: >-
  ID NODE API는 Dapp 연동 시 SymID를 발급 혹은 Keystore 생성 하기 위해 제공되는 Dapp Partner API
  입니다.
---

# ID NODE API

## 소개&#x20;

Symverse 블록체인 참여자(DApp)로써 활동하기 위해서는 ID Node를 통해 Keystore를 등록해야 합니다. 참여 하는 방법은 **Salt 지갑을 통해 회원가입**, **BBP 서비스 이용, Partner API** 등이 있습니다.

해당 페이지는 **Partner** **API**를 통한 Keystore 생성 혹은 등록 방법에 대해 작성하였고, Dapp 서비스 개발 시에 제공되는 Open API 형태입니다.

{% hint style="info" %}
SymID에 대한 상세 설명은 [해당 링크(SymID KOR)](../symverse/symid-kor.md)를 참조 하세요&#x20;
{% endhint %}

## 사전 요구사항

### 1. 퍼블릭 네트워크 설정

symverse 퍼블릭 네트워크로 크게 Testnet, Mainnet 두가지가 존재합니다.

**Testnet**은 테스트 전용 네트워크로써 블록체인 내 여러 기능을 테스트 할 수 있습니다. **Mainnet**은  심버스에서 운영중인 핵심 퍼블릭 네트워크입니다.&#x20;

API에 호출하기 전에 해당하는 퍼블릭 네트워크를 host로 사용합니다.

<table><thead><tr><th width="218">퍼블릭 네트워크</th><th>API Host</th></tr></thead><tbody><tr><td><strong>Testnet</strong></td><td><a href="https://testnet-ca.symverse.com"><strong><code>https://testnet-ca.symverse.com</code></strong></a></td></tr><tr><td><strong>Mainnet</strong></td><td><a href="https://mainnet-ca.symverse.com"><strong><code>https://mainnet-ca.symverse.com</code></strong></a></td></tr></tbody></table>

### 2. Secret 키 발급&#x20;

API를 사용하기 위해서는 Symverse 회사를 통해 **Secret (`32길이의 랜덤 값`)**를 발급받으셔야 합니다.

```yang
// 예시
SECRET: d139996edf514e7ba7883930c1db8f7b
```

발급 받은 Secret 를 통해 아래 API 목록을 확인해서 원하는 API 를 호출합니다.

## API Spec

{% swagger method="post" path="/caii/d1/{secret}/account/new" baseUrl="[host]" summary="블록체인 Keystore 생성 API" %}
{% swagger-description %}
Keystore 생성 및 해당 네트워크에 SymID를 등록합니다.
{% endswagger-description %}

{% swagger-parameter in="path" name="secret" required="true" %}
전달받은 고유 Secret 값
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="성공" %}
```json
{
    "message": "NewKeystoreReturn",
    "result": {
        "private_key": "0x99957b9d39dd63c40f8425b2d8e5a70c7b234e24f1aeaf6a34e38837627fc9e8",
        "public_key_x": "0x44c7402f600b88416ab9078748504513e8896047835c07bdfdc6a23c81ef9015",
        "public_key_y": "0x82763122d34de1aeae7bf47791eeea5f0dd619462bd88ca4b96782a7739f4cae",
        "public_key_hash": "0x85a784E634ab7644Ba4A43b7a1455Ba592C11B13",
        "txhash": "0x1deeb76d585b77917b39a75866aa9d9b3164c3eb3f55314920fde12a7a87fed8",
        "keystore": {
            "address": "0002b4611fb9a3740002",
            "crypto": {
                "cipher": "aes-128-ctr",
                "ciphertext": "1674da459ffbf64ed6ad531fb60752407279e8145ff9ede7079727dd53a51df5",
                "cipherparams": {
                    "iv": "7af7d5fa9b6695363e770e094fe6f39a"
                },
                "kdf": "scrypt",
                "kdfparams": {
                    "dklen": 32,
                    "n": 262144,
                    "p": 1,
                    "r": 8,
                    "salt": "49b8bab3ef71c945d691a3a2423bb285481d6424cc70a2de3688b2c6496bb64a"
                },
                "mac": "39f320bbe017fe288be2b7fa56c2a4430834aeb9a7230b267d4b787037fde01a"
            },
            "id": "c295863a-bf64-4463-9db0-d03ed5dbaa8a",
            "version": 3
        },
        "keystore_passphrase": "u4blGI{9x0MjqiuL"
    }
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="secret 인증 에러" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/caii/d1/{secret}/account/new/{hash}" baseUrl="[host]" summary="블록체인 Keystore 생성 By Public key Hash API" %}
{% swagger-description %}
Public Key Hash 기반으로 Keystore 생성 및 해당 네트워크에 SymID를 등록합니다.
{% endswagger-description %}

{% swagger-parameter in="path" name="secret" required="true" type="String" %}
전달받은 고유 Secret 값
{% endswagger-parameter %}

{% swagger-parameter in="path" name="hash" type="String" required="true" %}
Public Key Hash 
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="성공" %}
```json
{
    "message": "NewKeystoreReturn",
    "result": {
        "private_key": "0x99957b9d39dd63c40f8425b2d8e5a70c7b234e24f1aeaf6a34e38837627fc9e8",
        "public_key_x": "0x44c7402f600b88416ab9078748504513e8896047835c07bdfdc6a23c81ef9015",
        "public_key_y": "0x82763122d34de1aeae7bf47791eeea5f0dd619462bd88ca4b96782a7739f4cae",
        "public_key_hash": "0x85a784E634ab7644Ba4A43b7a1455Ba592C11B13",
        "txhash": "0x1deeb76d585b77917b39a75866aa9d9b3164c3eb3f55314920fde12a7a87fed8",
        "keystore": {
            "address": "0002b4611fb9a3740002",
            "crypto": {
                "cipher": "aes-128-ctr",
                "ciphertext": "1674da459ffbf64ed6ad531fb60752407279e8145ff9ede7079727dd53a51df5",
                "cipherparams": {
                    "iv": "7af7d5fa9b6695363e770e094fe6f39a"
                },
                "kdf": "scrypt",
                "kdfparams": {
                    "dklen": 32,
                    "n": 262144,
                    "p": 1,
                    "r": 8,
                    "salt": "49b8bab3ef71c945d691a3a2423bb285481d6424cc70a2de3688b2c6496bb64a"
                },
                "mac": "39f320bbe017fe288be2b7fa56c2a4430834aeb9a7230b267d4b787037fde01a"
            },
            "id": "c295863a-bf64-4463-9db0-d03ed5dbaa8a",
            "version": 3
        },
        "keystore_passphrase": "u4blGI{9x0MjqiuL"
    }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="잘못된 hash " %}
```json
{
    "message": "already existed pub_key_hash",
    "result": null
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="secret 인증 에러" %}

{% endswagger-response %}
{% endswagger %}



