---
title: Promise, async-await, axios
date: 2024-03-11
categories: [자바스크립트]
tags: [javascript, promise, async-await, axios, error-handling] # TAG names should always be lowercase
pin: true
---

***
> promise, async-await, axios 개념 요약 및 axios를 사용한 비동기 구문의 다양한 예시

업데이트 2024년 4월 3일


### **목차**
[**1. Promise**](#promise)  
[**2. async-await**](#async-await)  
[**3. Axios**](#axios)  
[**4. axios를 사용한 비동기 구문 리팩토링 예시**](#axios를-사용한-비동기-구문-리팩토링-예시)  

<br>

# 서론

프론트엔드 로드맵을 보다가 자바스크립트에 대해서 최소 `실행 컨텍스트, 프로토 타입, 프로미스, 호출 스택, 이벤트 루프`를 알아야 한다는 글을 봤다.

딥 다이브 책에서 한 번씩 봤었지만 설명은 못하겠더라. 그나마 프로미스는 설명할 수 있었는데 디테일한 설명은 할 수 없었다. 그래서 다시 한번 개념을 잡는 시간을 가졌다.

프로미스와 더불어 그와 관련 있는 async await, axios 개념을 살펴볼 것이다. 참고 글을 읽으면서 내용 복기 및 지식 체크를 했기 때문에 이번 글에서는 <u>부족했던 개념에 대해 요약정리만 했다.</u>

<br>
<br>

---
# Promise()

### **공부 전 내가 생각한 promise 란?**

- 비동기 구문을 동기로 사용할 수 있게 해주는 함수
- resolve(), reject()를 사용해 비동기 값에 대한 return 타입을 지정할 수 있다.

### **Promise 요약**

`new Promise( function(){ } );`

미래 시점에 결과를 주겠다는 약속  
<u>비동기 메서드가 동기적으로 작동하는 것처럼 값을 전달한다</u>: 결과값을 즉시 반환하지 않고, 비동기 메서드는 미래의 어떤 시점에 값을 전달하겠다는 ‘promise(약속)’을 반환한다.

 \+ `업데이트 2024년 4월 3일` 비동기 작업을 동기적으로 실행하는 것이 아닌, 비동기 작업이 성공 또는 실패했을 때의 결과를 나타낸다. <u>다시 말해, Promise를 사용해도 비동기적으로 작동하되, 비동기 작업의 결과를 다루는데</u> 있어서 편리하게 다룰 수 있게 도와준다. +

promise( )에 인자로 들어가는 함수는 executor로 실행 함수라고도 부른다.  
executor는 new promise 가 만들어질 때 자동으로 실행된다. executor는 콜백 함수를 반드시 호출한다. executor 인자에는 resolve와 reject가 들어간다. resolve와 reject는 자바스크립트에서 자체 제공하는 콜백이다.

보통 시간이 걸리는 일을 수행할 때 Promise를 사용한다.

finally 함수는 인수가 없다.(애초에 결과에 상관없이 일반적으로 행해지는 함수이기 때문에 인수가 필요 없다.) finally에서는 프로미스의 이행/거부 상태를 알 수 없다.

### **잊고 있었던 개념**

#Promise는 함수다. #콜백을 이용한다. #콜백 하나를 반드시 호출한다. #finally를 then(), catch() 전에 써도 된다.

<br>
<br>

---

# async await

### **공부 전 내가 생각한 async await 란?**
* Promise와 같은 기능으로 비동기 구문을 동기적으로 사용할 수 있게 해주는 함수
* ES17에 나온 기능으로 promise chain 을 사용하는 것보다 직관적으로 사용할 수 있다.
* 함수 앞에 async를 붙여 사용
* promise 객체를 반환

### **async await 요약**

일반적으로 <u>await의 대상이 되는 비동기 처리 코드는</u> Axios 등 같은 <u>프로미스를 반환하는 API 호출 함수이다.</u>  
> \+ `업데이트 2024년 4월 3일` 
 비동기와 promise, async-await가 완전히 이해됐다고 착각하고 테스트 코드를 짜보았다. 근데 생각처럼 동작하지 않았다. Chat gpt에게 물어봤다. 그 중 일부분을 발췌했다.  
**나:**  
 ```
 async function cooking() {
    await new Promise(resolve => {
        setTimeout(() => {
            console.log('cooking');
            resolve(); // 비동기 작업이 완료됐음을 알림
        }, 3000);
    });
} 에서 Promise안쓰면 안돼?
 ```
 **Chat gpt:** setTimeout 함수는 비동기적으로 실행되는 함수이기 때문에 Promise 없이는 await를 사용할 수 없습니다. await는 항상 Promise 객체를 반환하는 함수나 비동기 작업 앞에서만 사용될 수 있습니다. 그렇기 때문에 setTimeout을 Promise 없이 사용하려면 Promise를 직접 생성하여 사용해야 합니다. +

async await의 예외 처리는 promise에서는 catch를 사용하는 것처럼, try catch 구문을 이용한다.
try catch를 사용해서 에러 핸들링을 하여 예외 상황에 대해 대처해야 한다.


> 자바스크립트의 비동기 처리 코드는 콜백 또는 promise 등을 사용해야 코드의 실행 순서를 보장받을 수 있다.
{: .prompt-tip }


<br>
<br>

---

# Axios

통신을 할 때 사용하는 라이브러리.  
비동기 구문이 많이 쓰이는 곳으로 Promise와 async await를 다룰 때 같이 개념 정리를 하면 좋을 것 같아 정리했다.

### **Axios란**

브라우저와 Node.js에서 사용할 수 있는 promise 기반 HTTP 클라이언트 라이브러리.

.then(), .catch(), .finally() 사용이 가능하다.

axios는 promise API를 지원한다.

async await 도 사용이 가능하다.
```javascript
// async-await 예제
// 함수 외부에 `async` 키워드를 추가하세요.
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```
<br>
<br>

---

\+ `업데이트 2024년 4월 3일` 
# try catch
에러로 인해 스크립트가 죽지 않아야 할 때 사용

런타임 에러에만 동작한다.

동기적으로 작동한다.

예기치 못한 상황1 : [서버에서 전달받은 데이터가 잘못되어 에러를 발생하는 경우] 프런트에서 try catch를 통해 예외처리가 가능.
+

<br>
<br>

---

# **종합해 보면**

함수 내에 axios 구문만 있을 때는 axios만 사용해서 작성  
함수 내에 여러 메서드가 같이 있을 때는 async await를 사용하여 axios 구문 작성

함수 내에 axios 구문만 있을 경우 then()과 catch()로 api 관련 로직만 처리하면 되니까 promise나 async await가 필요 없다.
하지만 함수 내에 여러 메서드가 있을 때는 실행 순서를 보장받아야 하기 때문에 promise 또는 async await를 사용한다. 

<br>
<br>

---

막 JS를 처음 공부했을 때, 2년 전쯤, 그리고 지금. 적어도 3번 이상은 Promise에 대해 공부를 했다. 처음에는 몇 번을 읽어도 이해가 안 갔다. 그래도 막 처음 공부했을 때보다는 2년 후에 봤을 때는 좀 더 이해가 됐고, 2년 전보다는 지금 다시 보니 글 이해가 더 됐다. 이제 이론적인 부분은 90%를 흡수한 것 같고 나머지 10%는 실무에서 활용을 하면서 이해하면 될 것 같다.

처음 axios로 통신 코드를 작성했을 때가 생생하다. axios와 promise를 써야 할지 async await를 써야 할지, 예외 처리는 어떻게 해야 할지 막막했는데 이번에 다시 공부를 하면서 정리가 됐다. 그 당시 주먹구구식으로 작성했던 코드에 어떤 부분이 부족한 지가 보인다. 그뿐만 아니라 어떻게 수정해야 할지 명확하게 알게 되었다. 그렇기 때문에 이전에 작성했던 코드를 가져와 리팩토링 예시를 작성했다.

# axios를 사용한 비동기 구문 리팩토링 예시

**리팩토링 필요한 코드**  
[1. async await를 사용했으면서 에러 핸들링을 안한 것](#1async-await를-사용했으면서-에러-핸들링을-안한-것)  
[2. 에러 핸들링이 없는 것](#2에러-핸들링이-없는-것)  
[3. 굳이 promise(), asycn await를 사용한 것](#3굳이-promise-asycn-await를-사용한-것)  
[4. async await 도 없으면서 try catch 구문으로 감싼 것](#4async-await-도-없으면서-try-catch-구문으로-감싼-것)  

<br>


#### 1.async await를 사용했으면서 에러 핸들링을 안한 것  
axios에서 발생할 수 있는 에러에 대한 처리도 없는 상황

```javascript

[before]

async getApi(){
    return await axios.get(url, {
        headers: {
            'Accept': 'application/json',
            'Authorization': accessToken()
        }
    })
}

-------------------------------------------------------------------------------

[after]

async getApi(){
    try {
        return await axios.get(url, {
            headers: {
                'Accept': 'application/json',
                'Authorization': accessToken()
            }
        })
        .then(res=>{
            if(http통신 response가 성공인지 확인){...}
            
            throw new Error(에러 메세지);
        })
    } catch (error) {
        ...에러 핸들링...
    }
}

```
<br>

#### 2.에러 핸들링이 없는 것

```javascript
[before]

getApi: ()=>{
    return axios.get(url, {
        headers: {
            'Accept': 'application/json',
            'Authorization': 토큰
        }
    })
    .then(response=>{
        if(response.data.result === 'success' && response.data.resultCode === 200){
            const data = response.data.body;

            ...데이터 가공...

            return data;
        }
    })
}

-------------------------------------------------------------------------------

[after]

getApi: ()=>{
    return axios.get(url, {
        headers: {
            'Accept': 'application/json',
            'Authorization': 토큰
        }
    })
    .then(response=>{
        if(response.data.result === 'success' && response.data.resultCode === 200){
            const data = response.data.body;

            ...데이터 가공...

            return data;
        }
        throw new Error(에러 메세지);
    })
    .catch(err=>{
        // 에러 핸들링
    }
},
```
<br>

#### 3.굳이 promise(), asycn await를 사용한 것  
axios는 promise() 기반으로 작동하기 때문에 .then(), .catch()와 같은 promise chain이 가능하다. 또한 getApi 함수에는 비동기 구문만 담은 로직만 있기 때문에 async await 또는 promise() 사용을 안 해도 된다.

```javascript
[before]

getApi: async ()=>{
    return await axios.get(url, {
        headers: {
            'Accept': 'application/json',
            'Authorization': 토큰
        }
    })
    .then(res=>{
        if(res.data.result === 'success' && res.data.resultCode === 200){
            return res.data.body;
        }
        throw new Error(에러 메세지);
    })
    .catch(err=>{
        ...에러 핸들링...
    })
}

-------------------------------------------------------------------------------

[after]

getApi: ()=>{
    return axios.get(url, {
        headers: {
            'Accept': 'application/json',
            'Authorization': 토큰
        }
    })
    ...이하 동일
}
```
<br>

#### 4.async await 도 없으면서 try catch 구문으로 감싼 것  
   <u>try-catch문은 동기 코드에서 예외 처리를 하는데 사용되므로 비동기 코드의 에러를 캐치할 수 없다.</u>

```javascript
[before]

getApi: ()=>{
    try {
        return axios.get(url, {
            headers: {
                'Accept': 'application/json',
                'Authorization': 토큰
            }
        })
        .then(res=>{
            if(res.data.result === 'success' && res.data.resultCode === 200){
                return res.data.body;
            }
            else {
                throw new Error(res.message);
            }
        })
    } catch (error) {
        ...에러 핸들링...
    }
}

-------------------------------------------------------------------------------

[after]

getApi: ()=>{
    return axios.get(url, {
        headers: {
            'Accept': 'application/json',
            'Authorization': 토큰
        }
    })
    .then(res=>{
        if(res.data.result === 'success' && res.data.resultCode === 200){
            return res.data.body;
        }
        else {
            throw new Error(res.message);
        }
    })
		.catch((error) {
        ...에러 핸들링...
    });
}

```
<br>

이번 개념 다지기 시간 이후로 3월에는 그간 작성했던 API 코드를 리팩토링하는 시간을 가질 예정이다. 위의 예시를 바탕으로 상황별로 코드를 작성법을 만든 뒤 코드를 수정할 것이다.

<br>
<br>

---

참고

[프론트엔드 로드맵 2024 - Google Search](https://www.google.com/search?q=프론트엔드+로드맵+2024&sca_esv=65d2b102cd6037ef&biw=1920&bih=911&sxsrf=ACQVn0-J02Jzh1cIoKTuJUkVhYQmzfcqVA:1709596891508&ei=22DmZezUHoff2roPxu63wAY&udm=&oq=프론트엔드&gs_lp=Egxnd3Mtd2l6LXNlcnAiD-2UhOuhoO2KuOyXlOuTnCoCCAAyChAjGIAEGIoFGCcyChAjGIAEGIoFGCcyChAjGIAEGIoFGCcyBRAAGIAEMgUQABiABDILEAAYgAQYsQMYgwEyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAESIoZUABY0A1wAngAkAEAmAG7AaABlAuqAQQwLjEyuAEDyAEA-AEBmAILoAKZCMICCxAuGIAEGLEDGIMBwgIKEAAYgAQYigUYQ5gDAJIHAzIuOaAHx4wB&sclient=gws-wiz-serp#ip=1)

[프라미스](https://ko.javascript.info/promise-basics)

[자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)

[자바스크립트 async와 await](https://joshua1988.github.io/web-development/javascript/js-async-await/)

[기본 예제-Axios Docs](https://axios-http.com/kr/docs/example)
