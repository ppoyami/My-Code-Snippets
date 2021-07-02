# 매개변수와 return 문을 정리하기

## 매개변수의 기본 값을 설정하기

```javascript
function convertWeight(weight, ounces = 0, roundTo = 2) {
  const total = weight + (ounces / 16);
  const conversion = total / 2.2;

  return roundToDecimalPlace(conversion, roundTo);
}
```

## 해체 할당으로 유연하게 객체 속성 접근하

> displayPhoto의 매개변수가 객체라면, ...other 로 유연하게 속성을 받아올 수 있다.\(속성이 추가되어도 함수는 잘 동작함\), additional 변수에 추가적인 정보가 잘 저장된다.

```javascript
function displayPhoto({
  title,
  photographer = 'Anonymous',
  location: [latitude, longitude],
  src: url,
  ...other
}) {
  const additional = Object.keys(other).map(key => `${key}: ${other[key]}`);
  return (`
    <img alt="Photo of ${title} by ${photographer}" src="${url}" />
    <div>${title}</div>
    <div>${photographer}</div>
    <div>Latitude: ${latitude} </div>
    <div>Longitude: ${longitude} </div>
    <div>${additional.join(' <br/> ')}</div>
  `);
}
```

## 해체 할당으로 객체 만들기

> 매개변수로 넘어오는 객체의 속성을 location, ...details 로 구분한 다음, ...details를 그대로 유지한 객체를 반환하고 있다.

```javascript
function setRegion({ location, ...details }) {
  const { city, state } = determineCityAndState(location);
  return {
    city,
    state: state.abbreviation,
    ...details,
  };
}
```

## 나머지 매개변수 사용하기

> ...rest 는 a,b,c ,로 구분된 인자들을 배열로 받습니다.

```javascript
// 모든 items가 max 길이 이하인지 검사한다.
function validateCharacterCount(max, ...items) {
  return items.every(item => item.length < max);
}

// 나머지 변수들에 대한 정보를 받아온다
['Spirited Away', 'Princess Mononoke'].map((film, ...other) => {
    console.log(other);
    return film.toLowerCase();
  });
```

