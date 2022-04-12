---
layout: default
---

# Programming Assignment 3 - RISC-V Code Generation
\[[PA-3]({{site.data.compiler.pa3.pdf}})\]

# Notes on PA 3

**Input**:      Annotated AST  
**Output**:     RISC-V Code  

The assembly program does not need to perfectly match the reference compiler. The student compiler RISC-V program should print the same output as the reference compiler.

## Running and Testing Output

The output is written in RV32IM, a 32-bit version of RISC-V.

- Command to **generate** RISC-V code with student compiler:  
`java -cp "chocopy-ref.jar:target/assignment.jar" chocopy.ChocoPy --pass=rrs ./src/test/data/pa3/sample/<file> --out <assembly>`

- Command to **generate** RISC-V code with reference compiler:  
`java -cp "chocopy-ref.jar:target/assignment.jar" chocopy.ChocoPy --pass=rrr ./src/test/data/pa3/sample/<file> --out <assembly>`

- Command to **run** RISC-V code:  
`java -cp "chocopy-ref.jar:target/assignment.jar" chocopy.ChocoPy --run <assembly>`

## File Structure

Here are some of the classes that are a part of this assignment (we start at `/src/main`):

- **`java/chocopy/`**:
    - **`pa3/`**:
        - **`StudentCodeGen.java`**: This is the _entry point_. The `process` method takes the annotated AST and returns the RISC-V program.
        - **`CodeGenImpl.java`**: Implementation of `CodeGenBase`.
    - **`common/codegen/`**:
        - **`CodeGenBase.java`**: Abstract class with infrastructure for code generation. Don't edit. Sub-class to change.
        - **`*.java`**: Support classes for code generation infrastructure.
- **`asm/chocopy/common/`**:
    - **`*.s`**: Assembly implementations of built-in functions. Copied by `CodeGenBase` into the output program.

## Venus Simulator

Venus simulator helps us execute our generated code in a platform-independent manner. We use **Venus 164**, which is automatically added to the project using the `pom.xml`. It is added at build time.  
Venus 164 was created at UCB and is meant mainly to increase conformance to the GNU RISC-V toolchain through the commands listed below:
- **`.word <label>`**: Emit address in the data segment.
- **`.align <n>`**: Specify byte alignment in the data segment.
- **`.string <str>`**: Emit ASCII strings.
- **`.space <n>`**: Inserts `n` zero-bytes into data segment.
- **`.equiv <sym>, <val>`**: Defines label `<sym>` to have value `<val>` (`<val>`may be a numeral or another symbol).  

Here are some other Venus 164 resources:
- [Venus 164 front-end]({{site.data.compiler.links.venus164-front-end}}){:target="_blank"} with [docs]({{site.data.compiler.links.venus164-front-end-docs}}){:target="_blank"}.
- [Visual Studio Code Extension]({{ site.data.compiler.links.vs-code-venus-extension }}){:target="_blank"}.

## Memory Management

- Each program has **32 MB** to work with.
- The `gp` register points to the beginning of the heap before the program is executed.
- The reference compiler does _not_ implement garbage collection (GC).

## Error Handling

- Your code must generate the appropriate run-time error messages and exit codes. There is no need to hand code these things, there is some in-built stuff to handle this.
-  For _invalid args_ and _out-of-memory_, the code is already provided.
- For _operation on None_, _division by zero_ and _index out-of-bounds_ there are built-in routines emitted by `CodeGenImpl.emitCustomCode`.

## Extensions and Optimizations

The extension code for **const** and **walrus** needs to be written.  

Also, you need to implement 2 optimizations from chapter 8 and 9 in the textbook. These can be pretty straightfoward.  
Compilers that generate instructions with the fewest execution cycles will get a shout out at the end of the course.

## Implementation Notes


