# C Continued
- [C Continued](#c-continued)
    - [Structs](#structs)
    - [Initialization](#initialization)
    - [Copying Arrays](#copying-arrays)
    - [Copying structs](#copying-structs)
    - [Struct Members in Memory](#struct-members-in-memory)
    - [Arrays of Structs](#arrays-of-structs)
    - [Structs Cannot Be Compared Using `==`](#structs-cannot-be-compared-using)
- [C Continued Pt 2](#c-continued-pt-2)
    - [Unions](#unions)
    - [Union Size](#union-size)
    - [Function Pointers](#function-pointers)
        - [Function Pointer Example: Sorting Comparison](#function-pointer-example-sorting-comparison)
## Structs
```c
struct [ <optional tag> ] [ {
    <type declaration>;
    <type declaration>;
    ...
} ] [ <optional variable list> ];
```
- struct name can be the same as a member within struct
- definition vs declaration:
    - definition: simply allocates memory
    - declaration: formally gives type, value it holds, and initial value

```c
definition
______________________
struct mystruct_tag {
    int myint;
    char mychar;
    char mystr[20];
};
struct mystruct_tag {
    int myint;
    char mychar;
    char mystr[20];
} a;
struct mystruct_tag b, c, d;

declaration
______________________
struct {
    int myint;
    char mychar;
    char mystr[20];
} mystruct;
```
## Initialization
```c
struct mystruct_tag ms ={42, 'f', "goofy"};
```
## Copying Arrays
!!! error
```c
char a[10];
char b[10];
a = b;
```
!!!
- illegal because arrays have a constant pointer
    - solution: make function to copy, or put array inside a struct

## Copying structs
```c
struct s {
    int i;
    char c;
} s1, s2;
s1.i = 42;
s1.c = 'a';
s2 = s1;
--------> works as expected - s2.i = 42, s2.c = 'a'
```
- copies the values, byte by byte, into the members of the other struct
    - basically does a deep copy

## Struct Members in Memory
- cannot find exact size of structs, due to different compiler implementations
- can still find offsets though
    - use base + offset concept
```c
struct mystruct {
    int myint;  offset = 0
    char mychar; offset = 4
    char[] mystr; offset = 5
}
mystruct.mystr[4] = 'x';
if s is located at x1000, then members are located at x1000, x1004, x1005
```
- if we want to find element of array within struct, use base + offset but with more steps
!!! note
 want to find `mystr[4]` memory address:
 - get address of the struct: x1000
 - get offset of array: 5
 - get offset of element in array: sizeof(char) * 4 = 4
 - so result is x1009
!!!
## Arrays of Structs
```c
struct foo mystruct[25];
mystruct[6].mystr[3] = 'y';
```
- to find memory address of character
    - get address of mystruct: x1000
    - get offset of 6th element in the array: 6 * sizeof(struct foo) = 6 *25
    - get offset to mystr: 5
    - get offset to 3rd element within mystr: 3 * sizeof(char) = 3
    - 1000 + 6 * 25 + 5 + 3 = 2158

## Structs Cannot Be Compared Using `==`

# C Continued Pt 2
## Unions
```c
union {
    int myint;
    char mychar;
    char mystr[20];
} myun;
if union is at 1000, then all the members of union start at 1000
```
- look like structs, but all members have an offset of 0 - all in the same place
- used to implement polymorphism (like OOP)

## Union Size
- union size is the size of the largest member

## Function Pointers
- only use these phrases
    - pointer to
    - array of
    - function returning
    - lastly, add data type
    - start with name, then go right, then go left
- ex `int *(*x)[]()`
    - pointer to
    - array of
    - function returning
    - pointer to
    - an int
    - practice: [cdecl](https://cdecl.org/)

### Function Pointer Example: Sorting Comparison
- if we are writing a sorting function, we would need different function to compare the elements depending on the data type
    - use function pointer to specify which comparison type to use
- quicksort documentation:

```c
void qsort (
    void * base, 
    size_t nmemb, 
    size_t size, 
    int (* compar)(const void *, const void *) 
);
```
- `base` is pointer to beginning of array
- `nmemb` is number of elements in array
- `size` is size of each element in the array
- `int *compar` is a pointer to a comparison function

```c
compare ints
int compar_ints(const void *pa, const void *pb) {
    return *((int *)pa) - *((int *)pb);
}

compare strings
int compar_strings(const void *ppa, const void *ppb) {
    return strcmp( *((char **)ppa) , *((char **)ppb) );
}
```