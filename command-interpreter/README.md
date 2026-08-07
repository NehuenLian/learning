## UNIX command interpreter

### How it works?

First of all, a shell needs to run inside a loop to give the prompt to the user every time a command ends.
Inside this loop are 3 main features:
- Read the textline entered
- Split the command text to separate it in command name and arguments/flags
- Execute the command 

### To read a line:
The "text field" is a dynamic array (buffer) initialized with a size of 1042 bytes. Each time a command 
is entered we take every single char one by one. If any of them represents the last buffer position we 
need to expand the buffer to avoid buffer overflows using realloc.
If the char we are reading is equal to "`EOF`" or '`\n`' it means its the end of the command entered, so we return the buffer.

### To split the command:
We finally have the command that the user entered but is still a single string. 
To split it, we will use the `strtok()` function with a delimiter, in this case the delimiters are " " and "\n" principally.
By doing this we are separating the string in N individual strings (depending on the command and the number 
of args/flags) using the white spaces or the EOFs (\n) and storing each part in a buffer, so we can now 
return the buffer with the args inside and passing it to the execute function.

### To execute a command:
When the execute function receives the command args (args is the array returned by the split function), the first 
step is to verify if the command is a builtin command. Builtin command = a command that executes itself in the same 
shell, without the need of calling `fork()` (`cd` is a good example of a builtin command).
If the command is a builtin command, the code has an array that stores the memory address for the implementation 
of that command. (A local function that calls chdir, in the case of `cd`)
If the command isn't a builtin command we need to make a fork(); of the shell process, then, `execvp` will receive the command name (`args[0]`) and the args/flags (`args`) and then wait for the child process to finish before continuing.

<br>
    
--- 

<br>

This project is an UNIX shell made from the following guide: [brennan.io/2015/01/16/write-a-shell-in-c/](https://brennan.io/2015/01/16/write-a-shell-in-c/) that was in the build your own x repository: [github.com/codecrafters-io/build-your-own-x](https://github.com/codecrafters-io/build-your-own-x).

Only compatible with UNIX-based systems.

Compile:  
`gcc main.c -o main`

Execute:  
`./main`

- Use the command "help" to see available commands
