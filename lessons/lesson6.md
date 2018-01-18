The `sizeof` operator takes a parameter and returns the size of the parameter in bytes. Following example prints the size of an array with 3 element, each is a 32 bit integer:

```C
#include <stdio.h>
#include <stddef.h>
#include <stdint.h>

#define ARRAY_SIZE 3

int main()
{
	int32_t arr[ARRAY_SIZE] = { 0 };

	size_t size = sizeof(arr);
	printf("Array size %d bytes\n", size);

	return 0;
}
```

The return type of `sizeof` operator is `size_t` which is an integer. The example above will convey the message that the array size is 12 bytes (3 integers, 4 bytes each).

Now consider the following program:

```C runnable
#include <stdio.h>
#include <stddef.h>
#include <stdint.h>

#define ARRAY_SIZE 3

void print_array_size(const int32_t arr[])
{
	size_t size = sizeof(arr);
	printf("%d bytes\n", size);
}

int main()
{
	int32_t arr[ARRAY_SIZE] = { 0 };

	size_t size = sizeof(arr);
	printf("%d bytes\n", size);

	print_array_size(arr);

	return 0;
}
```


