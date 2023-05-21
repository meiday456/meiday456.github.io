---
layout:     post
title:      Testing library Render 톺아보기
subtitle:   srcset과 sizes, 설정등을 활용하여 이미지 최적화
categories: [tech]
tags:       [fe , test]
---

1. 순서가 있는 목차
{:toc}

## 개요
Front End 개발에서도 이제는? 테스트 코드에 대한 중요성이 강조되고 있고 또 많은 테스트 라이브러리와 방법, 패러다임 등이 등장하고 있는 것 같습니다.

수 많은 라이브러리 중 `Jest`{:.primary}와 `Testing library`{:.primary}는
가장 많이 사용된다고 해도 무방할 정도로 많은 기업에서 사용되며, 많은 레퍼런스를 찾아볼 수 있습니다.


저도 자주 사용하는 테스트 라이브러리인데 render 함수를 사용할때 아래와 같이 사용해왔습니다.

```jsx
    const {container} = render(<div></div>);
```

그런데 react query 테스트 코드를 작성하면서 [query test] 를 참고하여 테스트 코드를 작성하면서 render hook과 wrapper 를 조사하면서
render 함수를 제대로 알고 사용하지않는 것을 깨달았고 render()에 대한 내용을 작성합니다.

<hr/>

## render()란?
```typescript
    function render(
      ui: React.ReactElement<any>,
      options?: {
        /* You won't often use this, expand below for docs on options */
      },
    ): RenderResult
```


>Reference
>- https://testing-library.com/
>- https://tanstack.com/query/v4/

<!-- Links -->
[query test]: https://tanstack.com/query/v4/docs/react/guides/testing 
