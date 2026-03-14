---
title: Vue Transition 컴포넌트는 오직 최상위 태그에만 적용된다.
date: 2026-03-10
categories: [깨달음 한줄]
tags: [vue, transition] # TAG names should always be lowercase
pin: true
---


# 한 줄
### Vue의 Transition 컴포넌트는 내부의 최상위 단일 루트 엘리먼트에 애니메이션 클래스를 주입한다.

Vue에서 제공하는 내장 컴포넌트인 Transition을 사용하면 엘리먼트의 나타남(enter)과 사라짐(leave)에 따른 애니메이션을 구현할 수 있다.
Transition에 name 속성을 부여하면, 활성화 시점에 따라 .[name]--enter-active, .[name]--leave-active 등 약속된 CSS 클래스가 자동으로 부착된다.

**이때 중요한 점은 내부 HTML구조가 반드시 단일 엘리먼트여야 한다는 것이다.**

<br>

# 문제
### 현상: 트랜지션이 작동하지 않고 UI가 뚝뚝 끊기며 나타남.


```
// 예시 코드

  <Transition>
    <div class="parent">
      <div class="option__list">
      </div>

      <div class="background">
      </div>
    </div>
  </Transition>


// css
  .option__list {
    position: absolute;
  }
  .background {
    position: fixed;
    width: 100%;
    height: 100%
  }

```
1. Transition 바로 아래의 최상위 태그는 단순히 감싸는 용도로만 사용되어 아무런 스타일이 없었음
2. 실제 애니메이션이 필요한 .option__list에만 position: absolute가 부여되어 있었음

3. 이로인해 최상위 태그는 높이와 너비가 0인 상태가 되었고, 브라우저에서 애니메이션이 보이지 않음
4. 결과적으로 최상위 태그에 붙은 트랜지션 클래스는 빈 박스에만 적용되고, 실제 콘텐츠인 .option__list는 트랜지션 영향권 밖에서 뚝뚝 끊기며 나타났다 사라지는 현상 발생


<br>

# 해결
* 최상위 루트 엘리먼트에 position: absolute를 부여하여 레이아웃 흐름을 일치시킴.


<br>

---
# 참조
- [Vue.js 트랜지션](https://ko.vuejs.org/guide/built-ins/transition){:target="_blank" rel="noopener noreferrer"}
