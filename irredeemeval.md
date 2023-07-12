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

===============

## Bonus slide: no dunders, no spaces, no builtins

!unsetlocal exec

```python
[*([x.append(((x[0].gi_frame.f_back.f_back.f_globals)for()in((),)))or(x[0])for(x)in[[]]][0])][0]["\x5f\x5fbuiltins\x5f\x5f"].eval('((x.{0}call{0}.{0}globals{0})for(x)in"".{0}class{0}.{0}bases{0}[0].{0}subclasses{0}()if"{0}globals{0}"in(x).{0}call{0}.{0}dir{0}()).{0}next{0}()["sys"].modules["os"]'.format('\x5f\x5f')).system
```

(courtesy of Python Discord's `#esoteric-python` channel)
