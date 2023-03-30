# error 

- ISO C90 forbids mixed declarations and code in C
```c
{
    foo();
    int i = 0;
    bar();
}

// to
{
    int i = 0;
    foo();
    bar();
}
```
