# Learnings

## Exercise 11 - "Assigning Dynamic Keys to an Object"

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

## Exercise 14 - Extend Interfaces From Other Interfaces

`extends` allows you to use an interface as a "base" for a new interface and add new properties to it, reducing the amount of typing and improving consistency.

A basic example of using `extends` looks like this:

```
interface Entity {
  id: string;
}

interface User extends Entity {
  firstName: string;
  lastName: string;
}

```

But I learnt that you can actually use `extends` with multiple interfaces like below:

```
interface Entity {
  id: string;
}

interface User {
  firstName: string;
  lastName: string;
}

interface Post extends Entity, User {
  title: string;
  body: string;
}
```

## Exercise 14 - Combining Types to Create New Types

You can use `&` to combine types:

```
interface User {
  id: string;
  firstName: string;
  lastName: string;
}

interface Post {
  id: string;
  title: string;
  body: string;
}

type DefaultUserPostAndPosts = User & Post;
```

More verbose example used in a function return:

```
interface User {
  id: string;
  firstName: string;
  lastName: string;
}

interface Post {
  id: string;
  title: string;
  body: string;
}

type DefaultUserPostAndPosts = User & { posts: Post[]}

export const getDefaultUserAndPosts = (): DefaultUserPostAndPosts => {
  return {
    id: "1",
    firstName: "Matt",
    lastName: "Pocock",
    posts: [
      {
        id: "1",
        title: "How I eat so much cheese",
        body: "It's pretty edam difficult",
      },
    ],
  };
};
```

## Exercise 17 - Typing Functions

I learnt that the reason `void` is used as the return type for most function types is, `void` means that the function does not return anything.

```
const logAndDoNothing = (value: number) => {
  console.log('value is: ', value);
}

const addListener3 = (listenFunction: (value: number) => void) => {
  window.addEventListener("focus", () => {
    listenFunction(3);
  });
}
```

The parameter `listenFunction` is typed as `(value: number) => void)`, which basically saying the function passed in needs to take a value with a type of `number` and should return nothing.

If I used `undefined` as the return type for example, I would need to write the function and the function's definition for addListener like this:

```
const logAndDoNothing = (value: number) => {
  console.log("value is: ", value);
  return undefined;
};

const addListener = (listenFunction: (value: number) => undefined) => {
  window.addEventListener("focus", () => {
    listenFunction(3);
  });
};

addListener(logAndDoNothing);
```

## Typing Async Functions

Async functions return promises and promises are typed like this:

```
type ExamplePromise = Promise<string>;
```

To type a async function, you just combine the normal syntax for functions (see Exercise 17) with a promise:

```
const createThenGetUser = async (
  createUser: () => Promise<string>,
  getUser: (userID: string) => Promise<User>
): Promise<User> => {
  const userId: string = await createUser();

  const user = await getUser(userId);

  return user;
};
```
