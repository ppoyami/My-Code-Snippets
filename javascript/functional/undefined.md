# 이터러블 / 이터레이터 프로토콜

## 사용자 정의 이터러블

> 이터러블: 이터레이터를 리턴하는 Symbol.iterator 를 가진 값
>
> 이터레이터: { value, done } 객체를 리턴하는 next\(\) 를 가진 값 
>
> 이터러블/이터레이터 프로토콜: 이터러블을 for...of, 전개연산자 등과 함께 동작하도록한 규약\(이터레이터의 next를 호출하며 순회하는 방식\)

```javascript
  const iterable = {
    [Symbol.iterator]() {
      let i = 3;
      
      return { // 이터러블은 Symbol.iterator 함수로 이터레이터를 반환한다.
        next() { // 이터레이터는 next 메서드를 가진다.
          return i == 0 ? { done: true } : { value: i--, done: false }; // 순회가 끝나는 조건에서 {done: true} 반환한다.
        },
        // 이터레이터도 Symbol.iterator메서드를 가진다.(자기자신을 반환한다.)
        [Symbol.iterator]() {
          return this; // 이터레이터가 나중에 이터레이터를 반환하여도, 자기자신의 상태를 유지한다.
        },
      };
    },
  };
  
  
    let iterator = iterable[Symbol.iterator]();
  iterator.next();
  iterator.next();
  // log(iterator.next());
  // log(iterator.next());
  // log(iterator.next());
  for (const a of iterator) log(a); // 자기자신을 반환하기 때문에 next()로 순회를 진행한 시점을 기억할 수 있다.
```

## 이터러블/이터레이터 프로토콜을 지원하는 타입과 문법

> spread 연산자도,  {value, done} 을 반환하는 이터레이터 next\(\)를 호출하면서 value를 풀어주는 것이다.

```javascript
  const arr = [1, 2, 3];
  
  let iter1 = arr[Symbol.iterator]();
  for (const a of iter1) log(a);
  
  const set = new Set([1, 2, 3]);
  for (const a of set) log(a);
  
  const map = new Map([
    ['a', 1],
    ['b', 2],
    ['c', 3],
  ]);
  for (const a of map.keys()) log(a);
  for (const a of map.values()) log(a);
  for (const a of map.entries()) log(a);
  
  
  // MEMO: nodeList도 이터러블한 타입이다.
  for (const a of document.querySelectorAll('*')) log(a);
  
  const all = document.querySelectorAll('*');
  let iter3 = all[Symbol.iterator](); // 이터레이터를 반환하고
  
  log(iter3.next()); // 그 이터레이터의 next 메서드로 순회한다.
  log(iter3.next());
  log(iter3.next());
  
  const a = [1, 2];
  // a[Symbol.iterator] = null;
  log([...a, ...arr, ...set, ...map.keys()]);
```

