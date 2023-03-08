# Learnings

## Exercise 11 "Assigning Dynamic Keys to an Object"

If we have an object (`exampleCache`) returned from a function like below, that can have any key assigned to it.

```
const createCache = () => {
  const cache = {};

  const add = (id: string, value: string) => {
    cache[id] = value;
  };

  const remove = (id: string) => {
    delete cache[id];
  };

  return {
    cache,
    add,
    remove,
  };
};

const exampleCache  = createCache().cache;
```
We can type it how I first did:

```
const cache: {[id: string]: string} = {};
```

or we can use `Record<>` which looks a bit cleaner, and has a more informative tooltip explaining what `Record<>` does:

```
const cache: Record<string, string> = {};
```



