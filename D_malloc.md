# Malloc
- [Malloc](#malloc)
    - [K&R Malloc Implementation](#kr-malloc-implementation)
    - [Persistent Data](#persistent-data)
    - [Calloc](#calloc)
    - [Realloc](#realloc)
    - [Common Errors when Mallocing](#common-errors-when-mallocing)
## K&R Malloc Implementation
- structs, header, and unions
- heap layout
- allocating correct amount of memory
- `malloc()`-getting memory for work
- `free()`-recycling memory when done
- `morecore()`-OS requests for real memory

- high level: linked list of sections of memory too be freed
- look at `mem` c files for implementation

## Persistent Data
```c
char *foo(void){
    char ca[10];
    return ca;
}
```
- bad because ca is on the stack (local variable), and when the pointer is returned when ca is popped off, no way of knowing what value it will be

## Calloc
- same as malloc, but initializes memory block to 0

## Realloc
- finds space for new allocation
- copies data into new space
- frees old space
- return pointer to new space
- problems:
    - realloc can return `null`
    - so, problem arises when doing this: `cp = realloc(cp, n);`
        - realloc can return null, so reassigning old pointer to null will cause memory leak, because we lose pointer to original data
    - to do this properly, check for error first:

```c
void *tmp;
if((tmp = realloc(cp,...)) == NULL) {
    /* realloc error */
} else {
    cp = tmp;
    don't free temp! - doing so will free cp as well
}
```
- equivalencies

```c
realloc(NULL, n) == malloc(n);
realloc(cp, 0) = free(cp);
```

## Common Errors when Mallocing
- allocating a block on heap and losing by losing value of pointer
- allocate block on heap and use contents without initialization
- read/write beyond boundaries of memory block
- freeing block of memory but continuing to use contents
- call realloc to expand block of memory, but keep using old address