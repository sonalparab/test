## Wednesday, 10/25 Opening up a world of possibilities by Sonal Parab
**Tech News:** [High-tech Mirror for cancer patients only works if you smile](http://money.cnn.com/2017/10/24/technology/smile-mirror-cancer-patients/index.html)

We continued our discussion about files.  

**open**  
must have ```C#include <fcntl.h>``` to use open  
Add a file to the file table and returns it file descriptor  
If open fails, -1 is returned, extra error information can be found in errno.  
* errno is an int variable that can be found in ```C<errno.h>```, using strerror (in string.h) or errno will return a string description of the error  
```C
open(<PATH>,<FLAGS>,<MODE>)
```

<MODE>  
Only used when creating a file. Set the file's permissions using a 3 digit octal #  

<FLAGS>    
Determine what you plan to do with the file.  
Use the following constants:  
	```C0_RDONLY```  
	```C0_WRONLY```  
	```C0_RDWR```: read and write  
	```C0_APPEND```: write at the end  
	```C0_TRUNC```: start at beginning (if combined with write would overwrite file)  
	```C0_CREAT```: creates the file, must be provided if file does not exist, opens if it exists  
	```C0_EXCL```: must be combined with 0_CREAT, will return an error if the file exists  
Each flag is a number, to combine flags we use bitwise OR  
	0_WRONLY = 1		00000001  
	0_APPEND = 8		00001000  
	0_WRONLY | 0_APPEND 	00001001  

---
## Tuesday, 10/24 File This Under Useful Information by Charles Weng
**Tech News:** [Does the 3DS have a Future?](https://arstechnica.com/gaming/2017/04/after-nintendo-switch-does-the-3ds-have-a-future/)

Today was mostly a review of file permissions that we covered back in intro.

We started off by linking octal to the file permissions (read, write and execute, abbreviated to rwx respectively) and went over the following:
* there is 8 total combinations for those permissions
* permissions could be represented by a single digit in octal
  - this stems from a 3 digit binary representation in which a 1 represents permission given and 0 represents permission denied
  - 6 will get converted to 110 which cross referenced with rwx means read and write permissions
* there are 3 sets of permissions; one for user, one for group, and one for others
  - others does NOT refer to everyone, but everyone not included by user and group
  - you can view file permissions in the terminal with the 'ls -l' or 'll' command (long format listing), which shows metadata
* this is used by the terminal command chmod to more easily modify file permissions for a file
  - 644 is default
  - `chmod 644 name.c` as opposed to `chmod u=rw,g=r,o=r name.c` or using separate flags

We then went on to talk about the first character that we saw in `drw-r--r--` which denoted the file as a file descriptor or a directory. This was just a file that listed out the pointer to and names of other files and is used in GUIs for organization.

We also talked about how this relates to c with file interactions through stdio.h and how when you shouldn't be complaining about the limit on the amount of files you can have open (256). A relevant function for this is `getdtablesize()` which returns the first power of two greater than your table size. More on this tomorrow.

## Monday, 10/23 A bit of wisdom by Kenny Chen
**Tech News:** [Should We Fear the Rise of Intelligent Robots?](https://www.livescience.com/59802-should-we-fear-intelligent-robots.html)

Today we discussed a few more `printf` formatting options for printing integers in different bases, ways to represent an integer in different bases, and bitwise operators.

**Different Bases**

`"%o"` prints an integer in **octal**, and `"%x"` prints an integer in **hexadecimal**. (Correspondingly, `"%d"` prints an integer in **decimal**. There is no formatting character for binary.)  
So, for example, printing 13 with:  
`"%d"` results in `13`  
`"%o"` results in `15`  
`"%x"` results in `d`  

We can also represent integers in different bases:  
In **binary**, we precede the digits with `0b`.  
In **octal**, we precede the digits with `0`.  
In **hexadecimal**, we precede the digits with `0x`.  
```C
int b = 0b1011; //11
int o = 0122; //82
int x = 0xa3; //163
```
These integers are all stored the same way; that is,  
`0b1011`, `013`, `0xb`, and `11` are all the same.  

**Bitwise Operators**

`>>` is the right shift. It moves all the bits once to the right, and adds 0s to the front.  
`<<` is the left shift. It moves all the bits once to the left, and adds 0s to the end.  
Right shift and left shift do not affect memory outside of the integer.  

So, if `char b = 0b1011;` (that is, `b`'s bits are `00001011`, then  
`b = b >> 1;` would have `b` be the same as `00000101`. Then,  
`b = b << 1;` would have `b` be the same as `00001010`.  

`~` is negation. It flips every bit.  
`|` is or. It performs or for each pair of bits.  
`&` is and. It performs and for each pair of bits.  
Lastly, `^` is xor. It performs xor for each pair of bits.  

Now, suppose we have  
`unsigned char a = 0b10110111;` and  
`unsigned char b = 0b01011100;`. Then:  
`~a` would be `01001000`.  
`a | b` would be `11111111`.  
`a & b` would be `00010100`.  
`a ^ b` would be `11101011`.  


At the end of class, a swap algorithm using xor was given.  
Given the integers `a` and `b`:
```C
a = a ^ b;
b = a ^ b;
a = a ^ b;
```
swaps the values of `a` and `b`.

---

## Thursday, 10/19 Get Dem Bugs by Xing Tao Shi
**Tech News:** [Samsung to Give Linux Desktop Experience to Smartphone Users](https://www.technewsworld.com/story/Samsung-to-Give-Linux-Desktop-Experience-to-Smartphone-Users-84896.html)

**GDB**
* stands for GNU debugger
* You must compile with -g in order to use GDB
```C
gcc -g foo.c
```
* Usage:
```C
$ gdb ./a.out
```
There are many things you can do with GDB such as:
Run the program
```C
run
```
Place a break point at a line, this essential makes it so that when you run the program it stops here
```C
break line_number
```
Place a break point at a function, this essential makes it so that when you run the program it stops here
```C
break func_name
```
To continue from a break point until the next break point you can
```C
continue
```
To continue from a break point and execute the next line you can
```C
next
```
To continue from a break point and execute the next line or the next line of the function located in the next line you can
```C
step
```
Print a variable
```C
print variable
```
Show the broken code
```C
list
```
Quit the GDB shell
```C
quit
```
---

## Wednesday, 10/18 valgrind by Herman Lin
**Tech News:** [LIGO Detects Fierce Collision of Neutron Stars for the First Time](https://www.nytimes.com/2017/10/16/science/ligo-neutron-stars-collision.html)

**valgrind**
* tool for debugging memory issues in C programs
* You must compile with -g in order to use valgrind (and other similar tools)
```C
gcc -g foo.c
```
* Usage:
```C
$ valgrind --leak-check=yes <program>
```

---

## Friday, 10/13 calloc & realloc by Fabiola Radosav
**Tech News:** [Can we teach robots ethics?](http://www.bbc.com/news/magazine-41504285)

**calloc**
```C
void *calloc(size_t n, size_t x);
```
* like malloc but initializes memory to 0 (clears allocation)
* allocates n * x bytes of memory
* Example code:
```C
int *p;
p = (int *)calloc(5, sizeof(int));
```

**realloc**
```C
void *realloc(void *p, size_t x);
```
* changes the amount of memory allocated to a given block (x is the new size)
* p must be a pointer to the beginning of an allocated block of memory, but it does not have to be the original pointer
* if x is smaller than the original size of the allocation, the extra bytes will be released
* Example code:
```C
int *p;
p = (int *)malloc(20);
p = (int *)realloc(p, 40);
```

---

## Thursday, 10/12 malloc & free: the dynamic duo! by Penn Wu
**Tech News:** [Bitcoin rockets above $5,000 to all-time high](https://www.reuters.com/article/us-global-markets-bitcoin/bitcoin-rockets-above-5000-to-all-time-high-idUSKBN1CH0YQ)

We spent most of the class writing code demonstrating either the functions `malloc` and `free` or `calloc` and `realloc`.

**malloc**
```C
void *malloc(size_t x);
```
* allocates x bytes of memory dynamically (i.e., from the heap), and returns the address to the beginning of the allocated memory.
* returns a void pointer, which you should typecast to the correct type like so:
```C
int *p;
p = (int *)malloc(sizeof(int) * 5);
```

**free**
```C
void free(void *p);
```
* frees dynamically allocated memory starting from the pointer parameter
* doesn't immediately mean that whatever data was stored there is gone, it just means that it isn't guaranteed to be there anymore
* in other words, **don't use memory that you freed**

---

## Tuesday, 10/10 and Wednesday 10/11 If you don't pay attention you'll get into a heap of trouble! by William Hong
**Tech News:** [More Governments Test Out Cryptocurrency](http://www.investopedia.com/news/more-governments-test-out-cryptocurrencies/)

### AIM: If you don't pay attention you'll get into a heap of trouble!

We briefly discussed structs, below is prototyping a struct
#### struct
```C
struct foo {
    int a;
    char x;
};
```
Using the code above, we can declare variables as struct foo
```C
struct foo s;
struct foo s2; //this is a different variable of type foo
```
Prototypes are different than directly declaring a struct such as below
```C
struct {int a; char x;} s;
```
Prototypes are meant to be used later, like a template

IMPORTANT: We use the . operator to access a value inside a struct, see example below
```C
s.a = 10;
s.x = '@';
```
Structs can contain variables and even more structs. You can also have a pointer to a struct within the struct itself, thus making a linked list data structure
```C
struct foo{
    //variables, etc
    struct foo *next;
};
```

Note: even if we use a struct in the parameter of a function, it is stilled passed by value, which means a copy of the argument is made. Do this instead...
```C
void foo_stuff(struct foo *s); //as opposed to void foo_stuff(struct foo s)
```

Important note on the dot operator: . binds before *
```C
struct foo *p;
p = &s;
(*p).x; //this is accessing the data in a pointer to a struct
```
Additional note: anything you do to p will modify whatever's going on in x

IMPORTANT: to access data from a struct pointer, you can also use ->
```C
p->x; //this is the same as (*p).x
```
If there's a pointer inside a struct with another pointer to another struct, you can chain the arrows

!!! Caution: you can typedef structs, but this will hide the fact that you are working with a struct

We also discussed memory, but didn't have time to talk about heap memory yet. Computer programs separate memory usage into two parts: stack and heap. Every program can have its own stack and heap

Notes on stack memory
- Stack memory stores all normally declared variables (including pointers and structs), arrays and function calls
- Functions are pushed into the stack in the order they are called, and popped off when completed
- When a function is popped off the stack, the stack memory associated with it (like an array for instance) is released. You can't use that memory later because it's gone
-The stack contains all the memory we learned so far in C, including arguments to functions. The copy of the argument(s) associated with that function is put onto the stack

```C
struct node{
	int data;
	struct node *next;
}

struct node *insert_front(struct node *front, int d){
	struct node new;
	new.data = d;
	new.next = front;
	return &new;
}
```
- On the above example, this looks good on paper. This should implement a linked list data structure
- However, this is BAD. The struct node new would cease to exist because it's stack memory
- Declaring a variable in a function puts it into the stack and it subsequently gets popped off



Notes on Heap memory
- Stores dynamically allocated memory
- IMPORTANT DIFFERENCE FROM STACK MEMORY: data will remain in the heap until it is released manually by you or when the program terminates
- Can be accessed through pointers
- Can be accessed across many functions
- Persists throughout the duration of the program's execution, until it terminates of course
- A memory leak occurs when a program uses lots of heap memory

---

## Friday, 10/6 Finding your type by Jerome Freudenberg
**Tech News:** [YouTube Changes Search Algorithms](https://www.theguardian.com/us-news/2017/oct/06/youtube-alters-search-algorithm-over-fake-las-vegas-conspiracy-videos)

### AIM: Finding your type

On Friday, we discussed useful keywords in C:

#### typedef
-Usage:
```C
typedef <real type> <new name>;
```
-provides a new name for an existing data type
-Ex:
```C
typedef unsigned long size_t;
size_t x = 139; //x is really an unsigned long
```
-Can be used to improve portability

#### struct
-Usage:
```C
struct {int a; char x;} s;
```
-creates a new type that is a collection of values
-The example above is technically 5 bytes, but has extra padding so that the memory allocated is a power of 2

-You can also assign it to a variable:
```C
struct foo {int a; char x;};
```
-This means that foo is a prototype for this kind of struct, to be used later
		(e.g.)
```C
struct foo s1;
```
-Note the semicolon after the curly brace as people tend to forget to place it there

###### We also discussed what this dude, Linus, wrote about coding style:
[His thoughts](https://www.kernel.org/doc/Documentation/process/coding-style.rst)

-Noteworthy things included that Hungarian notation is for the brain damaged

-He also mentioned typedefs are not for readability and should not be used for structures and pointers

---

## Tuesday, 10/3 Make It So by Eric Chen
**Tech News:** [General Motors to go Electric](https://www.washingtonpost.com/news/innovations/wp/2017/10/02/death-of-diesel-begins-as-gm-announces-plans-for-all-electric-future/?utm_term=.7a940daf5ee4)

AIM: Make it so

Separate Compilation
- You can combine multiple C files into a C program by including them all in one gcc command.
- i.e. `gcc test.c string.c foo.c woohoo.c`
- One and only one of these files must have a main function.
- You cannot have duplicate functions or global variable names across these files.

`gcc -c`
- Compiles a C file into a .o file, it is not a fully functional program, but it is compiled code.  Do this to compile a .c file that does not contain a main() function.
- .o files can be liked together with .c files through gcc.

Make
- Create compiling instructions and setup dependencies.
- Standard name for the file is makefile.
- Syntax:
	```<TARGET>: <DEPENDENCIES>
	[TAB]<RULES>```
- `<TARGET>` can be either a file or just a name.
- i.e.

```
strtest: dwstring.o main.o
	gcc -o strtest dwstring.o main.o

dwstring.o: dwstring.c dwstring.h
	gcc -c dwstring.c

main.o: main.c dwstring.h
	gcc -c main.c

clean:
	rm *.o
	rm *~

run: all
	./strtest
```
- In order to run a makefile, the syntax is `make <TARGET>`
	It will only make that specific target.
	If `<TARGET>` is left blank, it will make the first target and all its dependencies only.
- The clean target will run using `make clean`
- The run target will compile all its dependencies and run automatically with `make run`

---

## Friday, 9/29 String Functions by Connie Lei
**Tech News** [Microsofts's Nadella Wants To Help Coders Take A Quantum Leap](https://www.wired.com/story/microsofts-nadella-wants-to-help-coders-take-a-quantum-leap/)

We started class in our groups and explained our homework, showing each function and running our test code. We then went over the functions together in class so we could have a class discussion about it and ask questions.

You need to use `#include <string.h>` at the top of your file

* **strcpy/strncpy**
   * `strcpy( char * dest, char * src );`
   * `strncpy( char * dest, char * src, int n);`
   * These two functions copy strings from the source to the destination. The latter one allows you to choose the length of the source string. If you choose to copy a string of a length longer than the destination memory allocation, the null terminating is overwritten. You need to make sure to copy over the null terminating as well.
* **strcat/strncat**
   * `strcat( char * dest, char * src );`
   * `strncat( char * dest, char * src, int n);`
   * The two functions allow you to concatenate strings from the source to the destination. The destination string has to have enough space for both functions. The null termination from the destination string is overwritten with the first character in the source string.
* **strcmp/strncmp**
   * `strcmp( char * s1, char * s2 );`
   * `strncmp( char * s1, char * s2, int n );`
   * These function allows you to compare two strings, it returns s1 - s2 so if s1 is less than s2, a negative number is returned, a positive if s1 is greater and 0 if the two are equal. The latter function will only compare the first n characters of the string.
* **strchr/strstr**
   * `strchr( char * s1, char c );`
   * `strstr( char * s1, char * s);`
   * These function return a pointer to the first instance of the character or the index for the string. A null pointer will be returned if the string/character does not exist.
---

## Thursday, 9/28 Functions with Strings by Gilvir Gill
**Tech Noos** [San Francisco Sues Equifax over data breach](https://techcrunch.com/2017/09/27/san-francisco-sues-equifax-on-behalf-of-15-million-californians-affected-by-the-breach/)

We started clas by writing code that will return the length of the string, abusing the fact that String literals are automatically terminated with '\0's (null, or 0). We had the idea of 0 being false reinforced by realizing that we could just terminate with the boolean `(*(str+ i))`, which will return 0 when it lands on the null.


```C
#include <stdio.h>
int str_len(char * str) {
    int i = -1;
    while ((str[++i]));
    return i;
}

void main() {
    char * s = "hello";
    char * s0 = "";
    printf("Length of \"%s\": %d\n", s, str_len(s));
    printf("Length of \"%s\": %d\n", s0, str_len(s0));
}
```
We also reviewed the difference between different expressions, particularly mutable pointers, immutable arrays, immutable String literals, and their mutable copies held in array locations.

For example:

* `char *s3 = "Mankind"` just points to the immutable piece of memory that holds "Mankind"

In addition, you can only assign char arrays with = at declaration. If you declare it with an = it first finds the literal that was created in the beginning in memory and then clones it in an O(n) procedure. On the other hand, if you declare a char pointer, it simply points to the literal. If you want to ensure that you have a mutable copy of a string, you want to declare it with an array. You can of course assign a pointer to it afterwards if needed.

We were later told to examine one of four functions:
* 0 : strcpy/strncpy
* 1 : strcat/strncat
* 2 : strcmp/strncmp
* 3 : strchr/strstr

for homework.

---
## Wednesday, 9/27 Strings in C by Anthony Hom

**Tech News** [A smaller Echo and thee new Echo Plus from Amazon](https://www.theverge.com/2017/9/27/16375984/amazon-echo-plus-hands-on-impressions-photos)

We started class off by writing a block of code that had a char-typed pointer pointing to the string 'hello' and a char-typed array containing 'hello'. At the end the code contained a print statement that referred to the address of 'hello'. Afterwards, the class went over why the print statement with the 'hello' address was the same as the pointer's address as opposed to the array's address. Then we went to talk more about strings and memory addresses.

```C
#include <stdio.h>
#include <stdlib.h>

int main(){
char *s = "hello";
char s1[] = "hello";
printf("s points to: %p\n", s);
printf("s1 points to %p\n", s1);
printf("address of \"hello\": %p\n", &"hello");

// -----------------------------------------------

char s2[5];
printf("s: %s\n", s);
s2[0]= 'h';
s2[1] = 'i';
s2[2] = 0;
printf("s2: %s\n", s2);

return 0;
}
```

### About that address...
* We determined that in the first part of the block of code above, &hello refers to an address of the literal string 'hello' which array s1 does not do.

### What are strings?
* Strings are char arrays, or an array of char.
* By convention, strings end with a null character.
* Examples include either `''` or `C 0 ` or ` '\0' ` (Escape 0). We do this for the escape 0 because it is an ascii value.
* IMPORTANT: The null character is useful for using while loops instead of length. Why? Because we do not know the length of the string in C and we cannot return the size back.

### More on the null
* When you make a literal string using ` "" `, an immutable string is created in the memory. These strings are automatically null terminated.

### How does C know our address?
* When using multiple literals, C does not make a copy of them and C will refer to the same string.
* All references to the same literal string refers to the same immutable string in the memory.

### Declaring Strings
* `char s[256]` is a normal array declaration. This array allocates 256 bytes. We could use all 256 bytes, but some things could break .
* It is important that we use 255 bytes of this array and leave the last byte null.

### Examples:
```C
char s[256] = "Imagine"; // Allocates 256 bytes and creates an immutable string "Imagine"
// It then copies (including the terminating null) into the array
```
```C
char s[] = "Tuesday"; //Array s allocates 8 bytes.
// It creates an immutable string "Tuesday" and then copies the string and the null into the array.
```

### The last part of the block of code in the Do Now!
* Given our array s2 with 5 bytes, we fill in index 0 to 1 with 'hi', followed by the terminating null.
* IMPORTANT: After the null, anything that we put in the array afterwards will not be displayed nor is used by the array.

---
## Tuesday, 9/26 Let's Head to Function Town, by Jonathan Wong

**Tech News** [Twitter is experimenting with 280-char tweets](https://www.theverge.com/2017/9/26/16363912/twitter-character-limit-increase-280-test)

Once we came into class, we were asked to bring up a copy of HW#3, the array_swap hw using pointers. Upon the completion of this, we went over how to create functions again.  

### Passing Arrays

* Arrays are pointers, and thus only store a memory address
*This means that if you pass an array to a function, you're simply passing the memory address, NOT a copy.

```C
int arr[2];
arr[0] = 0;

void changeArr(int arr[]){
  arr[0] = 5;
}

changeArr(arr);

printf("arr 0 is %d\n", arr[0]); // will return 5
```

### Getting around pass by value

* If we want to use a function to change a variable, we can pass a pointer as a parameter to the function.

### Using Functions in a Procedural-based language
* Since C is a procedural based language, using functions can get a little tricky, especially when calling functions within other functions. This leaves three options.
  1. Declare all the functions in such an order such that the functions that other functions rely on are declared first. This can get messy and very confusing.
  2. C has something called function headers. They look like this.
  ```C
  void changeArr(int arr[]);
  ```
  Or this.
  ```C
  void changeArr(int* );
  ```
  A function header is simply the first line of the function. It does not require parameter names, as it only type checks.

  3. Option 2, but outsourcing the function headers to another file. In C, these are header files, and they typically end in `.h`. To do this, you need to let the compiler know where your header file is.
  ```C
  #include “PATH/filename.h”
  ```
  Note that angle brackets are not included, being replaced by quotes instead. This is because the angle brackets are for special locations like `usr/bin` where standard libraries are stored.

### Misc.

* You can use pointers and arrays in very similar manners.
---
## Thursday, 9/25 How to Write Functioning Code, by alex lu

**Tech News** [In flight Netflix will be available on more airlines in 2018](https://www.engadget.com/2017/09/25/in-flight-netflix-comes-to-more-airlines/)

We kicked off class with a quick presentation of Charles' homework, followed by some relevant questions pertaining to pointers.
### Pointers and Arrays
* Array variables are immutable pointers
* Pointers can be assigned to array variables

We learned that `array[17]` is simply shorthand for `*(array + 3)`. This can get us into some fun and interesting trouble.
```
int array[100];
array[17] = *(array + 17)
==> *(17 + array)
==> 17[array]
```
Hm... This doesn't seem too logical. Don't do it! But this emphasises that `array[17]` is nothing more than shorthand, so the communative property gives us the strange expression `17[array]`. Let's talk about functions.

### Function Stuff
* Header format: <return type>, <name>, (<arguments>){...}
* ALL functions are "pass by value"
* This means that if a fxn `foo(int a)` takes in some int k, the fxn will create a copy of k, named a: `foo(k) ==> a = k`
* The arguments are just variables containing the value passed to them
---
## Wednesday, 9/20 Try not to hurt yourself, the point is Sharp by Max Chan

**Tech News** [Despite all odds, Hyperloop One just raised another $85 million](https://techcrunch.com/2017/09/22/despite-all-odds-hyperloop-one-just-raised-another-85-million/)

We started off class today by finishing up arrays from the previous day.  The array variable only tracks where the memory allocated for the array is, which is why we can go outside the bounds of the array, and won't get an error.  Then we moved onto pointers.

### Pointers
* Pointers are variable designed to store memory addresses.
* `*` is used to denote a pointer variable (`int *p` `double *q` `char *r`)
* We discussed different type of pointer variables, including int, double, and char
* Because pointer variables, regardless of type, all point to a memory location, they're all the same size.  Today, most machines will allocate 8 bytes of memory to a pointer variable, while previously it was 4 bytes (Which meant the maximum memory your program could have was 4GB)
* Pointer variables story the addresses in hexidecimal format
* When printing pointers, use the `%p` formatting character
* `&` is used to get the address of a variable:
```C
int i;
int *p;
p = &i; //p now stores the address of the int i
```
* `*` is also the dereference operator, which accesses the value at a memory location:

```C
int i = 42;
int *p = &i;
printf("i = %d\n", *p); // prints "i = 42"
```

### Pointer Arithmetic
C will automatically add the right increment of memory to a pointer variable, based on its type:
```C
int *p = &i;
char *c = &l;
p++; // will add 4 to p
c++; // will add 1 to c
```

The reason 4 is added to p is because p points to an int, which has 4 bytes of memory.  Likewise, 1 is added to c because a char is allocated only 1 byte.


## Tuesday, 9/19 Arrays and Memory by Winnie Chen
**Tech News:** [When Driver and Car Share the Same Brain](http://nautil.us/issue/51/limits/when-driver-and-car-share-the-same-brain)
---
**Aim:** What’s the point of it all?

### Arrays

Declaring an Array

`<elements' type> <array name>[<array size>]`
* E.g. ` int b[5]`
* An array must have a fixed size set at declaration as a literal (not a variable name nor a function).
* When declaring an array, the program is only requesting memory (generally thought to be consecutive) from the OS. The data currently in the memory is not changed.
* In this case, the program is requesting 20 bytes (5 * 4 bytes) of memory from the OS.
* This memory is allocated at compile time, so one cannot use expressions, which would be evaluated at runtime, as the array size.

    E.g. ` int b[5 + 1]` will not work because the 5 + 1 part is evaluated at runtime, not compile time.
* There is something different called dynamic memory allocation, which is done at runtime.

There is no length function, but the ` sizeof(<data type or expression>)` function can be used to calculate the length of the array in this way: ` sizeof(<array name>) / sizeof(elements’ type>)`

* E.g. `sizeof(b) / sizeof(int)`

There is no boundary checking.

* When you reference something using an out-of-bounds index, there is no warning nor compiler error.

    E.g. `printf(“%d\n”, b[5]);`

    This acts normally, printing the current value in the memory there (around the memory specified for the array).

    Imagine the memory:

    |Memory                   |            |        |        |        |        |        |           |           |
    |:------------------------|:-----------|:-------|:-------|:-------|:-------|:-------|:----------|:----------|
    |Address (4 bytes/element)|996         |1000    |1004    |1008    |1012    |1016    |1020       |1024       |

    If the array starts at 1000, the memory specified for the array spans from the block with the address 1000 to the block with the address 1016, but there still exists memory before and after that, which can be referenced with out of bounds indices.

    One can also modify/reference variables using out of bounds arrays, although it is not a good idea to do so.

    Different compilers allocate memory differently. For example, some compilers pad memory, so there are blocks of memory between each variable.

    Nowadays, we have protected memory, preventing our programs from referencing memory that other programs are using. Doing so will result in a segmentation fault because the OS will not allow the protected memory to be referenced.



---
## Monday, 9/18 More on Variables + Arrays by Mansour Elsharawy

**Tech News!:** [ANDROID OREO FEATURES Y'ALL](http://www.androidauthority.com/android-8-0-review-758783/)

Aim: A vast array of possibilities

In the Do Now, we discussed with our partners any interesting, weird, or confusing things/issues we found while working on our Project Euler problems. Then we delved a more into variables in C!

### More on Variables!

First off, character literals can be denoted by single quotes instead of their numerical denomination.
Example: `char c = 'A';` and `char c = 65;` effectively do the same thing, but one is much more obvious as representing the character A.

String literals also exist, even though there is no String data type in C.
Example: `"Hello!"`

When declaring variables, the *order* in which you declare variables (and by extension, functions) matters.
Example...

```C

int i = 7;
int j = i + 1;
```
...is an entirely viable piece of code. *However...*

```C
int j = i + 1;
int i = 7;
```
...this will not work.

Variables also cannot be initialized in for loops in C, unlike Java, but can be initialized beforehand.

Example of proper for loopage:

```C

int i;
for(i = 0; i < 10; i++){
printf("i is %d\n",i);
}
```

Variables can also be declared as "unsigned," prohibiting the variable from taking on a negative variable.
This expands the range of values a variable can have, with 0 as the lower bound and a higher upper bound than an unsigned variable.
Example: `unsigned char x;` 0 <= x <= 255, where just `char x;` has -128 <= x <= 127

Booleans don't exist in C, but any variable with a value of 0 is regarded as false. *Any* other value evaluates to true.

Example of an infinite loop:
```C
while(1){
printf("To infinity and beyond!");
}

```

**IMPORTANT**
```C
int x=5;
if(x=6){
printf("foo");"
}
```
Even though the intended purpose of this code may be to skip the block containing the print statement, "foo" will be printed, as C will interpret the assignment of x to 6, which returns what x was assigned to (6), and that will evaluate as true.

Sometimes we may actually want this effect in the future!

### Memory Management

Memory allocation can happen at either compile or run time (as it is dynamic).
When the compiler sees you declaring a character variable for example, it will reserve 8 bits in memory for it, and is packaged with the binary of the program.

There is *no* default value for any variable. You get assigned a piece of memory which *might* be zero, but no guarantees!

We also briefly touched arrays, but the bell rang before we got anywhere meaningful besides the initialization:

```C
int l[5];
```
Cheers!



---
## Thursday, 9/14 Primitives by Jeremy Sharapov

**Interesting News:** [US bans Kaspersky software from government agencies](https://www.cnet.com/news/us-bans-kaspersky-software-from-government-agencies-trump-dhs-russia/)

**Bonus (I'm sick of Apple so here's some Google):** [Pixel 2 Launches on October 4th](https://arstechnica.com/gadgets/2017/09/google-teaser-site-promises-a-pixel-2-launch-on-october-4/)

Aim: Always read the fine print!

For our Do Now, we listed all of the primitives in Java, which are:
* int
* char
* bool
* float
* double
* byte
* short
* long

In C we have all of them except bool and byte, and all of them are numeric, with the only differences between them being if they include floating points and the size of the variable in memory.
However, do note that the size can be platform dependent; use `sizeOf(<Type>)` to find the size in bytes.

|  Type  | Standard Size (in bytes) |                          Range                         |
|:------:|:------------------------:|:------------------------------------------------------:|
|  char  |             1            |                       -128 - 127                       |
|   int  |             4            |             -2,147,483,648 - 2,147,483,647             |
|  short |             2            |                    -32,768 - 32,767                    |
|  long  |             8            | -9,223,372,036,854,775,808 - 9,223,372,036,854,775,807 |
|  float |             4            |                    7 digit precision                   |
| double |             8            |                   14 digit presicion                   |

`printf()` - `stdio.h`
  * The most important C function
  * Prints formatted Strings
  * `printf(<String literal>, <var 1>, <var 2>, ...); `
  * Simple printf does not need the <var> arguments
  * printf does not add a newline at the end! Use `\n`
  * If you want to print variables, you must include formatting placeholders in the String argument
  * ex.
    ```C
    int i = 5;
    int main(){
      printf("i is %d", i);
      return 0;
    }
    ```
    print outs `"i is 5"`

|         Type        | Formatting Character |
|:-------------------:|:--------------------:|
|         int         |          %d          |
|         long        |          %ld         |
|        float        |          %f          |
|        double       |          %lf         |
|         char        |          %c          |
| char array (String) |          %s          |
|       Pointer       |          %p          |

---
## Wednesday, 9/13 Variables are the Spice of Life by Jennifer Zhang

**Interesting News:** [Apple Face ID](http://www.popsci.com/apple-face-ID)

* `man` is a command that will display a user manual
    * The manual is divided into sections (we talked about the first three):
        1. executable programs or shell commands
        2. system calls
        3. library routines
    * If you wanted to look up something from the manual, the command would be `man **keyword**`
        * For example:
            * `man ls`
            * `man man`
            * `man stdio`
    * Some functions may have the same name, and you might want to be able to specify which section the function you want is in.
        * For example, `man printf` will automatically give you the page for a printf in the 1st section. If you wanted to look up a printf from stdio, however, (stdio is in the 3rd section) you would instead type `man 3 printf`. (see below for more details)

If you recall from yesterday, when we ran our Hello World code, we were given a warning when using the function `printf()`:
* `printf()` is part of stdio, which is the standard input/output library.

When compiling your C code, `gcc` will go through two steps:
1. Checks if everything matches (syntax, function names, arguments and return type, etc.)
   * If it finds a function it does not see defined in the code, it assumes that you know what you're talking about, and will give you a warning.
2. Links the code for the external function
    * `gcc` can automatically link functions in standard libraries

Although warnings will still allow you to compile and run your code, it is ideal if your code has no warnings!

* `#include`
    * Include it in the header of your code
    * Put system libraries inside <>
    * Two major libraries we will be using:
        1. `#include <stdio.h>`
        2. `#include <stdlib.h>`

---
## Tuesday, 9/12 Hello, World! by Jake Goldman

**Interesting News:** [Can AI recreate a game engine?](https://www.digitaltrends.com/computing/ai-super-mario-bros-game-engine/)

**Bonus:** [Obligatory iPhone X hype](https://www.wired.com/story/apple-iphone-x-iphone-8/)

We began class by looking at a basic hello world program in C:

```C
int main(){
  printf("Hello Everybody!\n");
  return 0;
}
```
After running the code, we discussed several observations:
* The `main()` method in C doesn't have arguments
* `printf()` is used to print to the command line (it doesn't add a newline at the end)
* The `main()` method can have a return type (usually this will be an int and can indicate how a program terminated)

We then moved on to discuss why we use `./filename` to run C executables. In general, when one uses a command, the machine will search through certain directories stored in a variable known as PATH. As an example, when someone uses `ls`, the computer will go through each of the directories stored in PATH to find a file named `ls`, which will then be run. The computer only searches in these predetermined directories as a means of security, as certain programs could theoretically run themselves, leading to all kinds of potential issues. By specifying the directory of the executable we are trying to run, we are telling the machine that this is a safe file, and showing where to run it.

History of C:

The C language was developed by Denis Ritchie in the early 70s while working on UNIX at Bell Labs. The language ended up being the basis of the UNIX kernel, and in 1978, Ritchie along with Brian Kernighan published "The C Programming Language," which is considered to be one of the best books ever written about a programming language.

C Design Choices:
* C is designed to match very closely with machine level instructions (this creates leaner and faster programs)
* C is a procedural language (NOT object oriented)
* C syntax is very similar to Java syntax (Java syntax was based on C)

---
## Monday, 9/11 Basic C Code by Helen Ye

**Interesting News:** [Pluto's features are being named!](http://www.skyandtelescope.com/astronomy-news/first-pluto-features-officially-named/)

#### Aim: Let's C what you can do

Basic C code
* Source Code:
	* Put in .c files (convention)
	* Naming convention: snake_case
* Compiling (with command `gcc`)
	* Compiles to architecture specific (OS + Processor) executable files
	* Default name of compiled file is a.out
	* gcc option: -o \<file\> compiles to the given file name
	* Run file with ./\<file\>
		* C checks the $PATH for the file if you just type in the file name, unless you specify  it's directory

---
## Wednesday, 9/6 Blissfull respite, I hardly knew ye by Clyde "Thluffy" Sinclair

**Interesting Tech News:** [Do You Even Olin?](https://blog.ledwards.com/the-college-that-produces-founders-at-3-times-the-rate-of-stanford-2c53ea44f91e)

Don't forget, each new post should go above the one before. If you are late in posting, please put yours in the correct position.

#### Look, it's a list! ####
* Blah
* Blah
* Blah

Below you'll find a horizonal bar, please put one at the end of your post.

---
