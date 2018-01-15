C has a type of variable which can store address or memory location. This type of variable is called a pointer. Essentially a pointer holds address of a variable. To indicate a variable as a pointer, `*` is used before the variable name as seen in the example below:

```C runnable
#include <stdio.h>

int main()
{
	int a = 55;
	int *pa = &a;

	printf("The address of a is %u or %p\n", &a, &a);
	printf("The content of a is %d\n", a);
	printf("The content of pa is %u or %p\n", pa, pa);
	printf("The address of pa is %u or %p\n", &pa, &pa);

	return 0;
}
```

