# 프로젝트 설계하기

#### Reference.

[React로 사고하기 - React](https://ko.reactjs.org/docs/thinking-in-react.html#step-2-build-a-static-version-in-react)

[How to plan and organize a React project - by building a weather app](https://konstantinmuenster.medium.com/how-to-plan-and-organize-a-react-project-by-building-a-weather-app-95175b11bd01)

## 1. Planning our application

리액트 프로젝트는 재사용 가능하고, 독립적인 UI 컴포넌트로 구성되므로, 어떤 것이 UI 요소가 될 것인지를 결정하는 것이 중요합니다.

#### A. **mock up을 진행하고, UI 컴포넌트로 나누어 봅니다.**

* **컴포넌트로 나누는 기준은? 하나의 컴포넌트는 하나의 일을 해야 합니다.**
* 여러 컴포넌트가 조합된다면, 여러 컴포넌트를 담을 수 있는 컴포넌트가 필요합니다.

  → 하위 컴포넌트를 렌더링 한다던가, 하위 컴포넌트에서 상태 끌어올리기가 필요할 때

![](../../.gitbook/assets/image%20%282%29.png)

#### B. **컴포넌트 식별을 완료했다면, 계층으로 나열해봅시다.**

![](../../.gitbook/assets/image%20%283%29.png)

#### C. **상태를 관리하는 컴포넌트\(stateful component**\)를 정의해봅니다.

* 가능한 적은 stateful components을 가지도록 정의합니다.
* 리덕스 같은 것이 없다면, 하나의 비즈니스 로직에 대응하는 stateful components를 가지는 것이 좋습니다.

1. **state를 정의하기**
   * state 를 결정하는 기준
     * 상태 값은 가능한 적게 유지합니다. 예를 들어 날씨 정보를 바탕으로 색을 바꾸는 경우, 날씨 정보를 상태 값으로 하고, 색은 계산하여 사용하는 것이 좋습니다.
     * 부모에게서 전달하는 props 이 될 수 있다면, 상태 값이 아닙니다.
     * 변하지 않는 값은 상태 값이 아닙니다.
     * 다른 상태 값이나, props 으로 계산이 가능하다면 상태 값이 아닙니다.
     * 상태 값을 결정하는 예시
       * 제품의 원본 목록 → 외부 API에서 받아오는 값, 따라서 상태 값이 아니다
       * 유저가 입력한 검색어 → **시간이 지나면서 변하는 값이고, 계산될 수 없다 → 상태 값이다.**
       * 체크박스의 값 → **위와 동일한 이유로 상태 값이다.**
       * 필터링 된 제품들의 목록 → 검색어와 체크박스로 계산이 가능하다 상태값이 아니다.

**2. state 위치 시키기**

* state를 기반으로 렌더링하는 모든 컴포넌트를 찾으세요.
* 공통 소유 컴포넌트 \(common owner component\)를 찾으세요. \(계층 구조 내에서 특정 state가 있어야 하는 모든 컴포넌트들의 상위에 있는 하나의 컴포넌트\).
* 공통 혹은 더 상위에 있는 컴포넌트가 state를 가져야 합니다.
* state를 소유할 적절한 컴포넌트를 찾지 못하였다면, state를 소유하는 컴포넌트를 하나 만들어서 공통 오너 컴포넌트의 상위 계층에 추가하세요.

## 2. Organizing our project

[_**Project Tree Generator**_](https://woochanleee.github.io/project-tree-generator/)_\*\*\*\*_

1. **Folder Structure - 컴포넌트 나누기**
   * 컴포넌트를 `Elements` `Components` `Container` 나누어 봅시다.
     * `Elements` : 버튼, 아이콘, input과 같이 반복해서 재사용되는 요소들\(low level\)입니다.
     * `Components` : 독립적인 기능을 수행하는 요소입니다. SearchBar는 search query를 submit 하게 합니다. input 단독으로 불가능합니다. 따라서 Input은 element, SearchBar는 components입니다.
     * `Container` : 상태를 관리하는 컴포넌트입니다.
2. **Redux, React-Router를 사용하는 경우의 폴더구조**

## 3. Building the app

#### A. **먼저 정적 버전을 build 해 봅니다.**

* 목업에서 틀을 잡았던 것을 바탕으로 구현합니다. 기능이나 상태 조작은 신경쓰지 않습니다.
  * 정적버전 이기 때문에 state를 사용하지 않고, props를 이용해 데이터를 전달해줍니다.
* 디자인하는것에 집중합니다. 컴포넌트로 구분했던 것들을 작성합니다.
* 어떤 데이터 처리도 다루지 않으므로, 주석 처리를 사용해서 틀을 잡아 둡시다.

![](../../.gitbook/assets/image%20%284%29.png)

#### B. **상태를 변화시키는 기능들을 추가합니다. \(UI Interactive 만들기\)**

