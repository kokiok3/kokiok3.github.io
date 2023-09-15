---
title: Vue2 npm ERR! code 1 해결방법
date: 2023-09-15
categories: [Vue]
tags: [Vue, Vue2] # TAG names should always be lowercase
pin: true
---

> 

***

노트북을 변경해서 node와 npm 을 새로 설치했다. 

그리고 로컬 개발 환경을 키기 위해 `npm run serve` 를 입력했는데 에러가 났다.

```jsx
npm ERR! code 1
npm ERR! path \Users\...\node-sass
npm ERR! command failed
```

<br><br>
구글에 검색한 키워드는 ‘npm ERR! code 1’, 
새로 깐 node의 버전과 기존에 깔려있던 node-sass의 버전 호환이 안돼서 그렇다고 한다.
그래서 node-sass의 버전을 올렸다.
<br><br>

버전을 올리기 전에 node와 node-sass가 호환되는 버전을 알아봐야 한다. 나의 현재 node 버전은 ``18.17.1`` 이었다. 그래서 node-sass를 버전 8로 올려주었다.
<br><br>


아래의 표는 node-sass npm페이지에서 가져온 호환 표이다. 각자 node버전에 맞는 node-sass를 설치하면 된다.

![nodeSassNpmVersion](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/0c05d2af-8433-4097-9c13-699d972d0e29){: width="50%"}
<br><br>

그리고 다시 `npm install`을 하여 node modules를 설치하려고 하니까 이번에는 

ERR_OSSL_EVP_UNSUPPORTED 라는 에러가 생겼다.

```jsx
Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:69:19)
    at Object.createHash (node:crypto:133:10)
    at module.exports (C:\project\workmanager-frontend\node_modules\webpack\lib\util\createHash.js:135:53)
    at NormalModule._initBuildHash (C:\project\workmanager-frontend\node_modules\webpack\lib\NormalModule.js:417:16)
    at handleParseError (C:\project\workmanager-frontend\node_modules\webpack\lib\NormalModule.js:471:10)
    at C:\project\workmanager-frontend\node_modules\webpack\lib\NormalModule.js:503:5
    at C:\project\workmanager-frontend\node_modules\webpack\lib\NormalModule.js:358:12
    at C:\project\workmanager-frontend\node_modules\loader-runner\lib\LoaderRunner.js:373:3
    at iterateNormalLoaders (C:\project\workmanager-frontend\node_modules\loader-runner\lib\LoaderRunner.js:214:10)
    at Array.<anonymous> (C:\project\workmanager-frontend\node_modules\loader-runner\lib\LoaderRunner.js:205:4)
    at Storage.finished (C:\project\workmanager-frontend\node_modules\enhanced-resolve\lib\CachedInputFileSystem.js:55:16)
    at C:\project\workmanager-frontend\node_modules\enhanced-resolve\lib\CachedInputFileSystem.js:91:9
    at C:\project\workmanager-frontend\node_modules\graceful-fs\graceful-fs.js:123:16
    at FSReqCallback.readFileAfterClose [as oncomplete] (node:internal/fs/read_file_context:68:3) {
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}
```
<br><br>

이번엔 구글에 ‘ERR_OSSL_EVP_UNSUPPORTED’로 검색을 했다.

검색 결과 node 버전에 문제가 있기 때문에 node 버전을 내리거나 script에 -openssl-legacy-provider를 추가해야 한다고 한다.

처음에는 -openssl-legacy-provider를 추가해서 npm run serve를 해봤지만 vue-cli-service 문법에 맞지 않아서 실패했다.
시도한  script -  `"serve": "vue-cli-service -openssl-legacy-provider serve",`

결국 Node버전을 내려야 했다.

노드 버전을 변경하기 위해서는 Window에서는 nvm을 설치해야 한다. nvm이란 node version manager로 말 그대로 노드의 버전을 관리할 수 있다. 
nvm을 설치하기 위해선 [**링크**](https://github.com/coreybutler/nvm-windows/releases){:target="_blank"}로 접속한 뒤, Assets에서 nvm-setup.exe를 다운받아야 한다.

![nvmSetUp](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/1bc54ca0-f95f-4e09-8868-ca26ff0bb9fc)
<br><br>


nvm 설치가 끝난 뒤 설치할 수 있는 노드 버전 목록을 확인하기 위해 ``nvm list``를 입력했다.

하지만 아래와 같은 또다른 에러가 났다…

```jsx
C:\Users\������\AppData\Roaming\nvm could not be found or does not exist. Exiting. 
```
<br><br>


또 다시 구글링..

![aFewMonthsLater](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/a7722f6f-cfc1-4663-85ea-5fbb7077de20){: width="50%"}
<br><br>

알고보니 vsc의 터미널을 이용할 경우 위와같은 에러가 날 수 있으니 cmd 창을 이용하라는 글을 봤다.

시작에서 cmd 창을 켜고 `nvm root "C:\Users\본인의 사용자 명\AppData\Roaming\nvm”` 을 입력했다.

> 경로를 큰 따옴표로 감싸는 것이 중요하다.
>
> 또한 ‘본인의 사용자 명’에는 자신의 사용자 명을 넣어야 한다.

<br>


그러고 다음 줄에 아래의 명령어를 입력하면 된다.

`$ nvm install (설치할 major version)`

이렇게 하면 `nvm list` 명령어 입력 시 사용이 가능한 node 버전 리스트가 나온다.

그 중에서 사용할 버전을 `$ nvm use (설치한 버전 숫자로 기입)` 를 이용해 입력해주면 된다.

마지막으로 확인하기 위해 ``node -v`` 로 버전 확인을 한다.

자 이제 다시 ``npm run serve``를 하니까 개발 서버가 잘 켜진다.
<br><br>

---

참고

**code1 err**

https://velog.io/@ryeon5789/npmError-npm-ERR-code-1

**node와 node-sass 버전**

https://www.npmjs.com/package/node-sass

**ERR_OSSL_EVP_UNSUPPORTED**

https://kgu0724.tistory.com/280

https://ongamedev.tistory.com/484

**C:\Users\������\AppData\Roaming\nvm could not be found or does not exist. Exiting.**

https://jinnnkcoding.tistory.com/189