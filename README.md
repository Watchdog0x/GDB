
# GDB Advanced Cheat Sheet

## Starting GDB
```bash
gdb program                     # Start GDB with a program
gdb program core                # Start GDB with a core dump
gdb program --pid <pid>         # Attach to running process
gdb -tui program               # Start GDB with text user interface
```

## Basic Control
```bash
run [args]                      # Start program with [args]
start                          # Start program and break at main
kill                          # Kill running program
quit                          # Exit GDB
set args arg1 arg2            # Set program arguments
show args                     # Show current arguments
```

## Breakpoints and Watchpoints
```bash
# Setting Breakpoints
break main                    # Break at main
break *0x12345678            # Break at address
break file.c:123             # Break at file and line
break class::function        # Break at C++ function
break +2                     # Break 2 lines from current
break function if condition  # Conditional breakpoint

# Watchpoints
watch var                    # Break when var is modified
rwatch var                   # Break when var is read
awatch var                   # Break when var is read/written

# Manage Breaks
info breakpoints             # List breakpoints
delete 1                     # Delete breakpoint #1
delete                       # Delete all breakpoints
disable 1                    # Disable breakpoint #1
enable 1                     # Enable breakpoint #1
clear function              # Clear breaks at function
```

## Memory Examination (x command)
```bash
# Format Letters:
x/o - octal
x/x - hexadecimal
x/d - decimal
x/u - unsigned decimal
x/t - binary
x/f - float
x/a - address
x/i - instruction
x/c - character
x/s - string

# Size Letters:
x/b - byte (1 byte)
x/h - halfword (2 bytes)
x/w - word (4 bytes)
x/g - giant word (8 bytes)

# Common Combinations
x/10xb $sp                  # 10 bytes in hex from stack pointer
x/10i $pc                   # 10 instructions from program counter
x/s buffer                  # String at buffer
x/10xg $rsp                 # 10 giant words from stack
x/10wx array               # 10 words from array
```

## Stack Manipulation
```bash
# Stack Navigation
bt                          # Backtrace (stack frames)
bt full                     # Backtrace with locals
frame N                     # Select frame N
up                         # Move up one frame
down                       # Move down one frame
info frame                 # Info about current frame
info args                  # Show function arguments
info locals                # Show local variables

# Stack Examination
x/10xg $sp                 # Examine 10 quad-words at stack
x/x $rbp+8                 # Examine value at frame base + 8
```

## Data Examination
```bash
# Print Commands
print var                   # Print variable
print/x var                # Print in hex
print/t var                # Print in binary
print *array@10            # Print 10 elements of array
print $eax                 # Print register eax
print {int}0x123           # Print memory at 0x123 as int

# Display Commands
display var                # Display var on each break
undisplay 1               # Remove display #1
info display              # Show all displays
```

## Register Commands
```bash
info registers            # Show all registers
info registers eax ebx   # Show specific registers
print $eax               # Print eax value
print/x $eax            # Print eax in hex
set $eax = 123          # Set eax to 123
```

## Execution Control
```bash
# Stepping
stepi                    # Step one instruction
nexti                    # Step over instruction
step                     # Step one line (source)
next                     # Step over line
finish                   # Run until function returns
continue                 # Continue execution
until 123               # Run until line 123

# Jump Commands
jump *0x12345678        # Jump to address
jump +2                 # Jump 2 lines forward
```

## Source Code
```bash
list                    # List source code
list main              # List main function
list 1,20              # List lines 1-20
list file.c:main       # List main in file.c
directory path         # Add source directory
```

## Process Information
```bash
info proc mappings     # Show memory map
info functions         # List functions
info variables        # List variables
info sharedlibrary    # List shared libraries
```

## TUI (Text User Interface)
```bash
tui enable            # Enable TUI
tui disable           # Disable TUI
layout asm            # Show disassembly
layout src            # Show source
layout regs           # Show registers
focus cmd/src/regs    # Change focus
refresh               # Refresh screen
```

## Advanced Features
```bash
# Core Dumps
generate-core-file    # Generate core dump
core-file core        # Load core file

# Catchpoints
catch throw           # Catch C++ exceptions
catch syscall         # Catch system calls

# Scripts
source script.gdb     # Run GDB script
define cmd           # Define new command
end                  # End command definition
```

## Tips and Tricks
```bash
# Command History
show commands         # Show command history
save breakpoints f    # Save breakpoints to file
source f             # Restore breakpoints

# Shell Commands
shell command        # Run shell command
make                 # Run make

# Convenience Variables
set $i = 0           # Set variable
print $i             # Print variable
```


## CheatSheet 


x/s &Sample                 ; prints the whole string with escaping

printf "%s\n", 0x601065     ; prints the whole string without escaping

set {type}40050a=0x20       ; Chnage the value of bit (type can be int, char ....)
