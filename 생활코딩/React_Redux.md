
- [**Redux**](#redux)
- [**개념도**](#개념도)
- [**State 와 Render의 관계**](#state-와-render의-관계)
- [Redux를 사용 하는 이유](#redux를-사용-하는-이유)
  - [Redux가 없는 코드 예제](#redux가-없는-코드-예제)
  - [Redux를 사용한 코드 예제](#redux를-사용한-코드-예제)

## **Redux**
- 하나의 State(상태)를 가진다.
- App 의 복작성을 낮춘다.

## **개념도**
<img width="100%" alt="redux" src="https://user-images.githubusercontent.com/90334030/132699645-bae5dcd0-065c-4b15-b388-634bebaa7373.png">


## **State 와 Render의 관계**
- `getState` : 
  - State에 접근 할 수 있다.
- `subscribe` : 
  - State 가 변경 되었을 경우, render를 호출 할 수 있도록 등록 해야 하는데 subscribe를 통해서 함
- `dispatch` : 
  - 현재 state와 action을 reducer에게 전달 한다.
  - subscribe에 등록된 모든 render를 호출해 준다.
- `reducer` : 
  - 새로운 state값을 생성해서 state에 전달 한다.
- `action`
  - type:'create' 인 객체
- `render` :
  - 화면을 그려준다

## Redux를 사용 하는 이유
- Loose coupling을 유지 할 수 있도록 함
- Subscribe(구독)과 State(상태)를 통해서 간결하게 로직을 유지 할 수 있다.
- state history가 저장 되므로 app의 이전 상태로 돌아 갈 수 있다.
- 중앙 집중형의 Data Store를 통해서 관리 한다.

### Redux가 없는 코드 예제

> red() 안에서 Green , Blue 의 Component를 직접 점근해서 값을 변경하고 있다.  
> 강력한 결합으로 인해서 코드의 유연성을 낮춘다.

```JSX
<html>
    <body>
        <style>
            .container{
                border:5px solid black;
                padding:10px;
            }
        </style>
        <div id="red"></div>
        <div id="green"></div>
        <div id="blue"></div>
        
        <script>
function red(){
    document.querySelector('#red').innerHTML = `
        <div class="container" id="component_red">
            <h1>red</h1>
            <input type="button" value="fire" onclick="
            document.querySelector('#component_red').style.backgroundColor = 'red';
            document.querySelector('#component_green').style.backgroundColor = 'red';
            document.querySelector('#component_blue').style.backgroundColor = 'green';
            ">
        </div>
    `;
}
red();

//Green Blue 는 생략...

        </script>
    </body>
</html>
```

### Redux를 사용한 코드 예제

> red() function은 green, blue를 알지 못한다.  
> loose coupling 으로 인해서 코드가 유연해 졌다.  
> store.dispatch({type:'CHANGE_COLOR', color:'red'})를 통해서 reducer를 호출 함  
> reducer() function에서 state값을 변경한다.  

```JSX
<!DOCTYPE html>
<html>
                //Redux를 사용하기 위한 Script 선언
<head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.1/redux.js"></script>
</head>

<body>
    <style>
        .container {
            border: 5px solid black;
            padding: 10px;
        }
    </style>
    <div id="red"></div>
    <div id="blue"></div>
    <div id="green"></div>
    <script>
function reducer(state, action){
    console.log(state, action);
    if(state === undefined){
        return {color:'yellow'}
    }
    var newState;
    if(action.type === 'CHANGE_COLOR'){
        newState = Object.assign({}, state, {color:action.color});
    }
    return newState;
}
var store = Redux.createStore(reducer);
function red() {
    var state = store.getState();
    document.querySelector('#red').innerHTML = `
        <div class="container" id="component_red" style="background-color:${state.color}">
            <h1>red</h1>
            <input type="button" value="fire" onclick="
                store.dispatch({type:'CHANGE_COLOR', color:'red'});
            ">
        </div>
    `;
}
store.subscribe(red);
red();

//Green , Blue 생략...

    </script>
</body>
</html>
```


