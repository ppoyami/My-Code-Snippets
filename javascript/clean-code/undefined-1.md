# 조건문 깔끔하게 작성하기

## 복잡한 조건은 나누어 추상화한다.

```javascript

```

## 단락 평가

```javascript
function getIconPath(icon) {
    const path = icon.path || 'uploads/default.png';
    return `https://assets.foo.com/${path}`;
}

function getImage(userConfig) {
    const images = userConfig.images;
    return images && images.length ? images[0] : 'default.png'
}
```

## 옵셔널 체이닝 \(&& 평가를 대체\)

> `user?.address?.street:` 앞의 평가 대싱이 undefined, null이면 평가를 멈추고 undefined를 반환한다.
>
> user는 선언되어 있어야 오류가 발생하지 않는다. 그리고 없어도 괜찮은 값만 .?로 평가한다. \(오류가 나지 않아 디버깅이 어려워질 수도 있\)

```javascript
let user = {}

user && user.address && user.address.street 

// && 평가를 축약할 수 있다
user?.address?.street 

user = null;

user?.address // user가 존재하지 않으니, address 평가를 멈춥니다. -> undefined 반환
user?.address.street // undefined

```

## 옵서녈 체이닝 단락평가

> 에러 발생을 회피한다.

```javascript
let user = null;
let x = 0;

user?.sayHi(x++); // 아무 일도 일어나지 않습니다.


// 메서드가 존재하지 않아도 안전하게 호출할 수 있
let user1 = {
  admin() {
    alert("관리자 계정입니다.");
  }
}

let user2 = {};

user1.admin?.(); // 관리자 계정입니다.
user2.admin?.();

let user2 = null; // user2는 권한이 없는 사용자라고 가정해봅시다.
alert( user2?.[key] ); // undefined

delete user?.name; // user가 존재하면 user.name을 삭제합니다.
```

## 널 병합 연산자

> `x = (a !== null && a !== undefined) ? a : b; -> (a ?? b)`
>
> \|\| 연산자는 truthy 한 값을 반환 -&gt; 0, '' 은 반환하지 않음
>
> ?? 연산자는 정의된 값을 반환 , 0이 유효한 값으로 취급받는 상황에서 사용한다.
>
> ?? 연산자를 \|\|, && 연산자와 함께 사용할 수 없습니다. ??연산자는 우선순위가 낮아 \(\)를 사용해야한다.

```javascript
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

