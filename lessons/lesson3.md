An array is a contiguous memory location which holds data. The data can be integers, characters, floating point numbers, structures etc. Each array element takes a distinct memory location. They all have distinct address.

In the following example, `int32_t` data type is used. `int32_t` represents 32 bit signed integer and is defined in `stdint.h` header file for supported compiler. While `int` data type doesn't guarantee about its size, `int32_t` is guaranteed to be 4 bytes long in every compiler that supports it. If your compiler doesn't support `int32_t` data type, try an integer type (`int`, `long`, `long long`) which is 32 bit wide. Although according to C standard compilers aren't forced to implement `int32_t` data type, to make the size of integers predictable, `int32_t` type is used in following examples in this tutorial.

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
		printf("Index %d: %p\n", i, &arr[i]);
	}

	return 0;
}
```

Notice that each array element is 4 bytes long and takes contiguous memory locations. If `&arr[0]` is 10, then `&arr[1]` is 14, and so on. Do you know that replacing `&arr[i]` with `&i[arr]` also works in the example above? Try this! **Caution:** Try this for fun. Don't put `&i[arr]` in your final code of your coding project as `&i[arr]` is less intuitive to get the address of the `i`-th element in array `arr`.

It works because for compiler `arr[i]` and `i[arr]` are same operation. Compiler converts `arr[i]` to `arr + i` and similarly `i[arr]` to `i + arr`. Since `arr + i` and `i + arr` produce same result, so do `arr[i]` and `i[arr]`.

What does `arr + i` mean? It is the address of the `i`-th element of the memory location pointed to by `arr`. This sounds like `arr` is a pointer. In fact, an array name in C is a pointer to the first element of the array!

Try the following example and notice that `&arr[i]` and `arr + i` are producing the same address:

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
		printf("Index %d: %p | %p\n", i, &arr[i], arr + i);
	}

	return 0;
}
```

Explanation in words:

```
arr + 0 = address of 1st element of array arr
arr + 1 = address of 2nd element of array arr
arr + 2 = address of 3rd element of array arr
...
arr + n = address of (n + 1)-st element of array arr
```
