# StackLanguage2hackasembler

A Rust-based virtual machine translator that converts VM code to Hack assembly language. This project includes both a command-line translator and a web interface for interactive use.


![](https://raw.githubusercontent.com/jero98772/StackLanguage2hackasembler/refs/heads/main/pictures/2025-07-06-115102_1920x1080_scrot.png)


## Overview

This VM translator is designed to work with the Hack computer platform, translating high-level VM commands into low-level assembly instructions. It supports all major VM command types including arithmetic operations, memory access, program flow control, and function calls.

## Features

### Supported VM Commands

- **Arithmetic Operations**: `add`, `sub`, `neg`, `eq`, `gt`, `lt`, `and`, `or`, `not`
- **Memory Access**: `push`, `pop` with support for multiple memory segments
- **Program Flow**: `label`, `goto`, `if-goto`
- **Function Operations**: `function`, `call`, `return`

### Memory Segments

- **constant**: Constant values
- **local**: Local variables
- **argument**: Function arguments
- **this/that**: Heap access pointers
- **static**: Static variables
- **temp**: Temporary variables (R5-R12)
- **pointer**: Direct access to THIS/THAT pointers

### Additional Features

- Comment and whitespace handling
- Inline comment removal
- Automatic bootstrap code generation
- Function scope management
- Label scoping within functions

## Installation

Make sure you have Rust installed on your system. If not, install it from [rustup.rs](https://rustup.rs/).

```bash
# Clone the repository
git clone https://github.com/jero98772/StackLanguage2hackasembler
cd vm-StackLanguage2hackasembler

# Build the project
cargo build 
```

## Usage

### Command Line

```bash
# Run the translator
cargo run

# Or run the compiled binary
./target/release/vm-translator
```

### Web Interface

The project includes a web interface that runs on `localhost:8000`:

```bash
# Start the application
cargo run

# Open your browser and navigate to:
# http://localhost:8000
```

The web interface allows you to:
- Input VM code directly in the browser
- See the translated assembly output in real-time
- Download the generated assembly files
- Load and process multiple VM files

## Input Format

The translator accepts VM files with the following command formats:

### Arithmetic Commands
```
add         // x + y
sub         // x - y
neg         // -x
eq          // x == y
gt          // x > y
lt          // x < y
and         // x & y
or          // x | y
not         // !x
```

### Memory Access Commands
```
push segment index    // Push value to stack
pop segment index     // Pop value from stack
```

### Program Flow Commands
```
label LABEL_NAME      // Define a label
goto LABEL_NAME       // Unconditional jump
if-goto LABEL_NAME    // Conditional jump
```

### Function Commands
```
function functionName numVars    // Define function
call functionName numArgs        // Call function
return                          // Return from function
```

## Output

The translator generates Hack assembly code that can be run on the Hack computer platform. The output includes:

- Stack manipulation instructions
- Memory access operations
- Jump and branching logic
- Function call/return mechanisms
- Bootstrap initialization code

## Architecture

### Core Components

- **Parser**: Reads and parses VM commands from input files
- **CodeWriter**: Generates corresponding assembly code
- **Command Types**: Enumeration of all supported VM command types

### Key Design Decisions

- **Function Scoping**: Labels are scoped within functions to avoid conflicts
- **Memory Management**: Efficient stack operations with minimal assembly instructions
- **Bootstrap Code**: Automatic generation of system initialization code
- **Error Handling**: Robust error handling for file I/O and parsing operations

## Examples

### Basic Arithmetic
```vm
// VM Code
push constant 7
push constant 8
add
```

```asm
// Generated Assembly
@7
D=A
@SP
A=M
M=D
@SP
M=M+1
@8
D=A
@SP
A=M
M=D
@SP
M=M+1
@SP
M=M-1
A=M
D=M
@SP
M=M-1
A=M
M=M+D
@SP
M=M+1
```

### Function Calls
```vm
// VM Code
call Math.multiply 2
```

```asm
// Generated Assembly (simplified)
@RETURN_0
D=A
@SP
A=M
M=D
@SP
M=M+1
// ... (save LCL, ARG, THIS, THAT)
@Math.multiply
0;JMP
(RETURN_0)
```


### Building
```bash
# Debug build
cargo build
```

## License

This project is licensed under the GPLv3 License - see the LICENSE file for details.

## Acknowledgments

- Based on the Hack computer platform specification
- Inspired by the Nand2Tetris course
- Built with Rust for performance and safety
