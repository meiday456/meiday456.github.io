---
layout : post
title : "테스트 작성"
subtitle: "테스트"
category : tech
tags: javascript
---

테스트 작성 문서입니다.

1. 순서가 있는 목차
{:toc}

# 목차가 들어가는가?

## 하위 목차

# 다음 목차

## 다음다음 목차

### 다다음 목차

This is a text with a footnote[^1].

[^1]: Your footnote text.


{:.faded}
I'm faded, faded, faded.

> 일반 인용문

> 커다란 인용문
> {:.lead}

커다란 텍스트

일반 텍스트
뭐야 왜 안나와?
아 이거 줄바꿈 방법이 <br> 뿐인가?

테이블

| Default aligned | Left aligned            |  Center aligned   |    Right aligned |
|-----------------|:------------------------|:-----------------:|-----------------:|
| First body part | Second cell             |    Third cell     |      fourth cell |
|-----------------| :---------------------- | :---------------: | ---------------: |
| 값을 넣어보겠습니다.| 텍스트 길이가 길어질때 어떻게 변화하는지  |짧은 문구 |마지막 셀 |
{:.scroll-table}

아래는 ``` 로 만든 코드 block

```javascript
  function a(){
    console.log();
  //   lalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalalala
  }
```


아래는 ~~~ 로 만든 코드 불렁

~~~javascript
const handler = (e)=>{
  e.stopPropagation();
  console.log(e.target.value);
}
~~~