---
theme: wordman
class: text-center
highlighter: shiki
lineNumbers: false
info: |

drawings:
  presenterOnly: true
defaults:
  layout: default
title: Pragmatic Typings - Patterns to Reduce Friction and Spark Joy
monaco: false
layout: image
mdc: true
transition: slide-left
favicon: ./favicon.png
image: /title.png
---

<div style="position: absolute; margin: auto; bottom: 80px; left: 190px;">
    <h1 style="font-size: 3.2rem;color: #e42d30;"> Pragmatic Typings</h1>
    <h2>Patterns to Reduce Friction and Spark Joy</h2>
</div>


---
---

````md magic-move
```ts
interface Person {
    name: string;
    age: number;
}
```
 
```ts
type GetState<ThunkApiConfig> = ThunkApiConfig extends {
  state: infer State
}
  ? State
  : unknown
  
type GetExtra<ThunkApiConfig> = ThunkApiConfig extends { extra: infer Extra }
  ? Extra
  : unknown
  
type GetDispatch<ThunkApiConfig> = ThunkApiConfig extends {
  dispatch: infer Dispatch
}
  ? FallbackIfUnknown<
      Dispatch,
      ThunkDispatch<
        GetState<ThunkApiConfig>,
        GetExtra<ThunkApiConfig>,
        AnyAction
      >
    >
  : ThunkDispatch<GetState<ThunkApiConfig>, GetExtra<ThunkApiConfig>, AnyAction>
```
````

---
layout: center
---

<img src="/ts-meme.png" style="scale: 0.8" alt="">


---
layout: center
---

<img src="/ts-meme-2.jpg" style="scale: 0.6" alt="">


---
layout: introduction
---

---
layout: cover
---

<h1 style="text-align: center">"When Everything is Priority <br> Nothing is Priority" </h1>
---
---

```ts
interface User {
    email?: string;
    password?: string
    address?: {
        zipcode?: string;
        city?: string;
        country?: string;
    }
}

const country = user?.address?.country 
```

---
layout: center
---

# Why even Bother with TypeScript?!

---
layout: center
---

# Lesson #0: Be Intentional!

---
layout: center
---

<h1 style="text-align: center"> Duck Typing </h1>

## If it walks like a duck and talks like a duck, it is a duck

---
layout: center
---
# Set Theory Basics
<img src="/set-theory.webp"/>

---
---

````md magic-move

```ts
interface User {
    email: string;
    password: string
}

interface ExternalParty {
    email: string;
    ...
}

type SomeType = User | ExternalParty 
```

```ts
interface User {
    email: string;
    password: string
}

interface ExternalParty {
    email: string;
    ...
}

type SomeType = User | ExternalParty // {email: string}
```

```ts
interface User {
    email: string;
    password: string
}

interface ExternalParty {
    email: string;
    ...
}

type SomeType = User & ExternalParty
```

```ts
interface User {
    email: string;
    password: string
}

interface ExternalParty {
    email: string;
    ...
}

type SomeType = User & ExternalParty // {email: string, password: string, ...}
```
````

---
layout: cover
---

# Lesson #1: Types too Strict

---
layout: cover
---

````md magic-move
```ts
function sendEmail(user: User) {
    // take email property and call external api to send email with certain text
}
```

```ts
function sendEmail(user: User) {
    // take email property and call external api to send email with certain text
}

const user: User;
sendEmail(user) // ✅
```

```ts
function sendEmail(user: User) {
    // take email property and call external api to send email with certain text
}

const company: Company;
sendEmail(company) // ❌
```

```ts
function sendEmail(user: User | Company) {
    // take email property and call external api to send email with certain text
}

const user: User;
sendEmail(user) // ✅

const company: Company;
sendEmail(company) // ✅
```

```ts
function sendEmail(user: {email: string}) {
    // take email property and call external api to send email with certain text
}

const user: User;
sendEmail(user) // ✅

const company: Company;
sendEmail(company) // ✅
```
````

---
transition: view-transition
layout: center
---

<h1 style="view-transition-name: 'headline'">MVT </h1>

---
transition: view-transition
layout: center
---

<h1 style="view-transition-name: 'headline'">Minimal Viable Type </h1>

---
layout: cover
---

# Lesson #2: Reuse and Repurpose

---
layout: cover
---

````md magic-move
```ts
interface User {
    name: string;
    address: string;
    postCode: string;
}
```

```ts
interface User {
    name: string;
    address: string;
    postCode: string;
    id: string;
}
```

```ts
interface User {
    name: string;
    address: string;
    postCode: string;
    id: string;
}

function getUser(id: string): User;
```

```ts
interface User {
    name: string;
    address: string;
    postCode: string;
    id: string;
}

function getUser(id: string): User;

function createUser(user: User): User; // ❌ What about the ID?
```
````

---
layout: center
---

# Utility Types to the Rescue

---
layout: cover
---

````md magic-move

```ts
interface User {
    name: string;
    address: string;
    postCode: string;
    id: string;
}

export type CreateUser = Exclude<User, "id">;
```

```ts
interface User {
    name: string;
    address: string;
    postCode: string;
    id: string;
}

export type CreateUser = Exclude<User, "id">;

export type AlternativeUser = Pick<User, "name" | "address" | "postCode">
```

```ts
interface ID {
    id: string;
}

interface CreateUser {
    name: string;
    address: string;
    postCode: string;
}

type User = ID & CreateUser;
```
````

---
---


<img src="/spaghetti-lasagne.png">

---
layout: center
---

# Lasagne Rule
<h2 v-click>Don't have more than 3 (Type) Layers</h2>

---
layout: cover
transition: view-transition
---

<h1 style="view-transition-name: const"> Lesson #3 As Const makes Things easy </h1>

---
layout: cover
---

<h1 style="view-transition-name: const"> Lesson #3 As Const makes Things easier </h1>


---
layout: cover
---

<h1>Let's look at some <a class='color-primary italic' href="webstorm://">Code</a>!</h1> 

<style>
    a:hover {
        color: #8da8b2 !important;
    }
</style>

---
layout: cover
transition: view-transition
---

<h1 style="view-transition-name: 'generic-headline'"> Lesson #4 Use Generics 🤯 </h1>

---
layout: cover
transition: view-transition
---

<h1 style="view-transition-name: 'generic-headline'"> Lesson #4 Use Generics Properly </h1>

---
layout: cover
---

````md magic-move
```ts
export class Widget<
    Source extends Datasource,
    Type extends WidgetType,
    Handler extends ErrorHandling,
    Child extends Widget,
    ...
> {}
```

```ts
export class Widget<
    T extends Datasource,
    U extends WidgetType,
    V extends ErrorHandling,
    W extends Widget,
    ...
> {}
```

```ts
export class Widget<
    T extends Datasource,
    U extends WidgetType,
    V extends ErrorHandling,
    W extends Widget,
    ...
> {}
```

```ts
export type PartialOrValue<TValue> = TValue extends object
	? Partial<TValue>
	: TValue;
export type Reducer<TValue, TNext> = (
	previous: TValue,
	next: TNext,
) => PartialOrValue<TValue>;

type ConnectedSignal<TSignalValue> = {
	with<TObservableValue extends PartialOrValue<TSignalValue>>(
		observable: Observable<TObservableValue>,
	): ConnectedSignal<TSignalValue>;
	with<TObservableValue>(
		observable: Observable<TObservableValue>,
		reducer: Reducer<TSignalValue, TObservableValue>,
	): ConnectedSignal<TSignalValue>;
	with<TOriginSignalValue extends PartialOrValue<TSignalValue>>(
		originSignal: () => TOriginSignalValue,
	): ConnectedSignal<TSignalValue>;
	subscription: Subscription;
};
```
````

---
layout: center
---

# Libraries can have Complex Types
<h2 v-click> Your fancy CRUD App is not a Library </h2>

---
layout: quote
quote: Type Parameters Should Appear Twice
image: /effective-typescript-cover.jpeg
link: https://effectivetypescript.com/2020/08/12/generics-golden-rule/
---

---
---

<img src="/eslint-golden-rule-pr.png">

---
layout: quote
quote: Remove the noun 'generic' from your vocabulary. Replace it with 'type argument' and 'type parameter'.
image: /no-thing-as-a-generic.png
backgroundSize: contain
link: https://www.totaltypescript.com/no-such-thing-as-a-generic
---

---
layout: cover
---

<h1>Let's look at some <a class='color-primary italic' href="webstorm://">Code</a>!</h1> 

<style>
    a:hover {
        color: #8da8b2 !important;
    }
</style>
---
layout: cover 
---

# Lesson 5: TypeScript@Latest is Overrated

---
---
   
<Tweet id="1679173438974443530"  />

<a href="https://www.learningtypescript.com/articles/why-typescript-doesnt-follow-strict-semantic-versioning" target="_blank">Full Blog Post </a>

---
layout: center
---

<img src="/java21.webp">

---
layout: center
---

# You don't need to run the latest and greatest!

---
layout: cover
---

# Lesson 6: You Shouldn't use Enums

---
---

<a href="https://www.wordman.dev/blog/typescript-enums/" target="_blank"><img src="/ts-enums.png"></a>

---
---

<h1>TLDR;</h1>

<ul class="adjusted-list">
    <li class="p-4">Bundle Size</li>
    <li class="p-4" v-click>Not JavaScript Standard</li>
    <li class="p-4" v-click>No Single File Compilation</li>
    <li class="p-4" v-click>No Benefit over *-Literal Types</li>
</ul>

<style>
   
</style>

---
---

````md magic-move
```ts
export enum HttpStatusCode {
    OK = 200,
    BAD_REQUEST = 400,
    UNAUTHORIZED = 401,
    FORBIDDEN = 403,
    NOT_FOUN = 404,
    INTERNAL_SERVER_ERROR = 500,
};
```

```ts
export const enum HttpStatusCode {
    OK = 200,
    BAD_REQUEST = 400,
    UNAUTHORIZED = 401,
    FORBIDDEN = 403,
    NOT_FOUN = 404,
    INTERNAL_SERVER_ERROR = 500,
};
```

```ts {*|1-6|8-15|17-18}
export const HttpStatusCode_OK = 200;
export const HttpStatusCode_BAD_REQUEST = 400;
export const HttpStatusCode_UNAUTHORIZED = 401;
export const HttpStatusCode_FORBIDDEN = 403;
export const HttpStatusCode_NOT_FOUND = 404;
export const HttpStatusCode_INTERNAL_SERVER_ERROR = 500;

export const ALL_HTTP_STATUS_CODES = [
  HttpStatusCode_OK,
  HttpStatusCode_BAD_REQUEST,
  HttpStatusCode_UNAUTHORIZED,
  HttpStatusCode_FORBIDDEN,
  HttpStatusCode_NOT_FOUND,
  HttpStatusCode_INTERNAL_SERVER_ERROR,
] as const;

export type HttpStatusCodes = typeof ALL_HTTP_STATUS_CODES[number]
// same as type HttpStatusCode = 200 | 400 | 401 | 403 | 404 | 500
```
````

---
layout: cover
---

# Lesson 7: Use Proper Tools

---
---

<a href="https://www.jetbrains.com/webstorm/">
    <img src="/ts-pretty-errors.png" class="absolute top-0 left-0 z-20">
</a>

---
---

<a href="https://eslint.org/">
    <img src="/eslint.gif" class="absolute top-0 left-0 z-20">
</a>
---
---
<a href="https://www.jetbrains.com/qodana/">
    <img src="/qodana.png" class="absolute top-0 left-0 z-20">
</a>
---
---

<a href="https://github.com/michaelangeloio/does-it-throw?tab=readme-ov-file">
    <img src="/does-it-throw.png" class="absolute top-0 left-0 z-20">
</a>

---
---

# Summary

<div class="grid-cols-2 grid">
    <ul class="adjusted-list">
        <li class="p-4">Be Intentional!</li>
        <li class="p-4" v-click>Types too Strict</li>
        <li class="p-4" v-click>Reuse and Repurpose</li>
        <li class="p-4" v-click>As Const makes Things easier</li>
    </ul>
    <ul class="adjusted-list">
        <li v-click class="p-4">Use Generics Properly</li>
        <li class="p-4" v-click>TypeScript@Latest is Overrated</li>
        <li class="p-4" v-click>You Shouldn't use Enums</li>
        <li class="p-4" v-click>Use Proper Tools</li>
    </ul>
</div>

---
layout: center
---

# Bonus

---
layout: center
---

# Branded Types

---
---

````md magic-move
```ts
type Something = Brand<string, "Something">
```

```ts
type Email = Brand<string, "Email">
type Password = Brand<string, "Password">
type UserID = Brand<string, "UserID">
```

```ts
declare const brand: unique symbol;

type Brand<T, TBrand extends string> = T & {
    [brand]: TBrand
} 

// credit to Matt Pocock 
// https://twitter.com/mattpocockuk/status/1625173887590842369/photo/1
```

```ts
const updateUser = (id: UserID): User;

updateUser("someID") // ❌ compile error 
updateUser("someID" as UserID) // ✅ happy compiler, happy life 
```

```ts
const isPassword = (val: string): val is Password => {
     return val.length > 6;
}
```

```ts
const assertEmail = (val: string): asserts val is Email => {
  if (!value.includes("@") throw new Error("That doesn't look like an Email")
}
```
````

---
layout: center
---

# No More Working on Wrong IDs!!!!

---
layout: outro
url: https://wordman.dev/talk/2024/techorama
---
