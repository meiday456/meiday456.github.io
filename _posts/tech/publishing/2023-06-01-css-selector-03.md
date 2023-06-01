---
layout:     post
title:      CSS Selector 3 - 구조의사클래스(nth)
subtitle:   Css 기본적인 선택자
categories: [tech]
tags:       [publishing,css]
---

1. 순서가 있는 목차
{:toc}

## 개요
지난 [css-selector-01]에서 **`구조의사 선택자(nth~~)`**{:.primary}에 대한 포스트입니다.<br/>
왜 따로 작성하느냐 하면 다룰 내용도 많았고, 제가 사용하면서 헤매였던 것들을 기재하고자 함입니다.

**`구조의사 선택자`**{:.primary}는 다음 표의 선택자를 사용하여<br/> 
**`HTML`** 요소의 계층 구조에서 특정 위치에 있는 요소를 선택할 수 있습니다.<br/>
각각 예시와 함께 알아보도록 하겠습니다.

| 선택자                 | 설명                                            |
|---------------------|-----------------------------------------------|
| :first-child        | 모든 자식요소 중 맨 앞의 요소                             |
| :last-child         | 모든 자식요소 중 가장 마지막 요소                           |
| :nth-child()        | 모든 자식요소 중 ()의 조건에 해당하는 요소를 앞에서 부터 모두 선택       |
| :nth-last-child()   | 모든 자식요소 중 ()의 조건에 해당하는 요소를 뒤에서 부터 모두 선택       |
| :first-of-type      | 모든 자식요소 중 앞에서 등장하는 특정 Type의 요소를 모두 선택         |
| :last-of-type       | 모든 자식 요소 중 뒤에서 등장하는 특정 Type의 요소를 모두 선택        |
| :nth-of-type()      | 모든 자식요소 중 ()의 조건에 해당하는 Type의 요소를 앞에서 부터 모두 선택 |
| :nth-last-of-type() | 모든 자식요소 중 ()의 조건에 해당하는 요소를 뒤에서 부터 모두 선택       |
| :only-child         | 자식요소로 오직 단 하나의 요소를 가지는 요소를 선택                 |
| :only-of-type       | 자식요소로 오직 단 하나의 Type의 요소를 가지는 요소를 선택           |
| :empty              | 자식 요소를 하나도 가지지않는 요소를 모두 선택                    |
| :root               | 문서의 root 요소를 선택                               |
{:.indent-1}

## :first-child :last-child

{% include components/codepen.html hash="RwezBEY" title="1" height="400"%}<br/>

### :first-child
모든 자식요소 중 맨 **`앞의`** 요소를 찾는 선택자입니다.<br/>
여기서 주목해야하는 점은 `모든 자식`{:.primary} 입니다. 모든 자식 중에서 이기때문에 HTML 전체의 모든 자식을 의미합니다.<br/>
예시를 보면 처음 body의 자식인 첫번째 큰 div도 색상이 지정된 것을 볼 수 있습니다.

### :last-child
모든 자식요소 중 맨 **`뒤의`** 요소를 찾는 선택자입니다.<br/>
**`:first-child`**{:.primary}와 마찬가지로 모든 자식중에서 찾는 다는것을 알고있어야합니다.<br/>
**`codepen`** 예시에서 마지막 div에도 적용되어 나와야 하지만,<br/>
codepen의 result는 body 자식요소의 마지막이 `script`이기때문에 적용 되지않는 모습을 볼 수 있습니다.

~~~css
/* 첫번째 요소 선택 */
div:first-child { background-color: green; }
/* 마지막 요소 선택 */
div:last-child { background-color: yellow;}
~~~

***

## :nth-child() :nth-last-child()
모든 자식 요소 ()조건에 해당하는 자식요소를 모두 찾는 선택자입니다.<br/>
`가장 많이 사용되고 활용도가 높은 선택자`이며 아래에 설명하는 () 조건은 다른 선택자에도 동일하게 적용됩니다.

()안에 조건을 삽입할 수 있는데 2가지 형식으로 넣을 수 있습니다.
1. **키워드 값**
2. **함수형 표기법**

### 키워드 값
**`odd`**{:.primary} : 형제 요소에서 `홀수`번째 요소를 선택<br/>
**`even`**{:.primary} : 형제 요소에서 `짝수`번째 요소를 선택

{% include components/codepen.html hash="dygBqpg" title="2" height="400"%}<br/>
```css
/* 홀수 번째 선택*/
.odd li:nth-child(odd){ background-color : tomato;}

/* 짝수 번째 선택*/
.even li:nth-child(even){ background-color : tomato; }

```

### 함수형 표기법
**`An + B`**{:.primary}<br/>
사용자가 지정한 패턴으로 선택합니다.<br/>
- **`A`** 는 정수 인덱스 증가량
- **`B`** 는 정수 offset
- **`n`** 은 **`0`** 부터 시작하는 모든 양의 정수

:warning: **`n`**은 단 **`하나`**만 사용이 가능합니다. 

{% include components/codepen.html hash="Poyrdjo" title="3" height="400"%}<br/>

### :warning: 주의사항  
nth-child의 경우 예시 처럼 중간에 다른 태그가 들어오는 경우 의도와 다른 스타일이 입혀지는것을 볼 수 있습니다.<br/>
이는 부모를 기준으로 하는 `child` 를 찾기 때문입니다.

{% include components/codepen.html hash="mdzZGBN" title="4" height="400"%}<br/>

저는 앞에서 부터 읽는 것보다 뒤에서 부터 읽어 내는 것을 추천드립니다.(사실 css 선택자는 뒤에서 부터 읽는게 좋긴합니다.)
```css
/* 앞에서 부터 읽을 경우 li 중 첫번째 요소만 선택 으로 읽히기 쉽습니다.*/
/* 뒤에서 부터 읽을 경우 첫번째 요소이고, li 인 요소를 선택으로 읽힙니다.*/
li:nth-child(1){}

/* 앞에서 부터 읽을 경우 li 중 마지막 요소만 선택 으로 읽히기 쉽습니다.*/
/* 뒤에서 부터 읽을 경우 마지막 요소이고, li 인 요소를 선택으로 읽힙니다.*/
li:nth-last-child(1){}

```

### 활용 예시
{% include components/codepen.html hash="wvYLEQz" title="5" height="400"%}<br/>
~~~css
/* 앞에서 부터 3n + 1 번째 이면서 li 선택 */
section:nth-child(1) li:nth-child(3n+1){background-color : tomato;}
/* 뒤에서 부터 3n +1 번째 이면서 li 선택 */
section:nth-child(2) li:nth-last-child(3n+1){background-color : tomato;}
/* 앞에서 부터 2번째부터 5반째까지 이면서 li 선택 */
section:nth-child(3) li:nth-child(n+2):nth-child(-n+5){background-color : tomato;}
/* 뒤에서 부터 2번째부터 5반째까지 이면서 li 선택 */
section:nth-child(4) li:nth-last-child(n+2):nth-last-child(-n+5){background-color : tomato;}

/*  3개 이상이라면 전체 선택*/
section:nth-child(5) li:nth-last-child(n+3),
section:nth-child(5) li:nth-last-child(n+3) ~ li { background-color : tomato;}
~~~

***

## :first-of-type :last-of-type
### type과 child의 차이
`child`와 대조적으로 사용되는 것이 `type`입니다. `nth-child`를 설명할때 언급한 `주의사항`을 피할 수 있는 선택자 이기도합니다.

`child`는 자식 요소 중에서 찾는다면 `type`은 동일한 타입을 지정합니다.
{% include components/codepen.html hash="GRYbYZq" title="6" height="400" tab="result,css" %}<br/>

***

- **`:first-of-type`**{:.primary} : `모든 자식요소` 중 `앞에서` 등장하는 특정 `Type`의 요소를 모두 선택
- **`:last-of-type`**{:.primary} : `모든 자식요소` 중 `뒤에서` 등장하는 특정 `Type`의 요소를 모두 선택

{% include components/codepen.html hash="GRYbYEr" title="7" height="400" tab="result,css" %}<br/>

***

## :nth-of-type() :nth-last-of-type()
**`:nth-child()`**{:.primary} **`:nth-last-child()`**{:.primary} 과 거의 동일하게 동작하지만<br/>
앞서 언급한 `type`의 특성인 동일한 `type`으로 동작하는 차이가 있습니다.

{% include components/codepen.html hash="KKGjGyB" title="8" height="500" tab="result" %}<br/>
~~~css
/* 부모요소중 앞에서 부터 3n + 1 번째의 li를 선택 */
section:nth-of-type(1) li:nth-of-type(3n+1){ background-color : tomato; }
/* 부모요소중 뒤에서 부터 3n + 1 번째의 li를 선택 */
section:nth-child(2) li:nth-last-of-type(3n+1){ background-color : tomato; }
/* 부모요소중 앞에서 부터 부터 2 ~ 5 번째의 li를 선택 */
section:nth-child(3) li:nth-of-type(n+2):nth-of-type(-n+5){ background-color : tomato; }
/* 부모요소중 뒤에서 부터 부터 2 ~ 5 번째의 li를 선택 */
section:nth-child(4) li:nth-last-of-type(n+2):nth-last-of-type(-n+5){ background-color : tomato; }

~~~


## :only-child :only-of-type
- **`:only-child`**{:.primary} : 자식요소로 오직 단 하나의 요소를 가지는 요소를 선택
- **`:only-of-type`**{:.primary} : 자식요소로 오직 단 하나의 Type의 요소를 가지는 요소를 선택
  {% include components/codepen.html hash="eYPwPxw" title="9" height="500" tab="result,css" %}<br/>

## :empty :root
- **`:empty`**{:.primary} : 자식 요소를 하나도(text 조차) 가지지 않는 요소를 모두 선택
- **`:root`**{:.primary} : 문서의 root 요소를 선택

{% include components/codepen.html hash="ZEqdmzg" title="10" height="400" tab="result,css" %}<br/>

***

>Reference
>- https://velog.io/@hsecode/nth-child
>- https://developer.mozilla.org/ko/docs/Web/CSS/:nth-child


<!-- Links -->
[css-selector-01]: /tech/2023-05-30-css-selector-01
