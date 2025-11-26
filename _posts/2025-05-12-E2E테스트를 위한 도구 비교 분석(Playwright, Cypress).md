---
title: E2E테스트를 위한 도구 비교 분석(Playwright, Cypress)
date: 2025-05-12
categories: [E2E]
tags: [E2E, Playwright, Cypress] # TAG names should always be lowercase
pin: true
---

---

### **목차**

[**1. E2E 테스트 도입 개요**](#e2e-테스트-도입-개요)  
[**2. 테스트 종류**](#테스트-종류)  
[**3. 비교 분석 목록**](#비교-분석-목록)  
[**4. cypress에서 playwright으로 전환한 사례**](#cypress에서-playwright으로-전환한-사례)  

<br>

# E2E 테스트 도입 개요

배포를 할 때마다 기존의 기능들이 잘 동작을 하는지 검증을 하고 싶었다. 그래서 테스트의 종류를 알아보았고 그 중에서 사용자가 실제로 애플리케이션을 사용할 때 발생하는 일에 대한 테스트가 가능한 E2E 테스트를 진행할 것이다. 현재 vue3과 타입스크립트로 마이그레이션을 진행하고 있어서 vue3 공식문서에서 테스트 관련 문서를 읽었다.

vue3 공식문서에는 playwright와 cypress를 추천한다.
![image](/assets/img/2025-05-12/image.png)

npm trends로 확인했을 때 playwright이 최근 1년간 압도적인 수치를 보여줬다.

하지만 cypress는 그동안 축적해온 리소스가 방대하기에 cypress와 playwright 을 모두 사용해보고 비교 분석할 거다.
![image](/assets/img/2025-05-12/image_1.png)

<br>

---

# 테스트 종류

#### **유닛 테스트**

함수, 클래스, 컴포서블, 모듈 등 하나의 작은 코드 테스트. 일반적으로 유닛테스트는 함수로직에 에러가 있는지 확인한다.  
In general, unit tests will catch issues with a function's business logic and logical correctness.  
vue, vite환경에서는 vitest 를 추천한다.  

#### **컴포넌트 테스트**

컴포넌트 테스트는 props, events, slots, styles, classes, lifecycle hooks 등을 검사합니다.  
Component tests should catch issues relating to your component's props, events, slots that it provides, styles, classes, lifecycle hooks, and more.

#### **E2E 테스트**

사용자가 실제로 애플리케이션을 사용할 때 발생하는 일에 대한 커버리지를 제공합니다.

<br>

---

# 비교 분석 목록

- 라이브러리에 대한 러닝커브
- 빗버킷 자동화 파이프라인 구축
  - 자동화 구축이 얼마나 쉬운지
  - 자동화 시 테스트 소요 시간
- 차이점  

⚠️라이브러리 설치 전 최소 node, npm 버전을 확인하고 진행할 것.

<style>
    .comparison-table {
        table-layout: fixed;
        width: 100%;
    }
    .comparison-table td,
    .comparison-table th {
        word-wrap: break-word;
        overflow-wrap: break-word;
        white-space: normal;
        word-break: break-word;
    }
</style>
<table class="comparison-table" style="table-layout: fixed; width: 100%;">
    <colgroup>
        <col style="width: 33.33%;">
        <col style="width: 33.33%;">
        <col style="width: 33.33%;">
    </colgroup>
    <thead>
        <tr>
            <th style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">항목</th>
            <th style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">Cypress (기존에 많이 쓰이던)</th>
            <th style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">Playwright (라이징스타로 Microsoft에서 제작)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>라이브러리에 대한 러닝커브</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">공식 문서를 참고하여 install부터 test까지 어려움이 없었음<br>메서드에 대한 이해가 필요하나 cypress 설치 시 제공해 주는 예제를 읽으면 쉽게 이해 가능<br>초기 난이도: 하</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">공식 문서를 참고하여 install부터 test까지 어려움이 없었음<br>메서드가 직관적이어서 쉽게 이해 가능<br>초기 난이도: 하하</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>빗버킷 자동화 파이프라인 구축(로컬)</strong><br><strong>자동화 구축이 얼마나 쉬운지</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">난이도: 쉬움<br>bitbucket-pipelines.yml에 관련 설정 추가<br>단, 공식문서에 나와있지 않은 추가 로직이 필요하여 구축 난이도 자체는 쉽지만 조금의 검색 능력이 필요함</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">난이도: 쉬움<br>bitbucket-pipelines.yml에 관련 설정 추가<br>단, 공식문서에 나와있지 않은 추가 로직이 필요하여 구축 난이도 자체는 쉽지만 조금의 검색 능력이 필요함</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>빗버킷 자동화 파이프라인 구축(로컬)</strong><br><strong>자동화 시 테스트 소요 시간</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">4개 테스트 평균: 약 1m 11초</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">4개 테스트 평균: 약 1m 34초</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>공통점</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">공식 문서를 참고하여 install부터 test까지 어려움이 없었음</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">공식 문서를 참고하여 install부터 test까지 어려움이 없었음</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 명령 방식</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">메서드 위주의 명령 + 문자열 명령</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">메서드 위주의 명령</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 소스코드 확인</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">소스코드를 보기 위해 외부 프로그램 사용</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">Playwright 내부에서 소스코드 확인 가능</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 실행 방식</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">터미널 명령어 입력 후<br>- 실행창<br>- 테스트창</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">터미널 명령어 입력 후<br>- 테스트창</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - VSCode 확장</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">없음</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">VSCode에서 익스텐션 제공하여 간단히 테스트를 실행할 수 있다</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 디버깅 기능</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">headless에서 오류 발생 시 오류 발생지점의 스크린샷을 자동으로 기록한다<br>console.log()를 이용할 수 있다</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">오류파악을 위한 기본적인 디버그 기능만 제공함</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 테스트 속도</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">같은 로직일 경우 Cypress의 구조가 더 간결하고 테스트 속도도 빠름<br>CI에서도 빠를까?<br>간단한 로직(페이지 이동, 페이지에 문자열 확인, 유효성 메시지 확인)으로 테스트 시 headless, unHeadless, CI 모두 Cypress가 빨랐다. 하지만 이는 실제 소스코드를 기반으로 한 것이 아니기 때문에 신뢰성이 떨어짐</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">-</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - UI 모드 브라우저 테스트</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">UI 모드로 테스트 시 여러 브라우저를 한번에 테스트하는 것이 불가능</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">UI 모드로 테스트 시 여러 브라우저를 한번에 테스트하는 것이 가능</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 병렬 실행</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">병렬적 실행은 sorry-cypress 외부 라이브러리 필요<br>또는<br>병렬 실행을 위해 Cypress Cloud 유료 구독이 필요</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">병렬 실행 무료 지원</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - Codegen</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">Codegen:<br>experimental 기능으로 Cypress Studio가 있다</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">Codegen:<br>locator 자동 생성<br>npx playwright codegen [주소]</td>
        </tr>
        <tr>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;"><strong>차이점 - 리포트</strong></td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">리포트:<br>테스트 결과를 파일로 하나의 파일로 내보내기 위해서는 추가적인 npm 설치와 설정이 필요</td>
            <td style="word-wrap: break-word; overflow-wrap: break-word; white-space: normal; word-break: break-word;">리포트:<br>config파일에 속성을 추가하여 파일 내보내기 가능</td>
        </tr>
    </tbody>
</table>



<br>

---


# cypress에서 playwright으로 전환한 사례
> - [E2E Test 도구 Playwright 전환기](https://dongwonnn.dev/posts/transfrom-playwright)
> - [Playwright](https://seungahhong.github.io/blog/2023/04/2023-04-02-playwright/> #playwright-%ED%98%84%EC%8B%A4%EC%A0%81%EC%9D%B8-%EC%A0%81%EC%9A%A9%EB%B0%A9%EC%95%88)
> - [Migrating from Cypress to Playwright](https://medium.com/@lucgagan/migrating-from-cypress-to-playwright-a7add04b02b3)
> - [Why we switched from Cypress to Playwright](https://www.bigbinary.com/blog/why-we-switched-from-cypress-to-playwright)

위의 사례에서 cypress에서 playwright로 마이그레이션 한 이유는 동일했다.

자동화 소요시간을 줄이기 위해. 근데 Cypress의 병렬실행은 유료다.

마이그레이션의 원인은 cypress를 사용하던 유저들의 bitbucket, github에서 제공하는 CI 테스트 시간은 제한적인데 E2E가 CI 시간에 많은 비중을 차지하였고 제한적인 시간 안에 빠르게 테스트하기 위해 병렬실행을 무료로 제공해주는 playwright으로 마이그레이션했다.

어떤 이는 미약하게 테스트 속도가 빨라졌다고 했지만 결론적으로, 모든 유저가 테스트 속도가 빨라진 것을 경험했다고 한다.


<br>

---

참고

- [vue3 테스트 종류](https://ko.vuejs.org/guide/scaling-up/testing#e2e-testing)

- [cypress, playwright 비교 및 테스트 기준](https://velog.io/@osohyun0224/E2E-%ED%85%8C%EC%8A%A4%ED%8A%B8-Playwright-vs-Cypress-%EC%A4%91%EC%97%90-%EB%AD%98-%EC%93%B8%EA%B9%8C)

- [빗버킷 playwright 연동](https://kailash-pathak.medium.com/how-to-integrate-playwright-with-ci-cd-bitbucket-pipeline-189bf823de19)

- [빗버킷 파이프라인 공식문서](https://support.atlassian.com/bitbucket-cloud/docs/cache-dependencies/#Pre-defined-caches)

- [cypress 공식 문서](https://docs.cypress.io/app/get-started/why-cypress)

- [playwright 공식 문서](https://playwright.dev/docs/intro)

**E2E란?**

- [엔드투엔드(E2E) 테스트란 무엇인가? 명확하게 설명하다](https://apidog.com/kr/blog/e2e-testing-kr/)
- [테스트 자동화의 시작 - Cypress 기반 E2E 테스트 도입기](https://developers.kakaomobility.com/docs/techblogs/e2e-testing/)
- [E2E 테스트 도입 경험기](https://tech.kakaoent.com/front-end/2023/230209-e2e/)

