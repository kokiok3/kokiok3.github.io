---
title: 챕터 4 더 큰 그림
date: 2024-11-26
categories: [자바스크립트, You Don't Know JS]
tags: [javascript, You Don't Know JS] # TAG names should always be lowercase
pin: true
---

***
> JS를 지탱하는 세 개의 주요 기둥과 Appendix


### **목차**
[**첫 번째 기둥: 스코프와 클로저**](#첫-번째-기둥:-스코프와-클로저)  
[**두 번째 기둥: 프로토타입**](#두-번째-기둥:-프로토타입)  
[**세 번째 기둥: 타입과 타입 강제 변환**](#세-번째-기둥:-타입과-타입-강제-변환)  
[**JS의 본질 따르기**](#JS의-본질-따르기)  
[**Appendix A**](#Appendix-A)  

<br>

# 서론
챕터 4에서는 JS를 지탱하는 세 개의 주요 기둥과 2부에서는 어떤 내용을 다룰지, 책 전개는 어떻게 되는지 로드맵이 나온다. 해당 포스팅에는 세 개의 기둥과 Appendix A만 다룬다.

---

# 첫 번째 기둥: 스코프와 클로저

스코프를 함수/블록 단위로 변수의 유효 범위를 한정 짓는 것은 모든 프로그래밍 언어의 근본적인 특징이다.  


스코프는 양동이, 변수는 구슬에 비유할 수 있다.  
![변수](https://github.com/user-attachments/assets/8d7d5ef0-4c60-40b4-bb9a-5d74d063016c){: width="50%"}

스코프 안에는 다른 스코프가 올 수 있다.  
해당 레벨의 스코프 혹은 더 높거나 바깥 레벨에 있는 변수에 접근 가능  
낮거나 안쪽 레벨 스코프에 있는 변수에는 접근 불가능  
![변수2](https://github.com/user-attachments/assets/ac6859a6-536c-4cb2-a713-7dcdee9b8b4b){: width="35%"}

![렉시컬](https://github.com/user-attachments/assets/be102bfc-81d6-4df4-a405-120736ebc66f)

렉시컬 스코프는 개발자가 코드를 작성할 때 결정된다.   

js도 렉시컬 스코프 모델 채택  
하지만 다른 두가지 특징이 있다.  
* 호이스팅
* var 변수

### 호이스팅
변수가 선언된 위치와 상관없이 스코프 시작 부분에서 선언한 것 처럼 처리

### var 변수
함수 기준으로 스코프 생성  

<details>
  <summary style="padding: 10px; background-color: #f4f4f4; border-radius: 10px;">'var변수를 사용해 선언한 변수는 해당 변수를 선언한 블록 위치와 상관없이 함수를 기준으로 스코프를 만든다.' 가 무슨뜻이야</summary>

<div style="padding: 10px; border: 1px solid #ddd;">
  var로 선언한 변수는 <b>함수 스코프(function scope)</b>를 따르고, <b>블록 스코프(block scope)</b>를 따르지 않는다는 의미입니다. 이를 조금 더 자세히 설명하면 다음과 같습니다:    
  <br>
  <h2>1. 함수 스코프와 블록 스코프의 차이</h2>
  <li>
  <b>함수 스코프:</b> 변수가 선언된 위치가 함수 내부라면, 함수 전체에서 해당 변수를 사용할 수 있습니다.
  </li>
  <li>
  <b>블록 스코프:</b> 변수가 선언된 위치가 특정 블록({}) 안이라면, 해당 블록 내부에서만 변수를 사용할 수 있습니다.
  </li>

    <h2> 2. var는 함수 스코프를 따른다</h2>
    var로 선언된 변수는, 선언된 위치와 상관없이 함수 내부라면 어디에서든 접근 가능합니다.
    반면 let과 const로 선언된 변수는 블록 스코프를 따라 블록 내부에서만 유효합니다.  

<br>
  예제:  
<br>

<pre style="background-color: #f4f4f4;
            border: 1px solid #ddd;
            padding: 10px;
            overflow-x: auto;
            font-family: 'Courier New', Courier, monospace;
            line-height: 1.5;">
  <code>
  function example() {
      if (true) {
          var x = 10; // var로 선언
          let y = 20; // let으로 선언
      }
      console.log(x); // 10 (함수 스코프이므로 접근 가능)
      console.log(y); // Error (y는 블록 스코프이므로 블록 밖에서 접근 불가)
  }
  example();
  </code>
</pre>

  <h2> 3. var의 호이스팅과 함수 스코프</h2>
  var는 호이스팅(hoisting)이라는 동작으로 인해 선언문이 변수 선언부만 함수의 최상단으로 끌어올려진 것처럼 동작합니다. 하지만 값은 할당된 위치에 따라 결정됩니다.  

  예제:  

  ```javascript
  function example() {
      console.log(x); // undefined (변수 선언은 호이스팅되지만 값 할당은 이후에 일어남)
      var x = 10;
      console.log(x); // 10
  }
  example();
  ```

  위의 코드에서 console.log(x)가 함수 초반에 실행되었음에도 오류가 나지 않는 이유는 호이스팅 때문입니다. var x; 선언이 함수 맨 위로 끌어올려진 것처럼 동작하기 때문입니다.  

  <h2> 4. 정리</h2>
  var는 함수 스코프를 가지며, 블록(if, for 등)을 무시하고 함수 전체에서 유효합니다.
  let과 const는 블록 스코프를 가지며, 선언된 블록을 벗어나면 사용할 수 없습니다.
  var는 호이스팅으로 인해 선언이 함수의 최상단으로 끌어올려지는 것처럼 동작합니다.
</div>

</details>

<br>

위의 두가지 특징은 js가 렉시컬 스코프 모델을 사용하지 않는다는 주장의 근거로 사용될 때가 있지만 (앞서 말했듯이 렉시컬 스코프는 개발자가 정의한대로 함수 정의 위치에 따라 스코프 체인이 결정되기 때문이다. 근데 호이스팅은 정의 위치와 상관없이 스코프 시작 부분에서 선언한 것 처럼 처리),  
이는 부적합하다. var가 아닌 let/const는 TDZ 특징 때문에 오류가 난 것 처럼 특이하게 작동하지만 이것이 JS가 렉시컬 스코프 모델을 사용하지 않는다는 이유가 될 수 없다.  

> **TDZ(Temporal Dead Zone) 란**  
> : 초기화되지 않은 변수가 있는 곳. 즉 초기화 전에 변수 사용을 허용하지 않는다.    
>
> **var 변수 라이프 사이클**  
>    선언 + 초기화 -> 할당  
>   
> **let 변수 라이프 사이클**  
>    선언 -> TDZ -> 초기화 -> 할당
{: .prompt-info }




# 두 번째 기둥: 프로토타입
js는 객체 리터럴로 객체 생성이 가능  
프로토타입을 사용하면 this 컨텍스트가 공유되면서 두 객체를 간편하게 연결할 수 있다.  

class 키워드가 등장하면서 프로토 타입 시스템의 아름다움과 힘이 흐려진 상태다.   -> 클래스는 프로토타입을 기반으로 하는 하나의 패턴일 뿐이다.  

작동위임 패턴이 js의 본질에 더 가깝다.  
작동위임이란, 객체는 객체로서 그대로 두고 클래스 없이 프로토타입 체인을 통해 객체가 협력하도록 함.  
클래스 상속보다 더 강력.


# 세 번째 기둥: 타입과 타입 강제 변환
JS 본질에서 가장 간과되는 영역  

타입스크립트,플로 같은 정적 타입 개발 필요성도 이해하지만 똑똑하고 명확하게 타입설정하기 위해 꼭 정적 타입이 필요한건 아니다.  

저자는 개인적으로 앞서말한 두가지 보다 제일 중요하다고 생각.  

JS에서 어떻게 타입을 다루고 타입 강제 변환시기에 대해 배울거임.  


# JS의 본질 따르기
* TC39 명세를 기반하라  
* JS 고유 방식을 배우고 습득하는게 먼저.  
* 가독성 향상에 도움을 주는 방식이 있는지 탐구하라.





---

# Appendix A

## 1. 값 vs 참조
값 자체와 참조는 값의 타입으로 결정  
원시타입 - 복사  
객체타입 - 참조  

```javascript
// 예제1
let my = 'my';
let you = my;

you = 'you';
console.log('my:', my);
console.log('you:', you);

// 예제2
let myArr = ['my', 'me', 'i'];
let youArr = myArr;

youArr.push('you');

console.log('myArr:', myArr);
console.log('youArr:', youArr);
```

## 2. 다양한 형태의 함수
저는 책에서 다룬 여러 형태의 함수 중에서 익명 함수 표현식만 설명하겠습니다.

익명 함수 표현식 => 이름 추론  
하지만 = 연산자로 할당을 했을 때만 이름 추론이 가능하다.  
만약 변수에 기명함수표현식을 할당했을 때, 기명함수의 식별자가 변수명보다 우선순위를 가진다.  
기명 함수를 쓰는 것이 디버깅했을 때 코드 유추가 쉽다.


## 3. 강제 조건부 비교
중요한 점은 비교하기전에 강제 변환이 일어난다.  

## 4. 프로토타입 클래스
프로토타입 클래스란 프로토타입 연결장치를 사용하는 방법이다.  
ES6에서 클래스 시스템이 등장하기 전까지 프로토타입 클래스는 객체를 연결하는 역할을 했다.  
프로토타입을 통해 객체를 연결하면 연결돼 있는 객체의 함수를 위임할 수 있다. 이는 상속이라고 부른다.  

클래스 메커니즘과 프로토타입 클래스 모두 저 밑바닥은 동일한 프로토타입 연결 장치로 설정되어 있다. 하지만 프로토타입 클래스 패턴보다는 ES6의 클래스 메커니즘을 쓰는게 훨씬 낫다.




---


참고

[TDZ (Temporal Dead Zone) 이란?-예시가 도움됨](https://velog.io/@hoo00nn/TDZ-Temporal-Dead-Zone-%EC%9D%B4%EB%9E%80){:target="_blank"}

[TDZ(Temporal Dead Zone)이란?-설명이 도움됨](https://noogoonaa.tistory.com/78){:target="_blank"}

[일급 객체(first-class object) 란?](https://inpa.tistory.com/entry/CS-%F0%9F%91%A8%E2%80%8D%F0%9F%92%BB-%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4first-class-object){:target="_blank"}

[챗지피티에게 질문한 내용](https://chatgpt.com/share/674728d5-0bd4-800c-b0fa-16d40200da8a){:target="_blank"}

