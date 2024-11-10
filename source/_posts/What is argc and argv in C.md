---
title: What is argc and argv in C
date: 2024-07-20
categories:
- engineering
- cpp
tags:
- engineering
- cpp
---

When I started to learn C programming language in my freshman year, I was writing `int main(void)` for most of the time. And when I first encountered with `int main(int argc, char *argv[])`, I was totally confused with the arguments `argc` and `argv` .

They are actually pretty simple, lets break down into it.

As their name implicates, they stands for

- argc: argument count, how many arguments are set for the program
- argv: argument vector, the list of arguments, all taken as string

For example, let’s create a C/C++ file

```C++
\#include <iostream>
int main (int argc, char *argv[]) {
    std::cout << argc << "\n";
    for (int i = 0; i < argc; i++) {
        std::cout << argv[i] << " ";
    }
    std::cout << "\n";
    return 0;
}
```

We compile it

```Bash
>>> g++ YourFilename.cpp -o args
```

and run it without argument (I’m using macOS)

```Bash
>>> ./args
1
./a.out
```

we got `argc` as `1` and `argv` as `./a.out`

and if we run the executable with several arguments

```C++
>>> ./args 65535 Hello 114.514
4
./args 65535 Hello 114.514
```

Now you probably know what is happening here, the `argc` is (# of arguments + 1) we specified when running the executable, the +1 is for the name of the program. And the `argv` is the array of all the argument strings, it did not handle different variable types, but only strings. So if you would like to use the argument as integer or other types, you need to convert the string to what you need.
