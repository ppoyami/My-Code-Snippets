# 제너레이터

## 제너레이터 조합하기

> 이터레이터이자 이터러블을 생성하는 함수\(_well_-_formed_ _iterable_\) 
>
> \(next, Symbol.iterator 메서드를 둘 다 가지고 있음\)

```javascript
function* infinity(i = 0) {
    while (true) yield i++; // next 호출 될 때마다, 이전의 + 1 값을 반환
  }
  
function* limit(l, iter) {
    for (const a of iter) {
      yield a;
      if (a == l) return;
    }
  }
  
function* odds(l) {
    for (const a of limit(l, infinity(1))) { // limit 또한 이터러블을 반환
      if (a % 2) yield a; // 때문에, for of 의 프로토콜로 순회가 가능하다
    }
  }
  
for (const a of odds(40)) log(a);
```

## for of, spread, destructuring, rest operator

> 제너레이터는 \(_well-formed_\) 이터레이터를 반환하기 때문에, 이터러블 프로토콜을 따르는 많은 경우에서 사용이 가능하다.

```javascript
  log(...odds(10));
  log([...odds(10), ...odds(20)]);

  const [head, ...tail] = odds(5);
  log(head);
  log(tail);

  const [a, b, ...rest] = odds(10);
  log(a);
  log(b);
  log(rest);
```

