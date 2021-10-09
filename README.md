## First esoteric Python code

#### Playing with built-in functions

The point of this code is to abuse Python's flexibility and the fact that booleans are basically integers.



#### Explanations:

The code is supposed to print `Hello world !`.

It is composed of "tiny bricks": the boolean built-in type, called with

``` python
__import__("\x5f\x5f\x6d\x61\x69\x6e\x5f\x5f").__builtins__.__getattribute__("\x62\x6f\x6f\x6c")

# Can be used to create 0s and 1s:
__import__("\x5f\x5f\x6d\x61\x69\x6e\x5f\x5f").__builtins__.__getattribute__("\x62\x6f\x6f\x6c")(" ") # True
__import__("\x5f\x5f\x6d\x61\x69\x6e\x5f\x5f").__builtins__.__getattribute__("\x62\x6f\x6f\x6c")("")  # False
```

Here, the Unicode `\x5f\x5f\x6d\x61\x69\x6e\x5f\x5f` translates to `__main__`,`\x62\x6f\x6f\x6c` translates to `bool` and `\x20` translates to  (space).

Although this is the way it is coded in the file, we will write `__main_`, `bool` and ` ` for the rest of this text for clarity.



Every letter gets translated from its hex code. Let's use an example, the character `b`, which is `\x62`:

The idea is that 62 is the same as `int("6" + "2")`. Thus we use a lambda function to concatenate a tuple that contain "6" and "2" to "62". Those numbers are themselves generated using our "bricks", boolean values on which we apply two operations: the **bitwise left shift** and the **addition**.

Here, the number `6` is generated that way:

```python
# (1 << (1 << 1)) + (1 << 1) == (1 << 2) + 2 == 4 + 2 == 6
__import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
    __import__("__main__").__builtins__.__getattribute__("bool")(" ").__add__(
        __import__("__main__").__builtins__.__getattribute__("bool")(" ")
    )
).__add__(
    __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
        __import__("__main__").__builtins__.__getattribute__("bool")(" ")
    )
)
```

and the number `2`:

```python
# 1 << 2
__import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
	__import__("__main__").__builtins__.__getattribute__("bool")(" ")
)
```



This is plugged into an anonymous lambda function that converts them to strings, concatenates them, reconvert them to int then use the built-in `chr()` to make (again) convert it to the corresponding Unicode character. In the end, the lambda function does that: `chr(int("6" + "2", 16))`.



Know that some letter's hex code is made with the alphabetic part of the hex table. In that case, you can simply reapply the process to the letter. For example `l` = `\x6c`:

```python
# l
(
    lambda _, __:  chr(int(str(_) + str(__), 16))
)(
*(
    lambda: (
        
        # 6
        __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
            __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
                __import__("__main__").__builtins__.__getattribute__("bool")(" ")
            )
        ).__add__(
            __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
                __import__("__main__").__builtins__.__getattribute__("bool")(" ")
            )
        ),
        
        # c, which is \x63
        (
            lambda _, __:  chr(int(str(_) + str(__), 16))
        )(
            *(
                lambda: (
                    
                    # 6
                    __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
                        __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
                            __import__("__main__").__builtins__.__getattribute__("bool")(" ")
                        )
                    ).__add__(
                        __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
                            __import__("__main__").__builtins__.__getattribute__("bool")(" ")
                        )
                    ),
                    
                    # 3
                    __import__("__main__").__builtins__.__getattribute__("bool")(" ").__add__(
                        __import__("__main__").__builtins__.__getattribute__("bool")(" ").__lshift__(
                            __import__("__main__").__builtins__.__getattribute__("bool")(" ")
                        )
                    )
                )
            )()
        )
    )
)()
)
```



Apply that to all characters of the word you want to write.

Now wrap everything into a list, and `''.join()` it. Print the result.
