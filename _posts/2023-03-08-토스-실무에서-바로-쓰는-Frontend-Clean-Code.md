---
title: 토스 | 실무에서 바로 쓰는 Frontend Clean Code
date: 2023-03-08
categories: [개발자 컨퍼런스, 토스]
tags: [개발자 컨퍼런스, 토스, 클린코드] # TAG names should always be lowercase
pin: true
---

> 클린 코드는 짧은 코드가 아니다. 원하는 로직을 빠르게 찾을 수 있는 코드이다.

***

# 실무에서의 클린 코드의 의의 = 유지 보수 시간의 단축

우리는 보통 코드를 새로 짜는 것보다 수정하거나 추가하는 일을 많이 한다.  
하지만 코드를 계속 추가하다 보면 지뢰 코드가 되기 십상이다.

![image](https://user-images.githubusercontent.com/84312457/224027682-bb6dc37b-faed-4dfc-b19b-0a585d868e8a.png)

**지뢰 코드**는 흐름 파악이 ❌, 도메인 맥락 표현이 ❌, 동료에게 물어봐야 알 수 있는 코드이다.

<br>

## 우리가 주목할 것은 안일한 코드 추가의 함정이다.  
기능 추가 전에는 깔끔한 코드였다. 하지만 기능 추가 후에 더러워지는 경우가 있다.
바로, 하나의 목적인 코드가 흩뿌려진 경우이다. 이러한 경우 점점 지뢰코드가 되는 것이다.  
<br>
앞서 말한 지뢰 코드는 기능을 세부 목적별로 구분함으로 개선할 수 있다. 자 그럼 지뢰코드를 만들지 않고 클린코드를 짤 수 있는 방법을 알아보자.

<br>

## 클린 코드 ≠ 짧은 코드
> 클린 코드 = 원하는 로직을 빠르게 찾을 수 있는 코드이다.

👇아래에 클린 코드를 위한 방법 3가지가 있다.

| 클린 코드를 위한 방법 3 |
| :---: |
| 응집도 |
| 단일 책임 |
| 추상화 |

<br>

## 응집도
* 하나의 목적을 가진 코드가 흩뿌려져 있을 때 응집도를 높여 뭉친다.
* 핵심 구현만 빼놓고 세부 구현은 안쪽에(컴포넌트) 뭉친다.
  
이러한 특징은 선언적 프로그래밍을 갖는다. 코드에서 **무엇을 해야 하는지만 알려주고 나머지는 세부 구현한다.** 장점으로 코드 이해가 쉽다.
  
  ```javascript
// 선언적 프로그래밍 예시
<Popup 
    onSubmit={회원가입}
    onSuccess={프로필로이동}
>
```
```javascript
// 명령형 프로그래밍
// 단점으로 코드 파악이 어렵다.
<Popup>
    <button onClick={async () => {
        const res = await 회원가입();
        if(res.success){
            프로필로이동();
        }
    }}>전송</button>
</Popup>
```
<br>

## 단일책임
* 중요 포인트가 모두 담겨있는 함수명을 가져야 한다.  
* 한 가지 일만 하는 명확한 함수명 / 한 가지 일만 하는 기능성 컴포넌트 
* [영어 이름이 너무 길 때 / 상수를 직관적으로 보고 싶을 때 / 복잡한 조건문이 많을 때]는 한글 이름이 유용하다. 

**👇아래의 코드는 위험한 함수명 예시이다.**  
답은 코드 아래에 있으니 생각해 볼 것!
```javascript

async function handle질문제출() {
    const 약관동의 = await 약관동의_받아오기();
    if(!약관동의){
        await 약관동의_팝업열기();
    }
    await 질문전송(questionValue);
    alert("질문이 등록되었어요.");
}
```
<br>
>답: 중요 포인트가 모두 담겨있지 않다. 함수명은 '질문 제출'이지만 내용에는 약관 동의와 질문 제출이 있어서 적절하지 않은 함수명이다. 만약 위의 코드에서 기능을 추가한다면 미래의 지뢰 코드가 될 수 있다.
**이를 위해 한 가지 일만 하는 함수로 나누자.**

<br>

### 추상화

* 핵심 개념을 필요한 만큼 노출(추상화) 한다.

피카소의 소 그림을 보면 제일 왼쪽의 소처럼 구체적인 '소'에서, 디테일을 점차 지워서 맨 오른쪽의 소처럼 큰 특징만 표현한다.
![image](https://user-images.githubusercontent.com/84312457/224037448-cbbd21ca-e5e8-4140-95d5-5f0ff7b0c5e7.png)

* 코드 추상화 : 컴포넌트  

```javascript
// 중요한 개념만 남기고 추상화
<Popup 
    onSubmit={회원가입}
    onSuccess={프로필로이동}
>
```
```javascript
// 모두 구현
<Popup>
    <button onClick={async () => {
        const res = await 회원가입();
        if(res.success){
            프로필로이동();
        }
    }}>전송</button>
</Popup>
```
<br>

* 코드 추상화 : 함수  

```javascript
// planner 라벨을 얻는 코드 세부 구현
const planner = await fetchPlanner(plannerId)
const label = planner.new ? '새로운 상담사' : '연결중인 상담사'
```
```javascript
// 중요 개념을 함수 이름에 담아 추상화
const label = await getPlannerLabel(plannerId)
```
<br>

* 주의할 점으로는 한 레벨의 코드에서 추상화 수준이 다르면 코드 파악이 어렵다.  
  * 전체적인 코드가 어느 수준으로 구체적으로 기술된지 파악할 수 없다.


```javascript

// 각기 다른 추상화 수준 
<Title>별점 매겨주세요</Title>   // 높은 추상화
<div>                           // 낮은 추상화
    {STARS.map(()=><Star />)}
</div>
<Reviews />                    // 높은 추상화
{rating !== 0 && (             // 중간 추상화
    <>
        <Agreement />
        <Button rating={rating} />
    </>
)}
```

```javascript

// 동일한 추상화 수준 
<Title>별점 매겨주세요</Title>
<Stars />
<Reviews />                 
<AgreementButton show={rating !== 0} />
```
<br>

***

# 내일부터 할 수 있는 클린 코드를 위한 행동
1. **담대하게 기존 코드 수정하기**  
    a. <small>부모의 브랜치를 따서 리팩토링해보기</small>

2. **큰 그림 보는 연습하기**  
    a. <small>지금은 괜찮은 코드여도 기능 추가 후에 더러워질 수 있다.</small>  
    b. <small>또는 기능 추가 자체가 클린해도 큰 그림으로는 어지러운 코드일 수 있다.</small>
   
3. **팀과 공감대 형성하기**  
    a. <small>리뷰를 하자</small>  
    b. <small>명시적으로 코드에 대해 이야기하는 시간이 필요하다.</small>
   
4. **문서로 적어보기**  
   a. <small>향후 위험한 점</small>  
   b. <small>어떻게 개선할지</small>

   <br>

***
# 클린 코드를 적용해 보았다.
입사 전부터 있던 코드로 부분적으로 코드를 수정할 일이 생기면서 해당 코드의 존재를 알게 되었다. 해당 함수를 이해하는것도 오래 걸렸지만 시간이 흐른 뒤 다시봐도 한눈에 알기 힘든 코드였다. 그래서 이번 컨퍼런스의 클린 코드 방법을 사용해 리팩토링을 진행해 보았다.

**리팩토링 시 노력한 것**
1. 함수명이 함수의 기능을 잘 설명할 수 있게 제작
2. 되도록이면 하나의 기능별로 함수 생성
3. 특히나 여러번 중첩된 if문들로 로직의 흐름을 한눈에 알기가 어려운 점 개선

**코드에 대한 간략한 설명**  
* '봇 등록' 버튼을 눌러 구입한 봇을 사용할 수 있다.
* 봇 등록 전 여러 유효성 검사를 거친다.
* 봇 등록 성공 시 '봇 등록'을 진행하고 실패 시 에러 문구를 띄워준다.


### 리팩토링 전 코드
```javascript
<button v-on:click="botConnectionFunc('workbot', index)">
    봇 등록
</button>
```
```javascript
botConnectionFunc(type, index){
    const isBotConnection = true;
    this.$getLocalBotInfo(isBotConnection).then((localData) => {
        if(localData != null){
            this.installedBot = localData.programVersion;
            this.localBotId = localData.botId;
            this.localMac = localData.macAddress;
            this.localPcName = localData.machineName;

            this.botConnectionLogic(type, index);
        }
    });
},

// 2022-02-17 supply : botId 복사 -> 등록
botConnectionLogic(type, index){
    if(this.installedBot==null && this.localMac==null){
        this.$alertError($t("봇이 실행중이 아닙니다."));
        return;
    }

    if(semverLt(semverCoerce(this.installedBot).version, '3.1.2')){
        this.$alertInfo('', $t("설치된 버전이 3.1.2 미만일 경우에는 봇아이디를 복사후 시스템 트레이 아이콘 > 우클릭 > Bot ID 등록으로 진행해주세요."));
        return;
    }
    else{
        let typeName = '';
        if(type == 'designerBot'){
            typeName = 'type1';
        }
        else if(type == 'workbot'){
            typeName = 'type2';
        }

        // 비교자원
        const macDiv = "mac-" + typeName + "-" + index;             
        const macVal = document.getElementById(macDiv).innerText;
        const botIdDiv = "copy-" + typeName + "-" + index;
        const botIdVal = document.getElementById(botIdDiv).innerText;
        // 1. 선택 라이선스에 등록된 mac : null (첫 등록)
        // 2. 선택 라이선스에 등록된 mac, 현재 로컬의 mac : 일치 (진행)
        // 3. 선택 라이선스에 등록된 mac, 현재 로컬의 mac : 불일치 (재확인 선택)

        let confirmParams = {
            "msg" : "",
            "case" : "",
            "botId" : botIdVal,
            "mac" : this.localMac
        }

        if(macVal == "" || macVal == null){
            confirmParams.msg = $t("PC에 라이선스 사용정보를 등록합니다.");
            confirmParams.case = 1; 
            this.registretionBotPermissionConfirm(confirmParams);
        }
        else if(macVal == this.localMac){
            confirmParams.msg = $t("현재 PC에 등록된 라이선스입니다. 재등록 진행하시겠습니까?"); 
            confirmParams.case = 2; 
            this.registretionBotPermissionConfirm(confirmParams);
        }
        else if(macVal != this.localMac){
            confirmParams.msg = $t("다른PC에서 사용중인 라이선스입니다. 등록하시겠습니까?");
            confirmParams.case = 3;  
            this.registretionBotPermissionConfirm(confirmParams);
        }
        else {
            this.$alertError("mac check error");
        }
        return;
    }
},
// 라이선스 매핑등록전 check 확인 함수
// a) 첫번째확인
registretionBotPermissionConfirm(confirmParams){

    this.$alertInfoWithCancle("",confirmParams.msg)
    .then( result => {
        if ( result.isConfirmed ) {
            // console.log("a)확인click");
                if(confirmParams.case === 1 || confirmParams.case === 3){
                    // 1. 선택 라이선스에 등록된 mac : null (첫 등록)
                    // 3. 선택 라이선스에 등록된 mac, 현재 로컬의 mac : 불일치 (재확인 선택)

                    // b) localMac에 다른 라이선스가 등로되어있을 경우 마지막질문 b)으로 이동
                    this.$getBotPermissionByMac(confirmParams.mac).then((data) => {
                        if(data != null && data != ''){
                            // console.log('현재 PC에 등록된 라이선스 있음! -> 질문 b)로이동');
                            // alredyLicenseParams.userId = data.user_id;
                            // alredyLicenseParams.botId = data.botId;
                            this.registretionBotPermissionConfirmFinal(confirmParams);
                            
                        }else{
                            // console.log('현재 PC에 등록된 라이선스 없음! -> 진행');
                            this.registrationBotPermissionFunc(confirmParams.botId);
                        }
                    });
                }
                else if(confirmParams.case === 2){
                    // 2. 선택 라이선스에 등록된 mac, 현재 로컬의 mac : 일치 (진행)
                    // console.log('다시등록이므로 바로 진행! -> 진행');
                    this.registrationBotPermissionFunc(confirmParams.botId);

                }
                else{
                    this.$alertError("registretionBotPermissionConfirm error"); 
                }
        }else {
            // console.log("a)취소");
        }
    });
},

// 라이선스 매핑등록전 check 확인 함수
// b) 두번째확인
registretionBotPermissionConfirmFinal(confirmParams){
    const param = {
        html: $t("<span>현재 PC에 등록된 라이선스가 이미 있습니다.</span><br><span>재등록 진행하시겠습니까?</span>"),
        icon: "warning",
        showCancelButton: true,
        confirmButtonText: $t('확인'),
        cancelButtonText: $t('취소'),
    }
    this.$alertCustom(param)
    .then( result => {
        if ( result.isConfirmed ) {
            this.registrationBotPermissionFunc(confirmParams.botId);
        }
    });
},

// bot_permission 등록 함수
registrationBotPermissionFunc(botIdVal){
    const userId = JSON.parse(sessionStorage.getItem("user")).caller;           
    // if(true){
        var params = {
            "userId" : userId,
            "mac" : this.localMac,
            "botId" : botIdVal,
            "version" : this.installedBot,
            "pcName" : this.localPcName
        }

        // botPermission 정보 등록
        this.$registrationBotPermission(params).then((data) => {
            if(data > 0){
                this.localBotId = botIdVal;

                // 화면출력 리로드
                this.reloadRetrieveLicenseList();
                // wd정보 http 전송
                this.botConnectionStart(botIdVal);
            }else{
                // console.log('registrationBotPermission Fail');
            }
        });
    // }
},

// 봇등록 시작 함수
botConnectionStart(botIdVal){
        // 1. 해당 botId 가져오기
    this.$getWDInfo().then((data) => {
        if(data != null){
            let httpParams = {
                "workDesigner" : {
                    "scheme": data.scheme,
                    "host":  data.host,
                    "port": data.port,
                    "path": data.path,
                },
                "botId": botIdVal
            }

            const cleanBotV = semverCoerce(this.installedBot).version;
            const botVersionCompare = semverLt(cleanBotV, '3.2.4');
            if(botVersionCompare){
                const regExp = /session\/agent/;
                const replace = httpParams.workDesigner.path.replace(regExp, 'designer/agent');
                httpParams.workDesigner.path = replace;
            }

            this.$postBotIdAndServerInfo(httpParams).then(() => {});
        }
        else{
            // console.log(" # getWDInfo fail!" );
        }
    });
}
```
### 리팩토링 후 코드
```javascript
<button class="btn-nagative1" v-on:click="botConnectionLogic('workbot', index)">
    봇 등록
</button>
```
```javascript
botConnectionFunc(){
    const isBotConnection = true;
    this.$getLocalBotInfo(isBotConnection).then((localData) => {
        if(localData != null){
            this.installedBot = localData.programVersion;
            this.localBotId = localData.botId;
            this.localMac = localData.macAddress;
            this.localPcName = localData.machineName;
        }
    });
},
isBotRunning(){
    if(this.installedBot==null && this.localMac==null){
        this.$alertError($t("봇이 실행중이 아닙니다."));
        return;
    }
},
checkBotVersion(type){
    if(semverLt(semverCoerce(this.installedBot).version, '3.1.2')){
        this.$alertInfo('', $t("설치된 버전이 3.1.2 미만일 경우에는 봇아이디를 복사후 시스템 트레이 아이콘 > 우클릭 > Bot ID 등록으로 진행해주세요."));
        return;
    }
    else{
        let typeName = '';
        if(type == 'designerBot'){
            typeName = 'type1';
        }
        else if(type == 'workbot'){
            typeName = 'type2';
        }
        return typeName;
    }
},
categorizeLicenseState(typeName, index){
    // 비교자원
    const macDiv = "mac-" + typeName + "-" + index;             
    const macVal = document.getElementById(macDiv).innerText;
    const botIdDiv = "copy-" + typeName + "-" + index;
    const botIdVal = document.getElementById(botIdDiv).innerText;

    let confirmParams = {
        "msg" : "",
        "case" : "",
        "botId" : botIdVal,
        "mac" : this.localMac
    }

    // 선택 라이선스에 등록된 mac : null (첫 등록)
    if(macVal == "" || macVal == null){
        confirmParams.msg = $t("PC에 라이선스 사용정보를 등록합니다.");
        confirmParams.case = 1; 
    }
    // 선택 라이선스에 등록된 mac, 현재 로컬의 mac : 일치 (진행)
    else if(macVal == this.localMac){
        confirmParams.msg = $t("현재 PC에 등록된 라이선스입니다. 재등록 진행하시겠습니까?"); 
        confirmParams.case = 2; 
    }
    // 선택 라이선스에 등록된 mac, 현재 로컬의 mac : 불일치 (재확인 선택)
    else if(macVal != this.localMac){
        confirmParams.msg = $t("다른PC에서 사용중인 라이선스입니다. 등록하시겠습니까?");
        confirmParams.case = 3;  
    }
    else {
        this.$alertError("mac check error");
        return;
    }
    return confirmParams;
},
// localMac에 다른 라이선스가 등록 돼 있는지 확인
isSameLicenseMacAndLocalMac(confirmParams){
    this.$getBotPermissionByMac(confirmParams.mac).then((data) => {
        if(data != null && data != ''){
            const param = {
                html: $t("<span>현재 PC에 등록된 라이선스가 이미 있습니다.</span><br><span>재등록 진행하시겠습니까?</span>"),
                icon: "warning",
                showCancelButton: true,
                confirmButtonText: $t('확인'),
                cancelButtonText: $t('취소'),
            }
            this.$alertCustom(param)
            .then( result => {
                if ( result.isConfirmed ) {
                    this.registerBot(confirmParams.botId);
                }
            });
        }else{
            // console.log('현재 PC에 등록된 라이선스 없음! -> 진행');
            this.registerBot(confirmParams.botId);
        }
    });
},
askUserToRegisterBot(confirmParams){
    this.$alertInfoWithCancle("",confirmParams.msg)
    .then( result => {
        if ( result.isConfirmed ) {
            // (첫 등록) || (재확인 선택)
            if(confirmParams.case === 1 || confirmParams.case === 3){
                this.isSameLicenseMacAndLocalMac(confirmParams);
            }
            // 재등록
            else if(confirmParams.case === 2){
                this.registerBot(confirmParams.botId);
            }
            else{
                this.$alertError("registretionBotPermissionConfirm error");
                return;
            }
        }
    });
},

botConnectionLogic(type, index){
    this.botConnectionFunc();
    this.isBotRunning();
    const typeName = this.checkBotVersion(type);
    const confirmParams = this.categorizeLicenseState(typeName, index);
    this.askUserToRegisterBot(confirmParams);
},

registerBot(botIdVal){
    const userId = JSON.parse(sessionStorage.getItem("user")).caller;           
    const params = {
        "userId" : userId,
        "mac" : this.localMac,
        "botId" : botIdVal,
        "version" : this.installedBot,
        "pcName" : this.localPcName
    }

    // botPermission 정보 등록
    this.$registrationBotPermission(params).then((data) => {
        if(data > 0){
            this.localBotId = botIdVal;

            // 화면 리로드
            this.reloadRetrieveLicenseList();
            // wd정보 http 전송
            this.botConnectionStart(botIdVal);
        }else{
            this.$alertError("registrationBotPermission Fail");
        }
    });
},
```