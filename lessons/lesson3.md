An array is a contiguous memory location which holds data. The data can be integers, characters, floating point numbers, structures etc. Each array element takes a distinct memory location. They all have distinct address.

In the following example, `int32_t` data type is used. `int32_t` represents 32 bit integer and is defined in `stdint.h` header file. While `int` data type doesn't guarantee about its size, `int32_t` is guaranteed to be 4 bytes long in every compiler that supports it. To make the size of integers predictable, `int32_t` type is used in following examples in this tutorial.

```C runnable
#include <stdio.h>
#include <stdint.h>

#define ARRAY_SIZE 3

int main()
{
	int32_t arr[ARRAY_SIZE] = { 22, 33, 44 };

	printf("Addresses of array elements:\n");
	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("Index %d: %u\n", i, &arr[i]);
	}

	return 0;
}
```
