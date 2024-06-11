# 윤덕규 201930119

## 6월 11일 강의
### Containment와 Specialization을 같이 사용하기
* Containment를 위해서 props.children을 사용하고, Specialization을 위해 직접 정의한 props를 사용하면 됩니다.
* Dialog 컴포넌트는 이전의 것과 비슷한데 Containment를 위해 끝부분에 props.children을 추가했습니다.
* Dialog를 사용하는 SignUpDialog는 Specialization을 위해 props인 title, message에 값을 넣어주고 있고, 입력을 받기 위해 <input/>과 <button></button>을 사용합니다. 이 두 개의 태그는 모두 props.children으로 전달되어 Dialog에 표시됩니다.
* 이러한 형태로 Containment와 Specialization을 동시에 사용할 수 있습니다.

### 상속
* 합성과 대비되는 개념으로 상속(inheritance)이 있습니다.
* 자식 클래스는 부모 클래스가 가진 변수나 함수 등의 속성을 모두 갖게 되는 개념입니다.
* 하지만 리액트에서는 상속보다는 합성을 통해 새로운 컴포넌트를 생성합니다.
* 복잡한 컴포넌트를 쪼개 여러 개의 컴포넌트로 만들고, 만든 컴포넌트들을 조합하여 새로운 컴포넌트를 만들자!

### 컨텍스트란 무엇인가?
* 기존의 일반적인 리액트에서는 데이터가 컴포넌트의 props를 통해 부모에서 자식으로 단방향으로 전달되었습니다.
* 컨텍스트는 리액트 컴포넌트들 사이에서 데이터를 기존의 props를 통해 전달하는 방식 대신 '컴포넌트 트리를 통해 곧바로 컴포넌트에 전달하는 새로운 방식'을 제공합니다.
* 이것을 통해 어떤 컴포넌트라도 쉽게 데이터에 접근할 수 있습니다.
* 컨텍스트를 사용하면 일일이 props로 전달할 필요 없이 그림처럼 데이터를 필요로 하는 컴포넌트에 곧바로 데이터를 전달할 수 있습니다.
![alt text](image-1.png)
![alt text](image-2.png)

### 언제 컨텍스트를 사용해야 할까?
* 여러 컴포넌트에서 자주 필요로 하는 데이터는 로그인 여부, 로그인 정보, UI 테마, 현재 선택된 언어 등이 있습니다.
* props를 통해 데이터를 전달하는 기존 방식은 실제 데이터를 필요로 하는 컴포넌트까지의 깊이가 깊어질수록 복잡해집니다.
* 또한 반복적인 코드를 계속해서 작성해 주어야 하기 때문에 비효율적이고 가독성이 떨어집니다.
* 컨텍스트를 사용하면 이러한 방식을 깔끔하게 개선할 수 있습니다.
* 컨텍스트를 사용하려면 컴포넌트의 상위 컴포넌트에서 Provider로 감싸주어야 합니다.

### 컨텍스트를 사용하기 전에 고려할 점
* 컨텍스트는 다른 레벨의 많은 컴포넌트가 특정 데이터를 필요로 하는 경우에 주로 사용합니다.
* 하지만 무조건 컨텍스트를 사용하는 것이 좋은 것은 아닙니다.
* 왜냐하면 컴포넌트와 컨텍스트가 연동되면 재사용성이 떨어지기 때문입니다.
* 따라서 다른 레벨의 많은 컴포넌트가 데이터를 필요로 하는 경우가 아니면 props를 통해 데이터를 전달하는 컴포넌트 합성 방법이 더 적합합니다.
* 하지만 데이터가 많아질수록 상위 컴포넌트가 점점 더 복잡해지기 때문에 모든 상황에서 좋은 것은 아닙니다.
* 이런 경우 하위 컴포넌트를 여러 개의 변수로 나눠서 전달하면 됩니다.
* 하지만 어떤 경우에는 하나의 데이터에 다양한 레벨에 있는 중첩된 컴포넌트들의 접근이 필요할 수 있습니다.
* 이런 경우라면 컨텍스트가 유리합니다.
* 컨텍스트는 해당 데이터와 데이터의 변경사항을 모두 하위 컴포넌트들에게 broadcast 해주기 때문입니다.
* 컨텍스트를 사용하기에 적합한 데이터의 대표적인 예로는 '지역 정보', 'UI 테마' 그리고 '캐싱된 데이터' 등이 있습니다.

### React.createContext
* 컨텍스트를 생성하기 위한 함수입니다.
* 파라메타에는 기본값을 넣어주면 됩니다.
* 하위 컴포넌트는 가장 가까운 상위 레벨의 Provider로부터 컨텍스트를 받게 되지만, 만일 Provider를 찾을 수 없다면 설정한 기본값을 사용하게 됩니다.
```js
const MyContext = React.createContext(기본값);
```

### Context.Provider
* Context.Provider 컴포넌트로 하위 컴포넌트들을 감싸주면 모든 하위 컴포넌트들이 해당 컨텍스트의 데이터에 접근할 수 있게 됩니다.
```js
<MyContext.Provider value={/*some value*/}>
```
* Provider 컴포넌트에는 value 라는 prop이 있고, 이것은 Provider 컴포넌트 하위에 있는 컴포넌트에게 전달됩니다.
* 하위 컴포넌트를 consumer 컴포넌트라고 부릅니다.

### Class.contextType
* Provider 하위에 있는 클래스 컴포넌트에서 컨텍스트의 데이터에 접근하기 위해 사용합니다.
* Class 컴포넌트는 더이상 사용하지 않으므로 참고만 합니다.

### Context.Consumer
* 함수형 컴포넌트에서 Context.Consumer를 사용하여 컨텍스트를 구독할 수 있습니다.
* 데이터를 소비한다는 뜻에서 consumer 컴포넌트라고도 부릅니다.
```js
<MyContext.Consumer>
    {value => /* 컨텍스트의 값에 따라서 컴포넌트들을 렌더링*/}
</MyContext.Consumer>
```
* consumer 컴포넌트는 컨텍스트 값의 변화를 지켜보다가 값이 변경되면 재렌더링됩니다.
* 하나의 Provider 컴포넌트는 여러 개의 consumer 컴포넌트와 연결될 수 있습니다.
* 상위 레벨에 매칭되는 Provider가 없을 경우 기본값이 사용됩니다.

### Context.displayName
* 컨텍스트 객체는 displayName이라는 문자열 속성을 갖습니다.
* 크롬의 리액트 개발자 도구에서는 컨텍스트의 Provider나 Consumer를 표시할 때 displayName을 함께 표시해 줍니다.
```js
const MyContext = React.createContext(/* some value */);
MyContext.displayName = 'MyDisplayName';

// 개발자 도구에 "MyDisplayName.Provider"로 표시됨 */
<MyContext.Provider>

/* 개발자 도구에 "MyDisplayName.Consumer"로 표시됨 */
<MyContext.Consumer>
```

### 여러 개의 컨텍스트 사용하기
* 여러 개의 컨텍스트를 동시에 사용하려면 Context.Provider를 중첩해서 사용합니다.
* 두 개 또는 그 이상의 컨텍스트 값이 자주 함께 사용될 경우 모든 값을 한 번에 제공해 주는 별도의 render prop 컴포넌트를 직접 만드는 것을 고려하는 것이 좋습니다.

### useContext
* 함수형 컴포넌트에서 컨텍스트를 사용하기 위해 컴포넌트를 매번 Consumer 컴포넌트로 감싸주는 것보다 좋은 Hook을 사용하는 방법이 있습니다.
* useContext() 훅은 React.createContext() 함수 호출로 생성된 컨텍스트 객체를 인자로 받아서 현재 컨텍스트의 값을 리턴합니다.
```js
function MyComponent(props) {
    const value = use Context(MyContext);

    return ()
}
```
* 이 방법도 가장 가까운 상위 Provider로부터 컨텍스트의 값을 받아옵니다.
* 만일 값이 변경되면 변경된 값과 함께 useContext() 훅을 사용하는 컴포넌트가 재렌더링됩니다.
* useContext() 훅을 사용할 때에는 파라미터로 컨텍스트 객체를 넣어줘야 합니다.
* Consumer나 Provider를 넣으면 안됨!

## 6월 5일 강의
### Shared State
* Shared state는 state의 공유를 의미합니다.
* 같은 부모 컴포넌트의 state를 자식 컴포넌트가 공유해서 사용하는 것입니다.
* 다음 그림은 부모 컴포넌트가 섭씨 온도의 state를 갖고 있고, 이것을 컴포넌트C와 컴포넌트F가 공유해서 사용하는 것을 보여줍니다.

![alt text](image.png)

### Shared State 적용하기
* 다음은 하위 컴포넌트의 state를 부모 컴포넌트로 올려서 shared state를 적용합니다.
* 이것을 Lifting State Up (State 끌어올리기) 라고 합니다.
* 이를 위해 먼저 TemperatureInput 컴포넌트에서 온도 값을 가져오는 부분을 다음과 같이 수정합니다.
* 이렇게 수정하면 온도를 state에서 가져오지 않고 props를 통해 가져오게 됩니다.
```js
return (
    // 변경 전 : <input value={temperature} onChange={handleChange} />
    <input value={props.temperature} onChange={handleChange} />
)
```
* 또 한 가지 컴포넌트의 state를 사용하지 않기 때문에 입력 값이 변경되었을 때 상위 컴포넌트로 변경된 값을 전달해 주어야 합니다.

### Calculator 컴포넌트 수정하기
* State는 temperature, scale 2개를 사용합니다.
* 이것을 이용해서 변환 함수를 통해 섭씨와 화씨온도를 구해서 사용합니다.
* TemperatureInput 컴포넌트를 사용하는 부분에서는 온도와 단위를 props로 넣어줍니다.

### 합성(Composition)
* 여러 개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것입니다.
* 조합 방법에 따라 합성의 사용 기법은 다음과 같이 나눌 수 있습니다.
#### Containment(담다, 포함하다, 격리하다)
* 특정 컴포넌트가 하위 컴포넌트를 포함하는 형태의 합성 방법입니다.
* 컴포넌트에 따라서는 어떤 자식 엘리먼트가 들어올 지 미리 예상할 수 없는 경우가 있습니다.
* 범용적인 '박스' 역할을 하는 Sidebar 혹은 Dialog와 같은 컴포넌트에서 특히 자주 볼 수 있습니다.
* 이런 컴포넌트에서는 children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달하는 것이 좋습니다.
* 이때 children prop은 컴포넌트의 props에 기본적으로 들어있는 children 속성을 사용합니다.
* 다음과 같이 props.children을 사용하면 해당 컴포넌트의 하위 컴포넌트가 모두 children으로 들어오게 됩니다.
```js
function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
            {props.children}
        </div>
    )
}
```

##### React.createElement()
* jsx를 사용하지 않는 경우의 props 전달 방법입니다
* JSX를 사용하지 않고 리액트로 엘리먼트를 생성하는 방법입니다.

* 리액트에서는 props.children을 통해 하위 컴포넌트를 하나로 모아서 제공해 줍니다.
* 만일 여러 개의 children 집합이 필요한 경우는 별도로 props를 정의해서 각각 원하는 컴포넌트를 넣어줍니다.
* 예와 같이 SplitPane은 화면을 왼쪽과 오른쪽으로 분할해주고, App에서는 SplitPane을 사용해서 left, right 두 개의 props를 정의하고 있습니다.
* 즉, App에서 left, right를 props를 받아서 화면을 분할하게 됩니다. 이처럼 여러 개의 children 집합이 필요한 경우 별도의 props를 정의해서 사용합니다.
```js
function SplitPane(props) {
    return (
        <div className="SplitPane">
            <div className="SplitPane-left">
                {props.left}
            </div>
            <div className="SplitPane-right">
                {props.right}
            </div>
        </div>
    );
}

function App(props) {
    return (
        <SplitPane
            left={
                <Contacts />
            }
            right={
                <Chat />
            }
        />
    );
}
```
#### Specialization(특수화, 전문화)
* 웰컴다이얼로그는 다이얼로그의 특별한 케이스입니다.
* 범용적인 개념을 구별이 되게 구체화하는 것을 특수화라고 합니다.
* 객체지향 언어에서는 상속을 사용하여 특수화를 구현합니다.
* 리액트에서는 합성을 사용하여 특수화를 구현합니다.
* 다음 예와 같이 특수화는 범용적으로 쓸 수 있는 컴포넌트를 만들어 놓고 이를 특수한 목적으로 사용하는 합성 방식입니다.
```js
function Dialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <p className="Dialog-message">
                {props.message}
            </p>
        </FancyBorder>
    );
}

function WelcomeDialog(props) {
    return (
        <Dialog
            title="어서오세요"
            message="우리 사이트에 방문하신 것을 환영합니다!"
        />
    );
}
```

## 5월 29일 강의
### select 태그
* select 태그도 textarea와 동일
```js
<select>
    <option value="apple">사과</option>
    <option value="banana">바나나</option>
    <option selected value="grape">포도</option>
    <option value="watermelon">수박</option>
</select>
```
### File input 태그
* File input 태그는 그 값이 읽기 전용이기 때문에 리액트에서는 비제어 컴포넌트가 됩니다.
```js
<input type="file" />
```
### Input Null Value
* 제어 컴포넌트에 value prop을 정해진 값으로 넣으면 코드를 수정하지 않는 한 입력값을 바꿀 수 없습니다.
* 만약 value prop은 넣되 자유롭게 입력할 수 있게 만들고 싶다면 값이 undefined 또는 null을 넣어주면 됩니다.
```js
ReactDOM.render(<input value="hi" />, rootNode);

setTimeout(function() {
    ReactDOM.render(<input value={null} />, rootNode);
}, 1000);
```


## 5월 22일 강의
### '리스트'와 '키'란 무엇인가?
* 리스트는 자바스크립트의 변수나 객체를 하나의 변수로 묶어 놓은 배열과 같은 것
* 키는 각 객체나 아이템을 구분할 수 있는 고유한 값을 이미
* 리액트에서는 배열과 키를 사용하는 반복되는 다수의 엘리먼트를 쉽게 렌더링할 수 있음

* number 배열에 들어있는 각각의 요소를 map()를 이용해 하나씩 추출해 2를 곱한 후 doubled라는 배열에 다시 넣는 코드
```js
const doubled = numbers.map((number) => number * 2);
```
* map()함수 예제
```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) => 
    <li>{number}</li>
)
```
* numbers의 요소에 2를 곱하는 대신 li 태그를 결합해 리턴

### 리스트의 키에 대해 알아보기
* 리스트에서는 키는 "리스트에서 아이템을 구별하기 위한 고유 문자열
어떤 아이템이 변경, 추가되었는지

## 5월 8일 강의
### Arguments 전달하기
* 함수를 정의할 때는 파라미터(parameter) 혹은 매개변수, 함수를 사용할 때는 아귀먼트(Argument) 혹은 인수라고 부름
* 이벤트 핸들러에 매개변수를 전달해야 하는 경우도 많음
```js
<button onClick={(event) => this.deleteItem(id, event)}>삭제하기</button>
<button onClick={this.deleteItem.bind(this.id)}>삭제하기</button>
```
* 위 코드는 모두 동일한 역할을 하지만 하나는 화살표함수, 하나는 bind를 사용함
* event 매개변수는 리액트의 이벤트 객체를 의미
* 첫 번째 코드는 명시적으로 event를 매개변수로 넣어주었고, 두 번째 코드는 id 이후 두 번째 변수로 event가 자동 전달 됨
* 함수형 컴포넌트에서 이벤트 핸들러에 매개변수를 전달할 땐 다음과 같이 함

### ConfirmButton 컴포넌트 만들기
* 클래스 필드 문법 사용하기
* 함수 컴포넌트로 변경하기
```js
class ConfirmButton extends React.Component{
    constructor(props){
        super(props);

        this.state
    }
}
```

### 조건부 랜더링이란?
* 여기서 조건이란 우리가 알고 있는 조건문이 조건임
* props로 전달 받은 isLoggendln이 true면 을, false면 을 return함
랜더링 해야될 컴포넌트를 변수처럼 사용하는 방법이 앨리먼트 변수
* state에 따라 button 변수에 컴포넌트의 객체를 저장하여 return문에서 사용하고 있음

### 인라인 조건
#### 인라인 if
* if문을 직접 사용하지 않고, 동일한 효과를 내기 위해 && 논리 연산자를 사용
* &&는 and 연산자로 모든 조건이 참일 때만 참이 됨
* 첫 번째 조건이 거짓이면 두 번째 조건은 판단할 필요도 없이 단축평가
* 판단만 하지 않는 것이고 결과값은 그대로 리턴

#### 인라인 if-else
* 삼항 연산자를 사용
* 문자열이나 엘리먼트를 넣어 사용할 수도 있음

### 컴포넌트 렌더링 막기
* 컴포넌트를 렌더링하고 싶지 않을 때에는 null을 리턴


## 5월 1일 강의
### 훅의 규칙
* 첫 번째 규칙은 무조건 최상위 레벨에서만 호출해야 한다는 것
* 따라서 반복문이나 조건문 또는 중첩된 함수들 안에서 훅을 호출하면 안됨
* 이 규칙에 따라서 훅은 컴포넌트가 렌더링 될 때마다 같은 순서로 호출되어야 함
* 두 번째 규칙은 함수형 컴포넌트에서만 훅을 호출해야 한다는 것
* 따라서 일반 자바스크립트 함수에서 훅을 호출하면 안됨
* 혹은 함수형 컴포넌트 혹은 직접 만든 커스텀 훅에서만 호출 가능

### 커스텀 훅
* 직접 훅을 만들어 사용
* 일반 컴포넌트와 마찬가지로 다른 훅을 호출하는 것은 무조건 커스텀 훅의 최상위 레벨에서만 해야 함
* 이름은 use로 시작, 그렇지 않으면 다른 훅을 불러올 수 없음

### 이벤트 처리하기
* DOM에서 클릭 이벤트를 처리하는 예제 코드
```js
<button onclick>="activate()">
    Activate
</button>
```

* React에서 클릭 이벤트 처리하는 예제 코드
```js
<button onClick={activate}>
    Activate
</button>
```
#### 둘의 차이점
* 이벤트 이름이 onclick에서 onClick으로 변경(Camel case)
* 전달하려는 함수는 문자열에서 함수 그대로 전달
* 이벤트가 발생했을 때 해당 이벤트를 처리하는 함수를 "이벤트 핸들러(Event Handler)"
* 또는 이벤트가 발생하는 것을 계속 듣고 있다는 의미로 "이벤트 리스너(Event Listener)"

### 이벤트 핸들러 추가하는 방법
```js
class Toggle extends React.Component {
    constructor(props) {
        super(props);

        this.state = {isToggleOn: true};

        // callback에서 `this`를 사용하기 위해서는 바인딩을 필수적으로 해야 함.
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        this.setState(prevState => ({
            isToggleOn: !prevState.isToggleOn
        }));
    }

    render() {
        return (
            <button onClick={this.handleClick}>
                {this.state.isToggleOn ? '켜짐' : '꺼짐'}
            </button>
        );
    }
}
```

## 4월 17일 강의
### 훅(Hook)
* 클래스형 컴포넌트에서는 생성자에서 state를 정의하고, setState() 함수를 통해 state 업데이트
* 예전에 사용하던 함수형 컴포넌트는 별도로 state를 정의하거나, 컴포넌트의 생명주기에 맞춰서 어떤 코드가 실행되도록 하는 것이 불가
* 함수형 컴포넌트에서도 state나 생명주기 함수의 기능을 사용하게 해주기 위해 추가된 기능이 훅(Hook)
* 함수형 컴포넌트도 훅을 사용하여 클래스형 컴포넌트의 기능을 모두 동일하게 구현할 수 있게 됨
* Hook이란 'state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행되도록 만든 함수'
* 훅의 이름은 모두 'use'로 시작
* 사용자 정의 훅(custom hook)을 만들 수 있으며, 이 경우에 이름은 자유롭게 할 수 있으나 'use'로 시작할 것을 권장

### useState
* 함수형 컴포넌트에서 state를 사용하기 위한 Hook

### useEffect
* useState와 함께 가장 많이 사용하는 Hook
* 사이드 이펙트를 수행하기 위한 것
* 영어로 side effect는 부작용을 의미, 프로그래밍에서는 일반적으로 개발자가 의도하지 않은 코드가 실행되면서 버그가 발생하는 것
* 리액트에서는 효과 또는 영향을 뜻하는 effect의 의미에 가까움
* 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 등의 작업을 의미
* 이펙트라고 부르는 이유는 이 작업들이 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문
* 렌더링이 끝난 이후에 실행되어야 하는 작업들
* 클래스 컴포넌트의 생명주 기 함수와 같은 기능을 하나로 통합한 기능 제공
* 저자는 useEffect가 side effect가 아니라 effect에 가깝다고 설명하고 있지만, 이것은 부작용의 의미를 잘못 해석해서 생긴 오해
부작용의 부를 不로 생각했기 때문
* Side effect는 副作用으로 '원래의 용도 혹은 목적의 효과 외에, 부수적으로 다른 효과가 있는 것'을 의미
* 결국 sideEffect는 렌더링 외에 실행해야 하는 부수적인 코드를 의미
* 네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리(clean-up)가 필요없는 경우들
* 첫 번째 파라미터는 이펙트 함수가 들어가고, 두 번째 파라미터로는 의존성 배열이 들어감
```js
useEffect(이펙트 함수, 의존성 배열);
```
* 의존성 배열은 이펙트가 의존하고 있는 배열로, 배열 안에 있는 변수 중에 하나라도 값이 변경되었을 때 이펙트 함수가 실행
* 이펙트 함수는 처음 컴포넌트가 렌더링 된 이후, 그리고 재 렌더링 이후에 실행
* 만약 이펙트 함수가 마운트와 언마운트 될 때만 한 번씩 실행되게 하고 싶으면 빈 배열을 넣으면 됨
* props나 state에 있는 어떤 값에도 의존하지 않기 때문에 여러 번 실행되지 않음

### useMemo
* useMemo() 훅은 Memoized value를 리턴하는 훅
* 이전 계산값을 갖고 있기 때문에 연산량이 많은 작업의 반복을 피할 수 있음
* 이 훅은 렌더링이 일어나는 동안 실행됨
* 따라서 렌더링이 일어나는 동안 실행되서는 안될 작업을 넣으면 안됨
* 예를 들면 useEffect에서 실행되어야 할 사이드 이펙트 같은 것
* 메모이제이션(memoization)은 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술, 동적 계획법의 핵심이 되는 기술

### useCallback
* useMemo()와 유사한 역할
* 차이점은 값이 아닌 함수를 반환한다는 점
* 의존성 배열을 파라미터로 받는 것은 useMemo와 동일
* 파라미터로 받은 함수를 콜백이라고 부름
* useMemo와 마찬가지로 의존성 배열 중 하나라도 변경되면 콜백함수 반환

### useRef
* 레퍼런스를 사용하기 위한 훅
* 레퍼런스란 특정 컴포넌트에 접근할 수 있는 객체
* 이 레퍼런스 객체를 반환
* 레퍼런스 객체에는 .current라는 속성이 있는데, 이것은 현재 참조하고 있는 엘리먼트를 의미
```js
const refContainer = useRef(초깃값);
```
* 이렇게 반환된 레퍼런스 객체는 컴포넌트의 라이프타임 전체에 걸쳐서 유지
* 컴포넌트가 마운트 해제 전까지는 계속 유지


## 4월 3일 강의
### Props 사용법
* JSX에서는 key-value 쌍으로 props를 구성
* profile 컴포넌트로 전달해서 name, inroduction, viewCount Props를 전달
* 이때 전달되는 props는 자바스크립트
* JSX에서는 중괄호를 사용하면 JS 코드 삽입 가능
* props를 통해서 value를 할당 할 수 있고, 직접 중괄호를 사용하여 할당할 가능

### 컴포넌트의 종류
* 리액트 초기버전을 사용할 때는 클래스형 컴포넌트를 주로 사용
* 이후 K=Hook이라는 개념이 나오면서 최근에는 함수형 컴포넌트를 주로 사용
* 과거에 작성된 코드나 문서들이 클래스형 컴포넌트를 사용하고 있기 때문에, 클래스형 컴포넌트와 컴포넌트의 생명주기에 관해서도 공부

### 컴포넌트 이름짓기
* 이름은 항상 대문자로 시작
* 리액트는 소문자로 시작하는 컴포넌트를 DOM 태그로 인식
* 컴포넌트 파일 이름과 컴포넌트 이름은 동일

### 컴포넌트 합성
* 컴포넌트 합성은 여러개의 컴포넌트를 합쳐서 하나의 컴포넌트를 만드는 것
* 리액트에서는 컴포넌트 안에 또 다른 컴포넌트를 사욜할 수 있기 때문에, 복잡한 화면을 여러개의 컴포넌트로 나누어 구현

### 컴포넌트 추출
* 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 분할 가능
* 큰 컴포넌트에서 일부를 추출해서 새로운 컴포넌트를 만든다
* 실무에서는 처음부터 1개의 컴포넌트에 하나의 기능만 사용하도록 설계

## 3월 27일 강의
### JSX
* 내부적으로 XML/HTML 코드를 자바스크립트로 변환
* React가 createElement함수를 사용하여 자동으로 자바스크립트로 변환
* JS로 작업할 경우 직접 createElement 함수를 사용
* 가독성 향상

### JSX의 장점
* 코드 간결
* 가독성 향상
* Injection Attack이라 불리는 해킹 방법을 방어함으로써 보안이 강함

### JSX 사용법
* 모든 자바스크립트 문법 지원
* 자바스크립트 문법에 XML과 HTML을 섞어서 사용
* HTML이나 XML에 자바스크립트 코드를 사용하소 싶으면 {}괄호 사용
* 태그의 속성값을 넣고 싶을 때는 큰따옴표 사이에 문자열을 넣거나 중괄호 사이에 자바스크리트 코드를 삽입

### 엘리먼트의 정의
* 리액트 앱을 구성하는 요소
* 리액트 앱의 가장 작은 빌딩 블록들
* 웹사이트는 DOM 엘리먼트이며 HTML 요소를 의미

### 리액트 엘리먼트와 DOM 엘리먼트의 차이
* 리액트 엘리먼트는 Virtual DOM의 형태를 취함
* DOM 엘리먼트는 페이지의 모든 정보를 갖고 있어 무거움
* 리액트 엘리먼트는 변화한 부분만 갖고 있어 가벼움

### 엘리먼트의 생김새
* 리액트 엘리먼트는 자바스크립트 객체의 형태로 존재
* 컴포넌트, 속성 및 내부의 모든 children을 포함하는 일반 JS 객체
* 마음대로 변경할 수 없는 불변성

### 엘리먼트의 특징
* 리액트 엘리먼트의 가장 큰 특징은 불변성
* 한 번 생성된 엘리먼트의 children이나 속성을 바꿀 수 없음
* 내용이 바뀌면 컴포넌트를 통해 새로운 엘리먼트 생성
* 그 다음 이전 엘리먼트와 교체를 하는 방법

### Root DOM node
```js
<div id = "root"></div>
```
* div 태그 안에 리액트 엘리먼트가 렌더링되는 것

### 컴포넌트
* 컴포넌트 구조라는 것은 작은 컴포넌트가 모여 큰 컴포넌트를 구성하고, 다시 이런 컴포넌트들이 모여서 전체 페이지를 구성하는 것
* 재사용이 가능하기 때문에 전체 코드의 양을 줄일 수 있어 개발 시간과 유지 보수 비용도 감소
* 자바스크립트 함수와 입력과 출력이 있다는 면에서 유사
* 다만 입력은 Props가 담당하고, 출력은 리액트 엘리먼트의 형태로 출력
* 엘리먼트를 필요한 만큼 만들어 사용한다는 면에서는 객체 지향의 개념과 유사

### Props
* prop(property)의 준말
* 컴포넌트의 속성
* 컴포넌트에 어떤 속성, props를 넣느냐에 따라서 속성이 다른 엘리먼트 출력
* 컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체

### Props의 특징
* 읽기 전용, 변경 불가
* 속성이 다른 엘리먼트를 생성하려면 새로운 props를 컴포넌트에 전달

### Pure 함수 vs. Impure 함수
* Pure 함수는 인수로 받은 정보가 함수 내부에서도 변하지 않는 함수
* Impure 함수는 인수로 받은 정보가 함수 내부에서 변하는 함수

* 모든 리액트 컴포는트는 그들의 props에 관해서는 Pure 함수 같은 역할을 해야 함

## 3월 20일 강의
### React
* 웹 및 앱 유저 인터페이스를 위한 라이브러리

* 다양한 자바스크립트 UI 프레임워크 : Stack Overflow trends

### 리액트 개념 정리
* 복잡한 사이트를 쉽고 빠르게 만들고, 관리하기 위해 만들어진 것
* 다른 표현으로는 SPA를 쉽고 빠르게 만들 수 있도록 해주는 도구

### 리액트의 장점
#### 빠른 업데이트와 렌더링 속도
* DOM(Document Object Model)이란 XML, HTML 문서의 각 항목을 계층으로 표현하여 생성, 변형, 삭제할 수 있도록 돕는 인터페이스, W3C의 표준
* Virtual DOM은 DOM 조작이 비효율적인 이유로 속도가 느리기 때문에 고안된 방법
* DOM은 동기식, Virtual DOM은 비동기식 방법으로 렌더링

#### 컴포넌트 기반 구조
* 리액트의 모든 페이지는 컴포넌트로 구성
* 하나의 컴포넌트는 다른 여러 개의 컴포넌트의 조합으로 구성 가능
* 리액트로 개발을 하다 보면 레고 블록을 조립하는 것처럼 컴포넌트를 조합해서 웹사이트를 개발
* 재사용성이 뛰어남

#### 재사용성
* 반복적인 작업을 줄여줘서 생산성 증대
* 유지보수 용이
* 해당 모듈의 의존성이 없어야 함

#### 든든한 지원군
* 메타에서 오픈소스 프로젝트로 관리하고 있어 계속 발전

#### 활발한 지식 공유 & 커뮤니티

#### 리액트 네이티브라는 모바일 환경 UI 프레임워크를 사용하면 크로스 플랫폼(cross-platform) 모바일 앱 개발 가능

### 리액트의 단점
#### 방대한 학습량
* 자바스크립트를 공부한 경우 빠르게 학습 가능

#### 높은 상태 관리 복잡도
* state, component life cycle 등의 개념이 있지만 그리 어렵지 않음

## 3월 13일 강의
* var : 중복 선언 가능, 재할당 가능
* let : 중복 선언 불가능, 재할당 가능
* const : 중복 선언 불가능, 재할당 불가능
* Array type : 배열

### Object type
* ES6 (ECMAScript6) - 표준 ECMA-262
* Function statement 형 : 일반적 함수의 형태
* Arrow function expression 형 : 화살표 함수