---
layout: default
---

# Programming Assignment 3 - RISC-V Code Generation
\[[PA-3]({{site.data.compiler.pa3.pdf}})\]

# Notes on PA 3

**Input**:      Annotated AST  
**Output**:     RISC-V Code  

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

Venus simulator helps us execute our generated code in a platform-independent manner. We use Venus 164, which was created at UCB and is meant mainly to improve conformance to the GNU RISC-V toolchain in the following way:
- **`.word <label>`**: Emit address in the data segment.
- **`.align <n>`**: Specify byte alignment in the data segment.
- **`.string <str>`**: Emit ASCII strings.
- **`.space <n>`**: Inserts `n` zero-bytes into data segment.
- **`.equiv <sym>, <val>`**: Defines label `<sym>` to have value `<val>` (`<val>`may be a numeral or another symbol).  
  
Venus 164 is automatically added to the project using the `pom.xml`. It is added at build time.