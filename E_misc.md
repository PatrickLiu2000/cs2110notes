# Misc Topics
- [Misc Topics](#misc-topics)
    - [Variable Number of Args](#variable-number-of-args)
    - [Main Method Return Value](#main-method-return-value)
    - [Opening Files](#opening-files)
    - [Buffered Output](#buffered-output)
    - [Optimizations](#optimizations)
    - [Inline Functions](#inline-functions)
## Variable Number of Args
- function that doesn't take a fixed number of args
    - example: `printf`
- syntax:
```c
int add_em_up (int count,...)
{
  va_list ap;
  int i, sum;

  va_start (ap, count);         /* Initialize the argument list. */

  sum = 0;
  for (i = 0; i < count; i++)
    sum += va_arg (ap, int);    /* Get the next argument value. */

  va_end (ap);                  /* Clean up. */
  return sum;
}
```
## Main Method Return Value
- 0 means ran successfully
- anything else means a problem occurred
- can return 8 bit value

## Opening Files
```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream );
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

## Buffered Output
- for standard out, the output is line buffered
- if program crashes, not all buffer contents might be output
    - has not flushed out buffer
    - in order to ensure it is printed,

```c
printf(“Checkpoint 1”);
fflush(stdout);
```

## Optimizations
- can set compiler optimization level
- the higher the optimization level, the more likely the code will not work as intended

## Inline Functions
```c
inline int myfunc(int a, int b) {
    return a + b;
}
```
- basically "inserts" the function code into where the function is called, essentially eliminating a stackframe needing to be used, making it more optimized