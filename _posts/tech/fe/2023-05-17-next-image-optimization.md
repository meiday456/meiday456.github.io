---
layout:     post
title:      Next.JS Image 최적화
subtitle:   srcset과 sizes, 설정등을 활용하여 이미지 최적화
categories: tech
tags:       fe , next
---

1. 순서가 있는 목차
{:toc}

## 개요
저는 [thmemoviedb]를 활용한 영화, TV 프로그램 정보를 찾아볼 수 있는 `Next.js` 기반의 
토이프로젝트를 진행하고있었습니다.<br/>
메인페이지 구성 상 상단에 큰 이미지를 표출하고 하위로 여러 작은 이미지들 slider로 나열하는 형식의 레이아웃을 구성하고있습니다.<br/>


![이미지 용량 통계](/assets/img/posts/tech/fe/20230517/image_statistics.png)
{:.centered}
웹개발을 할때 이미지는 전체 웹사이트 크기의 `68%`를 차지하는 통계 자료가 있는 만큼 웹개발을 하는 우리들에게 있어서는 놓치면 안되는 요소일 것입니다.
### 문제 상황

앞서 말씀드린 것처럼 메인 페이지에는 많은 갯수의 이미지들을 표출하고 있는 상황에서 초기 개발 당시 [thmemoviedb]에서 제공하는
큰 이미지 **original** 이미지를 사용하여 이미지를 load 하고있었습니다.

![최적화 전 페이지](/assets/img/posts/tech/fe/20230517/image_previous.gif){:.centered width="300"}
### 해결 방법

문제 상황을 해결하고자 [Next.JS]에서 제공하는 [Image]를 사용하여 개선한 경험을 공유하고자합니다.
<hr/>
## Next.JS Image 컴포넌트


공식문서에 나와있는 것 처럼 Image 컴포넌트 사용은 굉장히 간단하게 사용할 수 있습니다.
```javascript
import Image from 'next/image';
 
export default function Page() {
  return (
    <Image
      src="/profile.png"
      width={500}
      height={500}
      alt="Picture of the author"
    />
  );
}
```

### 주요 기능
- `width`, `height` 속성을 이용하여 `LCP`를 방지할 수 있게 이미지 영역을 미리 지정
- 이미지 `우선순위` 지정
- `placeholder` 속성을 이용한 이미지 로딩시 처리
- `Custom Loader`를 사용한 이미지 로딩

이번 포스팅에서는 `우선순위`와 `Custom Loader`에 대한 내용을 중점적으로 다루고자합니다.

### 이미지 우선순위
```javascript
<Image
      src="/profile.png"
      width={500}
      height={500}
      loading={"lazy"}  // default lazy,  {lazy} | {eager}
      priority={false}  // default false, {true} | {false}
      alt="Picture of the author"
    />
```

이미지 로드 시점과 관련된 속성으로 `loading` 과 `priority`가 있습니다.
- **loading** : lazy 설정 시 viewport 밖의 이미지가 로드가 필요한 시점까지 지연  
- **priority** : true 설정 시 Lazy load가 자동적으로 해제

### Custom Loader
Image의 src를 계산하거나 Device에 따라 다르게 호출해야하는 경우 사용할 수 있는 속성입니다.

먼저 Next.config 설정부터 해주어야합니다.

next.config.js
{:.note title="file"}

```javascript
const nextConfig = {
  experimental: {},
  images: {
    domains: ["image.tmdb.org"],
    deviceSizes: [600, 800, 1000, 1400, 2000, 3840],
    // default : [640, 750, 828, 1080, 1200, 1920, 2048, 3840]
    
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
    // default : [16, 32, 48, 64, 96, 128, 256, 384]
  }
}
```
저의 경우 외부 리소스 이미지를 사용하고 있기 때문에 domains에 해당 domain을 추가하였습니다.

- **deviceSizes** : 사용자의 사용 디바이스의 width 지정
- **imageSizes** : 이미지 너비 목록
> next.js는 next.config.js의 deviceSizes, imageSizes 설정 값을 참조해서 srcSet을 생성합니다.
> srcSet과 sizes에 대한 자세한 내용은 [참고 블로그]에 잘 정리되어있으니 참고하시면 좋을것 같습니다.

<br/>

Item.tsx
{:.note title="file"}

```typescript
const Item = (props: Props): React.ReactElement => {
  const imageLoader = ({src, width, quality}: ImageLoaderProps) => {
    return `${urls.common.image(src, "content", width)}?w=${width}&q=${quality || 75}`;
  };

  return (
    <ItemStyle>
      <Image
        src={props.poster_path!}
        alt={`${props.title || props.name || "대체이미지"}`}
        fill={true}
        loader={imageLoader}
        placeholder={"blur"}
        loading={"lazy"}
        blurDataURL={
          "data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mOsaWm5AQAFZgJe6m0qkAAAAABJRU5ErkJggg=="
        }
        sizes="(max-width: 600) 200px,
                (max-width: 1000px) 300px,
                (max-width: 1400px) 400px,
                (max-width: 2000px) 500px, 100vw"
        className={"image"}
      ></Image>
    </ItemStyle>
  );
};
```
`Loader` 설정은 `Custom Loader` 함수를 정의하여 속성에 부여하는 것으로 설정이 간단하게 됩니다.<br/>
`Loader` 함수는 인자로 `ImageLoaderProps : {src:string, width:number, quality:number}`를 받을 수 있습니다.
- src : Image 컴포넌트에 정의한 src 속성
- width : deviceSizes 설정과 연관되어 viewport에 따라 다르게 값을 전달받음
- quality : Image 컴포넌트에 정의한 quality 속성

<br/>

Utils.tsx
{:.note title="file"}
- viewport width에 따라 uri parameter를 변경하기 위한 함수
- [thmemoviedb]는 이미지를 original(높은 해상도) w200 ~ 500 까지의 이미지를 제공합니다.

```typescript
export function getItemImgWPath(width: number, type: "banner" | "content" = "content") {
  let w = "w500";
  if (type === "banner") {
    return "original";
  }
  if (0 <= width && width <= 1000) {
    w = "w300";
  } else if (1001 <= width && width <= 1400) {
    w = "w400";
  } else {
    w = "w500";
  }
  return w;
}
```

Custom Loader를 사용하여 width에 따라 반응형으로 src를 호출하도록 변경하여 LCP를 개선하였습니다.

## 적용 결과

![최적화 전 페이지](/assets/img/posts/tech/fe/20230517/image_previous.gif){: width="45%"}
![최적화 후 페이지](/assets/img/posts/tech/fe/20230517/image_next.gif){: width="45%"}
{:.centered}

적용 전 / 적용 후
{:.figcaption}


>Reference
>- https://nextjs.org
>- https://hyeyeong1011.github.io/2020-05-17-post45/
>- https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/

<!-- Links -->
[thmemoviedb]: https://www.themoviedb.org
[Image]: https://nextjs.org/docs/app/api-reference/components/image
[Next.JS]: https://nextjs.org
[참고 블로그]: https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/ 
