# Address Of A Variable

A program being executed by a processor has two major parts - the code and the data. The code section is the code you've written and the data section holds the variables you're using in the program.

All code and variables are loaded into memory (usually RAM) and the processor executes the code from there. Each segment (usually a byte) in the memory has an address - whether it holds code or variable - that's the way for the processor to access the code and variables.

For example, consider a simple program of adding two numbers. When the program will run the computer will save these two numbers in two different memory locations. Adding these numbers can be achieved by adding the contents of two different memory locations.

A memory location where data is stored is the address of that data. In C prepending the character `&` to a variable name results in the address of that variable. Try the following program where `a` is a variable and `&a` is its address:

```C runnable
#include <stdio.h>

int main()
{
	int a = 55;

	printf("The address of a is %d", &a);

	return 0;
}
```

An address is a non-negative integer. Each time a program is run the variables may or may not be located in same memory locations. Each time you run the program above may or may not result in the same output.

