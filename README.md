
# 🧠 JavaScript Output-Based Interview Questions (Predict the Output)

This file covers the **"What will this code print?"** style questions — one of the most common formats in JavaScript interviews. These questions test whether you truly *understand* how JavaScript works under the hood, not just whether you've memorized definitions.

> 💡 **How to use this file**
> - For each question, read the code FIRST and try to guess the output yourself.
> - Then check the actual output and explanation below it.
> - Pay special attention to the **🔑 Thumb Rule** — it's the "shortcut" that helps you solve similar questions instantly, even if you've never seen that exact code before.

---

## 📂 Category 1: Data Types, Type Coercion & Operators (+, -, ==, ===)

This is the **most important category** — almost every JS interview has at least 2-3 questions from here. The core idea: JavaScript tries to be "helpful" by automatically converting (coercing) values from one type to another, and this leads to some surprising results.

### 1.1 The `+` operator with different data types

```javascript
console.log(1 + 1);        // ?
console.log("1" + 1);      // ?
console.log(1 + "1");      // ?
console.log("1" + "1");    // ?
console.log(1 + 2 + "3");  // ?
console.log("1" + 2 + 3);  // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
2
11
11
11
33
123
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `1 + 1` → both numbers → normal math → `2`
- `"1" + 1` and `1 + "1"` → one side is a string → the NUMBER is converted to a string, and they're joined together → `"11"`
- `"1" + "1"` → both strings → joined together → `"11"`
- `1 + 2 + "3"` → evaluated LEFT to RIGHT: `1 + 2 = 3` (both numbers, normal math), then `3 + "3" = "33"` (now a string appears, so it becomes concatenation)
- `"1" + 2 + 3` → `"1" + 2 = "12"` (string appears immediately), then `"12" + 3 = "123"`

</details>

🔑 **Thumb Rule:** The `+` operator works LEFT to RIGHT. **The moment a string appears anywhere in the chain, everything from that point onward becomes string concatenation** — but operations done BEFORE that point (if both sides were numbers) remain normal math.

---

### 1.2 The `+` operator with booleans, null, and undefined

```javascript
console.log(true + true);   // ?
console.log(true + 1);      // ?
console.log(false + 1);     // ?
console.log(null + 1);      // ?
console.log(undefined + 1); // ?
console.log(null + undefined); // ?
console.log(null + null);   // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
2
2
1
1
NaN
NaN
0
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `true` converts to `1`, `false` converts to `0` when used in math. So `true + true = 1 + 1 = 2`, `true + 1 = 1 + 1 = 2`, `false + 1 = 0 + 1 = 1`.
- `null` converts to `0` in math: `null + 1 = 0 + 1 = 1`. So `null + null = 0 + 0 = 0`.
- `undefined` converts to `NaN` in math: `undefined + 1 = NaN`. Anything combined with `NaN` using `+` (as numbers) results in `NaN`. So `null + undefined` → `0 + NaN = NaN`.

</details>

🔑 **Thumb Rule:** For arithmetic, `true → 1`, `false → 0`, `null → 0`, but `undefined → NaN`. Remember: **`null` behaves like `0`, but `undefined` "poisons" any math operation into `NaN`.**

---

### 1.3 The `-` (subtraction) operator with different types

```javascript
console.log("5" - 2);      // ?
console.log("5" - "2");    // ?
console.log("5" - "a");    // ?
console.log(true - 1);     // ?
console.log(false - 1);    // ?
console.log("10" - true);  // ?
console.log(null - 1);     // ?
console.log(undefined - 1);// ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
3
3
NaN
0
-1
9
-1
NaN
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Unlike `+`, the `-` operator has NO "string concatenation" mode — it ALWAYS tries to convert both sides to numbers and subtract.
- `"5" - 2` → `"5"` becomes `5` → `5 - 2 = 3`
- `"5" - "2"` → both convert to numbers → `5 - 2 = 3`
- `"5" - "a"` → `"a"` cannot be converted to a number → `NaN`
- `true - 1` → `1 - 1 = 0`; `false - 1` → `0 - 1 = -1`
- `"10" - true` → `10 - 1 = 9`
- `null - 1` → `0 - 1 = -1`
- `undefined - 1` → `NaN - 1 = NaN`

</details>

🔑 **Thumb Rule:** `+` is special because it ALSO means "string concatenation." Every OTHER math operator (`-`, `*`, `/`, `%`) has no such special case — they ALWAYS convert both operands to numbers first.

---

### 1.4 `==` (loose equality) with primitives — the classic gotchas

```javascript
console.log(0 == "0");          // ?
console.log(0 == "");           // ?
console.log(0 == false);        // ?
console.log("" == false);       // ?
console.log("" == "0");         // ?
console.log(" " == 0);          // ?
console.log(null == undefined); // ?
console.log(null == 0);         // ?
console.log(undefined == 0);    // ?
console.log(NaN == NaN);        // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
true
true
true
true
false
true
true
false
false
false
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

`==` converts both sides to a common type before comparing. Walking through the tricky ones:
- `0 == "0"` → `"0"` becomes `0` → `true`
- `0 == ""` → `""` becomes `0` → `true`
- `0 == false` → `false` becomes `0` → `true`
- `"" == false` → both become `0`/falsy-numeric → `true`
- `"" == "0"` → BOTH are strings, so NO coercion happens, and `""` is literally not the same text as `"0"` → `false`
- `" " == 0` → a whitespace-only string converts to `0` when coerced to a number → `0 == 0` → `true`
- `null == undefined` → **special case**: `null` and `undefined` are loosely equal to EACH OTHER and to NOTHING else → `true`
- `null == 0` → `false` — this surprises many people! `null` does NOT convert to `0` for `==` comparisons (even though it does for `+`/`-`). `null` is only `==` to `undefined` and itself.
- `undefined == 0` → `false` — same reason, `undefined` is also only `==` to `null` and itself.
- `NaN == NaN` → `false` — `NaN` is never equal to anything, including itself, by definition.

</details>

🔑 **Thumb Rule:** `null` and `undefined` are `==` to EACH OTHER and to NOTHING ELSE (not even `0`, `false`, or `""`) — this is a special hardcoded rule in JavaScript, completely separate from normal coercion rules.

---

### 1.5 `===` (strict equality) — no coercion at all

```javascript
console.log(0 === "0");          // ?
console.log(0 === false);        // ?
console.log(null === undefined); // ?
console.log(NaN === NaN);        // ?
console.log(1 === 1.0);          // ?
console.log("abc" === "abc");    // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
false
false
true
true
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

`===` checks BOTH the value AND the type — if the types differ, it's `false` immediately, no conversion attempted.
- `0 === "0"` → number vs string → `false`
- `0 === false` → number vs boolean → `false`
- `null === undefined` → different types → `false`
- `NaN === NaN` → `false` (special rule, same as `==`)
- `1 === 1.0` → `true` — JavaScript has only ONE number type, so `1` and `1.0` are literally the same value.
- `"abc" === "abc"` → same type, same value → `true`

</details>

🔑 **Thumb Rule:** `===` = "same type AND same value." If you're ever unsure about `==` behavior, just remember: `===` never does conversions, so it's predictable. **Always prefer `===` in real code.**

---

### 1.6 `==` and `===` with arrays and objects

```javascript
console.log([] == []);   // ?
console.log([] === []);  // ?
console.log([] == {});   // ?
console.log([] == ![]);  // ?
console.log([] + []);    // ?
console.log([] + {});    // ?
console.log({} + []);    // ?  (run this in browser console vs Node — tricky!)
console.log([1,2] + [3,4]); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
false
true
""
[object Object]
[object Object]
1,23,4
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `[] == []` and `[] === []` → both `false`. When BOTH sides of `==`/`===` are objects (arrays count as objects), there is NO coercion — it's purely a reference check. Two different `[]` literals create two different array objects in memory.
- `[] == {}` → `false` — same reason, two different objects, no coercion between two objects.
- `[] == ![]` → `true` 😱. Here's the trick:
  1. `![]` is calculated first. `[]` is a truthy value (all objects are truthy), so `![]` = `false`.
  2. Now the comparison is `[] == false`.
  3. NOW one side (`false`) is a primitive, so coercion kicks in: `[]` converts to a primitive → `""` (empty string), and `false` converts to `0`.
  4. `"" == 0` → `""` converts to `0` → `0 == 0` → `true`.
- `[] + []` → `+` converts both arrays to primitives (strings) → `"" + "" = ""`.
- `[] + {}` → `"" + "[object Object]" = "[object Object]"`.
- `{} + []` → ⚠️ This one depends on CONTEXT! Inside `console.log({} + [])`, `{}` is treated as an OBJECT (since it's an expression/argument), so it behaves like `[] + {}` above → `"[object Object]"`. But if written as a STANDALONE statement on its own line (`{} + []` at the start of a line), JavaScript may interpret `{}` as an empty BLOCK statement, and `+[]` as a separate expression that evaluates to `0`. This is a famous "it depends how you write it" trap.
- `[1,2] + [3,4]` → both arrays convert to strings: `"1,2" + "3,4" = "1,23,4"`.

</details>

🔑 **Thumb Rule:** 
- **Object vs Object** (for `==` or `===`) → always reference comparison, coercion NEVER happens.
- **Object vs Primitive** → the object gets converted to a primitive (arrays → comma-joined string, plain objects → `"[object Object]"`), THEN normal coercion rules apply.
- `{} + []` is famous specifically because of the ambiguity between "block statement" vs "object literal" — always wrap in parentheses `({} + [])` if you genuinely need this, but more importantly: **never write code like this in production!**

---

### 1.7 Quick-Reference Table: Common Coercion Results

| Expression | Result | Why |
|---|---|---|
| `1 + "2"` | `"12"` | number → string, concatenation |
| `"5" - 1` | `4` | string → number, subtraction |
| `"5" * "2"` | `10` | both strings → numbers |
| `true + true` | `2` | `true` → `1` |
| `"" + 1` | `"1"` | number → string |
| `null + 1` | `1` | `null` → `0` |
| `undefined + 1` | `NaN` | `undefined` → `NaN` |
| `0 == false` | `true` | both → `0` |
| `0 === false` | `false` | different types |
| `null == undefined` | `true` | special rule |
| `null === undefined` | `false` | different types |
| `NaN == NaN` | `false` | NaN ≠ NaN, ever |
| `[] == []` | `false` | different references |
| `[1] == "1"` | `true` | array → `"1"`, then `"1" == "1"` |

---

## 📂 Category 2: Hoisting & Scope (var, let, const)

### 2.1 Hoisting with var

```javascript
console.log(a);
var a = 5;
console.log(a);
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
undefined
5
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

JavaScript "hoists" the DECLARATION of `var a` to the top of the scope, but NOT the assignment (`= 5`). So at the first `console.log`, `a` exists but has no value yet (`undefined`). After the assignment line runs, `a` becomes `5`.

</details>

🔑 **Thumb Rule:** With `var`, think of it as: *"the variable name moves to the top, but the value stays where it is."*

---

### 2.2 Hoisting with let (Temporal Dead Zone)

```javascript
console.log(b);
let b = 5;
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
ReferenceError: Cannot access 'b' before initialization
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

`let` (and `const`) are also hoisted, but they are NOT usable before their declaration line. This gap between the start of the scope and the declaration line is called the **Temporal Dead Zone (TDZ)**. Accessing the variable during this zone throws an error.

</details>

🔑 **Thumb Rule:** `var` before declaration → `undefined` (no error). `let`/`const` before declaration → **ReferenceError** (TDZ).

---

### 2.3 Function hoisting vs variable hoisting conflict

```javascript
var greet = "Hello";

function greet() {
  return "Hi";
}

console.log(typeof greet); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
string
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Both function declarations AND `var` declarations are hoisted, but function declarations are hoisted "more strongly" — they're hoisted WITH their full definition. However, the code then runs top-to-bottom, and `var greet = "Hello"` executes AFTER the function declaration is hoisted, OVERWRITING `greet` with the string `"Hello"`. So by the time `console.log` runs, `greet` is a string.

</details>

🔑 **Thumb Rule:** When a `var` and a `function` have the SAME name, the function declaration is hoisted first, but any LATER assignment (even with `var`) will overwrite it once the code actually executes in order.

---

### 2.4 Global var vs window object

```javascript
var name = "Global";
let age = 25;

console.log(window.name); // ? (in a browser environment)
console.log(window.age);  // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Global
undefined
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

In browsers, variables declared with `var` at the top level (global scope) become properties of the `window` object. Variables declared with `let` or `const` do NOT become properties of `window` — they exist in a separate "script scope," even though they're still globally accessible by name.

</details>

🔑 **Thumb Rule:** `var` at global scope pollutes the global `window` object. `let`/`const` at global scope do not — this is one reason `let`/`const` are preferred (cleaner global scope, fewer naming collisions).

---

## 📂 Category 3: Loops, Closures & setTimeout

### 3.1 var inside a loop with setTimeout

```javascript
for (var i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
4
4
4
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

`var` does NOT create a new variable for each loop iteration — there is only ONE `i` shared across all iterations, stored in the function/global scope. By the time `setTimeout` actually runs (after 1 second), the loop has already finished completely, and `i` has become `4` (the value that made the loop condition `false` and exit).

</details>

🔑 **Thumb Rule:** `var` is function-scoped, NOT block-scoped. So loops using `var` share a single variable across all iterations — by the time any delayed code runs, the loop has already finished and the variable holds its FINAL value.

---

### 3.2 let inside a loop with setTimeout

```javascript
for (let i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
2
3
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Unlike `var`, `let` is block-scoped — JavaScript creates a NEW `i` for EACH iteration of the loop. So each `setTimeout` callback "remembers" its own separate copy of `i` (this is a closure).

</details>

🔑 **Thumb Rule:** If you ever see `var` causing unexpected "same value repeated" output in a loop, the fix is almost always: **replace `var` with `let`**.

🔄 **Variation — fixing `var` WITHOUT switching to `let` (using an IIFE):**
```javascript
for (var i = 1; i <= 3; i++) {
  (function(j) {
    setTimeout(() => console.log(j), 1000);
  })(i);
}
// Output: 1, 2, 3
```
This works because the IIFE creates a new `j` for each iteration — an old-school trick used before `let` existed.

🔄 **Variation — same problem, written slightly differently:**
```javascript
function printNumbers() {
  for (var i = 0; i < 3; i++) {
    setTimeout(function() {
      console.log("Number:", i);
    }, i * 1000);
  }
}
printNumbers();
// Output: Number: 3 / Number: 3 / Number: 3
```
Even though each `setTimeout` fires at a DIFFERENT time (0s, 1s, 2s), by the time ANY of them run, the loop has already finished and `i` is `3`. The fix is the same: use `let`, or pass `i` as a third argument to `setTimeout` (`setTimeout(fn, i*1000, i)`).

---

### 3.3 Closures inside a loop returning functions

```javascript
function createFunctions() {
  let funcs = [];
  for (let i = 0; i < 3; i++) {
    funcs.push(() => console.log(i));
  }
  return funcs;
}

const myFuncs = createFunctions();
myFuncs[0](); // ?
myFuncs[1](); // ?
myFuncs[2](); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
0
1
2
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Because `let` creates a new `i` for each loop iteration, each arrow function in the array "closes over" (remembers) its OWN separate `i`. If `var` were used instead, all three would print `3`.

</details>

🔑 **Thumb Rule:** Same root cause as 3.1/3.2 — `let` in loops = separate variable per iteration = each closure remembers its own value.

---

### 3.4 A simple counter closure

```javascript
function outerFunction() {
  let count = 0;
  return function innerFunction() {
    count++;
    console.log(count);
  };
}

const counter = outerFunction();
counter(); // ?
counter(); // ?
counter(); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
2
3
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Even though `outerFunction()` has already finished executing, `innerFunction` still "remembers" the `count` variable from its parent scope. Each call to `counter()` updates the SAME `count` variable, because it's the same closure being reused.

</details>

🔑 **Thumb Rule:** A closure "remembers" the variables from where it was CREATED, not where it's CALLED — and that memory persists across multiple calls if you keep reusing the same returned function.

---

## 📂 Category 4: `this`, Functions & Classes

### 4.1 `this` inside a regular function vs arrow function

```javascript
const obj = {
  name: "Alex",
  regular: function() {
    console.log("Regular:", this.name);
  },
  arrow: () => {
    console.log("Arrow:", this.name);
  }
};

obj.regular(); // ?
obj.arrow();   // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Regular: Alex
Arrow: undefined
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `regular` is called as `obj.regular()`, so `this` refers to `obj`, and `this.name` is `"Alex"`.
- `arrow` is an arrow function, which does NOT get its own `this`. It uses `this` from where it was DEFINED (the outer/global scope), where `name` doesn't exist — so `this.name` is `undefined`.

</details>

🔑 **Thumb Rule:** Never use arrow functions for object methods if you need `this` to refer to that object. Use regular `function` syntax for methods.

---

### 4.2 Class fields and detached `this`

```javascript
class Counter {
  count = 0;

  increment() {
    this.count++;
    console.log(this.count);
  }
}

const counter = new Counter();
const incrementFn = counter.increment;

counter.increment();  // ?
incrementFn();        // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
TypeError: Cannot read properties of undefined (reading 'count')
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

`counter.increment()` works fine because `this` refers to `counter`. But once you extract the method into a standalone variable (`incrementFn`) and call it WITHOUT the object (`incrementFn()`), `this` is no longer bound to `counter` — it becomes `undefined` (in strict mode/classes), causing an error when trying to access `this.count`.

</details>

🔑 **Thumb Rule:** `this` depends on HOW a function is CALLED, not where it's defined. If you "detach" a method from its object and call it standalone, `this` is lost. Fix using `.bind()`, arrow functions for class fields, or by always calling it as `obj.method()`.

---

### 4.3 Function returns vs no return

```javascript
function noReturn() {
  console.log("Hello");
}

let result = noReturn();
console.log(result); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Hello
undefined
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

The function `noReturn` doesn't have a `return` statement, so it implicitly returns `undefined`. The `console.log("Hello")` still runs (that's its side effect), but the VALUE returned and stored in `result` is `undefined`.

</details>

🔑 **Thumb Rule:** A function with no explicit `return` ALWAYS returns `undefined` — even if it does other things like printing to the console.

---

### 4.4 Default parameters and undefined

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}`);
}

greet();           // ?
greet(undefined);  // ?
greet(null);       // ?
greet("");         // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Hello, Guest
Hello, Guest
Hello, null
Hello,
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Default parameters are used ONLY when the argument is `undefined` (or not passed at all). Passing `null` or `""` (empty string) are still "real values" — they don't trigger the default.

</details>

🔑 **Thumb Rule:** Default parameters only kick in for `undefined`, not for `null`, `0`, `""`, or `false`.

---

### 4.5 IIFE (Immediately Invoked Function Expression)

```javascript
var result = (function() {
  var x = 10;
  return x * 2;
})();

console.log(result);   // ?
console.log(typeof x); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
20
undefined
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

An IIFE runs immediately after it's defined. The variable `x` is local to the IIFE's scope and doesn't leak outside — so `result` gets `20` (the returned value), but `x` doesn't exist in the outer scope. Using `typeof` on a completely undeclared variable returns `"undefined"` instead of throwing an error (this is a special safety behavior of `typeof`).

</details>

🔑 **Thumb Rule:** IIFEs create a private scope — variables declared inside never leak out. Also remember: `typeof someUndeclaredVariable` is safe and returns `"undefined"`, but actually USING that variable (e.g., `console.log(someUndeclaredVariable)`) throws a `ReferenceError`.

---

## 📂 Category 5: Truthy/Falsy & Logical Operators

### 5.1 Truthy and Falsy values

```javascript
const values = [0, "", null, undefined, NaN, false, "0", [], {}];

values.forEach(val => {
  console.log(Boolean(val));
});
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
false
false
false
false
true
true
true
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

There are only **6 falsy values** in JavaScript: `0`, `""` (empty string), `null`, `undefined`, `NaN`, and `false`. EVERYTHING else is truthy — including `"0"` (a non-empty string!), empty arrays `[]`, and empty objects `{}` (because they are objects, and all objects are truthy).

</details>

🔑 **Thumb Rule:** Memorize the 6 falsy values: `0`, `""`, `null`, `undefined`, `NaN`, `false`. If a value isn't on this list, it's truthy — even `"0"`, `[]`, and `{}`.

---

### 5.2 Short-circuit evaluation with && and ||

```javascript
console.log(0 || "default");      // ?
console.log("" || "fallback");     // ?
console.log("Hi" && "Bye");        // ?
console.log(null && "Never runs"); // ?
console.log(1 && 0 && "end");      // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
default
fallback
Bye
null
0
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `||` (OR) returns the FIRST truthy value it finds (or the last value if all are falsy). `0 || "default"` → `0` is falsy, so it returns `"default"`.
- `&&` (AND) returns the FIRST falsy value it finds (or the last value if all are truthy). `"Hi" && "Bye"` → both truthy, so it returns the LAST one, `"Bye"`.
- `null && "Never runs"` → `null` is falsy, so `&&` stops immediately and returns `null` — `"Never runs"` is never evaluated.
- `1 && 0 && "end"` → `1` is truthy (continue), `0` is falsy (STOP here and return `0`).

</details>

🔑 **Thumb Rule:** `||` returns the first TRUTHY value (or the last value). `&&` returns the first FALSY value (or the last value). Both operators return an actual VALUE, not just `true`/`false`.

---

### 5.3 NaN comparisons

```javascript
let result = 10 / "abc";
console.log(result);                  // ?
console.log(result === NaN);          // ?
console.log(Number.isNaN(result));    // ?
console.log(isNaN("hello"));          // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
NaN
false
true
true
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Dividing a number by a non-numeric string produces `NaN` ("Not a Number"). But `NaN === NaN` is always `false` — NaN is the only value in JavaScript that is NOT equal to itself. The correct way to check is `Number.isNaN()`.

`isNaN("hello")` returns `true` too, but `isNaN()` (the global function) first tries to CONVERT the value to a number before checking — so it can give misleading results for non-number inputs. `Number.isNaN()` is stricter and safer.

</details>

🔑 **Thumb Rule:** To check if a value is `NaN`, always use `Number.isNaN(value)`, never `value === NaN` or the loose global `isNaN()`.

---

### 5.4 Increment operator confusion

```javascript
let x = 5;
console.log(x++); // ?
console.log(x);   // ?
console.log(++x); // ?
console.log(x);   // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
5
6
7
7
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `x++` (post-increment) returns the CURRENT value (`5`), THEN increments `x` to `6`.
- `console.log(x)` now shows `6` (already incremented).
- `++x` (pre-increment) increments `x` to `7` FIRST, THEN returns the new value (`7`).
- `console.log(x)` confirms `x` is `7`.

</details>

🔑 **Thumb Rule:** "Post" (`x++`) = use OLD value, then increase. "Pre" (`++x`) = increase FIRST, then use NEW value. Say it as: *"post means the increment happens AFTER it's used."*

🔄 **Variation — combining both in one expression:**
```javascript
let a = 1;
let b = a++ + ++a;

console.log(a); // 3
console.log(b); // 4
```
Step by step: `a++` returns `1` (then `a` becomes `2`). `++a` makes `a` become `3` first, then returns `3`. So `b = 1 + 3 = 4`, and final `a = 3`.

---

### 5.5 Nested ternary operators

```javascript
let age = 20;

let category = age < 13 ? "Child" 
              : age < 18 ? "Teenager" 
              : age < 60 ? "Adult" 
              : "Senior";

console.log(category); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Adult
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Ternary operators (`condition ? valueIfTrue : valueIfFalse`) can be chained. JavaScript checks each condition in order:
- `age < 13`? `20 < 13` → false
- `age < 18`? `20 < 18` → false
- `age < 60`? `20 < 60` → **true** → returns `"Adult"`

The remaining conditions are never checked once a `true` is found.

</details>

🔑 **Thumb Rule:** Nested ternaries work like an `if / else if / else if / else` chain. Read them top to bottom, and the FIRST true condition "wins." (Deeply nested ternaries are discouraged in production code for readability — `if/else` or `switch` is often clearer.)

---

## 📂 Category 6: Arrays & Objects

### 6.1 Array vs Object reference (copy by reference)

```javascript
let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2.push(4);

console.log(arr1); // ?
console.log(arr2); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
[1, 2, 3, 4]
[1, 2, 3, 4]
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Arrays and objects in JavaScript are stored "by reference" — `arr2 = arr1` doesn't create a NEW array, it just makes `arr2` point to the SAME array in memory as `arr1`. So changing `arr2` also changes `arr1`, because they're literally the same object.

</details>

🔑 **Thumb Rule:** Primitives (numbers, strings, booleans) are copied BY VALUE. Objects and arrays are copied BY REFERENCE. To make a real independent copy, use `[...arr1]`, `{...obj1}`, or a deep clone for nested data.

---

### 6.2 Object comparison

```javascript
const obj1 = { name: "Tom" };
const obj2 = { name: "Tom" };
const obj3 = obj1;

console.log(obj1 == obj2);  // ?
console.log(obj1 === obj2); // ?
console.log(obj1 === obj3); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
true
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Objects are compared by REFERENCE (memory address), not by their contents. `obj1` and `obj2` look identical but are two SEPARATE objects in memory — so they are NOT equal. `obj3`, however, points to the EXACT SAME object as `obj1` (`obj3 = obj1`), so they ARE equal.

</details>

🔑 **Thumb Rule:** Two objects/arrays are only `===` equal if they are the SAME object in memory (same reference) — never because they "look" the same. To compare contents, you need a deep-equality check (e.g., `JSON.stringify(obj1) === JSON.stringify(obj2)` for simple cases, or a library like Lodash's `isEqual` for complex cases).

---

### 6.3 Spread operator — shallow copy gotcha

```javascript
let str = "abc";
let chars = [...str];
console.log(chars); // ?

let arr = [1, [2, 3], 4];
let copy = [...arr];
copy[1].push(99);
console.log(arr); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
['a', 'b', 'c']
[1, [2, 3, 99], 4]
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

- `[...str]` spreads a string into an array of its individual characters.
- `[...arr]` creates a **shallow copy** — the top-level array is new, but nested arrays/objects INSIDE it still point to the SAME memory as the original. So modifying `copy[1]` (the nested array) also affects `arr[1]`.

</details>

🔑 **Thumb Rule:** Spread (`...`) only copies ONE level deep. Nested objects/arrays are still shared between the original and the copy.

---

### 6.4 Object key order and duplicate keys

```javascript
const obj = {
  a: 1,
  b: 2,
  a: 3
};

console.log(obj);              // ?
console.log(Object.keys(obj)); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
{ a: 3, b: 2 }
['a', 'b']
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

If an object has duplicate keys, JavaScript silently keeps only the LAST value for that key — no error is thrown. So `a: 1` is overwritten by `a: 3`.

</details>

🔑 **Thumb Rule:** Duplicate object keys = last one wins, with no warning. Always double-check object literals for accidental duplicate keys (easy to miss in large objects).

---

### 6.5 Array.length after deleting an element

```javascript
let arr = [1, 2, 3, 4, 5];
delete arr[2];

console.log(arr);        // ?
console.log(arr.length); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
[1, 2, <1 empty item>, 4, 5]
5
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

`delete` removes the VALUE at that index but does NOT shift other elements or update `length`. It leaves a "hole" (empty slot) in the array. This is almost never what you want.

</details>

🔑 **Thumb Rule:** Never use `delete` on array elements. To properly remove an item, use `splice()` (which shifts elements and updates length) or `filter()` (which creates a new array without that item).

---

### 6.6 Chained array methods

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
  .filter(n => n % 2 === 0)
  .map(n => n * 10)
  .reduce((sum, n) => sum + n, 0);

console.log(result); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
120
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Step by step:
1. `.filter(n => n % 2 === 0)` → keeps even numbers → `[2, 4, 6]`
2. `.map(n => n * 10)` → multiplies each by 10 → `[20, 40, 60]`
3. `.reduce((sum, n) => sum + n, 0)` → adds them all together → `20 + 40 + 60 = 120`

</details>

🔑 **Thumb Rule:** When you see chained array methods, work through them ONE AT A TIME, writing down the resulting array after each step — don't try to do it all in your head at once.

---

### 6.7 Array sort() default behavior

```javascript
const numbers = [10, 1, 21, 2];
console.log(numbers.sort()); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
[1, 10, 2, 21]
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

By default, `.sort()` converts elements to STRINGS and sorts them alphabetically/lexicographically — NOT numerically. So `"10"` comes before `"2"` because `"1"` (first character) is less than `"2"`.

</details>

🔑 **Thumb Rule:** ALWAYS pass a comparator function when sorting numbers: `numbers.sort((a, b) => a - b)` for ascending order. Without it, `.sort()` treats everything as strings, which gives wrong results for numbers ≥ 10.

---

### 6.8 typeof a function and typeof an array

```javascript
function sayHi() {}
const arr = [1, 2, 3];
const arrowFn = () => {};

console.log(typeof sayHi);       // ?
console.log(typeof arr);         // ?
console.log(typeof arrowFn);     // ?
console.log(Array.isArray(arr)); // ?
console.log(typeof null);        // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
function
object
function
true
object
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Functions (whether regular or arrow) have their own special `typeof` result: `"function"`. Arrays, despite being a distinct "thing" conceptually, return `"object"` for `typeof` — you need `Array.isArray()` to specifically check for arrays. `typeof null` is also `"object"` — a famous long-standing bug in JS that can never be fixed now.

</details>

🔑 **Thumb Rule:** `typeof` has only 8 possible results: `"undefined"`, `"object"`, `"boolean"`, `"number"`, `"string"`, `"bigint"`, `"symbol"`, and `"function"`. Arrays AND `null` both fall under `"object"` — always double-check with `Array.isArray()` or `=== null` when it matters.

---

### 6.9 String comparison (number-like strings)

```javascript
console.log("apple" < "banana");  // ?
console.log("Apple" < "apple");   // ?
console.log("10" < "9");          // ?
console.log(10 < 9);              // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
true
true
true
false
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

Strings are compared character by character based on their **Unicode/ASCII values**, like a dictionary, but with a twist: uppercase letters have SMALLER character codes than lowercase letters. So `"A"` (65) comes before `"a"` (97), making `"Apple" < "apple"` → `true`.

For `"10" < "9"`, these are STRINGS, so they're compared character by character: `"1"` (code 49) is less than `"9"` (code 57), so `"10" < "9"` is `true` — even though numerically 10 is greater than 9!

</details>

🔑 **Thumb Rule:** Comparing strings that LOOK like numbers does NOT do numeric comparison — it compares character-by-character. Always convert to `Number()` first if you want numeric comparison.

---

## 📂 Category 7: Async, Promises & Event Loop

### 7.1 Order of console.log with Promises and setTimeout

```javascript
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
4
3
2
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

This tests your understanding of the **Event Loop** and its different queues:
1. `console.log("1")` and `console.log("4")` run immediately — they're part of the "main" synchronous code.
2. `Promise.then()` callbacks go into the **microtask queue**.
3. `setTimeout` callbacks go into the **macrotask (callback) queue**.
4. After the main code finishes, JavaScript ALWAYS empties the **microtask queue completely** before moving to the macrotask queue.

So the order is: synchronous code first (`1`, `4`), then microtasks (`3`), then macrotasks (`2`).

</details>

🔑 **Thumb Rule:** **Synchronous code → Microtasks (Promises) → Macrotasks (setTimeout/setInterval).** Promises always "jump the queue" ahead of setTimeout, even with `setTimeout(fn, 0)`.

---

### 7.2 Async function return value

```javascript
async function getValue() {
  return 42;
}

const result = getValue();
console.log(result); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Promise { 42 }
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

An `async` function ALWAYS returns a Promise — even if you write a plain `return 42`, JavaScript automatically wraps it as `Promise.resolve(42)`. To get the actual value `42`, you'd need to use `.then()` or `await` it inside another `async` function.

</details>

🔑 **Thumb Rule:** Calling an `async` function NEVER gives you the "final value" directly — it always gives you a Promise. You must use `.then()` or `await` to unwrap it.

---

### 7.3 Sequential vs parallel await

```javascript
function delay(value, ms) {
  return new Promise(resolve => setTimeout(() => resolve(value), ms));
}

async function sequential() {
  const a = await delay("A", 1000);
  const b = await delay("B", 1000);
  console.log(a, b);
}

sequential(); // How long does this take in total?
```

**Output (timing):**
```
Takes about 2 seconds total, then prints: A B
```

<details>
<summary><strong>📖 View Explanation</strong></summary>

Each `await` PAUSES the function until that Promise resolves. Since `b`'s delay doesn't start until AFTER `a` finishes, the two 1-second delays happen one after another — totaling about 2 seconds. This is called **sequential execution**.

🔄 **Variation — running them in PARALLEL instead:**
```javascript
async function parallel() {
  const [a, b] = await Promise.all([delay("A", 1000), delay("B", 1000)]);
  console.log(a, b);
}
parallel(); // Takes only about 1 second total
```
Here, both `delay()` calls START at the same time, and `Promise.all()` waits for BOTH to finish — so the total time is only as long as the SLOWER one (1 second), not the sum.

</details>

🔑 **Thumb Rule:** If tasks don't depend on each other's results, start them all FIRST (without `await`), then use `Promise.all()` to wait for all of them — this is much faster than awaiting one-by-one.

---

### 7.4 Try/catch/finally execution order

```javascript
function test() {
  try {
    console.log("Try");
    throw new Error("Oops");
  } catch (err) {
    console.log("Catch:", err.message);
  } finally {
    console.log("Finally");
  }
  console.log("After try-catch");
}

test();
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Try
Catch: Oops
Finally
After try-catch
```

</details>

<details>
<summary><strong>📖 View Explanation</strong></summary>

The `try` block runs first and throws an error. The `catch` block catches it and runs. The `finally` block ALWAYS runs — whether or not an error occurred, and even if there's a `return` inside `try` or `catch`. Code AFTER the try-catch-finally block continues normally (since the error was already caught/handled).

</details>

🔑 **Thumb Rule:** `finally` ALWAYS runs — no matter what happens in `try` or `catch` (including `return` statements). It's typically used for cleanup tasks (closing files, hiding loaders, etc.).

---

## 🎯 Master List of Thumb Rules (Quick Recap)

| # | Rule |
|---|---|
| 1 | The `+` operator: once a string appears, everything after becomes string concatenation. |
| 2 | For math, `true→1`, `false→0`, `null→0`, but `undefined→NaN`. |
| 3 | `-`, `*`, `/`, `%` ALWAYS convert both sides to numbers — they have no "concatenation" mode like `+`. |
| 4 | `null == undefined` is `true`, but BOTH are `==` to NOTHING else (not even `0`, `""`, or `false`). |
| 5 | `===` never converts types — always prefer it over `==`. |
| 6 | Object vs Object comparisons (`==`/`===`) are ALWAYS reference checks — coercion never applies between two objects. |
| 7 | Object vs Primitive comparisons convert the object to a primitive first (arrays → joined string, objects → `"[object Object]"`), then coercion applies. |
| 8 | `var` is function-scoped and shared across loop iterations — use `let` for per-iteration values. |
| 9 | `var` before declaration = `undefined`. `let`/`const` before declaration = ReferenceError (TDZ). |
| 10 | `typeof null` is `"object"` (a known JS bug). Use `Array.isArray()` to detect arrays. |
| 11 | Avoid `==`; to check NaN, use `Number.isNaN()`, never `value === NaN`. |
| 12 | Event loop order: Synchronous code → Microtasks (Promises) → Macrotasks (setTimeout). |
| 13 | Arrow functions don't have their own `this` — they inherit it from where they're defined. |
| 14 | Objects/arrays are copied by REFERENCE; primitives are copied by VALUE. |
| 15 | Only 6 falsy values: `0`, `""`, `null`, `undefined`, `NaN`, `false`. Everything else is truthy. |
| 16 | `x++` uses the OLD value then increments; `++x` increments FIRST then uses the new value. |
| 17 | String comparisons are character-by-character, NOT numeric — even for number-like strings. |
| 18 | Default parameters trigger ONLY for `undefined`, not `null`/`0`/`""`. |
| 19 | Spread (`...`) creates a SHALLOW copy — nested objects/arrays are still shared. |
| 20 | `&&` returns the first falsy value (or last value); `||` returns the first truthy value (or last value). |
| 21 | An `async` function always returns a Promise, even if you `return` a plain value. |
| 22 | `delete` on array elements leaves holes — use `splice()` or `filter()` instead. |
| 23 | `.sort()` without a comparator sorts as STRINGS — always pass `(a, b) => a - b` for numbers. |
| 24 | `finally` always runs, no matter what happens in `try`/`catch`. |
| 25 | `this` depends on HOW a function is called — detaching a method from its object loses `this`. |

---

## 💡 General Strategy for Output-Based Questions

1. **Read the code top to bottom first** — don't jump around.
2. **Identify if async code is involved** (Promises, `setTimeout`, `async/await`) — if so, separate the SYNCHRONOUS lines from the ASYNCHRONOUS ones and figure out their order using the event loop rules (7.1).
3. **Track variable values step-by-step** — write down each variable's value as it changes, especially with `++`/`--` or reassignments.
4. **Watch for type coercion** — anytime `+`, `-`, `==`, or comparison operators mix different types (string/number/boolean/object).
5. **Check scope** — is the variable `var`, `let`, or `const`? Is it inside a loop, function, or block?
6. **When in doubt, say your reasoning out loud** — even if you get the final answer wrong, interviewers give credit for correct REASONING and approach.

---

**Good luck — these questions become easy with practice! 🎉**

---

# 🚀 Additional High-Value Interview Questions (Often Asked for 3–5 Years Experience)

## Event Loop Priority Challenge

```javascript
console.log("A");

setTimeout(() => console.log("B"), 0);

queueMicrotask(() => console.log("C"));

Promise.resolve().then(() => console.log("D"));

console.log("E");
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
A
E
C
D
B
```

</details>

🔑 Rule: Synchronous → Microtasks (queueMicrotask/Promise) → Macrotasks (setTimeout).

---

## Promise.all vs Promise.allSettled

```javascript
Promise.all([
  Promise.resolve(1),
  Promise.reject("Error"),
  Promise.resolve(3)
])
.then(console.log)
.catch(console.log);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
Error
```

</details>

🔑 Rule: Promise.all fails fast if any promise rejects.

---

## Optional Chaining

```javascript
const user = {};

console.log(user.profile?.name);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
undefined
```

</details>

🔑 Rule: Optional chaining prevents runtime errors on null/undefined paths.

---

## Destructuring Default Values

```javascript
const { name = "Guest" } = {};
console.log(name);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
Guest
```

</details>

🔑 Rule: Defaults apply only when the property value is undefined.


---

# 🔥 Additional Pure Output-Based Questions (Senior Frontend Focus)

## What is the Output?

```javascript
console.log([] == ![]);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
true
```

</details>

---

```javascript
console.log(typeof NaN);
console.log(typeof null);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
number
object
```

</details>

---

```javascript
let a = { x: 1 };
let b = a;

a = { x: 2 };

console.log(a.x);
console.log(b.x);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
2
1
```

</details>

---

```javascript
const obj = {
  name: "JS",
  getName() {
    return this.name;
  }
};

const fn = obj.getName;

console.log(fn());
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
undefined
```

</details>

---

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
    return Promise.resolve();
  })
  .then(() => console.log(2));

console.log(3);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
3
1
2
```

</details>

---

```javascript
console.log(1);

setTimeout(() => console.log(2));

Promise.resolve().then(() => {
  console.log(3);
  setTimeout(() => console.log(4));
});

Promise.resolve().then(() => console.log(5));

console.log(6);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
1
6
3
5
2
4
```

</details>

---

```javascript
const arr = [1,2,3];

arr.length = 1;

console.log(arr);
console.log(arr[2]);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
[1]
undefined
```

</details>

---

```javascript
console.log(0 ?? "Hello");
console.log("" ?? "World");
console.log(null ?? "JS");
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
0

JS
```

</details>

---

```javascript
const a = [1,2];
const b = [1,2];

console.log(a == b);
console.log(a === b);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
false
false
```

</details>

---

```javascript
for (var i = 0; i < 3; i++) {
  Promise.resolve().then(() => console.log(i));
}
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
3
3
3
```

</details>

---

```javascript
for (let i = 0; i < 3; i++) {
  Promise.resolve().then(() => console.log(i));
}
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
0
1
2
```

</details>

---

```javascript
const user = {
  name: "Iqbal",
  greet() {
    console.log(this.name);
  }
};

setTimeout(user.greet, 0);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
undefined
```

</details>

---

```javascript
console.log([..."hello"]);
```

<details>
<summary><strong>👀 View Output</strong></summary>

```text
['h','e','l','l','o']
```

</details>

This file covers the **"What will this code print?"** style questions — one of the most common formats in JavaScript interviews. These questions test whether you truly *understand* how JavaScript works under the hood, not just whether you've memorized definitions.

> 💡 **How to use this file**
> - For each question, read the code FIRST and try to guess the output yourself.
> - Then check the actual output and explanation below it.
> - Pay special attention to the **🔑 Thumb Rule** — it's the "shortcut" that helps you solve similar questions instantly, even if you've never seen that exact code before.
> - The **🔄 Variation** sections show slightly changed versions of the same code — interviewers LOVE to tweak one line and ask "now what?"

---

## 📂 Category 1: Data Types, Type Coercion & Operators (+, -, ==, ===)

This is the **most important category** — almost every JS interview has at least 2-3 questions from here. The core idea: JavaScript tries to be "helpful" by automatically converting (coercing) values from one type to another, and this leads to some surprising results.

### 1.1 The `+` operator with different data types

```javascript
console.log(1 + 1);        // ?
console.log("1" + 1);      // ?
console.log(1 + "1");      // ?
console.log("1" + "1");    // ?
console.log(1 + 2 + "3");  // ?
console.log("1" + 2 + 3);  // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
2
11
11
11
33
123
```

</details>

**Explanation:**
- `1 + 1` → both numbers → normal math → `2`
- `"1" + 1` and `1 + "1"` → one side is a string → the NUMBER is converted to a string, and they're joined together → `"11"`
- `"1" + "1"` → both strings → joined together → `"11"`
- `1 + 2 + "3"` → evaluated LEFT to RIGHT: `1 + 2 = 3` (both numbers, normal math), then `3 + "3" = "33"` (now a string appears, so it becomes concatenation)
- `"1" + 2 + 3` → `"1" + 2 = "12"` (string appears immediately), then `"12" + 3 = "123"`

🔑 **Thumb Rule:** The `+` operator works LEFT to RIGHT. **The moment a string appears anywhere in the chain, everything from that point onward becomes string concatenation** — but operations done BEFORE that point (if both sides were numbers) remain normal math.

---

### 1.2 The `+` operator with booleans, null, and undefined

```javascript
console.log(true + true);   // ?
console.log(true + 1);      // ?
console.log(false + 1);     // ?
console.log(null + 1);      // ?
console.log(undefined + 1); // ?
console.log(null + undefined); // ?
console.log(null + null);   // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
2
2
1
1
NaN
NaN
0
```

</details>

**Explanation:**
- `true` converts to `1`, `false` converts to `0` when used in math. So `true + true = 1 + 1 = 2`, `true + 1 = 1 + 1 = 2`, `false + 1 = 0 + 1 = 1`.
- `null` converts to `0` in math: `null + 1 = 0 + 1 = 1`. So `null + null = 0 + 0 = 0`.
- `undefined` converts to `NaN` in math: `undefined + 1 = NaN`. Anything combined with `NaN` using `+` (as numbers) results in `NaN`. So `null + undefined` → `0 + NaN = NaN`.

🔑 **Thumb Rule:** For arithmetic, `true → 1`, `false → 0`, `null → 0`, but `undefined → NaN`. Remember: **`null` behaves like `0`, but `undefined` "poisons" any math operation into `NaN`.**

---

### 1.3 The `-` (subtraction) operator with different types

```javascript
console.log("5" - 2);      // ?
console.log("5" - "2");    // ?
console.log("5" - "a");    // ?
console.log(true - 1);     // ?
console.log(false - 1);    // ?
console.log("10" - true);  // ?
console.log(null - 1);     // ?
console.log(undefined - 1);// ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
3
3
NaN
0
-1
9
-1
NaN
```

</details>

**Explanation:**
Unlike `+`, the `-` operator has NO "string concatenation" mode — it ALWAYS tries to convert both sides to numbers and subtract.
- `"5" - 2` → `"5"` becomes `5` → `5 - 2 = 3`
- `"5" - "2"` → both convert to numbers → `5 - 2 = 3`
- `"5" - "a"` → `"a"` cannot be converted to a number → `NaN`
- `true - 1` → `1 - 1 = 0`; `false - 1` → `0 - 1 = -1`
- `"10" - true` → `10 - 1 = 9`
- `null - 1` → `0 - 1 = -1`
- `undefined - 1` → `NaN - 1 = NaN`

🔑 **Thumb Rule:** `+` is special because it ALSO means "string concatenation." Every OTHER math operator (`-`, `*`, `/`, `%`) has no such special case — they ALWAYS convert both operands to numbers first.

---

### 1.4 `==` (loose equality) with primitives — the classic gotchas

```javascript
console.log(0 == "0");          // ?
console.log(0 == "");           // ?
console.log(0 == false);        // ?
console.log("" == false);       // ?
console.log("" == "0");         // ?
console.log(" " == 0);          // ?
console.log(null == undefined); // ?
console.log(null == 0);         // ?
console.log(undefined == 0);    // ?
console.log(NaN == NaN);        // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
true
true
true
true
false
true
true
false
false
false
```

</details>

**Explanation:**
`==` converts both sides to a common type before comparing. Walking through the tricky ones:
- `0 == "0"` → `"0"` becomes `0` → `true`
- `0 == ""` → `""` becomes `0` → `true`
- `0 == false` → `false` becomes `0` → `true`
- `"" == false` → both become `0`/falsy-numeric → `true`
- `"" == "0"` → BOTH are strings, so NO coercion happens, and `""` is literally not the same text as `"0"` → `false`
- `" " == 0` → a whitespace-only string converts to `0` when coerced to a number → `0 == 0` → `true`
- `null == undefined` → **special case**: `null` and `undefined` are loosely equal to EACH OTHER and to NOTHING else → `true`
- `null == 0` → `false` — this surprises many people! `null` does NOT convert to `0` for `==` comparisons (even though it does for `+`/`-`). `null` is only `==` to `undefined` and itself.
- `undefined == 0` → `false` — same reason, `undefined` is also only `==` to `null` and itself.
- `NaN == NaN` → `false` — `NaN` is never equal to anything, including itself, by definition.

🔑 **Thumb Rule:** `null` and `undefined` are `==` to EACH OTHER and to NOTHING ELSE (not even `0`, `false`, or `""`) — this is a special hardcoded rule in JavaScript, completely separate from normal coercion rules.

---

### 1.5 `===` (strict equality) — no coercion at all

```javascript
console.log(0 === "0");          // ?
console.log(0 === false);        // ?
console.log(null === undefined); // ?
console.log(NaN === NaN);        // ?
console.log(1 === 1.0);          // ?
console.log("abc" === "abc");    // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
false
false
true
true
```

</details>

**Explanation:**
`===` checks BOTH the value AND the type — if the types differ, it's `false` immediately, no conversion attempted.
- `0 === "0"` → number vs string → `false`
- `0 === false` → number vs boolean → `false`
- `null === undefined` → different types → `false`
- `NaN === NaN` → `false` (special rule, same as `==`)
- `1 === 1.0` → `true` — JavaScript has only ONE number type, so `1` and `1.0` are literally the same value.
- `"abc" === "abc"` → same type, same value → `true`

🔑 **Thumb Rule:** `===` = "same type AND same value." If you're ever unsure about `==` behavior, just remember: `===` never does conversions, so it's predictable. **Always prefer `===` in real code.**

---

### 1.6 `==` and `===` with arrays and objects

```javascript
console.log([] == []);   // ?
console.log([] === []);  // ?
console.log([] == {});   // ?
console.log([] == ![]);  // ?
console.log([] + []);    // ?
console.log([] + {});    // ?
console.log({} + []);    // ?  (run this in browser console vs Node — tricky!)
console.log([1,2] + [3,4]); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
false
true
""
[object Object]
[object Object]
1,23,4
```

</details>

**Explanation:**
- `[] == []` and `[] === []` → both `false`. When BOTH sides of `==`/`===` are objects (arrays count as objects), there is NO coercion — it's purely a reference check. Two different `[]` literals create two different array objects in memory.
- `[] == {}` → `false` — same reason, two different objects, no coercion between two objects.
- `[] == ![]` → `true` 😱. Here's the trick:
  1. `![]` is calculated first. `[]` is a truthy value (all objects are truthy), so `![]` = `false`.
  2. Now the comparison is `[] == false`.
  3. NOW one side (`false`) is a primitive, so coercion kicks in: `[]` converts to a primitive → `""` (empty string), and `false` converts to `0`.
  4. `"" == 0` → `""` converts to `0` → `0 == 0` → `true`.
- `[] + []` → `+` converts both arrays to primitives (strings) → `"" + "" = ""`.
- `[] + {}` → `"" + "[object Object]" = "[object Object]"`.
- `{} + []` → ⚠️ This one depends on CONTEXT! Inside `console.log({} + [])`, `{}` is treated as an OBJECT (since it's an expression/argument), so it behaves like `[] + {}` above → `"[object Object]"`. But if written as a STANDALONE statement on its own line (`{} + []` at the start of a line), JavaScript may interpret `{}` as an empty BLOCK statement, and `+[]` as a separate expression that evaluates to `0`. This is a famous "it depends how you write it" trap.
- `[1,2] + [3,4]` → both arrays convert to strings: `"1,2" + "3,4" = "1,23,4"`.

🔑 **Thumb Rule:** 
- **Object vs Object** (for `==` or `===`) → always reference comparison, coercion NEVER happens.
- **Object vs Primitive** → the object gets converted to a primitive (arrays → comma-joined string, plain objects → `"[object Object]"`), THEN normal coercion rules apply.
- `{} + []` is famous specifically because of the ambiguity between "block statement" vs "object literal" — always wrap in parentheses `({} + [])` if you genuinely need this, but more importantly: **never write code like this in production!**

---

### 1.7 Quick-Reference Table: Common Coercion Results

| Expression | Result | Why |
|---|---|---|
| `1 + "2"` | `"12"` | number → string, concatenation |
| `"5" - 1` | `4` | string → number, subtraction |
| `"5" * "2"` | `10` | both strings → numbers |
| `true + true` | `2` | `true` → `1` |
| `"" + 1` | `"1"` | number → string |
| `null + 1` | `1` | `null` → `0` |
| `undefined + 1` | `NaN` | `undefined` → `NaN` |
| `0 == false` | `true` | both → `0` |
| `0 === false` | `false` | different types |
| `null == undefined` | `true` | special rule |
| `null === undefined` | `false` | different types |
| `NaN == NaN` | `false` | NaN ≠ NaN, ever |
| `[] == []` | `false` | different references |
| `[1] == "1"` | `true` | array → `"1"`, then `"1" == "1"` |

---

## 📂 Category 2: Hoisting & Scope (var, let, const)

### 2.1 Hoisting with var

```javascript
console.log(a);
var a = 5;
console.log(a);
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
undefined
5
```

</details>

**Explanation:**
JavaScript "hoists" the DECLARATION of `var a` to the top of the scope, but NOT the assignment (`= 5`). So at the first `console.log`, `a` exists but has no value yet (`undefined`). After the assignment line runs, `a` becomes `5`.

🔑 **Thumb Rule:** With `var`, think of it as: *"the variable name moves to the top, but the value stays where it is."*

---

### 2.2 Hoisting with let (Temporal Dead Zone)

```javascript
console.log(b);
let b = 5;
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
ReferenceError: Cannot access 'b' before initialization
```

</details>

**Explanation:**
`let` (and `const`) are also hoisted, but they are NOT usable before their declaration line. This gap between the start of the scope and the declaration line is called the **Temporal Dead Zone (TDZ)**. Accessing the variable during this zone throws an error.

🔑 **Thumb Rule:** `var` before declaration → `undefined` (no error). `let`/`const` before declaration → **ReferenceError** (TDZ).

---

### 2.3 Function hoisting vs variable hoisting conflict

```javascript
var greet = "Hello";

function greet() {
  return "Hi";
}

console.log(typeof greet); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
string
```

</details>

**Explanation:**
Both function declarations AND `var` declarations are hoisted, but function declarations are hoisted "more strongly" — they're hoisted WITH their full definition. However, the code then runs top-to-bottom, and `var greet = "Hello"` executes AFTER the function declaration is hoisted, OVERWRITING `greet` with the string `"Hello"`. So by the time `console.log` runs, `greet` is a string.

🔑 **Thumb Rule:** When a `var` and a `function` have the SAME name, the function declaration is hoisted first, but any LATER assignment (even with `var`) will overwrite it once the code actually executes in order.

---

### 2.4 Global var vs window object

```javascript
var name = "Global";
let age = 25;

console.log(window.name); // ? (in a browser environment)
console.log(window.age);  // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Global
undefined
```

</details>

**Explanation:**
In browsers, variables declared with `var` at the top level (global scope) become properties of the `window` object. Variables declared with `let` or `const` do NOT become properties of `window` — they exist in a separate "script scope," even though they're still globally accessible by name.

🔑 **Thumb Rule:** `var` at global scope pollutes the global `window` object. `let`/`const` at global scope do not — this is one reason `let`/`const` are preferred (cleaner global scope, fewer naming collisions).

---

## 📂 Category 3: Loops, Closures & setTimeout

### 3.1 var inside a loop with setTimeout

```javascript
for (var i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
4
4
4
```

</details>

**Explanation:**
`var` does NOT create a new variable for each loop iteration — there is only ONE `i` shared across all iterations, stored in the function/global scope. By the time `setTimeout` actually runs (after 1 second), the loop has already finished completely, and `i` has become `4` (the value that made the loop condition `false` and exit).

🔑 **Thumb Rule:** `var` is function-scoped, NOT block-scoped. So loops using `var` share a single variable across all iterations — by the time any delayed code runs, the loop has already finished and the variable holds its FINAL value.

---

### 3.2 let inside a loop with setTimeout

```javascript
for (let i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
2
3
```

</details>

**Explanation:**
Unlike `var`, `let` is block-scoped — JavaScript creates a NEW `i` for EACH iteration of the loop. So each `setTimeout` callback "remembers" its own separate copy of `i` (this is a closure).

🔑 **Thumb Rule:** If you ever see `var` causing unexpected "same value repeated" output in a loop, the fix is almost always: **replace `var` with `let`**.

🔄 **Variation — fixing `var` WITHOUT switching to `let` (using an IIFE):**
```javascript
for (var i = 1; i <= 3; i++) {
  (function(j) {
    setTimeout(() => console.log(j), 1000);
  })(i);
}
// Output: 1, 2, 3
```
This works because the IIFE creates a new `j` for each iteration — an old-school trick used before `let` existed.

🔄 **Variation — same problem, written slightly differently:**
```javascript
function printNumbers() {
  for (var i = 0; i < 3; i++) {
    setTimeout(function() {
      console.log("Number:", i);
    }, i * 1000);
  }
}
printNumbers();
// Output: Number: 3 / Number: 3 / Number: 3
```
Even though each `setTimeout` fires at a DIFFERENT time (0s, 1s, 2s), by the time ANY of them run, the loop has already finished and `i` is `3`. The fix is the same: use `let`, or pass `i` as a third argument to `setTimeout` (`setTimeout(fn, i*1000, i)`).

---

### 3.3 Closures inside a loop returning functions

```javascript
function createFunctions() {
  let funcs = [];
  for (let i = 0; i < 3; i++) {
    funcs.push(() => console.log(i));
  }
  return funcs;
}

const myFuncs = createFunctions();
myFuncs[0](); // ?
myFuncs[1](); // ?
myFuncs[2](); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
0
1
2
```

</details>

**Explanation:**
Because `let` creates a new `i` for each loop iteration, each arrow function in the array "closes over" (remembers) its OWN separate `i`. If `var` were used instead, all three would print `3`.

🔑 **Thumb Rule:** Same root cause as 3.1/3.2 — `let` in loops = separate variable per iteration = each closure remembers its own value.

---

### 3.4 A simple counter closure

```javascript
function outerFunction() {
  let count = 0;
  return function innerFunction() {
    count++;
    console.log(count);
  };
}

const counter = outerFunction();
counter(); // ?
counter(); // ?
counter(); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
2
3
```

</details>

**Explanation:**
Even though `outerFunction()` has already finished executing, `innerFunction` still "remembers" the `count` variable from its parent scope. Each call to `counter()` updates the SAME `count` variable, because it's the same closure being reused.

🔑 **Thumb Rule:** A closure "remembers" the variables from where it was CREATED, not where it's CALLED — and that memory persists across multiple calls if you keep reusing the same returned function.

---

## 📂 Category 4: `this`, Functions & Classes

### 4.1 `this` inside a regular function vs arrow function

```javascript
const obj = {
  name: "Alex",
  regular: function() {
    console.log("Regular:", this.name);
  },
  arrow: () => {
    console.log("Arrow:", this.name);
  }
};

obj.regular(); // ?
obj.arrow();   // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Regular: Alex
Arrow: undefined
```

</details>

**Explanation:**
- `regular` is called as `obj.regular()`, so `this` refers to `obj`, and `this.name` is `"Alex"`.
- `arrow` is an arrow function, which does NOT get its own `this`. It uses `this` from where it was DEFINED (the outer/global scope), where `name` doesn't exist — so `this.name` is `undefined`.

🔑 **Thumb Rule:** Never use arrow functions for object methods if you need `this` to refer to that object. Use regular `function` syntax for methods.

---

### 4.2 Class fields and detached `this`

```javascript
class Counter {
  count = 0;

  increment() {
    this.count++;
    console.log(this.count);
  }
}

const counter = new Counter();
const incrementFn = counter.increment;

counter.increment();  // ?
incrementFn();        // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
TypeError: Cannot read properties of undefined (reading 'count')
```

</details>

**Explanation:**
`counter.increment()` works fine because `this` refers to `counter`. But once you extract the method into a standalone variable (`incrementFn`) and call it WITHOUT the object (`incrementFn()`), `this` is no longer bound to `counter` — it becomes `undefined` (in strict mode/classes), causing an error when trying to access `this.count`.

🔑 **Thumb Rule:** `this` depends on HOW a function is CALLED, not where it's defined. If you "detach" a method from its object and call it standalone, `this` is lost. Fix using `.bind()`, arrow functions for class fields, or by always calling it as `obj.method()`.

---

### 4.3 Function returns vs no return

```javascript
function noReturn() {
  console.log("Hello");
}

let result = noReturn();
console.log(result); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Hello
undefined
```

</details>

**Explanation:**
The function `noReturn` doesn't have a `return` statement, so it implicitly returns `undefined`. The `console.log("Hello")` still runs (that's its side effect), but the VALUE returned and stored in `result` is `undefined`.

🔑 **Thumb Rule:** A function with no explicit `return` ALWAYS returns `undefined` — even if it does other things like printing to the console.

---

### 4.4 Default parameters and undefined

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}`);
}

greet();           // ?
greet(undefined);  // ?
greet(null);       // ?
greet("");         // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Hello, Guest
Hello, Guest
Hello, null
Hello,
```

</details>

**Explanation:**
Default parameters are used ONLY when the argument is `undefined` (or not passed at all). Passing `null` or `""` (empty string) are still "real values" — they don't trigger the default.

🔑 **Thumb Rule:** Default parameters only kick in for `undefined`, not for `null`, `0`, `""`, or `false`.

---

### 4.5 IIFE (Immediately Invoked Function Expression)

```javascript
var result = (function() {
  var x = 10;
  return x * 2;
})();

console.log(result);   // ?
console.log(typeof x); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
20
undefined
```

</details>

**Explanation:**
An IIFE runs immediately after it's defined. The variable `x` is local to the IIFE's scope and doesn't leak outside — so `result` gets `20` (the returned value), but `x` doesn't exist in the outer scope. Using `typeof` on a completely undeclared variable returns `"undefined"` instead of throwing an error (this is a special safety behavior of `typeof`).

🔑 **Thumb Rule:** IIFEs create a private scope — variables declared inside never leak out. Also remember: `typeof someUndeclaredVariable` is safe and returns `"undefined"`, but actually USING that variable (e.g., `console.log(someUndeclaredVariable)`) throws a `ReferenceError`.

---

## 📂 Category 5: Truthy/Falsy & Logical Operators

### 5.1 Truthy and Falsy values

```javascript
const values = [0, "", null, undefined, NaN, false, "0", [], {}];

values.forEach(val => {
  console.log(Boolean(val));
});
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
false
false
false
false
true
true
true
```

</details>

**Explanation:**
There are only **6 falsy values** in JavaScript: `0`, `""` (empty string), `null`, `undefined`, `NaN`, and `false`. EVERYTHING else is truthy — including `"0"` (a non-empty string!), empty arrays `[]`, and empty objects `{}` (because they are objects, and all objects are truthy).

🔑 **Thumb Rule:** Memorize the 6 falsy values: `0`, `""`, `null`, `undefined`, `NaN`, `false`. If a value isn't on this list, it's truthy — even `"0"`, `[]`, and `{}`.

---

### 5.2 Short-circuit evaluation with && and ||

```javascript
console.log(0 || "default");      // ?
console.log("" || "fallback");     // ?
console.log("Hi" && "Bye");        // ?
console.log(null && "Never runs"); // ?
console.log(1 && 0 && "end");      // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
default
fallback
Bye
null
0
```

</details>

**Explanation:**
- `||` (OR) returns the FIRST truthy value it finds (or the last value if all are falsy). `0 || "default"` → `0` is falsy, so it returns `"default"`.
- `&&` (AND) returns the FIRST falsy value it finds (or the last value if all are truthy). `"Hi" && "Bye"` → both truthy, so it returns the LAST one, `"Bye"`.
- `null && "Never runs"` → `null` is falsy, so `&&` stops immediately and returns `null` — `"Never runs"` is never evaluated.
- `1 && 0 && "end"` → `1` is truthy (continue), `0` is falsy (STOP here and return `0`).

🔑 **Thumb Rule:** `||` returns the first TRUTHY value (or the last value). `&&` returns the first FALSY value (or the last value). Both operators return an actual VALUE, not just `true`/`false`.

---

### 5.3 NaN comparisons

```javascript
let result = 10 / "abc";
console.log(result);                  // ?
console.log(result === NaN);          // ?
console.log(Number.isNaN(result));    // ?
console.log(isNaN("hello"));          // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
NaN
false
true
true
```

</details>

**Explanation:**
Dividing a number by a non-numeric string produces `NaN` ("Not a Number"). But `NaN === NaN` is always `false` — NaN is the only value in JavaScript that is NOT equal to itself. The correct way to check is `Number.isNaN()`.

`isNaN("hello")` returns `true` too, but `isNaN()` (the global function) first tries to CONVERT the value to a number before checking — so it can give misleading results for non-number inputs. `Number.isNaN()` is stricter and safer.

🔑 **Thumb Rule:** To check if a value is `NaN`, always use `Number.isNaN(value)`, never `value === NaN` or the loose global `isNaN()`.

---

### 5.4 Increment operator confusion

```javascript
let x = 5;
console.log(x++); // ?
console.log(x);   // ?
console.log(++x); // ?
console.log(x);   // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
5
6
7
7
```

</details>

**Explanation:**
- `x++` (post-increment) returns the CURRENT value (`5`), THEN increments `x` to `6`.
- `console.log(x)` now shows `6` (already incremented).
- `++x` (pre-increment) increments `x` to `7` FIRST, THEN returns the new value (`7`).
- `console.log(x)` confirms `x` is `7`.

🔑 **Thumb Rule:** "Post" (`x++`) = use OLD value, then increase. "Pre" (`++x`) = increase FIRST, then use NEW value. Say it as: *"post means the increment happens AFTER it's used."*

🔄 **Variation — combining both in one expression:**
```javascript
let a = 1;
let b = a++ + ++a;

console.log(a); // 3
console.log(b); // 4
```
Step by step: `a++` returns `1` (then `a` becomes `2`). `++a` makes `a` become `3` first, then returns `3`. So `b = 1 + 3 = 4`, and final `a = 3`.

---

### 5.5 Nested ternary operators

```javascript
let age = 20;

let category = age < 13 ? "Child" 
              : age < 18 ? "Teenager" 
              : age < 60 ? "Adult" 
              : "Senior";

console.log(category); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Adult
```

</details>

**Explanation:**
Ternary operators (`condition ? valueIfTrue : valueIfFalse`) can be chained. JavaScript checks each condition in order:
- `age < 13`? `20 < 13` → false
- `age < 18`? `20 < 18` → false
- `age < 60`? `20 < 60` → **true** → returns `"Adult"`

The remaining conditions are never checked once a `true` is found.

🔑 **Thumb Rule:** Nested ternaries work like an `if / else if / else if / else` chain. Read them top to bottom, and the FIRST true condition "wins." (Deeply nested ternaries are discouraged in production code for readability — `if/else` or `switch` is often clearer.)

---

## 📂 Category 6: Arrays & Objects

### 6.1 Array vs Object reference (copy by reference)

```javascript
let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2.push(4);

console.log(arr1); // ?
console.log(arr2); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
[1, 2, 3, 4]
[1, 2, 3, 4]
```

</details>

**Explanation:**
Arrays and objects in JavaScript are stored "by reference" — `arr2 = arr1` doesn't create a NEW array, it just makes `arr2` point to the SAME array in memory as `arr1`. So changing `arr2` also changes `arr1`, because they're literally the same object.

🔑 **Thumb Rule:** Primitives (numbers, strings, booleans) are copied BY VALUE. Objects and arrays are copied BY REFERENCE. To make a real independent copy, use `[...arr1]`, `{...obj1}`, or a deep clone for nested data.

---

### 6.2 Object comparison

```javascript
const obj1 = { name: "Tom" };
const obj2 = { name: "Tom" };
const obj3 = obj1;

console.log(obj1 == obj2);  // ?
console.log(obj1 === obj2); // ?
console.log(obj1 === obj3); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
false
false
true
```

</details>

**Explanation:**
Objects are compared by REFERENCE (memory address), not by their contents. `obj1` and `obj2` look identical but are two SEPARATE objects in memory — so they are NOT equal. `obj3`, however, points to the EXACT SAME object as `obj1` (`obj3 = obj1`), so they ARE equal.

🔑 **Thumb Rule:** Two objects/arrays are only `===` equal if they are the SAME object in memory (same reference) — never because they "look" the same. To compare contents, you need a deep-equality check (e.g., `JSON.stringify(obj1) === JSON.stringify(obj2)` for simple cases, or a library like Lodash's `isEqual` for complex cases).

---

### 6.3 Spread operator — shallow copy gotcha

```javascript
let str = "abc";
let chars = [...str];
console.log(chars); // ?

let arr = [1, [2, 3], 4];
let copy = [...arr];
copy[1].push(99);
console.log(arr); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
['a', 'b', 'c']
[1, [2, 3, 99], 4]
```

</details>

**Explanation:**
- `[...str]` spreads a string into an array of its individual characters.
- `[...arr]` creates a **shallow copy** — the top-level array is new, but nested arrays/objects INSIDE it still point to the SAME memory as the original. So modifying `copy[1]` (the nested array) also affects `arr[1]`.

🔑 **Thumb Rule:** Spread (`...`) only copies ONE level deep. Nested objects/arrays are still shared between the original and the copy.

---

### 6.4 Object key order and duplicate keys

```javascript
const obj = {
  a: 1,
  b: 2,
  a: 3
};

console.log(obj);              // ?
console.log(Object.keys(obj)); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
{ a: 3, b: 2 }
['a', 'b']
```

</details>

**Explanation:**
If an object has duplicate keys, JavaScript silently keeps only the LAST value for that key — no error is thrown. So `a: 1` is overwritten by `a: 3`.

🔑 **Thumb Rule:** Duplicate object keys = last one wins, with no warning. Always double-check object literals for accidental duplicate keys (easy to miss in large objects).

---

### 6.5 Array.length after deleting an element

```javascript
let arr = [1, 2, 3, 4, 5];
delete arr[2];

console.log(arr);        // ?
console.log(arr.length); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
[1, 2, <1 empty item>, 4, 5]
5
```

</details>

**Explanation:**
`delete` removes the VALUE at that index but does NOT shift other elements or update `length`. It leaves a "hole" (empty slot) in the array. This is almost never what you want.

🔑 **Thumb Rule:** Never use `delete` on array elements. To properly remove an item, use `splice()` (which shifts elements and updates length) or `filter()` (which creates a new array without that item).

---

### 6.6 Chained array methods

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
  .filter(n => n % 2 === 0)
  .map(n => n * 10)
  .reduce((sum, n) => sum + n, 0);

console.log(result); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
120
```

</details>

**Explanation:**
Step by step:
1. `.filter(n => n % 2 === 0)` → keeps even numbers → `[2, 4, 6]`
2. `.map(n => n * 10)` → multiplies each by 10 → `[20, 40, 60]`
3. `.reduce((sum, n) => sum + n, 0)` → adds them all together → `20 + 40 + 60 = 120`

🔑 **Thumb Rule:** When you see chained array methods, work through them ONE AT A TIME, writing down the resulting array after each step — don't try to do it all in your head at once.

---

### 6.7 Array sort() default behavior

```javascript
const numbers = [10, 1, 21, 2];
console.log(numbers.sort()); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
[1, 10, 2, 21]
```

</details>

**Explanation:**
By default, `.sort()` converts elements to STRINGS and sorts them alphabetically/lexicographically — NOT numerically. So `"10"` comes before `"2"` because `"1"` (first character) is less than `"2"`.

🔑 **Thumb Rule:** ALWAYS pass a comparator function when sorting numbers: `numbers.sort((a, b) => a - b)` for ascending order. Without it, `.sort()` treats everything as strings, which gives wrong results for numbers ≥ 10.

---

### 6.8 typeof a function and typeof an array

```javascript
function sayHi() {}
const arr = [1, 2, 3];
const arrowFn = () => {};

console.log(typeof sayHi);       // ?
console.log(typeof arr);         // ?
console.log(typeof arrowFn);     // ?
console.log(Array.isArray(arr)); // ?
console.log(typeof null);        // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
function
object
function
true
object
```

</details>

**Explanation:**
Functions (whether regular or arrow) have their own special `typeof` result: `"function"`. Arrays, despite being a distinct "thing" conceptually, return `"object"` for `typeof` — you need `Array.isArray()` to specifically check for arrays. `typeof null` is also `"object"` — a famous long-standing bug in JS that can never be fixed now.

🔑 **Thumb Rule:** `typeof` has only 8 possible results: `"undefined"`, `"object"`, `"boolean"`, `"number"`, `"string"`, `"bigint"`, `"symbol"`, and `"function"`. Arrays AND `null` both fall under `"object"` — always double-check with `Array.isArray()` or `=== null` when it matters.

---

### 6.9 String comparison (number-like strings)

```javascript
console.log("apple" < "banana");  // ?
console.log("Apple" < "apple");   // ?
console.log("10" < "9");          // ?
console.log(10 < 9);              // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
true
true
true
false
```

</details>

**Explanation:**
Strings are compared character by character based on their **Unicode/ASCII values**, like a dictionary, but with a twist: uppercase letters have SMALLER character codes than lowercase letters. So `"A"` (65) comes before `"a"` (97), making `"Apple" < "apple"` → `true`.

For `"10" < "9"`, these are STRINGS, so they're compared character by character: `"1"` (code 49) is less than `"9"` (code 57), so `"10" < "9"` is `true` — even though numerically 10 is greater than 9!

🔑 **Thumb Rule:** Comparing strings that LOOK like numbers does NOT do numeric comparison — it compares character-by-character. Always convert to `Number()` first if you want numeric comparison.

---

## 📂 Category 7: Async, Promises & Event Loop

### 7.1 Order of console.log with Promises and setTimeout

```javascript
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
1
4
3
2
```

</details>

**Explanation:**
This tests your understanding of the **Event Loop** and its different queues:
1. `console.log("1")` and `console.log("4")` run immediately — they're part of the "main" synchronous code.
2. `Promise.then()` callbacks go into the **microtask queue**.
3. `setTimeout` callbacks go into the **macrotask (callback) queue**.
4. After the main code finishes, JavaScript ALWAYS empties the **microtask queue completely** before moving to the macrotask queue.

So the order is: synchronous code first (`1`, `4`), then microtasks (`3`), then macrotasks (`2`).

🔑 **Thumb Rule:** **Synchronous code → Microtasks (Promises) → Macrotasks (setTimeout/setInterval).** Promises always "jump the queue" ahead of setTimeout, even with `setTimeout(fn, 0)`.

---

### 7.2 Async function return value

```javascript
async function getValue() {
  return 42;
}

const result = getValue();
console.log(result); // ?
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Promise { 42 }
```

</details>

**Explanation:**
An `async` function ALWAYS returns a Promise — even if you write a plain `return 42`, JavaScript automatically wraps it as `Promise.resolve(42)`. To get the actual value `42`, you'd need to use `.then()` or `await` it inside another `async` function.

🔑 **Thumb Rule:** Calling an `async` function NEVER gives you the "final value" directly — it always gives you a Promise. You must use `.then()` or `await` to unwrap it.

---

### 7.3 Sequential vs parallel await

```javascript
function delay(value, ms) {
  return new Promise(resolve => setTimeout(() => resolve(value), ms));
}

async function sequential() {
  const a = await delay("A", 1000);
  const b = await delay("B", 1000);
  console.log(a, b);
}

sequential(); // How long does this take in total?
```

**Output (timing):**
```
Takes about 2 seconds total, then prints: A B
```

**Explanation:**
Each `await` PAUSES the function until that Promise resolves. Since `b`'s delay doesn't start until AFTER `a` finishes, the two 1-second delays happen one after another — totaling about 2 seconds. This is called **sequential execution**.

🔄 **Variation — running them in PARALLEL instead:**
```javascript
async function parallel() {
  const [a, b] = await Promise.all([delay("A", 1000), delay("B", 1000)]);
  console.log(a, b);
}
parallel(); // Takes only about 1 second total
```
Here, both `delay()` calls START at the same time, and `Promise.all()` waits for BOTH to finish — so the total time is only as long as the SLOWER one (1 second), not the sum.

🔑 **Thumb Rule:** If tasks don't depend on each other's results, start them all FIRST (without `await`), then use `Promise.all()` to wait for all of them — this is much faster than awaiting one-by-one.

---

### 7.4 Try/catch/finally execution order

```javascript
function test() {
  try {
    console.log("Try");
    throw new Error("Oops");
  } catch (err) {
    console.log("Catch:", err.message);
  } finally {
    console.log("Finally");
  }
  console.log("After try-catch");
}

test();
```


<details>
<summary><strong>👀 View Output</strong></summary>

```
Try
Catch: Oops
Finally
After try-catch
```

</details>

**Explanation:**
The `try` block runs first and throws an error. The `catch` block catches it and runs. The `finally` block ALWAYS runs — whether or not an error occurred, and even if there's a `return` inside `try` or `catch`. Code AFTER the try-catch-finally block continues normally (since the error was already caught/handled).

🔑 **Thumb Rule:** `finally` ALWAYS runs — no matter what happens in `try` or `catch` (including `return` statements). It's typically used for cleanup tasks (closing files, hiding loaders, etc.).

---

## 🎯 Master List of Thumb Rules (Quick Recap)

| # | Rule |
|---|---|
| 1 | The `+` operator: once a string appears, everything after becomes string concatenation. |
| 2 | For math, `true→1`, `false→0`, `null→0`, but `undefined→NaN`. |
| 3 | `-`, `*`, `/`, `%` ALWAYS convert both sides to numbers — they have no "concatenation" mode like `+`. |
| 4 | `null == undefined` is `true`, but BOTH are `==` to NOTHING else (not even `0`, `""`, or `false`). |
| 5 | `===` never converts types — always prefer it over `==`. |
| 6 | Object vs Object comparisons (`==`/`===`) are ALWAYS reference checks — coercion never applies between two objects. |
| 7 | Object vs Primitive comparisons convert the object to a primitive first (arrays → joined string, objects → `"[object Object]"`), then coercion applies. |
| 8 | `var` is function-scoped and shared across loop iterations — use `let` for per-iteration values. |
| 9 | `var` before declaration = `undefined`. `let`/`const` before declaration = ReferenceError (TDZ). |
| 10 | `typeof null` is `"object"` (a known JS bug). Use `Array.isArray()` to detect arrays. |
| 11 | Avoid `==`; to check NaN, use `Number.isNaN()`, never `value === NaN`. |
| 12 | Event loop order: Synchronous code → Microtasks (Promises) → Macrotasks (setTimeout). |
| 13 | Arrow functions don't have their own `this` — they inherit it from where they're defined. |
| 14 | Objects/arrays are copied by REFERENCE; primitives are copied by VALUE. |
| 15 | Only 6 falsy values: `0`, `""`, `null`, `undefined`, `NaN`, `false`. Everything else is truthy. |
| 16 | `x++` uses the OLD value then increments; `++x` increments FIRST then uses the new value. |
| 17 | String comparisons are character-by-character, NOT numeric — even for number-like strings. |
| 18 | Default parameters trigger ONLY for `undefined`, not `null`/`0`/`""`. |
| 19 | Spread (`...`) creates a SHALLOW copy — nested objects/arrays are still shared. |
| 20 | `&&` returns the first falsy value (or last value); `||` returns the first truthy value (or last value). |
| 21 | An `async` function always returns a Promise, even if you `return` a plain value. |
| 22 | `delete` on array elements leaves holes — use `splice()` or `filter()` instead. |
| 23 | `.sort()` without a comparator sorts as STRINGS — always pass `(a, b) => a - b` for numbers. |
| 24 | `finally` always runs, no matter what happens in `try`/`catch`. |
| 25 | `this` depends on HOW a function is called — detaching a method from its object loses `this`. |

---

## 💡 General Strategy for Output-Based Questions

1. **Read the code top to bottom first** — don't jump around.
2. **Identify if async code is involved** (Promises, `setTimeout`, `async/await`) — if so, separate the SYNCHRONOUS lines from the ASYNCHRONOUS ones and figure out their order using the event loop rules (7.1).
3. **Track variable values step-by-step** — write down each variable's value as it changes, especially with `++`/`--` or reassignments.
4. **Watch for type coercion** — anytime `+`, `-`, `==`, or comparison operators mix different types (string/number/boolean/object).
5. **Check scope** — is the variable `var`, `let`, or `const`? Is it inside a loop, function, or block?
6. **When in doubt, say your reasoning out loud** — even if you get the final answer wrong, interviewers give credit for correct REASONING and approach.

---

**Good luck — these questions become easy with practice! 🎉**

---
