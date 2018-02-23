When a variable is declared compiler automatically allocates memory for it. This is known as **compile time memory allocation** or **static memory allocation**. Memory can be allocated for data variables after the program begins execution. This mechanism is known as **runtime memory allocation** or **dynamic memory allocation**.

The memory allocated at compile time is deallocated (i.e., the memory is _released_ so that the same memory can be reused by other data) automatically at the end of the scope of the variable. In the example below the memory allocated for the variable `i` will be deallocated when the closing brace (`}`) of the statement block is encountered (considering no compiler optimization).

```C
{
    int i = 0;
}
```

C provides several functions in `stdlib` library for dynamic memory allocation. It's easy to both use and misuse these functions. One should write a program following best coding practices when dynamic memory allocation library functions are used.

# Memory Allocation With `calloc`

Given a number of objects to be allocated and size of each object `calloc` allocates memory. `calloc` returns a pointer to the first element of the allocated elements. If memory cannot be allocated, `calloc` returns `NULL`. If the allocation is successful, `calloc` initializes all bits to 0.

```C
#define NUMBER_OF_ELEMENTS 100

int32_t *parr = calloc(NUMBER_OF_ELEMENTS, sizeof(int32_t));

if (parr == NULL) /* Memory allocation fails */
{
	printf("Couldn't allocate memory");
}
else  /* Memory allocation successful */
{
	printf("Memory allocation successful");
}
```

In the example above memory allocation of `NUMBER_OF_ELEMENTS` 32 bit integers is requested by `calloc`. If the memory allocation is successful then `parr` will point to the first element of the allocated memory. Using `parr` the allocated memory can be used as array. All integers in the array pointed to by `parr` is initialized to 0.

# Memory Allocation With `malloc`

`malloc` tries to allocate a given number of bytes and returns a pointer to the first address of the allocated region. If `malloc` fails then a `NULL` pointer is returned. `malloc` doesn't initialize the allocated memory and reading them without initialization invokes undefined behaviour.

The above example of `calloc` can be implemented as below with `malloc`:

```C
#define NUMBER_OF_ELEMENTS 100

int32_t *parr = malloc(NUMBER_OF_ELEMENTS * sizeof(int32_t));

if (parr == NULL) /* Memory allocation fails */
{
	printf("Couldn't allocate memory");
}
else  /* Memory allocation successful */
{
	memset(parr, 0, NUMBER_OF_ELEMENTS * sizeof(parr[0]));

	printf("Memory allocation successful");
}
```
To use the function `memset` header file `<string.h>` must be included. Given a pointer and number of bytes, `memset` initializes all bytes to zero.

# Memory Reallocation With `realloc`

Given a pointer to a previously allocated region and a size, `realloc` deallocates previous region, allocates memory having the new size and copies old content into the new region upto the lesser of old and new sizes. `realloc` returns a pointer to the first element of the allocated memory. If new memory allocation fails, old content isn't deallocated, the value of the old content is unchanged and `realloc` return `NULL`.

```C
#define NUMBER_OF_ELEMENTS 100

int32_t *parr = calloc(NUMBER_OF_ELEMENTS, sizeof(int32_t));

if (parr == NULL) /* Memory allocation fails */
{
	printf("Couldn't allocate memory");
}
else  /* Memory allocation successful */
{
	printf("Memory allocation successful\n");

	parr = realloc(parr, (NUMBER_OF_ELEMENTS / 2) * sizeof(int32_t));

	if (parr == NULL) /* Memory reallocation fails */
	{
		printf("Memory reallocation fails");
	}
	else /* Memory reallocation successful */
	{
		printf("Memory reallocation successful");
	}
}
```

In the above example, `realloc` is used to reduce the array size to half.

If the first parameter to `realloc` is `NULL`, then `realloc` behaves like `malloc`.

# Deallocation Of Allocated Memory With `free`

The function `free` takes a pointer as parameter and deallocates the memory region pointed to by that pointer. The memory region passed to `free` must be previously allocated with `calloc`, `malloc` or `realloc`. If the pointer is `NULL`, no action is taken.

**Warning:** If the pointer passed to `free` doesn't match to a memory region previously allocated by memory management function or is already passed to `free` or `realloc` to deallocate, then the behaviour is undefined.

It's a good practice to set the pointer value to `NULL` once it is passed to `free` to avoid accidentally triggered undefined behaviour.

Following example allocates dynamic memory, initializes array elements with random number, reduces the size of the array to half with `realloc` and output before and after the reallocation proves that `realloc` kept old values in the array.

```C runnable
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <time.h>

#define NUMBER_OF_ELEMENTS 100

int main()
{
	/* Initialize seed for pseudo random number generator */
	srand(time(0));

	int32_t *parr = calloc(NUMBER_OF_ELEMENTS, sizeof(int32_t));

	if (parr == NULL) /* Memory allocation fails */
	{
		printf("Couldn't allocate memory");
	}
	else  /* Memory allocation successful */
	{
		printf("Memory allocation successful\n");

		/* Generate and store random numbers */
		for (int i = 0; i < NUMBER_OF_ELEMENTS; i++)
		{
			parr[i] = rand();
		}

		printf("Before reallocation: %d %d %d\n", parr[3], parr[7], parr[11]);

		/* Reduce the array size to half */
		parr = realloc(parr, (NUMBER_OF_ELEMENTS / 2) * sizeof(int32_t));

		if (parr == NULL) /* Memory reallocation fails */
		{
			printf("Memory reallocation fails");
		}
		else  /* Memory reallocation successful */
		{
			printf("Memory reallocation successful\n");

			/* Proof that realloc keeps old array contents */
			printf("After  reallocation: %d %d %d\n", parr[3], parr[7], parr[11]);
		}
	}

	printf("\nsizeof parr = %d", sizeof(parr));

	free(parr);
	parr = NULL;

	return 0;
}
```

Notice that regardless of how much bytes the whole array pointed to by `parr` is taking, `sizeof(parr)` equals to the size of a pointer variable in your platform.

