Like `++`, other arithmetic operators (`--`, `+=`, `-=`, `+`, `-`) work on pointers too as long as the pointer stays in the boundary of declared variables.

Following example output a string in reverse order:

```C runnable
#include <stdio.h>
#include <string.h>

int main()
{
	char str[] = "Yet another example";
	char *p = str + strlen(str); /* p points to the NULL character */
	p--; /* Now p points to the last character */

	for (int i = 0; i < strlen(str); i++, p--)
	{
		printf("%c", *p);
	}

	return 0;
}
```

Two pointers can also be subtracted from each other if the following conditions are satisfied:

1. Both pointers will point to elements of same array; or one past the last element of same array
2. The result of the subtraction must be representable in `ptrdiff_t` data type, which is defined in `stddef.h` and is an integer type.

```C runnable
#include <stdio.h>
#include <stddef.h>

#define ARRAY_SIZE 10

int main()
{
	int arr[ARRAY_SIZE] = { 0 };

	int *p = arr + 2;
	int *q = arr + 6;

	ptrdiff_t diff = q - p;
	printf("Difference %d\n", diff);

	return 0;
}
```

Using subtraction operation between two pointers we can get how far the elements are from each other in the array.

**Warning:** Care must be taken to make sure that the low address always gets subtracted from the high address. Otherwise, the behaviour will be undefined as illustrated in the example below:

```C
#define ARRAY_SIZE 10

int arr[ARRAY_SIZE] = { 0 };

int *p = arr + 2;
int *q = arr + 6;

/* ptrdiff_t diff = p - q; */ /* Fatal: p - q < 0, it won't point within arr and the result may not fit in ptrdiff_t */
```
