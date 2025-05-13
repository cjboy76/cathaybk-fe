# 鄧詠澤

1-Q1.

```javascript
function sortUserName(users) {
    const temp = structuredClone(users)
    temp.sort((a, b) => {
        const keyA = a.firstName + a.lastName + a.customerID;
        const keyB = b.firstName + b.lastName + b.customerID;
        return keyA.localeCompare(keyB);
    });
    console.log(temp)
}
```

1-Q2.

```javascript
const map = {
    systemAnalytics: 1,
    engineer: 2,
    productOwner: 3,
    freelancer: 4, 
    student: 5
}

function sortByType(user) {
    const temp = structuredClone(user)
    temp.sort((a,b) => {
        return map[a.profession] - map[b.profession]
    })
    console.log(temp)
}

```

2-Q1.
Because of CSS specificity, you can apply the `li` element selector to fix it, like:

```css
.container .shop-list li.item {
    color: green;
}
.container .shop-list li.item { // apply here
    color: blue;
}
```

2-Q2.

```css
.shop-list li:nth-child(odd) + li {
  background-color: #f0f0f0;
}
```

3.

```javascript
function getUniqueNumber (items) {
    const map = {}
    items.forEach(item => {
        map[item] = (map[item] || 0) + 1
    })
    
    Object.entries(map, ([key, value]) => {
        if (value === 1) console.log(key)
    })
}
```

4. The virtual DOM is a set of JavaScript objects like real DOM elements. Modern frameworks use it to efficiently render updated views by comparing changes and updating only the necessary parts of the real DOM.

5. void: function returns nothing, never: infinite loop or throw error, the function never return.

6. The difference is that with none frameworks, developers need to manipulate the DOM manually. With frameworks, developers are able to focus on business logic, let the frameowkrs do the 'updating view' jobs.


7. 
```jsx
import { useState } from 'react';

export default function TaskManager() {
    const [isPersonAlice, setIsPersonAlice] = useState(true);
    const name = isPersonAlice ? 'Alice' : 'Bob'
    return (
        <div>
            <TaskCounter name={name} key={name} />
            <button onClick={() => {
                    setIsPersonAlice(!isPersonAlice);
                }}>
            Switch Person
            </button>
        </div>
    );
}
function TaskCounter({ name }) {
    const [tasks, setTasks] = useState(0);
    return (
    <>
        <h1>{name}'s tasks: {tasks}</h1>
        <button onClick={() => setTasks(tasks + 1)}>
            Complete Task
        </button>
    </>
    );
}
```

8.

Before:

```jsx
const TodoList = () => {
    const [todos, setTodos] = useState([
        { id: 1, text: '學習 React', completed: false, studyPoint: 3 },
        { id: 2, text: '建立專案', completed: false, studyPoint: 1 }
    ]);
    const { id, text, studyPoint } = todos; // 解構錯誤，類型應該是陣列
    const [basePoints, setbasePoints] = useState(3);
    const [sumPoints, setSumPoints] = useState(0);
    const toggleTodo = (id) => {
        const todo = todos.find(t => t.id === id);
        todo.completed = !todo.completed; // 操作到原陣列, 影響 React 無法偵測到資料更新 
        setTodos(todos);
    };
    const handleStudyPointsChange = (e) => {
        basePoints(e.target.value); // 應該要呼叫 setbasePoints
        setSumPoints(parseInt(value1) + parseInt(e.target.value)); // 不確定 value1 為何，假設與前值加總的話可以改用 updater syntax
    };
    return (
        <div>
            <p>課程名稱: {text}</p>
            <label>學習點數: </label>
            <input type="number" value={studyPoint} onChange={handleStudyPointsChange} />
            <p>總累積點數: {sumPoints}</p>
            <button onClick={toggleTodo(id)}>篩選課程</button>
        </div>
    );
}
```

After:
```jsx
const TodoList = () => {
    const [todos, setTodos] = useState([
        { id: 1, text: '學習 React', completed: false, studyPoint: 3 },
        { id: 2, text: '建立專案', completed: false, studyPoint: 1 }
    ]);
    const { id, text, studyPoint } = todos[0];
    const [basePoints, setbasePoints] = useState(3);
    const [sumPoints, setSumPoints] = useState(0);
    const toggleTodo = (id) => {
        const newTodos = todos.map(item => {
            if (item.id === id) {
                return {
                    ...item,
                    completed: !item.completed
                }
            }
            return item
        })
        setTodos(newTodos);
    };
    const handleStudyPointsChange = (e) => {
        setbasePoints(e.target.value);
        setSumPoints(prev => prev + parseInt(e.target.value));
    };
    return (
        <div>
            <p>課程名稱: {text}</p>
            <label>學習點數: </label>
            <input type="number" value={studyPoint} onChange={handleStudyPointsChange} />
            <p>總累積點數: {sumPoints}</p>
            <button onClick={toggleTodo(id)}>篩選課程</button>
        </div>
    );
}
```

9.
```jsx
function ParentComponent() {
    const [name, setName] = useState("Naro");
    const [age, setAge] = useState(12);
    return (
        <div>
            <ChildComponent name={name} age={age} />
            <GrandchildComponent name={name} age={age} />
        </div>
    );
}
const ChildComponent = React.memo(({ name, age }) => {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <GrandchildComponent name={name} age={age} />
    </div>
  );
});
const GrandchildComponent = React.memo(({ name, age }) => {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
    </div>
  );
});
```

10.

```jsx
function SearchButton({ onClick }) {
  return <button onClick={onClick}>Search</button>;
}

function SearchInput({ inputRef }) {
  return <input ref={inputRef} />;
}

export default function Page() {
  const inputRef = useRef(null);

  const handleSearchClick = () => {
    if (inputRef.current) {
      inputRef.current.focus();
    }
  };

  return (
    <>
      <nav>
        <SearchButton onClick={handleSearchClick} />
      </nav>
      <SearchInput inputRef={inputRef} />
    </>
  );
}
```