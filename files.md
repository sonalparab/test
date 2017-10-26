# 10/24 Aim: File this under useful information

What are the different kinds of file permissions?

## File permissions
* read|write|execute
* user|group|others

Permissions can be represented as 3-digit binary #s, or 1-digit octals.
* 100 <==> 4 => read, only
* 111 <==> 7, read, write, execute

There are 3 permission "areas": user, group, others. Each area can have its own
permissions.
* 0644 => user: read+write, group: read, other: read

## File Table
A list of all the files used by a program while it is running. Contains basic
information like the file's location & size.

The file table has a limited size, which is a power of 2 and commonly 256.
`getdtablesize()` will return this value.

*NOTE:* `getdtablesize()` *IS DEPRECATED*

Each file is given an integer index, starting at 0, this is referred to as the
file descriptor.

There are 3 files always open in the table:
* 0 or `STDIN_FILENO`: stdin
* 1 or `STDOUT_FILENO`: stdout
* 2 or `STDERR_FILENO`: stderr

# 10/25 Aim: Opening up a world of possibilities

## `open - <fctnl.h>`

Adds a file to the file table and returns its file dscriptor.
If open fails, -1 is returned, extra error information can be found
can be found in `errno`.
* `errno` is an int variable that can be found in `<errno.h>`, using
  strerror (in `string.h`) on errno will return a string description
  of the error.

`open( <PATH> , <FLAGS> , <MODE> )`

* mode
  - Only used when creating a file. Set the new file's permissions using a
    3 digit octal #
* flags
  - Determine what you plan to do with the file
  - Use only the following constants:
    * `O_RDONLY`
    * `O_WRONLY`
    * `O_RDWR`
    * `O_APPEND`
    * `O_TRUNC`
    * `O_CREAT`
    * `O_EXCL`: when combined with `O_CREAT`, will return an error if the file
      exists
  - Each flag is a number, to combine flags we use bitwise OR
    ```
    O_WRONLY = 1            00000001
    O_APPEND = 8            00001000
    O_WRONLY | O_APPEND =   00001001
    ```
