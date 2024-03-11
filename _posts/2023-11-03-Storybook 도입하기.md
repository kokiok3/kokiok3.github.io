---
title: Storybook 도입하기
date: 2023-09-18
categories: [Storybook]
tags: [Storybook] # TAG names should always be lowercase
pin: true
---

***

## 도입 개요

기존에는 ui 컴포넌트 정의서를 노션에 정리를 했다.  
하지만 불편한 점이 존재했다. 작성자와 UI 컴포넌트를 사용하는 개발자의 편리성이 떨어진다는 것이다.  

작성자 측면에서는 코드를 수정할 때마다 문서를 수정해야 하는 점이 있다.  
개발자 측면에서는 UI 컴포넌트의 모습을 볼 때 한계가 있다.  
개발자에게 UI 컴포넌트를 보여주기 위해서는 별도의 코드 에디터를 통해 보여주거나 캡처된 이미지로 보여줘야 했다.  
코드 에디터를 이용하면 또 다른 페이지로 넘어가야만 했고 캡처된 이미지를 사용하면 작성자가 각 UI 컴포넌트가 가지는 옵션들을 일일이 캡처해야 하는 번거로운 단점이 있었다.  

UI 컴포넌트 라이브러리를 찾아본 결과 Storybook이 있었다.  
Storybook을 사용하면 위에서 말한 단점을 모두 보완할 수 있는 방법이 있다.  

1. 코드와 문서의 동기화
2. 예제 페이지 제공

추가적으로, 배포를 통해 UI 컴포넌트가 변경됨에 따라 문서 또한 버저닝이 가능하다는 것이다.  
이제 아래에서 설치에 대한 설명을 시작하겠다.



---

## 기본 설치

사용자 프로젝트 스펙  
vue 2 vue cli 4

처음엔, Storybook 공식 문서를 따라 설치를 했으나 아래와 같은 이슈가 있었다.  
이슈를 잘 살펴보면 Webpack5 setup 중 발생하는 것을 확인할 수 있다.

```
npm run storybook

> watcher@2.1.0 storybook
> storybook dev -p 6006

@storybook/cli v7.3.2

info => Starting manager..
info Addon-docs: using MDX2
info => Using implicit CSS loaders
info => Using default Webpack5 setup // 이 부분
<i> [webpack-dev-middleware] wait until bundle finished
3% setup watch runError: Compiling RuleSet failed: Unexpected property test in condition (at ruleSet[1].rules[5].resource.test: (resource) => {
        currentResource = resource
        return true
      })
    at RuleSetCompiler.error (C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:373:10)
    at RuleSetCompiler.compileCondition (C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:309:17)
    at C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\BasicMatcherR    at Hook.eval [as call] (eval at create (C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\tapable\lib\HookCodeFactory.js:19:10), <anonymous>:19:1)
    at RuleSetCompiler.compileRule (C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:177:19)
    at C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:155:27
    at Array.map (<anonymous>)
    at RuleSetCompiler.compileRules (C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:155:5)
    at RuleSetCompiler.compileRule (C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:184:30)
    at C:\Project\workmanager-frontend\node_modules\@storybook\builder-webpack5\node_modules\webpack\lib\rules\RuleSetCompiler.js:155:27
```

이로써 공식 문서에서 알려준 `npx storybook@latest init` 로 설치를 하면서 package.json에 추가된 "@storybook/vue-webpack5": "^7.3.2", 가 문제가 됨을 알 수 있었다.
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/55d27d7f-d169-432a-b4f5-f12ec47d2367)

<br><br>
**다시 처음으로 돌아가자.**

vue2 기준으로 storybook을 설치하는 방법을 설명하겠다.  
vue-cli를 통해 프로젝트를 생성했기 때문에 storybook 또한 vue cli를 이용해 설치하겠다.  
참고: https://github.com/storybookjs/vue-cli-plugin-storybook  
storybook에서 제공하는  vue-cli-plugin-storybook설치 명령어는 `$ vue add storybook` 이다.  
해당 깃허브의 설명을 보면 아래와 같은 설명을 한다.  

> [설치 후에 config 폴더가 생성된다.
스토리북을 위한 webpack config는 vue-cli-service에 의해 resolve됩니다. 그렇기 때문에 별도의 webpack 설치를 하지 않아도 됩니다. 하지만 사용자 정의 플러그인으로 확장한다면 vue.config.js 에서 pluginsOptions를 통해 플러그인 이름을 전달하여 허용된 플러그인 목록을 확장할 수 있습니다.]  

이제 설치는 완료되었고 `npm run storybook:serve`를 이용해 스토리북을 실행할 수 있다.
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/6d49cc17-b83b-4500-93d2-ae7c1fcc4c58)
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/542e49d7-16f9-4b16-a9d5-9cb3f1ca7a4f)

---

preview.js

```jsx
// vue의 main.js 와 같은 것
import Vue from 'vue';

import i18n from '@/i18n';
Vue.prototype.$t = function(...args){
    return i18n.t(...args);
}

import StoreModules from '@/store';
Vue.prototype.$store = StoreModules;
```

---
참고

**다국어, vuex 설치**
[https://github.com/storybookjs/storybook/issues/1573#issuecomment-1094262477](https://github.com/storybookjs/storybook/issues/1573#issuecomment-1094262477)