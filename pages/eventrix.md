
# Eventrix

Defining eventix store looks very similar to redux store.


```ts {all|3-8|1|all}
import { EventrixProvider } from "eventrix";
...
  <React.StrictMode>
    <EventrixProvider eventrix={eventrixStore}>
      <App />
    </EventrixProvider>
  </React.StrictMode>
```

---

# Define store

Interface for creating an eventrix store is also similar to creating redux store:

```ts {0|19-21|13-17|12|1-10|all}
import { EventsReceiver } from "eventrix";
import { CartItem, Events, Product } from "./types";

const addToCart = new EventsReceiver(
  Events.ADD_TO_CART,
  (_, eventData, stateManager) => {
    ...
  }
);
...

const receiversList = [addToCart, removeFromCart];

const initialState = {
  cartItems: [],
  products: []
};

const eventrixStore = new Eventrix(initialState, receiversList);

export default eventrixStore;

```


---

# Selectors

To access data though selector we need to useEventrixState hook:

```ts {0|all}
import { useEventrixState } from "eventrix";
...
  const [cartItems] = useEventrixState<CartItem[]>("cartItems");
```

Eventrix gives us access to setter, to modify store value.

```ts {0|all}
  const [cartItems, setCartItems] = useEventrixState<CartItem[]>("cartItems");
```

---

# Updating stores

## Actions 
To trigger an action we need to emit an event of specific name and payload

```ts {0|1-3|5-8|all}
import { useEmit } from "eventrix";

...
  const emit = useEmit();
    
    
  emit(Events.ADD_TO_CART, product);
```

If event receiver is registered then the action will be triggered.
