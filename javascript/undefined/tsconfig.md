# tsconfig

## compilerOptions

```javascript
// 컴파일 할 자바스크립트 버전 명시(es5 이상 추천)
"target": "es5" 

// 모듈 시스템(node-commonjs, browser-ecma)
"module": "commonjs"

// 컴파일된 파일들의 저장 디렉토리 경로
"outDir": "./build"

// 컴파일할 타입스크립트 디렉토리 경
"rootDir": "./src"

// .map (.ts <-> .js) 파일 생성 (브라우저 탭 디버깅에 사용하는 파)
"sourceMap": true, 

// 변경된 부분만 컴파일하기(디스크 용량 커질 수 있)
"incremental": true,

// js 허용하고 error 체크(.ts, .js 파일들이 섞여 있다면 사용)   
"allowJs": true,
"checkJs": true,

// 하나의 js 파일로 컴파일 할 때
"outFile": "./"

// 이전 빌드된 내용을 기억한다.(더 빠르게 빌드하기 with incremental)
"composite": true
// incremental composite 관련 저장파일들의 경로
"tsBuildInfoFile": "./",

// 컴파일 시 코멘트 전부 삭제하기
"removeComments": true,
// 컴파일 에러만 체크하기(.js 파일 생성하지 않)
"noEmit": true,
```

## ETC.

```javascript
  // 제외하기
  "exclude": ["./src/dev.ts"],
  
  // 포함하
  "include": ["./src/main.ts"]
```

