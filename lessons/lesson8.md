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

What's the point of having a function pointer where we can directly call the function? Sometimes there are multiple similar (same prototype) functions to call and a decision must be made at runtime. Here is a complete example.

Suppose we want to create a database of students. For simplicity, each student will have a unique id, name and passing year.

```C
#define MAX_NAME_SIZE 1000

typedef struct
{
	uint32_t id;
	char name[MAX_NAME_SIZE];
	uint32_t passing_year;
} Student;
```

We declare a database (simply an array will work for us) of 1000 students.

```C
#define MAX_STD_SIZE 1000

Student std_db[MAX_STD_SIZE];
```


