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
layout: cover
mdc: true
transition: slide-left
favicon: ./favicon.png
---

# Pragmatic Typings
## Patterns to Reduce Friction and Spark Joy

<img src="/ts-logo.png" style="scale: 0.3; position: absolute; top: 20px; left: -160px;"  alt="">

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

# Lesson 0: Be Intentional!

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
layout: center
---

# Spaghetti & Lasagne Code

---
layout: center
---

# Lasagne Rule
<h2 v-click>Don't have more than 3 (Type) Layers</h2>

---
layout: cover
animation: view-transition
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
---

<div class="flex items-center justify-around flex-col h-full">
    <h1>Free WebStorm License*</h1>
    <a href="https://www.jetbrains.com/store/redeem/?product=WS&coupon=KCMEETUP">
        <qrcode value="https://www.jetbrains.com/store/redeem/?product=WS&coupon=KCMEETUP" />
<span>       https://www.jetbrains.com/store/redeem/?product=WS&coupon=KCMEETUP</span>
    </a>
    <span>* For 6 Months</span>
</div>

<style>
a {
    width: fit-content;
    height: fit-content;
    display: contents;
    text-decoration: none;
    border: none
}
</style>

---
layout: outro
url: https://wordman.dev/talk/2024/kcjs
---
