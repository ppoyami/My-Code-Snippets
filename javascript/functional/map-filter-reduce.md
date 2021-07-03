# map, filter, reduce

## map

> 어떻게 매핑하는지 명령하지 않음, 다른 함수에게 위임하고\(다형성 지원\), 매개변수와 리턴 값으로 서로 의사소통한다.\(함수 외부에 영향을 끼치지 않는다.\)

```javascript
  const map = (f, iter) => {
    let res = [];
    for (const a of iter) {
      res.push(f(a));
    }
    return res;
  };
  
  // Array.map 함수
  log([1, 2, 3].map(a => a + 1)); // array를 상속한 타입에만 적용가능하다.
  
  // MEMO: querySelectorAll 는 이터러블 프로토콜을 따른다. (for .. of)
  log(map(el => el.nodeName, document.querySelectorAll('*')));
  
  let m = new Map();
  m.set('a', 10);
  m.set('b', 20);
  // map의 value를 *2 매핑한 배열을 반환해서 맵을 생성하고 있다.
  log(new Map(map(([k, a]) => [k, a * 2], m))); // [[k, a*2], [k, a*2]]
  
  // MEMO: 이미 만들어진 이터레이터 뿐만 아니라, 문장(제너레이터 함수)도 map을 할 수 있다.(거의 모든 것)
  function* gen() {
    yield 2;
    if (false) yield 3;
    yield 4;
  }

  log(map(a => a * a, gen()));
```

## filter

```javascript
  const filter = (f, iter) => {
    let res = [];
    for (const a of iter) {
      if (f(a)) res.push(a);
    }
    return res;
  };
  
  log(...filter(p => p.price < 20000, products));
```

## reduce

```javascript
  const reduce = (f, acc, iter) => {
    if (!iter) {
      // acc의 매개변수가 비어있다면, acc = iter가 된다.
      iter = acc[Symbol.iterator](); // acc -> iter 변수에 옮긴다음
      acc = iter.next().value; // 하나를 순회하여, 그 값을 담는다.(iter에는 한 번 순회한 상태가 남아있게 됨)
    }
    for (const a of iter) {
      acc = f(acc, a);
    }
    return acc;
  };
  
  log(reduce(add, [1, 2, 3, 4, 5]));
```

## map, filter, reduce 조합으로 작성된 코드 읽기

```javascript
  const products = [
    { name: '반팔티', price: 15000 },
    { name: '긴팔티', price: 20000 },
    { name: '핸드폰케이스', price: 15000 },
    { name: '후드티', price: 30000 },
    { name: '바지', price: 25000 },
  ];

  const add = (a, b) => a + b;
  // MEMO: 함수 조합으로 작성 된 코드 읽기
  // 오른쪽부터 읽는다 -> 필터링하고, 필터링 된 값을 매핑하고, 매핑한 값을 add 로 축약해서 반환
  log(
    reduce(
      add,
      map(
        p => p.price,
        filter(p => p.price < 20000, products)
      )
    )
  );
```

## 함수형 사고하기

```javascript
  //MEMO: 함수형 사고하기
  // 1. 이미 잘 동작하는 코드를 확보하고(순수함수)
  // 2. 특정 값으로 평가될 수 있는 함수들을 작성하고 조합한다.
  reduce(add, [15000, 15000]); // <-- 어떤 함수가 평가되서 [15000, 15000]이 되면 된다.

  reduce(
    add,
    map(p => p.price, products) // <-- 숫자로 매핑하는데, 어떤 조건으로 걸러진 배열을 대상으로 하면 된다.
  );

  reduce(
    add,
    map(
      p => p.price,
      filter(p => p.price < 20000, products) // <-- 필터링 함수를 조합한다.
    )
  );

  log(
    reduce(
      add,
      filter(
        n => n >= 20000,
        map(p => p.price, products)
      )
    )
  );
```

