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
