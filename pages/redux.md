
# Redux

Redux follows the principles of a unidirectional data flow architecture. It helps manage the state of an application in a single immutable object.


```ts {all|3-8|1|all}
import { Provider } from "react-redux";
...
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
```

---

# Define store

The store is a central repository that holds the application state. It is created using the createStore function provided by Redux. The store dispatches actions to the reducers, which in turn produce a new state.

```ts {0|4-8|1|2|10-16|all}
import { configureStore, ThunkAction, Action } from "@reduxjs/toolkit";
import cartReducer from "./cart/reducer";

export const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;
export type AppThunk<ReturnType = void> = ThunkAction<
  ReturnType,
  RootState,
  unknown,
  Action<string>
>;

```


---

# Selectors

Selectors are functions that allow you to extract specific pieces of state from the store. They provide a convenient way to access the data stored in the state tree without worrying about its structure.

```ts {all|2|2-4|all}

export const getCartItems = (state: RootState) => {
  return state.cart.cartItems;
};

export const getProducts = (state: RootState) => {
  return state.cart.products;
};

```

To access data though selector we need to useSelector hook:

```ts {0|all}
import { useSelector } from "react-redux";
...
  const cartItems = useSelector(getCartItems);
```

---

# Updating stores

## Actions 
Actions are plain JavaScript objects that represent events or updates in your application. They are dispatched to the Redux store to trigger state changes. An action typically has a type property that describes the action's purpose and may include additional payload data.

```ts {0|1-3|5-8|all}

export const ADD_TO_CART = "ADD_TO_CART";
export const REMOVE_FROM_CART = "REMOVE_FROM_CART";

export const addToCart = (product: Product) => ({
  type: ADD_TO_CART,
  payload: product,
});

export const removeFromCart = (product: Product) => ({
  type: REMOVE_FROM_CART,
  payload: product,
});
```

--- 

# Updating stores

## Reducers
Reducers specify how the application's state changes in response to actions. They are pure functions that take the current state and an action as input and return a new state object. Reducers should not mutate the existing state but rather create a new state object with the desired changes.

```ts {0|1-3|5-9|all}
interface CartState {
  cartItems: CartItem[];
  products: Product[];
}

const initialState: CartState = {
  cartItems: [],
  products: []
};
```

---

# Updating stores

```ts {0|1|2-4|6-8|13-17|all}
const cartReducer = (state = initialState, action: any) => {
  switch (action.type) {
    case ADD_TO_CART:
        ...
    case REMOVE_FROM_CART:
        ...
    default:
      return state;
  }  
}

...
export const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

```

---

# Updating store - triggering action
The typical flow in Redux involves dispatching actions from components, which are then passed through middleware (Redux Thunk in examples) before reaching the reducers.

```ts {0|1-3|all}
  const handleAddToCart = (product: Product) => {
    dispatch(addToCart(product));
  };

  const handleRemoveFromCart = (product: Product) => {
    dispatch(removeFromCart(product));
  };
```

