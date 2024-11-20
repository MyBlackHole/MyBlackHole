# barrier

编译器优化会导致指令乱序执行，这会导致程序的行为与预期不符。
为了解决这个问题，编译器和运行时系统都会提供一些指令来保证指令的顺序执行。

memory barrier 是一种保证指令顺序的机制，它可以确保指令的顺序按照程序的顺序执行。

barrier就象是c代码中的一个栅栏，将代码逻辑分成两段，barrier之前的代码和barrier之后的代码在经过编译器编译后顺序不能乱掉
也就是说，barrier之后的c代码对应的汇编，不能跑到barrier之前去

barrier 可以分为两类：
- 编译器插入的 barrier：编译器在生成代码时，会插入一些指令来实现 barrier。
- 运行时插入的 barrier：运行时系统在执行指令时，也会插入一些指令来实现 barrier。

## 例如
```c
int x, y, r;
void f()
{
    x = r;
    y = 1;
}
```

- gcc -S 1.c
```asm
....
	movl	r(%rip), %eax
	movl	%eax, x(%rip)
	movl	$1, y(%rip)
	nop
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
....
```

- gcc -S -O2 1.c (不正常了)
```asm
....
	movl	$1, y(%rip)
	movl	r(%rip), %eax
	movl	%eax, x(%rip)
	ret
....
```

```c
int x, y, r;
void f()
{
    x = r;
    __asm__ __volatile__("" ::: "memory");
    y = 1;
}
```


- gcc -S -O2 1.c
```asm
....
	movl	r(%rip), %eax
	movl	%eax, x(%rip)
	movl	$1, y(%rip)
	ret
....
```
