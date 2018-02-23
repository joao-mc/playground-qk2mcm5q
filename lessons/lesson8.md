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
|     |   function parameter types inside parentheses
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

The name of the student with less characters can be considered to come first. If the lengths of the names are same, then a character-by-character comparison needs to be performed to determine which name comes first.

```C
int sort_by_name(const void* a, const void* b)
{
	const Student *sa = a;
	const Student *sb = b;

	int lena = strlen(sa->name);
	int lenb = strlen(sb->name);

	if (lena == lenb)
	{
		/* Names having same length */

		for (int i = 0; i < lena; i++)
		{
			if (sa->name[i] < sb->name[i])
			{
				return -1;
			}
			if (sa->name[i] > sb->name[i])
			{
				return 1;
			}
		}

		/* Names are same */
		return 0;
	}

	/* Consider the name with less character to be the lesser of two objects */

	if (lena < lenb)
	{
		return -1;
	}

	return 1;
}
```

With the comparator functions defined, the array of student objects can be sorted by name:

```C
qsort(std_db, MAX_STD_SIZE, sizeof(std_db[0]), sort_by_name);
```

or passing year:

```C
qsort(std_db, MAX_STD_SIZE, sizeof(std_db[0]), sort_by_passing_year);
```

Full example together:

```C runnable
#include <stdio.h>
#include <stdint.h>
#include <time.h>
#include <stdlib.h>
#include <string.h>

/* Constant definitions */
#define MAX_NAME_SIZE 1000
#define MAX_STD_SIZE 30

/* Student record */
typedef struct
{
	uint32_t id;
	char name[MAX_NAME_SIZE];
	uint32_t passing_year;
} Student;

int sort_by_name(const void* a, const void* b)
{
	const Student *sa = a;
	const Student *sb = b;

	int lena = strlen(sa->name);
	int lenb = strlen(sb->name);

	if (lena == lenb)
	{
		/* Names having same length */

		for (int i = 0; i < lena; i++)
		{
			if (sa->name[i] < sb->name[i])
			{
				return -1;
			}
			if (sa->name[i] > sb->name[i])
			{
				return 1;
			}
		}

		/* Names are same */
		return 0;
	}

	/* Consider the name with less character to be the lesser of two objects */

	if (lena < lenb)
	{
		return -1;
	}

	return 1;
}

int sort_by_passing_year(const void* a, const void* b)
{
	const Student *sa = a;
	const Student *sb = b;

	return sa->passing_year - sb->passing_year;
}

int main()
{
	/* Initialize seed for pseudo random number generator */
	srand(time(0));

	/* Database of students */
	Student std_db[MAX_STD_SIZE];

	/* Initialize everything to 0 */
	memset(std_db, 0, sizeof(std_db));

	/* Randomly populate students */
	for (uint32_t i = 0; i < MAX_STD_SIZE; i++)
	{
		/* Assign unique id */
		std_db[i].id = i + 1;

		/* Randomly populate name */
		int max_name_size = min(MAX_NAME_SIZE, 10);
		int name_size = rand() % max_name_size + 5;
		for (int j = 0; j < name_size; j++)
		{
			std_db[i].name[j] = rand() % 26 + 'a';
		}

		/* Randomly populate passing year
		from 1999 to 2018 */
		std_db[i].passing_year = 2018 - rand() % 20;
	}

	/* Sort by name */

	qsort(std_db, MAX_STD_SIZE, sizeof(std_db[0]), sort_by_name);

	printf("Students sorted by name:\n\n");
	for (int i = 0; i < MAX_STD_SIZE; i++)
	{
		printf("%5d %5d %s\n", std_db[i].id, std_db[i].passing_year, std_db[i].name);
	}

	/* Sort by passing year */

	qsort(std_db, MAX_STD_SIZE, sizeof(std_db[0]), sort_by_passing_year);

	printf("\nStudents sorted by passing year:\n\n");
	for (int i = 0; i < MAX_STD_SIZE; i++)
	{
		printf("%5d %5d %s\n", std_db[i].id, std_db[i].passing_year, std_db[i].name);
	}

	return 0;
}
```

