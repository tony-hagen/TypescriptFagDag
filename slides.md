---
theme: seriph
layout: image
image: https://www.sitepen.com/typescript-code-blocks.34Qg3nlF.svg
# background: https://img-c.udemycdn.com/course/750x422/986406_89c5_3.jpg
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
mdc: true
hideInToc: true
---

<div class="abs-br h-fit flex">
  <a href="https://github.com/tony-hagen/TypescriptFagDag" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github /> GitHub repo
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: slide-up
hideInToc: true
---

# Foredragsholdere 

<div class="flex gap-20 mt-20">
    <div class="flex flex-col gap-3">
        <img class="w-30%" src="imgs/Tony Hagen.jpg" alt="big man" />
        <span class="text-md">Tony Hagen</span>
        <p class="text-sm">Senior frontend-utvikler, mentor, disc golf entusiast. Mest erfaring med React, Typescript og Angular.</p>
    </div>
    <div class="flex flex-col gap-3">
            <img class="w-30%" src="imgs/Deividas Svaikauskas.jpg" alt="dave" />
        <span class="text-md">Deividas Svaikauskas</span>
        <p class="text-sm">Bare frontend-utvikler, mentee (ikke Tony sin), volleyball pro wannabe. Mest erfaring med React, Typescript og diverse CSS biblioteker.</p>
    </div>
</div>

---
transition: fade-out
hideInToc: true
---

# Table of contents

<Toc class="text-sm" minDepth="1" maxDepth="2"></Toc>

---
transition: fade-out
layout: two-cols
---

# Utility Types
<br />
<div v-after class="grid grid-cols-2 gap-2 text-md mt-10">
    <span>Partial</span>
    <span>Required</span>
    <span>Readonly</span>
    <span>Record</span>
    <span>Pick</span>
    <span>Omit</span>
    <span>Union Types</span>
    <span>Awaited</span>
</div>

::right::
<br />
<br />
<br />

<img v-after src="/imgs/typescript utility types.JPG" alt="utility types nav" />

---
transition: fade-out
---

## Partial
<br />

```ts {1-5|1-13|15-16|18-19} twoslash
type Person = {
    name: string;
    age: number;
    email: string;
};

function createPerson(data: Partial<Person>): Person {
    return {
        name: data.name || "Unknown",
        age: data.age || 0,
        email: data.email || "unknown@example.com",
    };
}

const person1 = createPerson({ name: "John" });
console.log(person1); // Output: { name: 'John', age: 0, email: 'unknown@example.com' }

const person2 = createPerson({ name: "Alice", age: 30, email: "alice@example.com" });
console.log(person2); // Output: { name: 'Alice', age: 30, email: 'alice@example.com' }
```

---
transition: fade-out
---
## Required
<br />

```ts {1-5|1-7|9-11|13-17} twoslash
type PartialPerson = {
    name?: string;
    age?: number;
    email?: string;
};

type RequiredPerson = Required<PartialPerson>;

function createRequiredPerson(data: RequiredPerson): RequiredPerson {
    return data;
}

const person1: RequiredPerson = createRequiredPerson({ name: "John", age: 30, email: "john@example.com" });
console.log(person1); // Output: { name: 'John', age: 30, email: 'john@example.com' }

// const person2: RequiredPerson = createRequiredPerson({ name: "Alice" });
// Dette vil resultere i en TypeScript-feil fordi alle egenskapene er påkrevd
```

---
transition: fade-out
---
## Readonly
<br />

```ts {1-5|1-13|15|17-21} twoslash
type Person = {
    readonly name: string;
    readonly age: number;
    readonly email: string;
};

function createPerson(name: string, age: number, email: string): Person {
    return {
        name,
        age,
        email,
    };
}

const person1: Person = createPerson("John", 30, "john@example.com");

// Forsøk på å endre egenskapen til en readonly-egenskap vil føre til en TypeScript-feil
// person1.name = "Alice"; Dette vil resultere i en feil: Cannot assign to 'name' 
//because it is a read-only property.

console.log(person1); // Output: { name: 'John', age: 30, email: 'john@example.com' }
```

---
transition: fade-out
---
## Pick

```ts {1-7|9-18|20-22|20-24} twoslash
type Person = {
    name: string;
    age: number;
    email: string;
    address: string;
}
type MainInfo = Pick<Person, "name" | "email">;

const person: Person = {
    name: "Ola Nordmann",
    age: 30,
    email: "ola@example.com",
    address: "Gateveien 123, Enhverby"
};
const mainInfo: MainInfo = {
    name: person.name,
    email: person.email
};

function showMainInfo(info: MainInfo): void {
    console.log(`Navn: ${info.name}, E-post: ${info.email}`);
}

showMainInfo(mainInfo); // Output: Navn: Ola Nordmann, E-post: ola@example.com
```

---
transition: fade-out
---
## Omit

```ts {1-7|9-18|19-23|19-24} twoslash
type Person = {
    name: string;
    age: number;
    email: string;
    address: string;
}
type PersonInfo = Omit<Person, "age" | "address">;

const person: Person = {
    name: "Ola Nordmann",
    age: 30,
    email: "ola@example.com",
    address: "Gateveien 123, Enhverby"
};
const personInfo: PersonInfo = {
    name: person.name,
    email: person.email
};

function showPersonInfo(info: PersonInfo): void {
    console.log(`Navn: ${info.name}, E-post: ${info.email}`);
}

showPersonInfo(personInfo); // Output: Navn: Ola Nordmann, E-post: ola@example.com
```

---
transition: fade-out
---
## Union Types - Exclude og Extract
<br />

```ts {1|1-4|1,6-7} twoslash
type Car = "Sedan" | "SUV" | "Truck" | "Van" | "Convertible";

type NonCommercialCar = Exclude<Car, "Truck" | "Van">;
// Resultatet vil være "Sedan" | "SUV" | "Convertible"

type LuxuryCar = Extract<Car, "Sedan" | "Convertible">;
// Resultatet vil være "Sedan" | "Convertible"
```

---
transition: fade-out
---
## Record
<br />

```ts {1|1-7|9-15|*} twoslash
type Vocabulary = Record<string, string>;

const dictionary: Vocabulary = {
    "apple": "A fruit with a rounded shape, typically red, yellow, or green skin, and crisp flesh.",
    "banana": "A long curved fruit that grows in clusters and has soft pulpy flesh and yellow skin when ripe.",
    "orange": "A round juicy citrus fruit with a tough bright reddish-yellow rind.",
};

function findDefinition(word: string, vocabulary: Vocabulary): string {
    if (vocabulary[word]) {
        return vocabulary[word];
    } else {
        return "Definition not found.";
    }
}

findDefinition("apple", dictionary); 
// Output: "A fruit with a rounded shape, typically red, yellow, or green skin, and crisp flesh."
findDefinition("banana", dictionary); 
// Output: "A long curved fruit that grows in clusters and has soft pulpy flesh and yellow skin when ripe."
findDefinition("grape", dictionary); 
// Output: "Definition not found."
```

---
transition: fade-out
---
## Awaited
#
#### Awaited "pakker ut" Promises
<br />

```ts {1| *} twoslash
type MyPromise = Promise<string>;

type MyAwaited = Awaited<MyPromise>

```

---
transition: fade-out
---
## Awaited
<br /> 

```ts {1-7|1-10} twoslash
async function fetchUserData(): Promise<{ name: string; age: number }> {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve({ name: "John Doe", age: 30 });
        }, 2000);
    });
}

type UserData = Awaited<ReturnType<typeof fetchUserData>>;
// Vi bruker Awaited for å få typen av det som blir returnert når vi venter på fetchData-funksjonen
```

---
transition: fade-out
---
## Generic types
<br />

````md magic-move
```ts {*} twoslash
//Hvis vi ser på koden fra forrige slide så kan man se en mulighet til å gjøre denne generisk.
async function fetchUserData(): Promise<{ name: string; age: number }> {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve({ name: "John Doe", age: 30 });
        }, 2000);
    });
} 
```
```ts {*} twoslash
//Bytter til et litt mer reelt eksempel
async function fetchUserData(): Promise<{ name: string; age: number }> {
    const response = await fetch("api/user/123")
    const data = await response.json()
    return data as Promise<{ name: string; age: number }>;
} 
```
```ts {*|3|4-5|6|*} twoslash
// Her bruker vi generics til å spesifisiere hvilken type vi forventer å få returnert
// På denne måten er funksjonen mer generisk og kan hente data av alle typer uten å miste type safety
async function fetchData<T>(url: string ): Promise<T> {
    const response = await fetch(url)
    const data = await response.json()
    return data as Promise<T>;
}
```
```ts {*} twoslash
async function fetchData<T>(url: string ): Promise<T> {
    const response = await fetch(url)
    const data = await response.json()
    return data as Promise<T>;
}

fetchData<User>("api/user/123");
```
```ts {*} twoslash
async function fetchData<T>(url: string ): Promise<T> {
    const response = await fetch(url)
    const data = await response.json()
    return data as Promise<T>;
}

fetchData<User>("api/user/123");
fetchData<User[]>("api/users");
```
````

---
transition: fade-out
---
## Generic types
<br />

```ts {monaco}
// Man kan også ha mer enn en generic type. 
function getProperty<T, Key extends keyof T>(obj: T, key: Key){
  return obj[key]
}

let x = { a: 1, b: "b", c: true, d: 4 };
getProperty(x, "c");
```

---
transition: fade-out
---
## String Manipulation types

<ul class="mt-10">
  <li>Uppercase&lt;T&gt;</li>
  <li>Lowercase&lt;T&gt;</li>
  <li>Capitalize&lt;T&gt;</li>
  <li>Uncapitalize&lt;T&gt;</li>
</ul>

---
transition: fade-out

---

### String Manipulation types
<br />

```ts {monaco}
// Teknisk sett kan du bruke dette til å for eksempel påkreve uppercase
declare function onlyUpper<T extends string>(str: T extends Uppercase<T> ? T : never): void;

onlyUpper('ABCDEF');
```
---
transition: fade-out

---
### String Manipulation types
 <br />

```ts {monaco}
// Eller så kan man lage en funksjon som returnerer en transformert versjon av en string

declare function toUpper<T extends string>(val: T): Uppercase<T>;

toUpper('foo'); // "FOO"

```

---
transition: fade-out

---
### String Manipulation types
<br />

```ts {monaco}
// Eller så kan man kombinere..

declare function toCapitalizedLower<T extends string>(val: T): Capitalize<Lowercase<T>>;

toCapitalizedLower('FOO'); // "Foo"

```

---
transition: fade-out
---

# TypeScript Oppgaver
<br />
<iframe src="https://i.gifer.com/6DMO.gif" width="480" height="270" frameBorder="0"></iframe>

---
transition: fade-out
---

## Enklere og mer realistiske oppgaver

<div class="abs-br">
  <a href="https://typescript-exercises.github.io/#exercise=1&file=%2Findex.ts" target="_blank" alt="challenges_easy" title="challenges_easy"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <img class="w-20%" src="/imgs/ts logo.webp" /> Enklere TS Oppgaver
  </a>
</div>

<br></br>
<br></br>
<img class="w-50%" v-after src="/imgs/typescript easier exercises.JPG" alt="ts challenges" />


---
transition: fade-out
---

## Vanskeligere oppgaver

<div class="abs-br">
  <a href="https://ghaiklor.github.io/type-challenges-solutions/en/" target="_blank" alt="challenges_hard" title="challenges_hard"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <img class="w-20%" src="/imgs/ts logo.webp" /> Vanskeligere TS Oppgaver
  </a>
</div>
<br></br>
<br></br>
<img class="w-80%" v-after src="/imgs/typescript challenges.JPG" alt="ts challenges" />

---
transition: fade-out
---
