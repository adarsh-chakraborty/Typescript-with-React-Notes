## What is TypeScript

TypeScript is so called "Superset" to JavaScript. It's a programming language that builds up on JavaScript.

It basically expands JavaScript, So the core programming language would still be JavaScript. Base Syntax remains the same _e.g Loops, If statements, objects_.

TypeScript adds more features to JavaScript Syntax, It extends the core JavaScript Syntax and most importantly it adds static typing to JavaScript.

JavaScript is dynamically typed programming language.

For example, if we write a function which accepts two parameters, we don't have to define what kind of parameters it has to be, It can accept anything from numbers,strings to objects or even functions.

## Installing TypeScript

**Installing typescript to your project:**

```
npm install typescript
```

As we learned, TypeScript is a superset to JavaScript, It expands the JavaScript, adds more features etc, but the browser no understand typescript, so we have to compile our typescript code to JavaScript.

**Compile:**

```
npx tsc
```

Just running the command just like that, it expects a typescript config file in the project folder, to compile just one file without any config, we have to give the filename to tsc.

```
npx tsc myfileName.ts
```

## Exploring the Base Types

**Primitive Types:** String, Boolean, numbers, null, undefined

**Reference Types:** Arrays, Objects

**Function Types:** parameters

### Declaring variables with Types

**Primitive Types:**

```typescript
let age: number; // age can only have numbers
let userName: string = 'Adarsh Chakraborty';
let isAdmin: boolean;
isAdmin = true; // no problem at all
let hobbies: null;
hobbies = 'something'; // Error cuz it's not null
```

_Please note: We used `number`, `string` all lowercase because writing `Number`, `String` points to JavaScript objects not ts types._

**Reference Types:**

```typescript
let hobbies: string[]; // Array of strings
hobbies = ['Sports', 'Cooking'];

// let person: {};
let person: {
  name: string;
  age: number;
};

person = {
  name: 'Adarsh',
  age: 23
}; // no problem

// Array of Object

// let people:{}[]
let peoples: {
  name: string;
  age: number;
}[];
```

### Understanding Type Inference

By Default, TypeScript tries to Infer as many types as possible, It tries to know which types are used and where.

If I write this line,

```typescript
let course = 'How to train your dog';
course = 123; // error cuz of Type Inference
```

Typescript, detect it's a string and set the type as string without as explicitly telling it to.

So it detects the value type and Infer it as variable type when creating a variable and assinging a value at same line.

Conclusion, It's redundant to explicitly declare types when assinging values, typescript is capable to do it itself.

### Using Union Types

Till now,every variable just had one type, Often that's all we need but sometimes we might want to set a variable to have more than one types.

For example, a variable should contain a string or number.

To add more than one type to a variable

```typescript
let course: string | number = 'How to train your dog';
```

We use pipe symbol to allow multiple types for the variable, and here it's not redundant to set a type explicitly.

### Type Aliases

As our project grows, we type more and more typescript, at some point we might be repeating some type definition.

For example,

we have a type object person with name,age.
and we want to have an array of object type person, we don't have to dublicate the definition of person again, Instead we can create an Alias.

We can define our own base types, in which more complex type definition is stored.

Defining Alias:

```typescript
// type anyNameOfMyChoice = {myTypeDefination}

type Person = {
  name: string;
  age: number;
};

let person: Person; // Person is my custom type
let persons: Person[]; // Array of Person
```

### Functions & Types

When the define a function, typescript can infer the return type and can detect what type of value will be returned from a function.

```typescript
function add(a: number, b: number) {
  return a + b;
}

function addd(a: number, b: number): number | string {
  // only do this if u got a good reason
}
```

Return type of `add()` function is set to `number`, automatically. (cuz of Inference), we can also set it explicitly like in fn `addd()`.

So typescript, doesn't only set types for parameters of functions but also return types.

There is a special return type - `void`

If a function doesn't return anything, the return type is void. It's comparable to null or undefined but it's only used with return value of functions.

## Generics

Let's say we have a function here, which accepts two parameters which accepts any type of value.

What this function does is it accepts an array, inserts a value at beginning at returns it.

```typescript
const demoArray = [1,2,3,4];

const updatedArray = insertAtBeginning(demoArray, -1);

function insertAtBeginning(array: any[], value: any){
    const newarray = [value, ...array];
    return newarray;
}
```

So when we do things like that, due to any type, the function return value is infered as any, and that kind of destroys the typescript support,then when we try to perform a method on any element of updatedArray, we won't get any error like, if the array contains numbers and we try to perform `.split()` operation on it, typescript won't detect it and later we would face runtime error. **Pathetic**

For this scenario, We have this feature known as `generics`.

With Generics, We convert the function to a Generic Function with a *special syntax*.

We add \<angle brackets\> after the *function name* infront of parameter list,inside angle brackets we set an `identifer` of some choice.  

Usually, It's `T` for type and then we put the same for parameters type.

Now typescript will actually look at the concrete value types provided in the function and set the type of returned value automatically depending on the parameters.

So if we an array of numbers is returned, typescript would know when we try to perform string operations on it and It's just one example

```typescript
function insertAtBeginning<T>(array: T[], value: T){
    const newarray = [value, ...array];
    return newarray;
}

const newArray = insertAtBeginning([1,2,3,4], 4);
newarray[4].split(''); // error cuz newArray contains numbers

const newarray2 = insertAtBeginning(['a','b','c'], 'd');
newarray2.split(''); // no array cuz same type
```

So with Generics function, we are giving some extra information to typescript like -

- functions returns an array of `some` type.
- the first parameter accepts an array of `some` type.
- the second parameter accepts an value of again, `some` type.
- So, the typescript would look into that `some` type and set it dynamically.
- Flexibility and type-safety ez.

---

## Another Example of generics

```typescript
let numbers = [1, 2, 3];
```

Here, the type is inferred, but if we would assign it explicitly, we could do it like this:

```typescript
let numbers: number[] = [1, 2, 3];
```
`number[]` is the TypeScript notation for saying `"this is an array of numbers"`.

But actually, `number[]` is just **syntactic sugar!**

The actual type is `Array`. ALL arrays are of the `Array` type.

BUT: Since an array type really only makes sense if we also describe the type of items in the array, Array actually is a generic type.

You could also write the above example liks this:

```typescript
let numbers: Array<number> = [1, 2, 3];
```

Here we have the angle brackets (<>) again! But this time NOT to create our own type (as we did it above) but instead to tell TypeScript which actual type should be used for the "generic type placeholder" (T in the previous example).

As we know, TypeScript would be able to infer this as well - we rely on that when we just write:

```typescript
let numbers = [1, 2, 3];
```

But if we want to explicitly set a type, we could do it like this:

```typescript
let numbers: Array<number> = [1, 2, 3];
```

Of course it can be a bit annoying to write this rather long and clunky type, that's why we have this alternative (syntactic sugar) for arrays:

```typescript
let numbers: number[] = [1, 2, 3];
```

If we take the example from above example, we could've also set the concrete type for our placeholder `T` explicitly:

```typescript
const stringArray = insertAtBeginning<string>(['a', 'b', 'c'], 'd');
```

So we can not just use the angle brackets to define a generic type but also to USE a generic type and explicitly set the placeholder type that should be used - sometimes this is required if TypeScript is not able to infer the (correct) type.

---

# Creating a new project with TypeScript support

**C-R-A** supports creating project that is configured for TypeScript, Out of the Box... Isn't that great.

We can also add TypeScript to existing project, but let's see how to create one...

```
npx create-react-app my-app --template typescript
```

If we run the above command, It will create a new react project but this time, with TypeScript support out of the box!

### Analyzing the Created Project

- We can find files with `.tsx` extensions, we should use this extensions if we use JSX code in the file.
- We can use `.ts` extension if we don't use JSX code in the file.
- Now, starting server for development or building for production, It compiles the `ts` code to `js` behind the scenes.
- We can find some couple of extra dependencies, including `typescript` and some `@types` dependencies.
- These `@types` packages acts as translation bridges between vanilla javascript and typescript.

### Working with Components and props

Working with the Components is almost same as we learned it, but working with props is a little different.

```typescript
const Todos: React.FC<{ items: string[] }> = (props) => {
  return <ul>{props}</ul>;
};

export default Todos;

```

We defined Todos Component of type `React.FC` which is a predefined type for `Functional Components`. Doing this, we're telling typescript that this function is a functional Component which will have props and special props like children.  

Now to have some props of our own, we have to tell typescript that this prop is going to have them.  

We can pass a object to `React.FC` in \<angular brackets\>, to define our prop types. In this object we define all props we're going to have which then merges with the base prop types, so we will have all typescript support including the base props (children) as well as our custom props. (items)

*We're using a generic type here, Before we defined one instead.*


### Adding a Data Model

Create a folder named `Models` and a `.ts` file for defining the model.

We can create a data model with `type`, `interface` or `class`.

Let's create a class for `Todos`

```typescript
// Todo.ts
class Todo {
  id: string;
  text: string;

  constructor(todoText: string) {
    this.text = todoText;
    this.id = new Date().toISOString();
  }
}

export default Todo;
```

Here, we had to declare all the properties we're going to use along with it's type in the class.

Now, import the class in app and create objects from it.

```typescript
// App.tsx
import Todos from './components/Todos';
import Todo from './models/Todo';

function App() {
  const todos = [new Todo('Learn React'), new Todo('Learn Typescript')];

  return (
    <div>
      <Todos items={todos} />
    </div>
  );
}

export default App;
```

Now, as we're sending data of type todo from here, we have to do some changes in the Component as well.

Import the class, set the props type to that class, so we're telling we will receive array of todos here.
```typescript
// Todos.tsx
import Todo from '../models/Todo';

const Todos: React.FC<{ items: Todo[] }> = (props) => {
  return (
    <ul>{[props.items.map((item) => <li key={item.id}>{item.text}</li>)]}</ul>
  );
};

export default Todos;
```

*li item can be outsourced as a separate Component*.

### Form Submissions with TypeScript 

```typescript
const NewTodo = () => {
  const formSubmitHandler = (e: React.FormEvent) => {
    e.preventDefault();
  };

  return (
    <form onSubmit={formSubmitHandler}>
      <label htmlFor="text">Todo Text</label>
      <input type="text" id="text" />
      <button>Add Todo</button>
    </form>
  );
};

export default NewTodo;
```

Here we had to define the type of Event object as `React.FormEvent` because react is expecting a function to handle onSubmit with such type.

If it were a Click Event, It would've `React.MouseEvent`, any mismatch in Event types, TypeScript would let us know about it.  


### Using Refs and useRef

Using refs with Typescript is simple..

import the useRef hook
```typescript
import React, { useRef } from 'react';
```

In a component, Initialize the hook.
```typescript
const textInputRef = useRef<HTMLInputElement>(null);
```

Here, we have to do 2 extra step.

- Give Generic type to useRef hook to explicitly tell which kind of Html it will connect to.
- Pass a Initial value to the ref. `null`

That's all, we can now connect it to a html element as we would normally do.

```typescript
<input type="text" id="text" ref={textInputRef} />
```

`HTMLInputElement` type is nothing typescript specific, It's a general type used in HTML, there are more `HTMLParagraphElement`, `HtmlButtonElement` and more.


**Extracting values from Refs:**

```typescript
const enteredText = textInputRef.current?.value;
```

Here, IDE added a **?** before accessing `.value` property on current Ref, because TypeScript doesn't deeply analyze our code and tell at this point Ref is not `null`.

Hence, `enteredText` is of type `string` || *OR* `undefined`.

To tell typescript, that at this point we won't have a possible null value we use exclaimation mark **!** instead of question mark **?**

 ```typescript
const enteredText = textInputRef.current!.value;
```

So the type of `enteredText` is set to `string` only.

Because, this code written in a function that can only be called after form is populated, and if it's populated refs are already connected, so it's okay here to be explicit about it.


### Working with Function props

Setting the type as Functional Component.
```typescript
const NewTodo: React.FC = () => {}
```

Setting the props type.
Here, we know `onAddTodo` is a reference to a function, We can set Type to a function by defining an arrow function `() => {}`

So, setting the object in generic type as `React.FC<{ onAddTodo: () => {} }>`

But, here, it needs arguements too, so `(text) => {}` and it doesn't return anything so `(text) => void`.

We're not done here yet, We have to set type of the parameter as well, so `(text: string) => void`

*Just like defining properties inside `{}` doesn't create a new object, defining type like this doesn't create a new arrow function.*

```typescript
const NewTodo: React.FC<{onAddTodo: (text: string) => void}> = (props) => {}
```

### Managing State & TypeScript 

Okay, Let's look at a funny thing.. If we setup a state like we usually do with an Initial value of empty array.

```typescript
const [todos, setTodos] = useState([]);
```

The type of `todos[]` is set to `never`, which means it should always be an empty arrow and no value should be assigned to it ever, and that's ofcourse not what we want.

But typescript, infers this type because it cannot tell which type of array it should, to make it clear we again need to tell TypeScript somehow...

`useState` is out of the box, is a generic Function for this reason, we're able to tell it the kind of data managed by `useState` in \<angular brackets\>

```typescript
// This state will manage an array of todos
const [todos,setTodos] = useState<Todo[]>([]);

// This state will manage an array of string
const [names,setNames] = useState<string[]>([]);

// This state will manage an array of numbers
const [names,setNames] = useState<number[]>([]);

// The Initial value is an Empty array for all.
```

Now we can manage a state of todos, and update the todos when required.

```typescript
const [todos, setTodos] = useState<Todo[]>([]);

  const addTodoHandler = (todoText: string) => {
    const newTodo = new Todo(todoText);

    setTodos((prevTodos) => {
      // concat creates a new array
      return prevTodos.concat(newTodo);
    });
};
```
### Removing Todo Items

We can either drill the ID to last component that uses it to invoke the function that removes the todo **OR...**

We can bind the value to the function, so we can pre-configure it what arguements to pass when it's executed.

**Bind() is a method which enables us to pre-configure a function for future executions**

```typescript
onRemoveTodo={props.onRemoveTodo.bind(null, props.id)}
```

- First arugement sets the `this` context of Fn, we set it to null.
- Second arguement is the first arguement which is passed to the function when it's invoked.

Or If you're using prop drilling, we have to use functional approach

`onClick={() => {// do something}}`

because `onClick={pointer}` would throw TypeScript error cuz `onClick` is a `React.MouseEvent` Type function.



