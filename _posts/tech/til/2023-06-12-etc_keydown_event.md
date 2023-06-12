---
layout:     post
title:      React onKeydown event와 focus에 대한 삽질
subtitle:   React Keydown event와 focus에 대한 삽질
categories: [tech]
tags:       [til]
---

- 순서가 없는 목차
{:toc}

## React onKeydown Event

**`React`**{:.primary} 컴포넌트는 **`onKeyDown`**,**`onKeyUp`**,**`onKeyPress(deprecated)`** 의 [keyboard 이벤트]를 정의 할 수 있습니다.
이러한 keyboard 이벤트는 아래와 같이 정의하여 쉽게 사용이 가능합니다.

```tsx
function Input() {
  const handlerKeydown = (e: React.KeyboardEvent) => {
    const {key} = e;
    console.log(key);
  };
  return <input type="text" onKeyDown={handlerKeydown} />;
}
```
>속성 값                                  <br/>
> altKey : boolean = Alt 키 입력 여부                      <br/>
> charCode : number = [Deprecated]                      <br/>
> ctrlKey :boolean = Control 키 입력 여부                     <br/>
> getModifierState(key) : boolean =  Caps Lock, Fn 키 등이 켜져 있는지의 여부      <br/>
> key : string  = 사용자의 입력키 정                         <br/>
> keyCode : number  [Deprecated]                      <br/>
> locale : string                         <br/>
> location : number = 입력 키의 위치 정보(shift 왼쪽 1 , 오른쪽 2)                  <br/>
> metaKey : boolean = windows 의 win 키, mac의 command  입력 여부                    <br/>
> repeat : boolean  =   키가 반복되서 입려되는지 여부(press 대용을 사용가능)                   <br/>
> shiftKey : boolean =  shift 키 클릭 여부                   <br/>
> which : number    [Deprecated]                      <br/>


대부분 **`input`** 컴포넌트에 할당하여 사용할 것입니다.

그렇지만 간혹 **`div`**{:.primary} 와 같은 Element에 이벤트를 부여해야하는 경우가 있습니다.

이 경우에는 웹접근성을 위하여 tabIndex를 할당 해주시는게 좋습니다.<br/>
<p>tabIndex를 지정하지않아도 keyboard 이벤트가 동작합니다.</p>
{:.faded}

~~~tsx
const NumberFrame = ({addCalcCode}: {addCalcCode: (n: string) => void}): ReactElement => {
  const handlerKeyDown = (e: React.KeyboardEvent<HTMLDivElement>) => {
    const {key} = e;
    if (NUM_REG.test(key)) {
      e.preventDefault();
      addCalcCode(key);
    }
  };

  return (
    <Container onKeyDown={handlerKeyDown}>
      {NUMBERS.map(num => (
        <Button key={num} value={num} onClick={e => addCalcCode(e.currentTarget.value)}></Button>
      ))}
    </Container>
  );
};
~~~

## div keyboard 이벤트
css 연습과 리팩토링 연습을 위해 계산기를 만들고 keyboard event를 부여하는 과정에 저의 삽질이 시작이 되었습니다....<br/>
지금 생각해보면 당연한 것인데 왜? 라는 생각이 뇌를 잡아먹어버렸죠... :sob:


**`input`**{:.primary} Element 에 이벤트를 부여한 경우에는 당연하게도 `focus`가 된 상황을 인지하지않아도 되었지만<br/>
**`div`**{:.primary}에 키보드 이벤트를 부여한 경우 `focus` 가 되지않으면 이벤트를 부여했어도 동작하지 않습니다.<br/>

저의 경우 숫자 버튼 Container(div)에 이벤트를 정의하였기 때문에 실제 동작을 위해서는 버튼을 하나 클릭하고 난뒤 부터 동작을 하는 것 이였습니다.


결국에는 `document.addEventListener("keydown", handlerKeydown);` 를 사용하여 이벤트를 정의하는 것으로 정리하였습니다.
~~~tsx
  useEffect(() => {
    document.addEventListener("keydown", handlerKeydown);
    return () => {
      document.removeEventListener("keydown", handlerKeydown);
    };
  }, []);
~~~

### 결론
keyboard 이벤트를 정의할때 focus 에 대한 이해가 부족하고, 이벤트만 할당하면 된다고만 생각하여 문제가 있었습니다.
조금만 생각해보면 input과의 차이로 바로 알 수 있었을텐데 말이죠.




<!-- Links -->
[#38256]: https://github.com/vercel/next.js/discussions/38256
[keyboard 이벤트]: https://ko.legacy.reactjs.org/docs/events.html#keyboard-events 
