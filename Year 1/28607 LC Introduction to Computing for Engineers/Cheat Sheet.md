# C Cheat Sheet

## Basic Syntax
```c
#include <stdio.h>

int main() {
    printf("Hello World!");
    return 0;
}
```
## Variables
```c
data_type variable_name;
data_type variable_name = value;
```
Types of variables include local, global, static.
## Data Types
### Basic Types
- char
- int
- float
- double
- void

Modifiers: short, long, signed, unsigned
### Derived Types
- arrays
- pointers
### User-Defined Types
- enum
## Identifiers and Keywords
Identifiers can contain letters, digits, and underscores. They must start with a letter or underscore. Keywords such as int, return, float, switch cannot be used as identifiers.
## Input and Output
```c
printf("format", ...);
scanf("format", &variable);
getchar(); // Returns next character in buffer (might include the \n)

// For strings:
char str[20];  
scanf("%19[^\n]", str); // Leaving one space for the \0, and keep reading until \n
printf("This is your string: %s", str);

// Evem better for strings
char MyString[50];  
fgets(MyString, sizeof(MyString), stdin);  
```
Common format specifiers:
- `%c` character
- `%d` signed integer
- `%f` float
- `%lf` double
- `%s` null terminated string
- `%u` unsigned integer
- `%p` pointer address
- `%%` prints a literal percent sign
## Escape Sequences
 `\n` newline
- `\t` tab
- `\\` backslash
- `\"` double quote
- `\0` null
- `\r` carriage return
- `\b` backspace
## Operators
### Arithmetic
`+ - * / %`
### Relational
`< > <= >= == !=`

### Bitwise
`& | ^ ~ << >>`
### Logical
`&& || !`
### Conditional
`?:`
### Assignment
`= += -= *= /= %=`
## Conditional Statements
```c
if (boolean) { }

if (boolean) { }
else { }

if (boolean) { }
else if (boolean) { }
else { }

switch (expression) {
    case value:
        break;
    default:
        break;
}
```
Ternary:
```c
boolean ? ifTrue : ifFalse;
```
## Loops
### For
```c
for (variable init; condition; increment) { }
```
### While
```c
while (condition) { }
```
### Do-While
```c
do { } while (condition);
```
### Jump Statements
- `break;`Exit loop
- `continue;` Skip to next iteration
## Arrays
```c
type name[size];
type name[size1][size2];
```
Example:
```c
int arr[5] = {10, 20, 30, 40, 50};
```
## Strings
Strings are character arrays which end with `'\0'`
```c
char str[] = "Hello";
```
Common functions (with <string.h> header):
- strlen: Returns length of string
- strcpy: Copies one string to the other
- strcat: Concatenate: adds one string to the end of another
- strcmp: At first different character between two strings, returns the signed difference in the ASCII value of the characters (string1 - string2)
- strchr: Returns pointer to the location of the first occurrence of a character in a string
- strstr: Returns pointer to the location of the first character of the first occurrence of a substring in a string
- strcspn: Searches for multiple characters, returning the index of the first match with any of the characters
```c
int strlen(const char* str);
char* strcpy(char* dest, const char* src);
char* strcat(char* dest, const void* src);
int strcmp(const char* str1, const char* str2);
char* strchr(const char* str, int character);
char* strstr(const char* str, const char* substring);
size_t strcspn(const char* str, const char* search); // string is used for the characters to be searched
```
## Pointers
```c
data_type *ptr;
ptr = &variable;
```
Dereference:
```c
*ptr;
```
Example:
```c
int a = 10;
int *p = &a;
```
## Functions
```c
return_type function_name(parameters);

return_type function_name(parameters) {
    statements;
}
```
Example:
```c
int sum(int a, int b) {
    return a + b;
}
```
## Files
The `FILE` datatype is defined in `<stdio.h>`
```c
FILE* in_file;
in_file = fopen("my_file.dat", "r");
// do things
fclose(in_file);
```
- "r": Open existing text file for reading, starting at the beginning
- "w": Create new file or rewrite existing and write into it
- "a": Create new file if one doesn't exist, the append
- "b": added to the end ("rb", "wb" etc) to open a binary file
### Reading Characters from Files
```c
int ch;
FILE* in_file;
//...
ch = getc(in_file);
```
Used in loop:
```c
int ch;
while ((ch = getc(in_file)) != EOF) { // read file until the end of file
	//...
}
```
### Writing Characters to Files
```c
int ch;
FILE* out_file;
// ...
putc(ch, out_file);
```

### Reading Strings from Files
```c
char buffer[100];
int n;
FILE* in_file;
// ...
fgets(buffer, n, in_file);
```
- fgets() reads a string of characters from a file into a char array until:
	- The end of a line is reached
	- A specified (max) number of characters have been read from a line
	- The EOF is reached
- It reads the file one line at a time, and using the max number parameter it ensures that there isn't a buffer overflow
- The fgets() returns `NULL` if EOF is reached
### Writing String to Files
```c
char buffer[100];
FILE* out_file;
// ...
fputs(buffer, out_file);
```
fputs() writes a string of characters into a file.
### Formatted File I/O
`fprintf()` and `fscanf()`:
- Function similar to `printf()` and `scanf()`
- They allow file i/o of C built-in data types into files
- Same modifiers `%f`, `%d` etc are used

To write to a file with `fprintf()`:
```c
FILE* out_file;
out_file = fopen("text_file.txt", "w");
if (outfile != NULL) {
	for (i=0; i<10; i++)
		fprintf(out_file, "%s %d\n", "Line", i);
	fclose(out_file);
}
return 0;
```
This program writes the following into `text_file.txt`:
``` 
Line 0
Line 2
...
Line 9
```
 To read a file with `fscanf()`:
 ```c
char buffer[20];
int end_of_line, i;
FILE* in_file;
in_file = fopen("text_file.txt", "r");
if (in_file != NULL) {
	do {
		end_of_line = fscanf(in_file, "%s %d", buffer, &i);
		if (end_of_line != EOF)
			printf("%s %d \n", buffer, i);
	} while (end_of_line != EOF);
	fclose(in_file);
}
return 0;
 ```

```
Line 0
Line 2
...
Line 9
```
### Binary File I/O
```c
int nread, n, buffer[100];
FILE* in_file;
// ...
nread = fread(buffer, sizeof(int), n, in_file);
```
`fread()` reads n number of ints (each with sizeof(int) bytes) from `in_file` into `buffer`, and returns the number of elements read (may be less than n if EOF is reached)
```c
int nwrite, n, buffer[100];
FILE* out_file;
// ...
nwrite = fwrite(buffer, sizeof(int), n, out_file);
```
`fwrite()` writes n number of ints (each with sizeof(int) bytes) into `out_file` from `buffer`, and returns the number of elements written (may be less than n if something went wrong)