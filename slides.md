---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://www.sitepen.com/typescript-code-blocks.34Qg3nlF.svg
# background: https://img-c.udemycdn.com/course/750x422/986406_89c5_3.jpg
# some information about your slides, markdown enabled
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
hideInToc: true
---

# 

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
        <p class="text-sm">Bare frontend-utvikler, mentee (ikke Tony sin), volleyball pro wannabe. Mest erfaring med React, Typescript og Vue.</p>
    </div>
</div>

---
transition: fade-out
hideInToc: true
---

# Table of contents

<Toc class="text-xs" minDepth="1" maxDepth="2"></Toc>

---
transition: fade-out
layout: two-cols
---

# Utility Types

```ts {*|2|3-6|*} twoslash

interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}
```

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

<img v-after src="/imgs/typescript utility types.JPG" alt="utility types nav" />

---
transition: fade-out
---

## Partial
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
## Record

```ts {1|1-9|11-14|11-18|11-20|11-22} twoslash
type Vocabulary = Record<string, string>;

function findDefinition(word: string, vocabulary: Vocabulary): string {
    if (vocabulary[word]) {
        return vocabulary[word];
    } else {
        return "Definition not found.";
    }
}

const dictionary: Vocabulary = {
    "apple": "A fruit with a rounded shape, typically red, yellow, or green skin, and crisp flesh.",
    "banana": "A long curved fruit that grows in clusters and has soft pulpy flesh and yellow skin when ripe.",
    "orange": "A round juicy citrus fruit with a tough bright reddish-yellow rind.",
};

console.log(findDefinition("apple", dictionary)); 
// Output: A fruit with a rounded shape, typically red, yellow, or green skin, and crisp flesh.
console.log(findDefinition("banana", dictionary)); 
// Output: A long curved fruit that grows in clusters and has soft pulpy flesh and yellow skin when ripe.
console.log(findDefinition("grape", dictionary)); 
// Output: Definition not found.
```

---
transition: fade-out
---
## Pick

```ts {1-7|9-11|12-21|23} twoslash
type Person = {
    name: string;
    age: number;
    email: string;
    address: string;
}
type GrunnleggendeInfo = Pick<Person, "name" | "email">;

function visGrunnleggendeInfo(info: GrunnleggendeInfo): void {
    console.log(`Navn: ${info.name}, E-post: ${info.email}`);
}
const person: Person = {
    name: "Ola Nordmann",
    age: 30,
    email: "ola@example.com",
    address: "Gateveien 123, Enhverby"
};
const grunnleggendeInfo: GrunnleggendeInfo = {
    name: person.name,
    email: person.email
};

visGrunnleggendeInfo(grunnleggendeInfo); // Output: Navn: Ola Nordmann, E-post: ola@example.com
```

---
transition: fade-out
---
## Omit

```ts {1-7|9-11|13-22|24} twoslash
type Person = {
    name: string;
    age: number;
    email: string;
    address: string;
}
type PersonligInfo = Omit<Person, "age" | "address">;

function visPersonligInfo(info: PersonligInfo): void {
    console.log(`Navn: ${info.name}, E-post: ${info.email}`);
}

const person: Person = {
    name: "Ola Nordmann",
    age: 30,
    email: "ola@example.com",
    address: "Gateveien 123, Enhverby"
};
const personligInfo: PersonligInfo = {
    name: person.name,
    email: person.email
};

visPersonligInfo(personligInfo); // Output: Navn: Ola Nordmann, E-post: ola@example.com
```

---
transition: fade-out
---
## Union Types - Exclude og Extract

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
## Awaited

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
Husk å gjøre denne listen finere

Uppercase
Lowercase
Capitalize
Uncapitalize

---
transition: fade-out

---

### String Manipulation types

```ts {monaco}
// Teknisk sett kan du bruke dette til å for eksempel påkreve uppercase
declare function foo<T extends string>(str: T extends Uppercase<T> ? T : never): void;

foo('ABCDEF');
```
---
transition: fade-out

---
### String Manipulation types
```ts {*} twoslash
// Eller så kan man lage en funksjon som returnerer en transformert versjon av en string

declare function toUpper<T extends string>(val: T): Uppercase<T>;

toUpper('foo'); // "FOO"

```

---
transition: fade-out
---

# TypeScript Oppgaver

<div class="abs-br">
  <a href="https://ghaiklor.github.io/type-challenges-solutions/en/" target="_blank" alt="challenges" title="challenges"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <img class="w-20%" src="/imgs/ts logo.webp" /> TS Challenges
  </a>
</div>

<img v-after src="/imgs/typescript challenges.JPG" alt="ts challenges" />

---
transition: fade-out

---

# What is Slidev?

Slidev is a slides maker and presenter designed for developers, consist of the following features

- 📝 **Text-based** - focus on the content with Markdown, and then style them later
- 🎨 **Themable** - theme can be shared and used with npm packages
- 🧑‍💻 **Developer Friendly** - code highlighting, live coding with autocompletion
- 🤹 **Interactive** - embedding Vue components to enhance your expressions
- 🎥 **Recording** - built-in recording and camera view
- 📤 **Portable** - export into PDF, PNGs, or even a hostable SPA
- 🛠 **Hackable** - anything possible on a webpage

<br>
<br>

Read more about [Why Slidev?](https://sli.dev/guide/why)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
transition: slide-up
level: 2
---

# Navigation

Hover on the bottom-left corner to see the navigation's controls panel, [learn more](https://sli.dev/guide/navigation.html)

## Keyboard Shortcuts

|     |     |
| --- | --- |
| <kbd>right</kbd> / <kbd>space</kbd>| next animation or slide |
| <kbd>left</kbd>  / <kbd>shift</kbd><kbd>space</kbd> | previous animation or slide |
| <kbd>up</kbd> | previous slide |
| <kbd>down</kbd> | next slide |

<!-- https://sli.dev/guide/animations.html#click-animations -->
<img
  v-click
  class="absolute -bottom-9 -left-7 w-80 opacity-50"
  src="https://sli.dev/assets/arrow-bottom-left.svg"
  alt=""
/>
<p v-after class="absolute bottom-23 left-45 opacity-30 transform -rotate-10">Here!</p>

---
layout: two-cols
layoutClass: gap-16
---

# Table of contents

You can use the `Toc` component to generate a table of contents for your slides:

```html
<Toc minDepth="1" maxDepth="1"></Toc>
```

The title will be inferred from your slide content, or you can override it with `title` and `level` in your frontmatter.

::right::

<Toc v-click minDepth="1" maxDepth="2"></Toc>

---
layout: image-right
image: https://cover.sli.dev
---

# Code

Use code snippets and get the highlighting directly, and even types hover![^1]

```ts {all|5|7|7-8|10|all} twoslash
// TwoSlash enables TypeScript hover information
// and errors in markdown code blocks
// More at https://shiki.style/packages/twoslash

import { computed, ref } from 'vue'

const count = ref(0)
const doubled = computed(() => count.value * 2)

doubled.value = 2
```

<arrow v-click="[4, 5]" x1="350" y1="310" x2="195" y2="334" color="#953" width="2" arrowSize="1" />

<!-- This allow you to embed external code blocks -->
<<< @/snippets/external.ts#snippet

<!-- Footer -->
[^1]: [Learn More](https://sli.dev/guide/syntax.html#line-highlighting)

<!-- Inline style -->
<style>
.footnotes-sep {
  @apply mt-5 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---
level: 2
---

# Shiki Magic Move

Powered by [shiki-magic-move](https://shiki-magic-move.netlify.app/), Slidev supports animations across multiple code snippets.

Add multiple code blocks and wrap them with <code>````md magic-move</code> (four backticks) to enable the magic move. For example:

````md magic-move
```ts {*|2|*}
// step 1
const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})
```

```ts {*|1-2|3-4|3-4,8}
// step 2
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
    }
  }
}
```

```ts
// step 3
export default {
  data: () => ({
    author: {
      name: 'John Doe',
      books: [
        'Vue 2 - Advanced Guide',
        'Vue 3 - Basic Guide',
        'Vue 4 - The Mystery'
      ]
    }
  })
}
```

Non-code blocks are ignored.

```vue
<!-- step 4 -->
<script setup>
const author = {
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
}
</script>
```
````

---

# Components

<div grid="~ cols-2 gap-4">
<div>

You can use Vue components directly inside your slides.

We have provided a few built-in components like `<Tweet/>` and `<Youtube/>` that you can use directly. And adding your custom components is also super easy.

```html
<Counter :count="10" />
```

<!-- ./components/Counter.vue -->
<Counter :count="10" m="t-4" />

Check out [the guides](https://sli.dev/builtin/components.html) for more.

</div>
<div>

```html
<Tweet id="1390115482657726468" />
```

<Tweet id="1390115482657726468" scale="0.65" />

</div>
</div>

<!--
Presenter note with **bold**, *italic*, and ~~striked~~ text.

Also, HTML elements are valid:
<div class="flex w-full">
  <span style="flex-grow: 1;">Left content</span>
  <span>Right content</span>
</div>
-->

---
class: px-20
---

# Themes

Slidev comes with powerful theming support. Themes can provide styles, layouts, components, or even configurations for tools. Switching between themes by just **one edit** in your frontmatter:

<div grid="~ cols-2 gap-2" m="t-2">

```yaml
---
theme: default
---
```

```yaml
---
theme: seriph
---
```

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-default/01.png?raw=true" alt="">

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-seriph/01.png?raw=true" alt="">

</div>

Read more about [How to use a theme](https://sli.dev/themes/use.html) and
check out the [Awesome Themes Gallery](https://sli.dev/themes/gallery.html).

---

# Clicks Animations

You can add `v-click` to elements to add a click animation.

<div v-click>

This shows up when you click the slide:

```html
<div v-click>This shows up when you click the slide.</div>
```

</div>

<br>

<v-click>

The <span v-mark.red="3"><code>v-mark</code> directive</span>
also allows you to add
<span v-mark.circle.orange="4">inline marks</span>
, powered by [Rough Notation](https://roughnotation.com/):

```html
<span v-mark.underline.orange>inline markers</span>
```

</v-click>

<div mt-20 v-click>

[Learn More](https://sli.dev/guide/animations#click-animations)

</div>

---

# Motions

Motion animations are powered by [@vueuse/motion](https://motion.vueuse.org/), triggered by `v-motion` directive.

```html
<div
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }"
  :click-3="{ x: 80 }"
  :leave="{ x: 1000 }"
>
  Slidev
</div>
```

<div class="w-60 relative">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-square.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-circle.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-triangle.png"
      alt=""
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 30, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn More](https://sli.dev/guide/animations.html#motion)

</div>

---

# LaTeX

LaTeX is supported out-of-box powered by [KaTeX](https://katex.org/).

<br>

Inline $\sqrt{3x-1}+(1+x)^2$

Block
$$ {1|3|all}
\begin{array}{c}

\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &
= \frac{4\pi}{c}\vec{\mathbf{j}}    \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\

\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\

\nabla \cdot \vec{\mathbf{B}} & = 0

\end{array}
$$

<br>

[Learn more](https://sli.dev/guide/syntax#latex)

---

# Diagrams

You can create diagrams / graphs from textual descriptions, directly in your Markdown.

<div class="grid grid-cols-4 gap-5 pt-4 -mb-6">

```mermaid {scale: 0.5, alt: 'A simple sequence diagram'}
sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```

```mermaid {theme: 'neutral', scale: 0.8}
graph TD
B[Text] --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```mermaid
mindmap
  root((mindmap))
    Origins
      Long history
      ::icon(fa fa-book)
      Popularisation
        British popular psychology author Tony Buzan
    Research
      On effectiveness<br/>and features
      On Automatic creation
        Uses
            Creative techniques
            Strategic planning
            Argument mapping
    Tools
      Pen and paper
      Mermaid
```

```plantuml {scale: 0.7}
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}

database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}

[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

</div>

[Learn More](https://sli.dev/guide/syntax.html#diagrams)

---
foo: bar
dragPos:
  square: 691,33,167,_,-16
---

# Draggable Elements

Double-click on the draggable elements to edit their positions.

<br>

###### Directive Usage

```md
<img v-drag="'square'" src="https://sli.dev/logo.png">
```

<br>

###### Component Usage

```md
<v-drag text-3xl>
  <carbon:arrow-up />
  Use the `v-drag` component to have a draggable container!
</v-drag>
```

<v-drag pos="671,205,253,_,-15">
  <div text-center text-3xl border border-main rounded>
    Double-click me!
  </div>
</v-drag>

<img v-drag="'square'" src="https://sli.dev/logo.png">

---
src: ./pages/multiple-entries.md
hide: false
---

---

# Monaco Editor

Slidev provides built-in Monaco Editor support.

Add `{monaco}` to the code block to turn it into an editor:

```ts {monaco}
import { ref } from 'vue'
import { emptyArray } from './external'

const arr = ref(emptyArray(10))
```

Use `{monaco-run}` to create an editor that can execute the code directly in the slide:

```ts {monaco-run}
import { version } from 'vue'
import { emptyArray, sayHello } from './external'

sayHello()
console.log(`vue ${version}`)
console.log(emptyArray<number>(10).reduce(fib => [...fib, fib.at(-1)! + fib.at(-2)!], [1, 1]))
```

---
layout: center
class: text-center
---

# Learn More

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
