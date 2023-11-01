---
title: Chromatic 과연 믿을만한가?
date: 2023-09-20
categories: [Storybook]
tags: [Storybook, Chromatic] # TAG names should always be lowercase
pin: true
---

***

Chromatic이란 무엇인가?  

간단히 말해서 Storybook에서 만든 것으로,  
Storybook을 배포해주고 UI test 등을 하게 해주는 cloud-based 체인이다.

***
<br>
문제 제기  

깃을 연동해야 하는 만큼 회사 코드 노출에 대한 위험은 없을까?  
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/f4a90ab6-fa08-4852-a854-ea5e0a29a3aa){: width="50%"}

<br>
Storybook 글로벌 커뮤니티가 있다. (Storybook 공식문서에서 찾을 수 있다.)  
처음에는 거기다가 ‘*Chromatic 믿을만한가요?’* 라는 문의글을 올렸다.  
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/8640aa6f-855d-4161-9f41-9643108b6e01){: width="50%"}

답글에는 ‘Intercom bot’ 을 이용해보라고 했다.  
나는 처음에 그냥 bot과의 대화인줄 알았지만, 그게 아니라 Chromatic의 직원과 대화할 수 있는 수단이었다.  
(파파고 번역을 해보니 intercom 은 한국말로 ‘인터폰’이었다.)  

<br>
그래서 chromatic에 있는 메세지를 이용하여 문의를 했다.  
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/39ce0f65-4220-4920-91cb-0a116dacdee4)  

<br><br>
대화 내용  
```
Conversation with Chromatic
Started on September 20, 2023 at 01:27 AM Central Time (US & Canada) time CDT (GMT-0500)

---

01:34 AM | 꼬끼: Hi I'm considering using Chromatic. 
But i'm not sure that it's reliable. What I mean about reliable is... 
I'm going to using this for my work. and I hope company's source code can't be exposed to anyone. 
but Chromatic can access my git. Therefore I'm quite a bit worried about being exposed.

01:42 AM | Hannah from Chromatic: Hi 꼬끼,

Thank you for contacting us at Chromatic! I hope you're doing well?😄

Rest assured, we don't access your private data, source code, or deploy keys.

You can learn more about permissions here (https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps#available-scopes).

For more information on security at Chromatic, please go to: https://www.chromatic.com/docs/security/
﻿
﻿Here, you can request access to reports and policies and view the controls we continuously monitor.
﻿
﻿If you have additional questions, please let me know!

Best regards,
Hannah
@ Chromatic

01:46 AM | 꼬끼: Thank you for answering me!
Have a good day.

---

Exported from Chromatic on September 20, 2023 at 01:46 AM Central Time (US & Canada) time CDT (GMT-0
```

<br><br>
답변에서 말한 문서를 확인했다.  
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/e21f0bcb-cfaa-459b-a1a7-560948307b07)  

보안 정책에서 말한 내용과 모니터링 결과 등을 봤지만.. 결국 중요한 것은 해당 자료에 대한 내 믿음이었다.  
위의 내용에도 불구하고 chromatic에 회사 코드를 올리기엔 내용증명이 부족하다고 판단했다. 동료의 의견으로는 git, bitbucket 같은 규모면 괜찮았을 것 같다고 했다.  

그래서 다른 회사들은 스토리북을 어떻게 배포를 했나 열심히 구글링을 했지만 안타깝게도 스토리북 도입관련한 글은 몇개 있었지만 배포에 대한 언급은 없었다. 이러한 까닭에 더 신뢰가 안가는 것도 사실이었다.  

***
<br>
스토리북 배포를 위해 백엔드 개발자에게 부탁하는 것이 또다른 업무/부담을 주는 것 같아서, 우선으로는 내가 마무리를 지을 수 있는지 찾아보았다.  

1️⃣그 첫번째 시도로는, 스토리북만 레포지토리와 운영 소스코드의 레포지토리를 따로 운영하는 것이었다. 하지만 생각보다 많은 의존성 문제가 있었다. 스토리북 레포지토리에 기존에 필요한 코드/라이브러리 등을 중복적으로 생성해야하는 것들이 너무 많았다.  

<br>
2️⃣그러다 발전한 것이 두번째 시도인,  Git 서브모듈이다. 서브 모듈이란 쉽게 말해  레포지토리에 필요한 레포지토리를 클론하는 것이다. 서브 모듈을 통해 다른 레포지토리끼리 코드 공유가 가능하다. 지금은 서브모듈에 대한 이해가 어느정도 있었지만 처음 서브 모듈을 접했을 때는 다른 구조로 이해했다.  

나는 처음에 서브모듈을 설명한 아래의 이미지를 보고 공통 모듈 저장소만 따로 관리가 돼서 서브 모듈을 사용해서 회사의 소스코드와 스토리북 코드를 완전히 분리한 다음 chromatic에 배포를 하면 되겠구나 싶었다.  
![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/bf0b9c15-6a88-4ac5-8efe-cb22bfd6744f){: width="50%"}  

하지만 **공통 모듈 저장소가 분리돼 있는 것이 아닌 참조할 레포지토리를  메인 레포지토리 안에 클론한다는 개념이 더 적절하다.**  
결국 이렇게 되면 코드의 완전 분리는 되지 않기 때문에 chromatic에 소스코드가 노출된다.  
그래서 결국 서브 모듈 만들기라는 방법도 배제했다.  

<br>
3️⃣다행히 동료와 이야기를 나누다가 쉽게 문제가 해결되었다. 남는 서버가 있었고 이미 깔려있는 톰캣에 빌드파일을 옮기면 됐다. 결국엔 누군가의 도움을 얻었지만 백엔드 개발자가 뭔가 수고스럽게 안해도 돼서 다행이었고 추후에도 내가 관리할 수 있다는(관리도 쉬움) 점이 아주 만족스러운 결과였다.  

그와 동시에 여러 사람과 이야기를 주고 받으며 얻는 지식이 많다는 것을 깨달았다.  
맨 처음엔 누군가의 도움 없이 배포 하려고 했다. 하지만 이 글을 쓰다보니 도움 없이 발전이 없었지 않았나 싶다.  

<br>
결론: 동료  
<br><br>

---
사실 위 과정에서 만들었던 계정을 지우면서 chromatic에 대한 신뢰가 더 낮아졌다. 계정 삭제를 하기 위해서는 메세지를 보내야하고, 메세지를 보내기만 하면 추가적인 절차없이 바로 탈퇴를 시켜준다는 점이 요인이었다.  

![image](https://github.com/kokiok3/kokiok3.github.io/assets/84312457/244f50a8-a351-4ea8-b1a5-beeeec6014a7)  

신기한 점이 계정 삭제 요청 메세지를 보낸 후에 계정 삭제가 되었는데도 브라우저를 껐다 켜도 메세지 내의 내 이름과 내용이 유지가 된다는 것이다. 이 부분도 물어보려고 했는데 탈퇴한 계정이 아닌 다른 계정으로 로그인/로그아웃을 하니까 메세지의 내용이 최근 계정의 것으로 바뀐 것을 보고 관두었다.  
<br><br>

---
참고

[https://engineering.linecorp.com/ko/blog/ui-component-library-for-developers-with-typescript-storybook](https://engineering.linecorp.com/ko/blog/ui-component-library-for-developers-with-typescript-storybook)

[https://moonsupport.oopy.io/post/3](https://moonsupport.oopy.io/post/3)