---
title: Github Fork가 풀렸을 때
date: 2023-11-23
categories: [Github]
tags: [Github, fork, private] # TAG names should always be lowercase
pin: true
---

***
<br>
팀 프로젝트를 위해 fork해서 가져온 레포지토리의 fork가 어느순간 풀려 있었다.  
원래는 아래의 이미지 처럼 내 레포지토리 명 하단에 fork의 원본을 표시해줘야 하는데 fork from 이 없어졌다.  

![image](https://github.com/kokiok3/OlymPos/assets/84312457/89b9a5b8-621b-4bcf-9f24-4499e03a76c3)

![image](https://github.com/kokiok3/OlymPos/assets/84312457/7f5d161a-12e3-499c-aa3f-0c205dd10a35)  

fork가 풀린건, 풀리퀘스트를 할 때 알게 되었다.  
풀리퀘스트를 할 때, fork 한 원본 레포지토리에 요청을 하는데 해당 레포지토리가 보이지 않았기 때문이다.  

원인은 즉슨, 원래 Private이었던 원본 레포지토리를 Public으로 바뀌면서 fetch가 풀렸던 것이다.

<br>

***
<br>
fork 상태로 되돌리기 위해 두가지 방법이 있었다.  

1️⃣ 하나는 이전 상태로 되돌리기인데, 원래 Private이었던 원본 레포와 나의 레포를 다시 Private으로 전환한 뒤 Github에 문의를 하는 방법이고,  
2️⃣ 다른 방법은 Public 상태의 원본 레포를 다시 fork 한 뒤 구fork로컬 레포를 머지하는 것이다.  

<br>

---

## 방법

1. 원본 레포지토리를 fork 한다.
2. fork한 레포지토리를 로컬에 clone 한다.
3. fork한 레포지토리에 구fork 레포지토리를 머지한다.
   1. git bash 또는 Gui를 이용해서 fork 레포지토리에 구fork레포를 가져온다.(나는 git 명령어를 이용했다.)  
      `git remote add [레포이름] [경로]`  
      `git remote update`  

      나같은 경우에는 git remote update 후에, 구fork레포에 있던 여러 브랜치들이 신fork레포에 생성이 되었다.  
        ![image](https://github.com/kokiok3/OlymPos/assets/84312457/7af52c39-c65b-467b-90c2-f424ba2c1ff6)  

    2. 생성된 브랜치를 원격 저장소에 push한다.  
       push하기 위해서는 push할 로컬 브랜치로 체크아웃을 해준다.  
       `git switch [브랜치 명]`  

    3. 로컬 브랜치를 원격 저장소에 push 한다.  
        `git push origin [브랜치 명]`  

<br>

---

## 끝맺으며
* github에서 private 레포지토리의 fork가 public으로 전환하면서 풀린다는 사실을 알게 되었다.
* 평소에 bitbuckt 또는 github desktop과 같은 GUI를 많이 써서, git 명령어는 자주 안쓰는데 이번 기회에 써볼 수 있어서 재밌었다.

<br>

---
참고

**Merge git repo into branch of another repo**  
[https://stackoverflow.com/a/21353836/16105032](https://stackoverflow.com/a/21353836/16105032)

**Git clone, remote 차이**  
[https://choiiis.github.io/git/how-to-clone-project/](https://choiiis.github.io/git/how-to-clone-project/)


**Git 로컬 브랜치를 원격 저장소로 푸시하기**  
[https://www.freecodecamp.org/korean/news/git-push-to-remote-branch/](https://www.freecodecamp.org/korean/news/git-push-to-remote-branch/)