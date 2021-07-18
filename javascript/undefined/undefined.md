---
description: 타입스크립트 문법 예제
---

# 예제 코드

## 기본 타입

### undefined와 null

> _undefined -&gt; 값이 비었는 지, 아닌 지 모른다.\(아직 결정되지 않아서 자동으로 설정되는 값\)_ 
>
>  __null -&gt; 비어있음을 명확히 표현한다.\(프로그래머가 지정하는 값\) - 타입으로는 거의 불필요

```typescript
  let age: number | undefined; // * 데이터가 있거나 아직 결정되지 않았거나(일반적으로 사용함)
  age = undefined;
  age = 1;
  
  function find(): number | undefined {
    // * 찾았으면 숫자, 아니면..
    if (true) return 1;
    return;
  }
  
  let person2: string | null; // * 있거나 없거나를 나타 낼때 사용
```

### void, never

> never : 함수에서 절대 리턴되지 않음을 명시

```typescript
  function print(): void {
    console.log('hello');
    return;
  }
  
  function throwError(message: string): never {
    // message -> server (log)
    throw new Error(message); // 에러를 던지기 때문에 리턴이 절대 되지 않음
    while (true) {} // 무한 루프를 돌거니까 절대 리턴하지 않을거야
  }
```

### Optional, Default, Rest parameter

```typescript
  //lastName: string | undefined -> printName('Ellie', undefined);
  function printName(firstName: string, lastName?: string) {
    console.log(firstName);
    console.log(lastName); // undefined
  }
  
  // Default parameter
  function printMessage(message: string = 'default message') {
    console.log(message);
  }
  printMessage();
  
  // Rest parameter
  function addNumbers(...numbers: number[]): number {
    return numbers.reduce((a, b) => a + b);
  }
```

### union

객체 타입을 공통 속성에 다른 값으로 정의하고 union 하면 조건 검사 시 가독성이 좋아진다.

```typescript
  type SuccessState = {
    result: 'success';
    response: {
      body: string;
    };
  };
  
  type FailState = {
    result: 'fail';
    reason: string;
  };
  
  type LoginState = SuccessState | FailState;

  function login(): LoginState {
    return {
      result: 'success',
      response: {
        body: 'logged in!',
      },
    };
  }

  function printLoginState(state: LoginState) {
    if (state.result === 'success') {
      console.log(`🎉 ${state.response.body}`);
    } else {
      console.log(`😭 ${state.reason}`);
    }
  }
```

### 튜플 사용하기

> 동적으로 값을 생성해서 다른 타입을 가진 배열로 반환하고, 사용 시 이름을 정해서 쓰고 싶다 -&gt; 튜플 
>
> _useState -&gt; \(\(\) =&gt; S\)\): \[S, Dispatch&gt;\]_
>
> Tuple\(반환 타입으로 권장되지 않음\) -&gt; interface, type alias, class\(권장됨\)

```typescript
  let student: [string, number];
  student = ['name', 123];
  student[0]; // name 0 인덱스에 뭐가 들어있는지 모름
  student[1]; // 123
  const [name, age] = student; // 구조분해 할당으로 가독성있게 접근하기
```

### Intersection Types

```typescript
  type Student = {
    name: string;
    score: number;
  };

  type Worker = {
    empolyeeId: number;
    work: () => void;
  };
  // M: 타입 조합하기
  function internWork(person: Student & Worker) {
    console.log(person.name, person.empolyeeId, person.work());
  }

  internWork({
    name: 'ellie',
    score: 1,
    empolyeeId: 123,
    work: () => {},
  });
```

### 타입 체크를 무시하기 &lt;&gt;, as, !

```typescript
  const result = jsStrFunc(); // 바닐라 자바스크립트에서 문자열을 반환하는 것이 확신한다면.
  // 아래와 같이 문자열 처럼 사용할 수 있음
  console.log((result as string).length); // ! 애러 발생이 안됨
  console.log((<string>result).length);

  // querySelector<E extends Element = Element>(selectors: string): E | null;
  const button = document.querySelector('class')!;

  // 아래와 같이 사용하면 되지만.
  if (button) {
    button.nodeValue;
  }
  // 간편하게 사용할 수도 있다 100% 장담한다면..
  button!.nodeValue;
```

## Generic

### function, class 에서의 제네릭

```typescript
function checkNotNull<T>(arg: T | null): T {
    if (arg == null) {
      throw new Error('not valid number!');
    }
    return arg;
  }
const number = checkNotNull(123);
const boal: boolean = checkNotNull(true);
    
interface Either<L, R> {
  left: () => L;
  right: () => R;
}


class SimpleEither<L, R> implements Either<L, R> {
  constructor(private leftValue: L, private rightValue: R) {}
  left(): L {
    return this.leftValue;
  }

  right(): R {
    return this.rightValue;
  }
}

const either: Either<number, number> = new SimpleEither(4, 5);

either.left(); // 4
either.right(); //5

const best = new SimpleEither({ name: 'ellie' }, 'hello');
```

### Gerneric constrain

> 세부적인 타입을 인자로 받아서 정말 추상적인 타입으로 다시 리턴하는 함수는 💩
>
> extends keyof

```typescript
interface Employee {
  pay(): void;
}

class FullTimeEmployee implements Employee {
  pay() {
    console.log(`full time!!`);
  }
  workFullTime() {}
}

class PartTimeEmployee implements Employee {
  pay() {
    console.log(`part time!!`);
  }
  workPartTime() {}
}
// 구현체는 모두 받을 수 있으나, 인터페이스 타입으로만 반
function payBad(employee: Employee): Employee {
  employee.pay();
  return employee; // Employee 인터페이스의 명세대로만 사용할 수 있
}

function pay<T extends Employee>(employee: T): T {
  employee.pay();
  return employee;
}


// extends keyof로 제네릭에 조건을 걸기

// K가 T의 키가 아니면 에러를 발생시킨다.
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  // K는 T의 속성 중 하나여야 한다.
  return obj[key];
}

console.log(getValue(obj, 'name')); // ellie
console.log(getValue(obj, 'age')); // 20
console.log(getValue(obj2, 'animal')); // 🐕
```

