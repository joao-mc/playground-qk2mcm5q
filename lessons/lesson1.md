# Address Of A Variable

A program being executed by a processor has two major parts - the code and the data. The code section is the code you've written and the data section holds the variables you're using in the program, plus any other variable that the compiler feeds in the program as the compiler sees appropriate.

All code and variables are loaded into memory (usually RAM) and the processes executes the code from there. Each segment (usually a byte) in the memory has an address - whether it holds code or variable - that's the way for the processor to access the code and variables.

For example, consider the simple program of adding two numbers.

```C runnable
#include <stdio.h>

int main() {
	printf("Hello World!");
}
```

