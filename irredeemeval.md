# Irredeem`eval()`

> «Any person can invent a security system so clever that she or he can’t think of how to break it.»
>    — Schneier's law

===============

- "Wouldn't it be nice if users couldn't just enter a number, but a
  mathematical expression, like `sin(23.4)`?"
- "For this filtering configuration, users should be able to write simple,
  restricted functions"

===============

- You're tempted to use `exec()`/`eval()`, even though you've heard it's
  dangerous.

===============

- Goal: Execute binary called `payload` in this directory.

// import os; os.system('./payload')
```python
def e():
    while True:
        exec(input("> "))
```

// next: use eval instead

===============

"Alright, let's prevent imports."

// __import__('os').system('./payload')
```python
def e():
    while True:
        print(eval(input("> ")))
```

// next: remove builtins

===============

"No more builtins!"

// [x for x in ().__class__.__bases__[0].__subclasses__() if "port" in x.__name__][0].load_module('os').system('./payload')
```python
def e():
    while True:
        print(eval(input("> "), {"__builtins__": {}}))
```


// next: replace dangerous names

===============

"Let's get rid of dangerous names"

// __import__('os').ſystem('./payload')
```python
def e():
    while True:
        code = input("> ")
        if "system" not in code:
            print(eval(code))
        else:
            print("bad user!")
```

// could normalize, then getattr
// next: and we could go on

===============

*...and we could go on*

// at some point I wouldn't know how to break out anymore
// but someone else will.

===============

// infinite loops, intentionally leak memory, ...

Even if you were successful:
- abuse of resources

===============

# Don't try to make `eval` safe for user input.

===============

## What to use instead:

- ast.literal_eval
- json.loads
- custom parsing
- <https://github.com/python-discord/snekbox/>
