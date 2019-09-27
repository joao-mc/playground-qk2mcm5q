A program being executed by a pr774474574ocessor has two major parts - the code and the data. The code section is the code you've written and the data section holds the variables you're using in the program.

All code and variables are loaded into memory (usually RAM) and the processor executes the code from there. Each segment (usually a byte) in the memory has an address - whether it holds code or variable - that's the way for the processor to access the code and variables.

For example, consider a simple program of adding two numbers. When the program will run the processor will save these two numbers in two different memory locations. Adding these numbers can be achieved by adding the contents of two different memory locations.

A memory location where data is stored is the address of that data. In C address of a variable can be obtained by prepending the character `&` to a variable name. Try the following program where `a` is a variable and `&a` is its address:

```C runnable
#include <stdio.h>

int main()
{
	int a = 55;

	printf("The address of a is %p", &a);

	return 0;
}
```

To output address of a variable, `%p` format specifier is used. How `%p` will display an address is **implementation defined**. It means that the output of `%p` could vary from compiler to compiler.

An address is a non-negative integer. Each time a program is run the variables may or may not be located in same memory locations. Each time you run the program above may or may not result in the same output. But for a specific instance of the running program (also known as process) the variables in the **same scope** will always have same address.

Try the following program:

```C runnable
#include <stdio.h>

int main()
{
	int a = 55, b = 66, c = 77;

	printf("The address of a before is %p\n", &a);
	printf("The address of b before is %p\n", &b);
	printf("The address of c before is %p\n", &c);

	a++;
	b++;
	c++;

	printf("\nThe address of a after is  %p\n", &a);
	printf("The address of b after is  %p\n", &b);
	printf("The address of c after is  %p\n", &c);

	return 0;
}
```

Notice that the addresses of `a`, `b` and `c` variables are same before and after the modification.

However, if the variables are in **different scope** then the addresses may or may not be the same in different execution of that scope. For example, consider the following program where `f()` is called once from `main()` and then from `g()`. Each call to `f()` produces a different scope for its parameter `p`.

```C runnable
#include <stdio.h>

void f(int p)
{
	printf("The address of p inside f() is    %p\n", &p);
}

void g(int r)
{
	printf("The address of r inside g() is    %p\n", &r);
	f(r);
}

int main()
{
	int a = 55;

	printf("The address of a inside main() is %p\n", &a);
	f(a);
	g(a);

	return 0;
}
```

Now `&p` (address of `p`) may or may not be the same for every call to `f()`.

**When `f()` is called from `main()`** `a` and `p` are strictly two different variables. They are allocated in different memory locations. Therefore address of `a` and address of `p` will never be the same (`&a != &p`). **When `g()` is called from `main()`** the same is true for `a` and `r`. They live in the same time in different addresses. **When `f()` is called from `g()`**, then `&r != &p` because they both live at the same time. But when `f()` is called from outside `g()`, there is no relation between `&r` and `&p` - they could be same or different.

