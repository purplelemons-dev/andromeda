# andromeda
my programming language

## Overview
i want it to write like Python, but have a few things pulled in from other languages. A nice thing would be if all Python files could be interactively converted to Andromeda code (e.g. `$ andromeda translate foobar.py`).

### Basics
* They are called `( curved brackets )`, `[ square brackets ]`, and `{ curly brackets }`
* File extension: `.and`.
* Uses semicolons and is whitespace-insensitive.
* Most things are done inside of curly brackets.
* No semicolons after curly brackets because i think it looks stupid. otherwise, most lines should have semicolons. 
* It is automatically assumed that a variable is a constant if it is all uppercase, otherwise constants can be declared with `const`.
* Functions, classes, and files can use the keyword `export ${var/func/etc.};` to allow code of the same or higher scope to access that object. E.g.:
```
def fibonacci(until=1000) {
  comptime {
    a, b = 0, 1;
    const test_var = 42;
    export test_var; // export being allowed in comptime block is subject to change because i have a bad feeling about this for some reason.
  }
  while a < until {
    yield a;
    a, b = b, a + b;
  }
}

fibonacci.test_var;    // 42
fibonacci().test_var;  // 42
```
( you'll see what `comptime` is doing here under [ZIG](#zig) )
* Anywhere (i.e. in any file), at any time. Globals can be created or accessed using `global ${name};` or via `__${name}__`. However, if one file or code block tries to create a global variable that was already initialized in a higher scope, then an error is thrown.
* Expressions after keywords (e.g. `while`, `if`, `for`, etc.) do not need to be wrapped in curved brackets. (this is subject to change because I'm not sure how hard it is to get a compiler to play nice with that.
* If no main file is specified in the config file (`.andromeda.toml`), it is assumed to be `./src/__main__.and`.
* Project name is assumed to be the name of the folder containing the config file unless otherwise specified in config.
* Version number is read from config and injected into the project's global variables at compile time.
* I like the way Python imports packages, so it stays.
* All operators are dunder/magic methods and can be modified. E.g.:
```
def foo() {
  42
}
foo.__call__ = (*args,**kwargs) => {
  print("calling foo!");
  foo.__call__(*args,**kwargs);
}
```
* Project structure should look like this:
```
MyProject
├── .andromeda.toml
├── src
│   ├── __main__.and
│   └── SubProject
│       └── __main__.and
└── out
    ├── build
    │   └── v0.0.1
    │       └── MyProject.cand
    └── exec
        └── v0.0.1
            ├── MyProject.exe
            └── MyProject
```

### Javascript
* declaration of anonymous callbacks, e.g.:
```
>>> list(map( (n) => {1+n}, [1,2,3,4,5]))
[2,3,4,5,6]
```
* Python f-strings stay, but we also use javascript's formatted strings, and they use Python formatting:
```js
const test = `we are using ${__version__=}`
```

### Rust <sup>(not affiliated with the Rust Foundation)</sup>
* Scopes can be created anonymously via `{ ... }` and are evaluated immediately.
* The return value of a block of code is the final statement in the block *without* a semicolon.

### Java
* Inline variable declaration from a conditional statement:
```java
if (world.getServer().getOnlinePlayers()[0] instanceof Player player) {
  // `player` was declared inline
}
```
* Comments are done with `//` and `/* ... */`.

### ZIG
* `comptime { ... }` code block will evaluate everything inside it and contents will be hard-coded into build/executable. 
