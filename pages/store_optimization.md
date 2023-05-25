
# Redux store optimization
There are multiple ways to optimize redux store

### Creating memoized selectors

```ts {0|1-2|4-8|10|all}
import { useSelector } from 'react-redux'
import { createSelector } from 'reselect'

const selectNumCompletedTodos = createSelector(
  (state) => state.todos,
  (todos) => todos.filter((todo) => todo.completed).length
)

export const CompletedTodosCounter = () => {
  const numCompletedTodos = useSelector(selectNumCompletedTodos)

  // ...
}
```

---

# Redux store optimization
There are multiple ways to optimize redux store

### Using HoC connect
Fun fact: Redux documentation suggests not to use connect!

```ts {0|1-3|5|9-11|all}
type Prop = {
    count: number;
};

const Component = ({count}: Props) => {
   // ...
}
//...
connect((state: RootState) => ({
    count: state.counter.value
}))(Component);


```

---

# Redux store optimization
There are multiple ways to optimize redux store

## Writing own useSelector comparator functions:

```ts
useSelector(getData, (old, new) => old.field === new.field)
```

- others?