---
layout:     post
title:      CSS Selector 2 - 우선순위와 상속
subtitle:   Css 기본적인 선택자
categories: [tech]
tags:       [publishing,css]
---

1. 순서가 있는 목차
{:toc}

## 개요
**`Css`**{:.primary}는 **`Cascading Style Sheet`**{:.primary}의 약자로 직역하면 `위에서 아래로 떨어지는 스타일 문서`입니다.<br/>
많은 선택자와 스타일이 작성이 되고 의도하지않은 `중복된 스타일`들이 생겨나게 될 것입니다.<br/>

**`Css`**{:.primary}에는 이러한 경우를 위한 중요한 `두가지 원칙`이 존재합니다.

1. **스타일 우선순위**
2. **스타일 상속**

***

## 우선순위
**`Css`**{:.primary}에 선언 될 수 있는 많은 `스타일`들은 각각 우선순위를 가지고 우선순위가 가장 높은 스타일이 결정됩니다.<br/>

이러한 **`우선순위`**를 책정하는 **`3가지 규칙`**이 존재합니다.
1. **중요도**
2. **명시도**
3. **코드가 작성된 순서**

***

### 중요도
**`Css`**{:.primary}가 어디에서 선언됬는가? 를 평가하는 중요도입니다.<br/>
Css는 다양한 방법으로 선언되고 적용될 수 있습니다. 아래의 순서대로 중요도가 높습니다.

1. `<head>`의 `<style>` 선언
2. `<head>`의 `<style>` 내의 `@import`
3. `<link>`로 링크된 `.css` 파일
4. `<link>`로 링크된 `.css` 파일 내의 `@import`
5. `브라우저`의 기본 `Css`

***

### 명시도
**`Css`**{:.primary}의 스타일 선언 시 선택자에 따라 가중치가 측정되고<br/>
이 가중치의 **`합산`**이 스타일 적용에 반영되며 선택자의 **`조합`**의 **`합계`**로 적용됩니다.<br/>

가중치 계산은 아래와 같이 합니다.

| 선택자                            | 점수    | 예시                                           |
|--------------------------------|-------|----------------------------------------------|
| **`!important`**{:.primary}    | 무한점   | position : relative !important;              |
| **`inline`**{:.primary}        | 1000점 | <h1 style='background-color: black'>인라인</h1> |
| **`id 선택자`**{:.primary}        | 100점  | #id{}                                        |
| **`class 선택자`**{:.primary}     | 10점   | .class{}                                     |
| **`가상 클래스 선택자`**{:.primary}    | 10점   | .class:hover{}                               |
| **`가상 요소 선택자`**{:.primary}     | 10점   | .class::after{}                              |
| **`Attribute 선택자`**{:.primary} | 10점   | input[type]{}                                |
| **`태그 선택자`**{:.primary}        | 1점    | div{}                                        |
| **`전역 선택자`**{:.primary}        | 0점    | *{}                                          |
| **`상속`**{:.primary}            | X점    ||

이 중 !important의 경우 앞선 모든 명시도와 중요도를 무시하고 style이 적용되는데요<br/>
이러한 경우 라이브러리의 style을 무시할 수 있고, 재정의 하는 유일한 방법은 다른 !important를 적용하는 방법 뿐이라
사용시 주의 하셔야합니다.

참고용으로 이러한 선택자 구분을 할 수 있는 [Specificity Calculator]을 남기겠습니다.<br/>

***

### 코드가 작성된 순서
우선순위의 마지막인 `코드가 작성된 순서`입니다.<br/>
마지막에 언급드리는 이유는 동일한 선택자를 중복해서 작성하는 경우에 적용되는 요소이기 때문인데요<br/>
스타일을 중복해서 여러군데요 작성되면 코드의 유지보수,확장성 등이 떨어지기 때문입니다.<br/>

차치하고, 코드가 작성된 순서가 **`더 나중에 작성된`** `코드`를 **`우선적`**으로 적용합니다.


```css

p {
    color : blue; /* 적용 X */
}

p {
    color : red; /* 적용 O */
}
```

***

## 상속(inheritance)
**`Css`**{:.primary} `Style`{:.primary}을 작성하다 보면 `"적용한적 없는 스타일이 적용되어 있거나"`, `"원치 않게 자식 요소에 반영된 스타일"` 등을 만날 수 있는데요

이는 부모의 요소에서 설정된 일부 `Style`{:.primary} 들이 자식요소에 의해 `상속`{:.primary}되어서 그렇습니다.<br/>
하지만 이런 `상속`{:.primary}은 모든 요소에 적용되지않습니다.<br/>

적용되는 대표적인 `속성`{:.primary}들은<br/>
`color`, `font`, `visibility`, `opacity`, `line-height`, `text-align`, `white-space`....<br/>
등이 있습니다.

{% include components/codepen.html hash="RwezpMb" title="1" height="400" %}<br/>

### 상속제어하기
이러한 상속은 **`4가지의 특수 범용 속성`** 값을 제공합니다.<br/>
자동으로 `상속되지 못하는` 다른 `속성`에 해당 `속성값`을 부여하여 `상속`을 받을 수 있습니다.

| 속성값                      | 설명                                                                      |
|--------------------------|-------------------------------------------------------------------------|
| **`inherit`**{:.primary} | 부모의 요소로부터 상속받습니다.                                                       |
| **`initial`**{:.primary} | 브라우저의 기본값을 사용                                                           |
| **`unset`**{:.primary}   | 속성이 자연적으로 상속되면 inherit, 그렇지않으면 initial으로 동작                             |
| **`revert`**{:.primary}  | 현재 엘리먼트에 선언 된 css 속성을 부모 속성 또는 user agent에 따라 default로 선언 된 속성으로 되돌립니다. |


{% include components/codepen.html hash="YzJoVEG" title="2" height="220" %}<br/>

<!-- Links -->
[Specificity Calculator]: https://polypane.app/css-specificity-calculator