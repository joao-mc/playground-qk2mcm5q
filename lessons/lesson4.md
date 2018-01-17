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
		printf("%p | %d\n", arr + i, arr[i]);
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

This is what happens in the example above. `arr` is initialized as follows:

```
arr:         1    2    3    4
address:     10   14   18   22
```

The address is hypothetical here. `p` points to the first element of `arr` by `int32_t *p = arr;`:

```
arr:         1    2    3    4
address:     10   14   18   22
             ^
             |
             p
```

A loop iterates `ARRAY_SIZE` times. In each iteration `*p` is multiplied by `*p` - the dereferenced value of `p` is squared - and stored in the memory location pointed to by `p` by the statement `*p *= *p;`. This modifies one array element in every iteration. The next statement, `p++;`, increments the pointer `p`, not by 1, but by `sizeof(*p)`, equivalently `sizeof(int32_t)` - in this case 4 at a time.

In the hypothetical address above:

```
before 1st iteration: p = 10
after 1st iteration : p = 14
after 2nd iteration : p = 18
after 3rd iteration : p = 22
after 4th iteration : p = 26
```

This can be verified by another small example with 16 bit integer:

```C runnable
#include <stdio.h>
#include <stdint.h>

#define ARRAY_SIZE 4

int main()
{
	int16_t arr[ARRAY_SIZE] = { 0 };
	int16_t *p = arr;

	for (int i = 0; i < ARRAY_SIZE; i++)
	{
		printf("%p\n", p);
		p++;
	}

	return 0;
}
```

Notice that p increments by 2, or `sizeof(int16_t)`, at every iteration.

**Warning:** When the loop exits in the above example, `p` points to one past the last element of `arr`. That area is outside the declared variables in the program. Accessing that location is undefined behaviour.

```C
int16_t arr[ARRAY_SIZE] = { 0 };
int16_t *p = arr;

for (int i = 0; i < ARRAY_SIZE; i++)
{
    printf("%p\n", p);
	p++;
}

/* printf("%p\n", p); */ /* Fatal: p points to outside location of declared variable - undefined behaviour */
```

