---
title: Jekyill Chripy 테마에서 avatar 변경 안되는 이유
date: 2023-02-26
categories: [Git blog]
tags: [jekyill, chripy, avatar] # TAG names should always be lowercase
---

깃 테마를 적용한 후 블로그를 꾸며 보기 시작했다.

다른 블로그에서 설명한대로 avatar를 바꾸기 위해 원하는 이미지 파일을 root/assets/img 경로에 붙여 넣었다.
![image](https://user-images.githubusercontent.com/84312457/221367496-349717c7-2a3e-40af-a2b5-867b0d5483db.png){: width="50%"}

하지만 avatar를 확인해 보면 자꾸 검은색으로 떴다.
![image](https://user-images.githubusercontent.com/84312457/221367453-18db9464-2b13-40c7-97c2-22c0d343357d.png){: width="50%"}

오류의 원인을 확인하기 위해 우선 개발자 도구에서 avatar의 상태를 확인해 봤다.

분명 경로는 잘 들어가 있다. 근데 style이 display:none인 것을 확인할 수 있었다.
![image](https://user-images.githubusercontent.com/84312457/221367666-4efbb128-c398-42c9-a3c2-5d5e841972ac.png)

display: none을 해제시켜봤지만 avatar는 엑박만 떴다.

![image](https://user-images.githubusercontent.com/84312457/221367838-8f08418e-ec68-4f4a-917c-f9f4a1d7cb38.png)

문제의 원인을 알기 위해 페이지를 만들어주는 파일을 찾아내어 확인을 해봤더니

오류가 있을 때는 display 속성을 none으로 해주는 것을 발견할 수 있었다.
![image](https://user-images.githubusercontent.com/84312457/221368135-1ce0ad31-a9a3-4741-9ab7-41bf4cc420fc.png)

그 후로 경로가 잘못되었나 싶어서 default 값이었던 commons/파일명으로 경로도 바꿔보고 파일 경로에 큰따옴표도 넣어봤다. 아무것도 안되던 찰나에 chripy 테마를 쓰는 다른 사람의 블로그를 보고 내 것과 어떤 차이가 있는지 확인해야겠다는 생각이 들었다.

그렇게 찾은 랜덤의 블로그...

아바타 적용이 잘 되어있네.. 도대체 뭐 때문일까 궁금해..!!

개발자 도구로 검사한 결과 내 경로 앞에는 "https://chirpy-img.netlify.app" 주소가 추가로 붙어있었다.

알고 보니 \_config.yml에서 img_cdn에 기본값이 있는데 그게 "https://chirpy-img.netlify.app"였다.
![image](https://user-images.githubusercontent.com/84312457/221368885-2ac96b43-690f-42d0-ac53-024e92ceaf60.png)

어쩐지 맨 처음에 avatar를 바꾸기 위해 avatar의 기본값인 commons/avatar.jpg에서 commons 폴더를 찾는데 안 찾아지더랬지.

img_cdn의 주석을 읽으니 img_cdn에 cdn 주소를 입력한 뒤 avatar에서는 "/" + 파일명으로 cdn의 이미지를 불러올 수 있다고 한다.

결국 img_cdn의 주소는 주석처리를 했다.
\_config.yml파일 변경 후 새로고침으로는 브라우저에서 변경된 사항을 확인할 수 없기 때문에 꼭 bundle exec jekyll serve를 다시 하고 확인해야 한다.
![image](https://user-images.githubusercontent.com/84312457/221369322-903ee1ce-a12a-44b7-9d1b-4d936cca6ae9.png)

짜잔-

드디어 avatar가 적용이 되었다.

적용된 모습을 보니 사이즈가 잘 안 맞아서 다시 적용을 해야겠다.

![image](https://user-images.githubusercontent.com/84312457/221369431-33835abf-c961-47df-8ec3-ea2c79ff961b.png)

다른 블로그를 보고 avatar의 경로만 바꾸면 될 줄 알았지만 cdn 주석 처리까지 해줘야 한다는 걸 알게 됐다. 별거 아닌 것 같지만 생각보다 구글 검색에 나오지 않아서 5분 만에 끝내야 할 일이 배로 들게 됐다. 이 글을 본다면 avatar 적용에 애를 먹지 않고 다른 것에 더 많은 시간 투자를 할 수 있을 것이다.
