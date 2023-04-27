### [과제] 숙련주차 과제 답

##### 답안 제출 내용 : 어떤부분이 문제였고 어떻게 해결했는지에 대해 소스코드를 포함해서 자세한 설명을 적어주세요. (메모장에 적어두고 복사-붙여넣기 하시면 편해요!)

- 추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음.

```javascript
// 기존 코드 : form이 submit되면 setTodo에서 todo가 초기화 되도록 되어있다.
setTodo({
  id: 0,
  title: "",
  body: "",
  isDone: false,
});

// 해결 코드 : todo를 초기화하는 코드를 지우고, dispatch를 이용해 todos모듈의 creator함수인 addTodo에 todo를 보내주어
// todos 전역상태에 추가될 수 있도록 하였다.
dispatch(addTodo(todo));
```

답안 : 기존 코드에서는 form이 submit되면 setTodo에서 todo가 초기화 되도록 되어있기 때문에
todo를 초기화하는 코드를 지우고, dispatch를 이용해 todos모듈의 creator함수인 addTodo에 todo를 보내주어 todos 전역상태에 추가될 수 있도록 하였다.

<br>
- 추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.

```javascript
// 기존 코드 : todos를 추가하는 ADD_TODO의 return문에서 state를 복사한 후, todos자체를 배열 안에서 전달받은 객체로 변경 하고 있다.
const todos = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...action.payload],
      };

// 해결 코드 : 기존의 todos state에 존재하는 객체들을 전부 복사해 유지한 후, 추가해야할 객체를 추가해 주었다.
const todos = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload],
      };
```

답안 : 기존 코드에서는 todos를 추가하는 ADD_TODO의 return문에서 기존 todos state를 복사한 후, todos키의 배열 안의 값 자체를 전달받은 객체로 변경 하고 있다. 따라서 기존의 todos state에 존재하던 객체 데이터들을 전부 복사해 유지한 후, 추가해야할 객체를 전달받아 추가해 주도록 수정하였다.

<br>
- 삭제 기능이 동작하지 않음.

```javascript
// 기존 코드 첫 번째 문제 : todos.js모듈의 switch문에서 case DELETE_TODO문이 존재하지 않아 state가 업데이트되지 않는다.
// 해결 코드 : switch문에서 case DELETE_TODO: 로직을 추가해 주어 state를 업데이트 되도록 하였다.
case DELETE_TODO:
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.payload),
      };

// 기존 코드 두 번째 문제 : todo추가 후 todo state를 초기화 해주지 않아 그대로 다음 입력값을 입력하면 id값을 그대로 복사하게 되어 다음 todo의 id값이 모두 같아져 삭제 시 id값이 같은 todo가 전부 삭제되는 등 삭제가 정상적으로 처리되지 않는다.

  const onChangeHandler = (event) => {
    const { name, value } = event.target;
    setTodo({ ...todo, [name]: value });
  };

  const onSubmitHandler = (event) => {
    event.preventDefault();
    if (todo.title.trim() === "" || todo.body.trim() === "") return;
    dispatch(addTodo(todo));
  }


// 해결 코드 : initialTodo를 만들고, onSubmitHandler에서 todo추가 후에는 setTodo를 initialTodo로 초기화 해주어 id값이 변경되게 하였다.
const initialTodo = {
    id,
    title: "",
    body: "",
    isDone: false,
  };

  const [todo, setTodo] = useState(initialTodo);

  const onChangeHandler = (event) => {
    const { name, value } = event.target;
    setTodo({ ...todo, [name]: value });
  };

  const onSubmitHandler = (event) => {
    event.preventDefault();
    if (todo.title.trim() === "" || todo.body.trim() === "") return;
    dispatch(addTodo(todo));
    setTodo(initialTodo); //todo state 초기화
  };
```

답안 :
첫 번째 문제 : todos.js모듈의 switch문에서 case DELETE_TODO문이 존재하지 않아 state가 업데이트되지 않는다. => switch문에서 case DELETE_TODO: 로직을 추가해 주어 state를 업데이트 되도록 하였다.

두 번째 문제 : todo추가 후 todo state를 초기화 해주지 않아 그대로 다음 입력값을 입력하면 id값을 그대로 복사하게 되어 다음 todo의 id값이 모두 같아져 삭제 시 id값이 같은 todo가 전부 삭제되는 등 삭제가 정상적으로 처리되지 않는다. => initialTodo를 만들고, onSubmitHandler에서 todo추가 후에는 setTodo를 initialTodo로 초기화 해주어 id값이 변경되게 하였다.

<br>
- 상세 페이지에 진입 하였을 때 데이터가 업데이트 되지 않음.

```javascript
// 기존 코드 : useParams의 상세페이지 id를 dispatch를 통해 getTodoByID 함수로 전달 해주어야 todo의 state가 업데이트 되는데, id를 보내주고 있지 않았다.
const dispatch = useDispatch();
const todo = useSelector((state) => state.todos.todo);

const { id } = useParams();
const navigate = useNavigate();

// 해결 코드 : dispatch를 통해 getTodoByID에 id값을 전달하여 todo state가 업데이트 되도록 하였다.
useEffect(() => {
  dispatch(getTodoByID(id));
}, [id, dispatch]);
```

답안 : useParams의 상세페이지 id를 dispatch를 통해 getTodoByID 함수로 전달 해주어야 todo의 state가 업데이트 되는데, id를 보내주고 있지 않았다. 따라서 dispatch를 통해 getTodoByID에 id값을 전달하여 todo state가 업데이트 되도록 하였다.

<br>
- 완료된 카드의 상세 페이지에 진입 하였을 때 올바른 데이터를 불러오지 못함.

```javascript
// 기존 코드 : done list의 상세페이지 경로의 params값이 index로 되어있어 todo의 id와 일치하지 않아 todo를 반환하지 못해 상세페이지를 가져오지 못하는 문제 발생
<StLink to={`/${index}`} key={todo.id}>
                  <div>상세보기</div>
                </StLink>

// 해결 코드 : index를 todo.id로 수정하여 해결
<StLink to={`/${todo.id}`} key={todo.id}>
                  <div>상세보기</div>
                </StLink>
```

답안 : done list의 상세페이지 경로의 params값이 index로 되어있어 todo의 id와 일치하지 않아 todo를 반환하지 못해 상세페이지를 가져오지 못하는 문제 발생 => index를 todo.id로 수정하여 해결

<br>
- 취소 버튼 클릭시 기능이 작동하지 않음.

```javascript
// 기존 코드 : done list에서 토글 버튼 클릭 시 onToggleStatusTodo에 id값을 전달해야 하는데 그냥 실행만 하고 있기 때문에 id값을 전달하지 않아 state가 변경되지 않음
<StButton borderColor="green" onClick={onToggleStatusTodo}></StButton>

// 해결 코드 : 콜백으로 실행하여 id값을 전달 onToggleStatusTodo(todo.id)
<StButton
                    borderColor="green"
                    onClick={() => onToggleStatusTodo(todo.id)}
                  >
```

답안 : done list에서 토글 버튼 클릭 시 onToggleStatusTodo에 id값을 전달해야 하는데 그냥 실행만 하고 있기 때문에 id값을 전달하지 않아 state가 변경되지 않음 => 콜백으로 실행하여 id값을 전달 onToggleStatusTodo(todo.id)
<br>
