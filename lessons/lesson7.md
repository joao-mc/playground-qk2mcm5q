When a variable is declared compiler automatically allocates memory for it. This is known as **compile time memory allocation** or **static memory allocation**. Memory can be allocated for data variables after the program begins execution. This mechanism is known as **runtime memory allocation** or **dynamic memory allocation**.

The memory allocated at compile time is deallocated (i.e., the memory is _free_d so that the same memory can be reused by other data) automatically at the end of the scope of the variable. In the example below the memory allocated for the variable `i` will be deallocated when the closing brace (`}`) of the statement block is encountered.

```C
{
    int i = 0;
}
```

C provides several functions in `stdlib` library for dynamic memory allocation. It's easy to both use and misuse these functions. One should write a program following best coding practices when dynamic memory allocation library functions are used.

# Memory Allocation With `calloc`

Given a number of objects to be allocated and size of each object `calloc` allocates memory. `calloc` returns a pointer to the first element of the allocated elements. If memory cannot be allocated, `calloc` returns `NULL`.

```C
int32_t *parr = calloc(100, sizeof(int32_t));
if (parr == NULL)
{
	printf("Couldn't allocate memory with calloc");
}
else
{
	printf("Memory allocation successful wih calloc");
}
```

In the example above memory allocation of 100 32 bit integers is requested by `calloc`. If the memory allocation is successful then `parr` will point to the first element of the allocated memory. Using `parr` the allocated memory can be used as array.
