# 브랜치 관리하기

## 브랜치 생성, 이동, 확인하기

```bash
git branch testing

git checkout testing

git checkout -b iss53 // 브랜치를 만들면서 체크아웃할 수 있다.


git branch
  iss53
* master
  testing

git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes

```

## 브랜치 삭제하기

> 삭제하기 전에, merged 된 브랜치 인지 체크하자.
>
> `git branch --no-merged`에서 나오는 branch 또 `git branch --merged` -&gt; \* 가 붙어 있지 않는 브랜치는 삭제해도 된다.

```bash
git branch --merged
  iss53
* master

git branch --no-merged
testing

git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.

git branch -D testing // 강제 삭
```



