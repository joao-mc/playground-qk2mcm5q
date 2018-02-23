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

# Sorting student database

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

We declare a database (simply an array will work for us) of students and initialize everything to 0 (good practice to avoid undefined behaviour).

```C
#define MAX_STD_SIZE 30

Student std_db[MAX_STD_SIZE];
memset(std_db, 0, sizeof(std_db));
```

We populate this array somehow (in this example, students will be randomly populated). Now we've a huge collection of students. Looking them one by one for specific information may not be efficient. For example, we may want a list of students sorted by passing year. Or we may want a list of student names sorted by their names for easy lookup.

In this example we're going to sort the student array using `qsort` function available in `stdlib.h` library. But simply calling `qsort` with the array isn't enough because `qsort` has no idea about how to sort two `Student`s. Instead, `qsort` takes a function pointer as a parameter to compare two `Student` objects with one another to determine their position with respect to each other.

This kind of comparison function is also known as comparator. It has the following prototype for `qsort`:

```C
int (*compar)(const void *, const void *)
```

This function takes two pointers to objects to compare with each other. If the objects are equal, it returns 0. If the first object should be placed _before_ the second object in the sorted array, it returns a negative integer. If the first object should be placed _after_ the second object in the sorted array, it returns a positive integer.

We'll define two compartors in this example to pass to `qsort` so that the student array can be sorted as we wish.

## Sort by passing year

Sort by passing year is easy. The objects pointed to by the void pointers are cast to Student object. Then if the second passing year is subtracted from first passing year the requirement of comparator function is fulfilled.

```C
int sort_by_passing_year(const void* a, const void* b)
{
	const Student *sa = a;
	const Student *sb = b;

	return sa->passing_year - sb->passing_year;
}
```

## Sort by name

