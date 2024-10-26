https://x64.syscall.sh/

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
x/units|format|size addr

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

## GDB Essential Display Commands
```bash
# Common Display Commands
display/i $rip              # Display current instruction
display/5i $rip            # Display next 5 instructions
display/x $rax             # Display rax in hex
display/s $rsi             # Display string pointer in rsi
display/wx $rsp            # Display word at stack pointer
display/2wx $rsp           # Display two words at stack pointer

# Useful Combinations
display/i $rip             # Current instruction
display/x $rsp             # Stack pointer in hex
display/x $rbp             # Base pointer in hex
display/10i $rip           # Next 10 instructions
display/8xg $rsp          # 8 quadwords from stack

# Remove Displays
undisplay                  # Remove all displays
undisplay 1               # Remove display #1
disable display 1         # Temporarily disable display #1
enable display 1          # Re-enable display #1
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


# X86 Assembly Control Flow Cheat Sheet

## Conditional Jump Instructions
| Instruction | Description | Condition Flags |
|-------------|-------------|-----------------|
| je  | Jump if equal | ZF=1 |
| jne | Jump if not equal | ZF=0 |
| jg  | Jump if greater | ZF=0 and SF=OF |
| jl  | Jump if less | SF≠OF |
| jle | Jump if less than or equal | ZF=1 or SF≠OF |
| jge | Jump if greater than or equal | SF=OF |
| ja  | Jump if above (unsigned) | CF=0 and ZF=0 |
| jb  | Jump if below (unsigned) | CF=1 |
| jae | Jump if above or equal (unsigned) | CF=0 |
| jbe | Jump if below or equal (unsigned) | CF=1 or ZF=1 |
| js  | Jump if signed | SF=1 |
| jns | Jump if not signed | SF=0 |
| jo  | Jump if overflow | OF=1 |
| jno | Jump if not overflow | OF=0 |
| jz  | Jump if zero | ZF=1 |
| jnz | Jump if not zero | ZF=0 |

## Important Flags
- **Carry Flag (CF)**: Set when 65th bit is 1
- **Zero Flag (ZF)**: Set when result is 0
- **Overflow Flag (OF)**: Set when result wraps between positive/negative
- **Sign Flag (SF)**: Set when result is negative (signed bit set)

### Flag Register Quick Reference
```
RFLAGS Register Bits:
CF (0)  - Carry Flag
PF (2)  - Parity Flag
AF (4)  - Auxiliary Flag
ZF (6)  - Zero Flag
SF (7)  - Sign Flag
OF (11) - Overflow Flag
```

## Flag Updates
Flags are modified by:
1. Most arithmetic instructions
2. `cmp` instruction (like `sub` but discards result)
3. `test` instruction (like `and` but discards result)



# Ultimate PWN Cheat Sheet

## Advanced Memory Examination
```bash
# Memory Examination Patterns
x/10i $rip                # View next 10 instructions
x/10xg $rsp               # View 10 quad-words on stack
x/s $rdi                  # View string at rdi
x/wx $rbp-0x8            # View word at frame pointer - 8
x/16xb $rip              # View 16 bytes of machine code

# String Operations
x/s &variable            # Print string at variable address
x/s 0x401234            # Print string at address
printf "%s\n", 0x401234  # Print string without escaping

# Memory Modification
set {int}0x401234=0x90   # Write int to address
set {char}0x401234=0x90  # Write byte to address
set *0x401234=0x90       # Write word to address
```

## X86 Assembly Control Flow
### Conditional Jumps and Flags
```nasm
# Common Compare Patterns
cmp rax, rbx            # Compare rax and rbx
test rax, rax           # Test if rax is zero
and rax, rbx            # Logical AND
sub rax, rbx            # Subtract and set flags

# Flag Testing
je  label              # Jump if equal (ZF=1)
jne label              # Jump if not equal (ZF=0)
jg  label              # Jump if greater (signed)
jl  label              # Jump if less (signed)
ja  label              # Jump if above (unsigned)
jb  label              # Jump if below (unsigned)
```

## Common GDB Patterns for PWN
```bash
# Pattern Creation and Search
pattern create 100              # Create cyclic pattern
pattern search 0x61616161       # Search for offset

# Memory Operations
vmmap                          # Show memory mappings
find 0x7fff00000000,+2000,0x41414141  # Search memory range
telescope 0x401234 20          # View memory as different formats

# Breakpoint Patterns
break *0x401234               # Break at address
break *main+51                # Break at offset
break *main                   # Break at main start
commands 1                    # Commands to run at breakpoint 1
    silent
    x/i $rip
    continue
end

# Process Information
info proc mappings            # Memory layout
info registers               # All registers
info frame                   # Stack frame info
```

## One-liners and Tricks
```bash
# Quick Debug Commands
run < <(python -c 'print "A"*100')  # Run with python input
run < <(cyclic 100)                # Run with cyclic pattern
set follow-fork-mode child        # Follow child process
set disassembly-flavor intel     # Use intel syntax

# Memory Manipulation
set {int}($rbp-0x8)=0x41414141   # Write to stack variable
generate-core-file               # Create core dump
```

## Tips for PWN
1. Always check ASLR: `cat /proc/sys/kernel/randomize_va_space`
2. Useful environment control:
   ```bash
   unset LINES COLUMNS          # For clean core dumps
   set env LD_PRELOAD          # Remove LD_PRELOAD
   ```
3. Common breakpoint locations:
   - `main+0`
   - `*puts@plt`
   - `*system@plt`
   - Stack return addresses

4. Quick memory protections check:
   ```bash
   checksec --file=binary      # Check binary protections
   readelf -l binary          # Check segment permissions
   ```
