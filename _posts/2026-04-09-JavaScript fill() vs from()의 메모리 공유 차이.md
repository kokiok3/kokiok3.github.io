---
title: 'JS fill() vs from()의 메모리 공유 차이'
date: 2026-04-09
categories: [자바스크립트]
tags: [자바스크립트, javascript, js] # TAG names should always be lowercase
pin: true
---
아래의 코드가 다른 이유는?  
```javascript
Array(101).fill(Array(101).fill(0)) 
vs
Array.from({length: 101}, () => Array(101).fill(0))
```


<br>


차이는 자바스크립트가 데이터를 메모리에 올리는 방식입니다.

# ❌ fill()이 주소를 공유하는 이유
fill(value) 함수는 인자로 받은 value를 **단 한 번만 평가(생성)**합니다.

Array(101).fill(0)을 실행하면 0이라는 숫자를 101칸에 복사합니다. (숫자는 원시 값이라 괜찮음)

하지만 fill(Array(101).fill(0))을 하면, 안쪽의 Array(101)이 메모리 상에 딱 하나 만들어지고, 그 **메모리 주소(번지수)**가 101칸에 똑같이 복사됩니다.

결과적으로 board[0]이나 board[100]이나 모두 **"메모리 0x123번지에 있는 그 배열"**을 가리키게 되는 거죠.

<br>

# ✅ Array.from()이 공유하지 않는 이유
Array.from(length, callback)은 두 번째 인자로 콜백 함수를 받습니다.

이 함수는 배열의 각 칸을 만들 때마다 매번 새롭게 실행됩니다.

() => Array(101).fill(0) 코드가 총 101번 실행되면서, 실행될 때마다 new Array()가 호출되는 것과 같습니다.

따라서 각 행은 서로 다른 고유한 메모리 주소를 가진 독립적인 배열이 됩니다.


<br>


# 💡 한 줄 정리
**fill()**은 "이거(하나) 가지고 다 채워!"라고 시키는 것이고,  
**Array.from()**은 "채울 때마다 이거(함수) 실행해서 새로 만들어!"라고 시키는 것입니다.

<br>
