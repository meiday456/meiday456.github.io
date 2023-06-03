---
layout:     post
title:      Next js title에 주석이 같이 나타났다가 사라진다?!
subtitle:   next js title에 주석이 같이 나타났다가 사라진다?!
categories: [tech]
tags:       [til,fe,next]
---

- 순서가 없는 목차
{:toc}

## Next JS title에 `<!-- -->` 이 나타났다가 사라져요?!

### 배경

**`NextJS`**{:.primary} 를 활용한 **`영화 앱`**{:.primary}을 개발하는 도중 각 상세 페이지의 **`title`**{:.primary}을<br/>
각 영화나 TV 프로그램의 명칭으로 하려고 작업을 하고 화면을 확인해보니

![주석이 달린 title 이미지](/assets/img/posts/tech/til/20230603/nextJs_title_with_comment.png){:.centered}

:scream: 이게 뭔일인가???? 의도 하지않은 `주석`이 달려서 나오는 것입니다.

### console log

또한 콘솔 log에 이러한 문구가 나타납니다.

>Warning: A title element received an array with more than 1 element as children. In browsers title Elements can only have Text Nodes as children. If the children being rendered output more than a single text node in aggregate the browser will display markup and comments as text in the title and hydration will likely fail and fall back to client rendering

### 원인
이 문제는 Next Js에서도 한번 언급이 되었던 문제로 [#38256] 에서 확인 할 수 있었고 쉽게 해결 할 수 있었습니다.

저의 경우 아래 처럼 **`title`**{:.primary} 을 출력하고 있었는데요
```tsx
const title = info.name || info.title;
// 중략
<Head>
    <title>{title} | Meiday</title>
</Head>

```

이 경우 "name <!-- --> \| <!-- --> Meiday" 이렇게 `5`개의 **`nodes`** 를 가지게 됩니다.<br/>

### 해결
이를 해결하는 방법은 매우 간단합니다. 아래 처럼 변경하여 1개의 **`nodes`** 만을 가지도록 변경 해주면 됩니다.

```tsx
const title = `${info.name || info.title || "detail"} | Meiday`;
// 중략
<Head>
    <title>{title}</title>
</Head>

```

>Reference
>- https://github.com/vercel/next.js/discussions/38256
>- https://www.youtube.com/watch?v=rhvec8cXLlo




<!-- Links -->
[#38256]: https://github.com/vercel/next.js/discussions/38256