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


