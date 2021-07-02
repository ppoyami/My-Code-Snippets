# Set 활용하기

## 중복 제거하기

> 중복되지 않는 color 배열을 생성하는 것을 한 줄로 처리할 수 있다.

```javascript
function getUnique(attributes) {
    return [...new Set(attributes)]
}

const colors = [...dogs.reduce((colors, {color}) => colors.add(color), new Set())];
```

