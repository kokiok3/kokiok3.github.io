---
title: Storybook에서 제공하는 튜토리얼 정리
date: 2023-09-18
categories: [Storybook]
tags: [Storybook] # TAG names should always be lowercase
pin: true
---

***

## 컴포넌트
스토리북은 기본적으로 두개의 레벨을 가진다.
컴포넌트와 그것의 story들 이다. 
- Component
    - Story
    - Story
    - Story
    - …

스토리북에 컴포넌트에 대해 말하기 위해 컴포넌트를 문서화해야 한다.
default export를 작성할 것이다. 
- component: import한 컴포넌트
- title: 스토리북 사이드바에 표시할 컴포넌트의 카테고리 명
- tags: 컴포넌트에 대한 문서를 자동으로 작성해준다.
- excludeStories: 스토리북 렌더 시 포함하지 않는 것
- argTypes: 각 스토리에서 args 동작을 지정한다.

스토리를 작성하기 위해 Component Story Format3(CSF3)를 사용할 것이다.
Arguments(args)는 controls의 addon을 통해 스토리북 재실행 없이 실시간으로 편집이 가능하다.

## Config
./stroybook/previews.js 에서 parameters는 보통 스토리북의 features와 addons 의 동을 컨트롤하기 위해 사용된다.

![image](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/81a83a50-29f6-456b-8937-eb6108ca62b7/Untitled.png)

## addon : 플러그인 같은 것
addon 설치 후에 .storybook/main.js 파일의 addons에 배열 형태로 추가한 addons를 넣어줘야 한다.

- addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-interactions',
    '@storybook/addon-a11y',
]

- 플러그인 추천
    - `yarn add --dev @storybook/addon-a11y` :접근성 테스트는 시각 장애, 청각 장애, 인지 장애 등의 장애가 있는 사람을 포함하여 최대한 많은 사람이 애플리케이션을 사용할 수 있도록 노골적인 접근성 위반을 잡아내는 QA의 첫 번째 단계 역할을 한다.

---
## Decorator
Decorator 는 스토리에 임의의 wrapper를 제공하는 방법입니다. decorators 키를 사용하여 컴포넌트에 컨텍스트를 추가할 수 있습니다.

## 스토리북을 통해 bottom-up 개발을 할 수 있다.
CDD를 사용하면 컴포넌트 계층 구조가 올라갈수록 복잡성을 점진적으로 확장할 수 있습니다.

**Component-Driven Development: Build from the bottom up**

Design the UI > 컴포넌트 생성(Build Component one at a time) > 스토리북 사용하여 컴포넌 스토리 생성(Using Storybook) > 컴포넌트 조립 (Assemble composite Components) > 화면 생성(Build Screens)

## 스토리북과 피니아 연결하는 방법
아래의 코드를 .storybook/preview.js 에 추가한다.

```jsx
import { setup } from '@storybook/vue3';
import { createPinia } from 'pinia';

setup((app)=>{
  app.use(createPinia());
});
```

## 컴포넌트 상호 작용을 자동으로 테스트하는 법
새로운 스토리를 추가할 때마다 UI가 깨지지 않도록 다른 모든 스토리도 수동으로 확인해야 합니다. 이는 많은 추가 작업을 필요로 합니다.

---
## Deploy

컴포넌트를 빌드한 뒤 팀원들에게 피드백을 얻고자 한다면 스토리북 온라인을 배포할 수 있다.

1. Exporting as a static app 
    스토리북을 배포하려면 먼저 스토리북을 정적 웹 앱으로 내보내야 합니다. 이 기능은 이미 스토리북에 내장되어 있으며 사전 구성되어 있습니다.
    yarn build-storybook을 실행하면 스토리북-정적 디렉터리에 정적 스토리북이 출력되며, 이 스토리북은 모든 정적 사이트 호스팅 서비스에 배포할 수 있습니다.
    
2. Publish Storybook
    스토리북에서 만든 Chromatic이라는 무료 배포사이트를 이용할 것입니다.
    2-1. 우선 git remote 저장소에 배포할 내용이 있어야 합니다.
    2-2. `yarn add -D chromatic` 를 추가해줍니다.
    2-3.  Chromatic에 깃허브 계정을 이용해 로그인 합니다.
    2-4. 새로운 프로젝트를 하나 생성한 뒤 깃허브 레포와 싱크를 맞춥니다. 이때 모든 변경사항은 push되어 있어야 합니다. 싱크를 맞추기 위해서는 `yarn chromatic --project-token=<project-token>` 를 입력합니다. 
    2-5. 완료 시 아래와 같은 화면이 뜹니다. 
    Catch a UI change버튼 클릭
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/501d3aaa-3a12-438b-9739-92755576d837/Untitled.png)
    2-6. 
    master와 비교했을 때 UI 변경이 없다면 Go to your project 버튼을 누릅니다.
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/1a8e56c2-5695-40bd-a5cf-33914933896e/Untitled.png)
    master와 비교했을 때 UI 변경이 있다면, github/bitbucket 등의 commit 페이지에 가면 UI Review가 완료되지 않은 것을 볼 수 있다.
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/6dacbdec-e0a3-4c84-8178-771e8bda5173/Untitled.png)
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/693605de-d4f2-454c-9039-a692f6615d92/Untitled.png)
    
    2-7 빌드를 모두 마쳤다. 사이드메뉴에서 Library를 눌러 등록한 컴포넌트를 확인해보자.
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/9b30d20d-7426-4a88-a3f9-f0bcd3d231c5/Untitled.png)
    
     (주의) master 빌드를 안하고 Library 메뉴를 클릭하면 아래와 같은 화면이 나온다.
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/e875038e-0108-42fd-b284-f9dae62e0635/Untitled.png)
    
    (주의) master를 빌드해야지 라이브러리가 보인다. 추측상 Library를 누르면 master의 library로 이동하는 것 같다. master 빌드가 되어 있지 않다면 build를 눌러 아래의 컴포넌트를 확인하면 된다.
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/6f808edf-102d-4d5c-8459-d5ecdafb9aa3/Untitled.png)
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/bff17b2c-f2ac-4374-a118-d6d0618882c5/Untitled.png)
    
    ![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/e48312ef-d94a-403c-a4a5-fbd00ad2bbff/6d80486a-bff3-4d99-86a2-58ee5b5c342c/Untitled.png)
    
3. Continuous deployment with Chromatic
    2를 통해 배포를 해지만 지속적인 UI피드백을 받기 위해서는 지속적으로 배포를 해야 합니다. 이러한 지속적 배포는 continuous integration(CI)라 불립니다. 깃허브 액션을 통해 continuous integration(CI)를 할 수 있습니다. 
    

## ****Add a GitHub Action to deploy automatically Storybook****
1. root폴더에 .github/workflows 폴더를 만든다. 해당 경로는 github actions 를 위한 경로이다.
2. chromatic.yml 파일 생성 후 코드 아래 작성한다.
    
    ```yaml
    # Workflow name
    name: 'Chromatic Deployment'
    
    # Event for the workflow
    on: push
    
    # List of jobs
    jobs:
      test:
        # Operating System
        runs-on: ubuntu-latest
        # Job steps
        steps:
          - uses: actions/checkout@v3
            with:
              fetch-depth: 0
          - run: yarn
            #👇 Adds Chromatic as a step in the workflow
          - uses: chromaui/action@v1
            # Options required for Chromatic's GitHub Action
            with:
              #👇 Chromatic projectToken, see https://storybook.js.org/tutorials/intro-to-storybook/vue/en/deploy/ to obtain it
              projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
              token: ${{ secrets.GITHUB_TOKEN }}
    ```
    
3. 맨 하단의 secrets.CHROMATIC_PROJECT_TOKEN, secrets.GITHUB_TOKEN 는 GitHub secrets > action 에 등록한다. chromatic 토큰과 github 토큰을 등록하면 된다.
4. 그 뒤 GitHub에 커밋&푸시하면 해당 내역부터 GitHub actions에 빌드된다.
---
## Visual Testing

Visual Testing이란 visual regression testing 이라고도 불린다. 
모든 스토리의 스크린샷을 가지고 커밋마다 비교를 하는 방식이다. 이는 레이아웃의 표면적인 변화를 감지하기 때문에 그래픽적 요소를 검사하기 유용하다.
사용하기 위해서는 visual regression testing 툴을 사용해아하는데 튜토리얼에서는 Chromatic을 추천한다.

1. 테스트를 위해 새로운 브랜치를 만든 뒤 css 하나만 바꿔서 git push를 한다.
2. 그 뒤 pull request를 처리한다.
3. 그럼 아래와 같이 UI Test를 진행한다. 하지만 UI Test는 완료되지 않았다.
    
    ![image](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a05e390-8e87-438d-881c-dfe953eeecc8/Untitled.png)
    
4. UI Test의 Details를 클릭하면 테스트할 목록과 리뷰 상황을 볼 수 있다. 이를 통해 하나의 작은 변화가 많은 변화를 일으킨다는 것을 알 수 있다.
    
    ![image](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0affa5a2-ec31-431e-ac8b-51281a05592c/Untitled.png)
    
    1. Verify changes 버튼을 클릭해 변경사항을 리뷰하면 검토가 완료된다.

이로써 Visual Testing을 통해 버그에 대한 걱정 없이 컴포넌트를 출시할 수 있다.

---
Actions는 UI 컴포넌트를 개별적으로 빌드할 때 상호 작용을 검증하는 데 도움이 됩니다. 운영하는 app의 functions나 state에 접근할 수 없을 때 쓰입니다.

Controls는 비개발자가 컴포넌트와 스토리를 사용하기 좋은 방법이다. 그렇기 때문에 문자열 ellipsis 와 같은 vistal testing을  Controls로 일일이 하기 보다는 새로운 story를 추가하여 여러 계층에서 확인할 수 있게 하는게 더 효율적이다.

---
참고
**Intro to Storybook**
[ https://storybook.js.org/tutorials/intro-to-storybook/]( https://storybook.js.org/tutorials/intro-to-storybook/)