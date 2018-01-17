Given an array `arr[ARRAY_SIZE]` we can get the address of the `i`-th element by `arr + i` as `arr` works as a pointer to the first element of the array. This is true for any pointer pointing any element of `arr` array.

```C runnable
#include <stdio.h>
#include <stdint.h>

#define ARRAY_SIZE 8

int main()
{
	int32_t arr[ARRAY_SIZE] = { 22, 33, 44 };
	int32_t *p = arr + 2;

	printf("Array elements before modification: ");
	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("%d ", arr[i]);
	}

	*p = 55;

	printf("\nArray elements after modification:  ");
	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("%d ", arr[i]);
	}

	return 0;
}
```

In the example above, `p` points to the 3rd element of array using the assignment `p = arr + 2`. The statement `*p = 55;` modifies the contents of the memory location pointer to by `p`, essentially `arr[2]`.

Just like `arr + i` works, `p + i` will work too.

```C runnable
#include <stdio.h>
#include <stdint.h>

#define ARRAY_SIZE 4

int main()
{
	int32_t arr[ARRAY_SIZE] = { 0 };

	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		arr[i] = i + 1;
	}

	printf("Array elements' address and content before modification:\n");
	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("%u | %d\n", arr + i, arr[i]);
	}

	int32_t *p = arr; /* p points to the first element of arr */
	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("Modifying content of address %p\n", p); /* Current address p is holding */
		*p *= *p; /* Squaring the content pointed to by p (eg., 3 becomes 9) */
		p++; /* Increamenting the content of p, points to the next element of the array */
	}

	printf("Array elements' address and content after modification:\n");
	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("%p | %d\n", arr + i, arr[i]);
	}

	return 0;
}
```

The example above incre
