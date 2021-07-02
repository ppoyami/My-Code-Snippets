# 펼침 연산자

## 배열에서 특정 항목 삭제하기

```javascript
function removeItem(items, removable) {
    if (items.includes(removable)) {
        const index = items.indexOf(removable);
        return  [...items.slice(0, index), ...items.slice(index+1)]
    }
}
```



