---
layout:     post
title:      CSS Selector 1 - 선택자
subtitle:   Css 기본적인 선택자
categories: [tech]
tags:       [publishing,css]
---

1. 순서가 있는 목차
{:toc}

## 개요
CSS 란 `Cascading Style Sheets`{:.primary} 의 약어로 <br/>
직역하면 `위에서 아래로 내려가는(폭포처럼) 스타일 문서` 정도로 이해할 수 있는데요.

여기서 `Cascading`{:.primary} 이라는 뜻은 **`중요도`**, **`명시도`**, **`코드 순서`**에 따라 적용되는 의미를 가집니다.(`우선순위`{:.primary}를 의미합니다.)

이러한 CSS의 가장 기초가 되는 것이 바로 `Selector` 입니다.

<br/>

CSS 선택자는 크게 3가지로 분류할 수 있습니다.
- **`기본 선택자`**{:.primary}
- **`복합 선택자`**{:.primary}
- **`가상 선택자`**{:.primary}

각각의 Selector에 대해서 예시와 함께 알아보고 중요한 `우선순위`{:.primary}에 대해서 다루고자 합니다.

### 기본 선택자
가장 기본이 되는 선택자로 `전체 선택자`{:.primary}, `태그 선택자`{:.primary}, `클래스 선택자`{:.primary},
`Id 선택자`{:.primary}, `Attribute 선택자`{:.primary} 5가지가 있습니다.

{% include components/codepen.html hash="mdzYGLe" title="basic selector" %}

<br/>

#### 전체 선택자
**선택자** : **`*`**{:.primary}<br/>
웹페이지의 모든 요소를 선택자하는 `Universal Selector` 입니다.<br/>
전체 요소에 적용되는 선택자이기 때문에 사용할 수 있는 경우가 적은 선택자입니다.<br/>
스타일을 초기화 하거나, 자식 요소에 대해서 사용하는 경우가 많은 선택자입니다.


~~~css
*{
  background-color : skyblue;
}
~~~

#### 태그 선택자
**선택자** : **`태그명칭`**{:.primary}<br/>
HTML의 태그 요소 명칭으로 요소를 찾는 선택자입니다.
~~~css
span {
  background-color : greenyellow;
}
~~~

#### 클래스 선택자
**선택자** : **`.class-name`**{:.primary}<br/>
HTML 요소에 부여된 class 명칭으로 요소를 찾는 선택자입니다.
~~~css
.item{
  background-color : blueviolet;
}
~~~

#### Id 선택자
**선택자** : **`#id`**{:.primary}<br/>
HTML에서 고유한 값으로 사용되는 `id`로 요소를 찾는 선택자입니다.<br/>
~~~css
#first{
  background-color : orangered;
}
~~~

#### Attribute 선택자
**선택자** : **`[]`**{:.primary}<br/>
태그가 가지고 있는 속성(attribute)으로 요소를 찾는 선택자입니다.<br/>
속성에 대한 선택자이기 때문에 다른 기본 선택자들과 다르게 정의 할수 있는 선택 option이 많습니다.<br/>

- **일치선택자** `[속성]`, `[속성="속성값"]` : 속성값과 일치하는 요소를 찾습니다.
- **포함선택자** `[속성~="속성값"]`, `[속성*="속성값"]` : 속성값을 포함하는 요소를 찾습니다. 
- **시작선택자** `[속성^="속성값"]`, `[속성 \|="속성값"]` :  속성값으로 시작하는 요소를 찾습니다.
- **맺음선택자** `[속성$="속성값"]` : 속성값으로 끝나는 요소를 찾습니다. 

***

##### 일치 선택자 
- `[속성]` : 입력한 속성을 가지는 요소를 찾습니다.
{% include components/codepen.html hash="yLRWQLB" title="attribute selector 1" %}<br/>

~~~css
[type] {
    background-color: orangered;
}
/* 혹은 */
input[type] {
    background-color: orangered;
}
~~~
- `[속성이름="속성값"]` : 속성이름의 값이 속성값인 요소를 찾습니다.

{% include components/codepen.html hash="QWZRJbR" title="attribute selector 2" %}<br/>
~~~css
input[type="submit"] {
  background-color: yellow;
}
~~~

***

##### 포함선택자
- `[속성이름 ~= "속성값"]` : 속성값을 포함하는 요소를 찾습니다. 이때 속성값은 띄어쓰기 구분하는 문자열 중 일치하는 **값**으로 찾습니다.
  {% include components/codepen.html hash="JjmqeKN" title="attribute selector 3" %}<br/>
~~~css
input[value~="button1"] {
  background-color: yellow;
}
~~~

- `[속성이름 *="속성값"]` : 속성값이 **포함/일치** 하는 요소를 찾습니다. 속성값이 **일부만 일치**하여도 적용됩니다.
  {% include components/codepen.html hash="rNqgQYW" title="attribute selector 4" %}<br/>
~~~css
input[value*="button"] {
  background-color: orangered;
}
~~~

***

##### 시작 선택자
- `[속성이름 ^= "속성값"]` : 속성값으로 **시작(포함/일치)**하는 요소를 찾습니다.
  
{% include components/codepen.html hash="KKGLrGz" title="attribute selector 5" %}<br/>
~~~css
input[value ^="button"] {
  background-color: yellow;
}
~~~

- `[속성이름 |= "속성값"]` : 속성값으로 정확히 **일치**하거나, `-(하이픈)`앞의 `prefix` 속성값과 일치하는 요소를 찾습니다.<br/> 
속성값이 정확히 일치한다는 것은, 하나의 속성값만을 가지고 그 값이 일치하는 것을 말합니다. <br/>
\-(하이픈) prefix 란 것은 속성값이 `ko-KR` 일때 `[속성 \|= "ko"]`로 일치하는 것을 말합니다. <br/>
{% include components/codepen.html hash="eYPabOE" title="attribute selector 6" %}<br/>
~~~css
input[value |="btn"] {
  background-color: yellow;
}
~~~

***

##### 맺음 선택자
- `[속성이름 $= "속성값"]` : 속성값으로 **끝맺음** 하는 요소를 찾습니다.
  {% include components/codepen.html hash="MWPdZjv" title="attribute selector 7" %}<br/>
~~~css
input[value$="버튼"] {
  background-color: yellow;
}
~~~

***
### 복합 선택자(결합자)
선택자를 결합하여 만드는 선택자로 **`특정 기호`**{:.primary}를 사용하여 결합하는 방식입니다.

> HTML의 태그들은 포함관계를 이룰 때 부모, 자식으로 구분할 수 있습니다. 
>   ```html
>   <html> <!--가장 밖의 부모-->
>       <body> <!--div의 부모이면서 html의 자식-->
>           <div></div><!--body의 자식-->
>       </body>
>   </html>
>   ```
> 이러한 부모/자식 관계를 알고 보시면 더 좋습니다.


#### 일치 선택자
**기호** : **`""`**{:.primary}<br/>
몇가지 선택자를 연결하고, 지정한 선택자를 모두 만족하는 요소를 찾는 선택자입니다.
{% include components/codepen.html hash="QWZXbbQ" title="attribute selector 8" %}<br/>
~~~css
div.item {
  background-color: orange;
}
div#first.item {
  background-color: orangered;
}
~~~


#### 자손 선택자
**기호** : **`" "(공백)`**{:.primary}<br/>
첫번째 요소의 `하위 요소` 중 일치하는 `모든 요소`를 찾는 선택자입니다.<br/>
자식 결합자와 명칭도 비슷하고 동작이 유사하지만 `모든 하위`, `직계`의 차이가 있어서 명칭이 `자손`,`자식`으로 나뉩니다.
{% include components/codepen.html hash="KKGjpze" title="attribute selector 9" height="450" %}<br/>
~~~css
section div{
  background-color: orange;
}
~~~

#### 자식 선택자
**기호** : **`>`**{:.primary}<br/>
첫번째 요소의 `하위 요소` 중 일치하는 `직계 하위(자식) 요소`를 찾는 선택자입니다.<br/>
`자손 결합자`는 하위 모든 요소들중 일치하는 요소를 모두 찾지만 `자식 결합자`는 `직계 하위요소`에서 일치하는 요소만을 찾습니다.
{% include components/codepen.html hash="NWOZqaM" title="attribute selector 10" %}<br/>
~~~css
section > div {
  background-color: orange;
}
~~~

#### 인접(후속) 형제 선택자
**기호** : **`+`**{:.primary}<br/>
`같은 부모`의 자식요소 중 `첫번째 요소`의 `바로 다음`에 오는 `하나의 요소`를 찾는 선택자입니다.<br/>
{% include components/codepen.html hash="jOejPpQ" title="attribute selector 11" height="220"%}<br/>
~~~css
.primary + div {
  background-color: orange;
}
~~~

#### 일반 형제 선택자
**기호** : **`~`**{:.primary}<br/>
`같은 부모`의 자식요소 중 `첫번째 요소`의 `다음`에 오는 `모든 요소`를 찾는 선택자입니다.<br/>
`인접 형제 선택자`와 다르게 `모든 형제 요소`를 찾습니다.
{% include components/codepen.html hash="vYVqOMJ" title="attribute selector 12" height="220" %}<br/>
~~~css
.primary ~ div {
  background-color: orange;
}
~~~

***

### 가상 선택자
**기호** : **`:`**{:.primary}<br/>
CSS에는 `가상 요소(:pseudo-element)`{:.primary}와 `가상 클래스(:pseudo-class)`{:.primary}가 있습니다.<br/>
이 것들은 html의 태그 요소로써 요소의 `특정한 상태(state)`를 나타내거나 `디자인적인 요소`를 추가할 때 사용됩니다.<br/>

`가상 선택자`{:.primary}는 가상 클래스를 찾는 선택자입니다.<br/> 

크게 `동적 의사 클래스`, `상태 의사 클래스`, `구조 의사 클래스` 선택자가 있습니다.<br/> 
가상클래스의 경우 정말 많은 클래스가 있기때문에 [가상클래스]에서 더 많은 클래스를 확인하시기 바랍니다.

```css
a:link{
    background-color: orange;
}
input:focus{
    background-color: orange;
}
```

#### 동적 의사 선택자

| 선택자      | 설명                            |
|----------|-------------------------------|
| :link    | 사용자가 한번도 방문하지않는 상태            |
| :visited | 사용자가 한번이라도 방문한 상태             |
| :hover   | 사용자가 마우스 커서를 요소에 올린 상태        |
| :active  | 사용자가 요소를 마우스 좌클릭으로 클릭하고 있는 상태 |
| :focus   | 대화형 콘텐츠의 focus 상태 (ex:input)  |


#### 상태 의사 선택자

| 선택자      | 설명                                                |
|----------|---------------------------------------------------|
| :checked | input 요소 중 checked 된 상태 \<input type="" checked/> |
| :enable  | input 요소 중 사용한 상태 \<input type="" />              |
| :disable | input 요소 중 사용이 불가한 상태 \<input type="" disabled/>  |

#### 구조 의사 선택자

`child`와 `type`의 차이가 존재하고 ()안에 조건식을 넣을 수 있는 등 이 포스트에 다 담지 못하는 내용이기에<br/>
구조의사 선택자의 경우 다른 포스트에서 자세히 다루도록 하겠습니다.

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

### 가상 요소 선택자
**기호** : **`::`**{:.primary}<br/>
`가상 요소 선택자`{:.primary}는 가상 요소(:pseudo-element)를 찾는 선택자입니다.<br/>

| 선택자               | 설명                                              |
|-------------------|-------------------------------------------------|
| ::first-letter    | 텍스트의 첫 글자만을 선택                                  |
| ::first-line      | 텍스트의 첫 라인만을 선택함                                 |
| ::before          | 특정 요소의 내용(content) 부분 바로 앞에 다른 요소를 삽입할 때 사용     |
| ::after           | 특정 요소의 내용(content) 부분 바로 뒤에 다른 요소를 삽입할 때 사용     |
| ::selection       | 해당 요소에서 사용자가 선택한 부분만 선택                         |

### 기타 선택자

| 선택자     | 설명                           |
|---------|------------------------------|
| :not()  | () 조건에 해당하지않는 요소 선택          |
| :lang() | HTML lang 속성값이 ()로 지정된 요소를 선택 |
| :target | # target으로 이동이 된 요소를 선택      |


<!-- Links -->
[가상클래스]: https://developer.mozilla.org/ko/docs/Web/CSS/Pseudo-classes