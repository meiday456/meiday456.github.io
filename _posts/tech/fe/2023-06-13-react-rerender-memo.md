---
layout:     post
title:      React 렌더링과 성능향상(Memoization)
subtitle:   React 렌더링 상황과 이해, Memoization의 활용과 오해
categories: [tech]
tags:       [fe , react]
---


1. 순서가 있는 목차
{:toc}

## 개요
리액트에서의 렌더링과 리렌더링에 대해서 자세히 알아본 정보를 공유하고, Memoization으로 사용하는 useCallback, useMemo 사용의 작은팁? 을 소개하고자합니다.

리액트에서 렌더링 과정을 정확히 알고자한다면 브라우저의 렌더링 과정과 Virtual Dom에 대해서 정확히 알아야 하지만
이번 포스트에서는 컴포넌트의 렌더링(React에서의 렌더링)에 대해서 작성하겠습니다.


***


## 렌더링이란?
렌더링을 쉽게 말하면  `사용자에게 보여지는 UI 를 업데이트 하는 과정`이라고 할 수 있습니다.<br/>

![리액트렌더링 이미지](/assets/img/posts/tech/fe/20230613/render.png)

리액트의 렌더링은 공식문서에 의하면 두가지가 있습니다. 
- **`엘리먼트 렌더링`**{:.primary} : 엘리먼트의 변경된 부분만을 DOM 업데이트
- **`컴포넌트 렌더링`**{:.primary} : 리액트가 컴포넌트를 실행하여 엘리먼트를 반환합니다.

여기서 중요한 부분이 리액트에 의해 컴포넌트가 호출되고 컴포넌트는 엘리먼트를 반환한 다음 DOM에 반영된다는 것입니다.

DOM을 업데이트하는 과정에서 컴포넌트의 상태 변경을 추적하고 업데이트하는 재조정(Reconciliation) 과정,<br/>
React Element Tree를 비교하여 변경사항을 찾는 Diffing 과정을 거칩니다.

React 16 이전까지는 재조정하는 방식의 한계로 
React 16에 등장한 새로운 재조정 알고리즘 [Fiber]을 사용하여 재조정(Reconciliation)을 수행합니다.
[Fiber] 에 대해서 따로 포스팅 하도록 하겠습니다.

***

### 리액트의 렌더링 과정
리액트의 렌더링은 3가지 단계를 거칩니다.
1. **`Trigger`** : 고객의 주문을 주방에 전달
2. **`Rendering`** : 주방에서 주문을 준비
3. **`Commit`** : 손님에게 주문(결과)을 전달

![리액트 렌더링 과정](/assets/img/posts/tech/fe/20230613/render_phase.png)

**`1단계 : Trigger`** 
{:.lead}
컴포넌트가 렌더링되는 **`두가지 경우`** 가 트리거가 됩니다.

1. 초기 렌더링
2. 컴포넌트의 상태가 업데이트됨(부모 컴포넌트 포함)

**`2단계 : Render`**
{:.lead}
Trigger 이후 리액트는 컴포넌트를 호출하여 화면에 표시할 항목을 결정합니다.<br/>
이 과정은 *`재귀적`*으로 컴포넌트 트리를 탐색합니다.

초기 렌더링시 **`root 컴포넌트`**를 호출하고 **`DOM 노드`**를 생성합니다.<br/>
컴포넌트의 상태 업데이트의 경우 해당 컴포넌트를 호출하여 변경된 정보를 찾고`(재조정 과정)` 다음 단계로 넘어갑니다.

**`3단계 : Commit`**
{:.lead}
컴포넌트를 Rendering(호출)한 뒤 React는 DOM을 업데이트합니다.

초기 렌더링 시 DOM API의 **`appendChild()`** 를 호출하여 **`Root 컴포넌트`**에 **`DOM node`**를 생성합니다.<br/>
리렌더링의 경우 최소한의 작업으로 변경된 사항을 **`DOM`**에 업데이트합니다.

***

### 리렌더링이란?
리렌더링이란 렌더링 제외 기준을 만족하지 못할 경우에 발생하는 렌더링이라고 할 수 있을 것 같습니다.

#### 렌더링 제외 조건
1. 컴포넌트가 **`이미 마운트`** 되어있음
2. 변경된 **`Props`**가 없음
3. 컴포넌트에서 사용되는 **`Context`** 값 중 변경된 사항이 없음
4. **`컴포넌트 자체`**에서 업데이트를 예약하지않음(Fiber와 연관되어 예약이라고 표기하였습니다.)(state의 변경이 없음)

앞선 렌더링과정 중 **`render`** 과정에서 리액트는 *`재귀적`*으로 컴포넌트 트리를 탐색하여 컴포넌트를 렌더링합니다
이때 렌더링 제외조건을 만족하지 못하는 컴포넌트들이 호출되어 다시 렌더링 되는 것을 **`리렌더링`**이라고 합니다. 

> *3번 Context의 변경사항이 없음* 은 Context Api의 값을 참조하는 컴포넌트들이 값이 변경되었을 때
> 다시 렌더링 과정을 거칩니다. 이는 많은 상태관리 라이브러리에서 사용하는 방식입니다.

***

## 성능향상

렌더링이란 결국 변경사항을 DOM에 반영하여 UI 업데이트를 하는 과정이라서 결국에는 많이 발생할 수 밖에 없는 것같습니다.<br/>
이러한 렌더링 과정 중 불필요한 과정을 줄여서 컴포넌트 렌더링 성능을 향상 시킬 수 있습니다.

### 메모이제이션
대표적으로 리액트에서 제공하는 *`Hooks`* 중 **`메모이제이션 Hook`**인 *`useMemo`*와 *`useCallback`*을 사용한
최적화 방법이 있습니다. 이 *`Hooks`*의 사용과 예시를 확인하고, 오해?를 알아보고자 합니다.

그리고 고차함수인 *`React.memo`* 또한 알아보겠습니다.

:clipboard: 예제를 준비하였는데 모든 예제는 1초마다 Parent의 state가 변경되는 예제입니다.<br/>
자식 컴포넌트는 각각 메모이제이션이 된것 과 안된 컴포넌트로 구성되어있습니다.

모든 자식 컴포넌트는 자신이 다시 렌더링이 될때마다 숫자가 증가합니다.

***

#### React.memo

~~~javascript
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
~~~

**`React.memo`**는 고참함수로 첫번째 인수로 컴포넌트 타입을 받고,<br/>
두번째 인수로 **`props`** 의 변경을 확인하는 함수를 전달하여 리액트의 기본적인 **props 비교`(shallow equality (얕은 비교))`**를 변경 할 수 있습니다.

{% include components/codepen.html hash="NWEGryb" title="1" height="450" %}<br/>

- 첫번째 컴포넌트 : **`React.memo`** 미사용
- 두번째 컴포넌트 : **`React.memo`** 사용

~~~jsx

const Child = ({ title }) => {
  const count = React.useRef(0);

  return (
    <div className="black-tile memo">
      {title}
      <p>update : {count.current++}</p>
    </div>
  );
};

const MemoChild = React.memo(Child, (prevProps, nextProps) => {
  return prevProps.title === nextProps.title;
});
~~~

***

#### useMemo
~~~javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
~~~
객체나 복잡한 계산의 결과 등의 **`value`**{:.primary} 를 **`메모이제이션`**{:.primary} 하여 반환합니다.

- 첫번째 인수로 값을 반환하는 함수를 전달
- 두번째 인수로 배열을 전달합니다. 이 배열은 의존성을 부여하는 것으로 배열로 전달된 값이 변경되면 다시 메모이제이션 하여 값을 반환합니다. 만약 배열을 전달하지 않는다면 매번 다시 메모이제이션 값을 반환합니다.

{% include components/codepen.html hash="OJayXxy" title="2" height="600" %}<br/>

- 첫번째 컴포넌트 : **`useMemo`** 사용, **`React.memo`** 미사용
- 두번째 컴포넌트 : **`useMemo`** 사용, **`React.memo`** 사용

~~~jsx
const Child = ({ value, title }) => {
  const count = React.useRef(0);
  const memoResult = React.useMemo(() => value * 100, [value]);

  return (
    <div className="black-tile memo">
      {title}
      <p>update : {count.current++}</p>
      <p>props value * 100 : {memoResult}</p>
    </div>
  );
};

const MemoChild = React.memo(Child);
~~~




#### useCallback
~~~javascript
const memoizedCallback = useCallback(() => { doSomething(a, b) }, [a, b]);
~~~

**`메모이제이션`**{:.primary}된 **`Callback 함수`**{:.primary}를 반환합니다.

- 첫번째 인수로 *`callback`* 함수를 전달
- 두번째 인수로 배열을 전달합니다. 이 배열은 의존성을 부여하는 것으로 배열로 전달된 값이 변경되면 다시 메모이제이션 하여 값을 반환합니다. 만약 배열을 전달하지 않는다면 매번 다시 메모이제이션 함수를 반환합니다.

{% include components/codepen.html hash="OJayXzM" title="3" height="450" %}<br/>

- 첫번째 컴포넌트 : **`useCallback`** 사용, **`React.memo`** 미사용
- 두번째 컴포넌트 : **`useCallback`** 사용, **`React.memo`** 사용

~~~jsx
  const memoHandleClick = React.useCallback(handleClick, []);
  <Child title={"useCallback"} onClick={memoHandleClick}></Child>
  <MemoChild
    title={"useCallback and React.memo"}
    onClick={memoHandleClick}
  ></MemoChild>
~~~



**React.memo와 함께 사용**
{:.lead}
여기서 작은 팁을 하나 드리고 싶습니다.<br/>
**`useCallback`**과 **`useMemo`**를 사용하면 **`callback`** 함수와 값에 대해서 **`메모이제이션`**이 되어 불필요한 자원을 낭비하지않아도 된다고 설명을 드렸는데요
여기서 하나 더 나아가 **`React.memo`**를 함께 사용해주시면 **`Props`**의 변경사항이 있을 경우 리렌더링이 발생하기때문에 더욱 효과적일 것입니다.

![useCallback](/assets/img/posts/tech/fe/20230613/useCallback.png)

***

## 맺음

이렇게 React에서 Rendering 이란 무엇인지와 과정을 알아보았습니다.<br/>
메모이제이션을 사용하여 성능향상을 도모할 수 있었지만, 반대로 성능저하를 야기할 수 있습니다.<br/>
메모이제이션을 통해 메모리에 해당 정보를 저장하기때문인데요. 무분별하게 메모이제이션을 하게 된다면<br/>
오히려 하지않았을 때 보다 성능이 저하 될 수 있습니다. 역시 뭐든지 적당히 적절히가 중요한가봐요.


<br/>

>Reference
>- https://velog.io/@mogulist/understanding-react-rerender-easily
>- https://velog.io/@eunbinn/when-does-react-render-your-component
>- https://ko.legacy.reactjs.org/docs/hooks-reference.html
>- https://ko.legacy.reactjs.org/docs/components-and-props.html
>- https://ko.legacy.reactjs.org/docs/reconciliation.html

<!-- Links -->
[Fiber]: https://github.com/acdlite/react-fiber-architecture
