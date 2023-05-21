
# Redux store optimization
There are multiple ways to optimize redux store:

- creting memoized selectors

```ts
createSelector
```

- using connect

```ts
connect(

)

```

- writing own useSelector comparator functions:

```ts
useSelector(getData, (old, new) => old.field === new.field)
```

- others?