Similar to variables, all code written in a program are also stored in memory during program execution. Therefore all executable code have a distinct address to be referred to. C has the flexibility to obtain the starting address of a function through a pointer - known as function pointer.

Consider the following function

```C
int multiply(short a, short b)
{
	return (int)a * (int)b;
}
```

To store the address of this function in a function pointer, following syntax is used:

```C
int (*pf)(short, short) = multiply;
^     ^   ^               ^
|     |   |               function name
|     |   function parameters inside parentheses
|     variable name followed by *, everything enclosed inside parentheses
function return type
```

Once declared, the variable name (`pf` here) can be used as a function:

```C runnable
#include <stdio.h>

int multiply(short a, short b)
{
	return (int)a * (int)b;
}

int main()
{
	int (*pf)(short, short) = multiply;

	printf("Multiplied through function pointer: %d\n", pf(10, 9));

	return 0;
}
```

