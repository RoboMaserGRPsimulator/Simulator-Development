# Quantric Project

Quantric is a dialect of [atriclang](docs/atric.md) which supports quantum programming from the language level. Please see [`docs/atric.md`](docs/atric.md) for more details.



## Technical Solutions

| Item                                   | Solution                                                     | Language                      |
| -------------------------------------- | ------------------------------------------------------------ | ----------------------------- |
| Command-Line Interface (CLI)           | [clap-rs](https://github.com/clap-rs/clap), [cxx-rs](https://github.com/dtolnay/cxx) | Rust                          |
| Lexer / Parser / Semantic Analyzer     | [ANTLR4](https://github.com/antlr/antlr4)                    | C++, g4                       |
| Atric AST to Atric-IR                  | MLIR                                                         | C++, TableGen, mlir           |
| Atric-IR to Quantric-IR Transformation | MLIR                                                         | C++, TableGen, mlir           |
| Atric-IR to LLVM-IR Transformation     | MLIR, LLVM                                                   | C++, TableGen, mlir, llvm     |
| Quantric-IR to QCIS-IR Transformation  | MLIR                                                         | C++, TableGen, mlir           |
| QCIS-IR to QCIS assmbly Transformation | MLIR, QCIS                                                   | C++, TableGen, mlir, QCISasm  |
| Quantric-IR to eQASM Transformation    | MLIR, [eQASM](https://arxiv.org/pdf/1808.02449)              | C++, TableGen, mlir, eQASM    |
| Quantric-IR to OpenQASM Transformation | MLIR, OpenQASM                                               | C++, TableGen, mlir, OpenQASM |

### CLI

The Command-Line Interface, mainly responsible for handling the input parameters, calling the compiler library (`libatric.so`), and output error messages. 

### Lexer / Parser / Semantic Analyzer

The front-end of the compiler, responsible for compiling the Atric source codes into the abstract syntax trees (AST), in which also contains a static type checker. This project will use ANTLR4 as the lexer and parser generator. The imports are processed (but the source codes will not be linked together), and the generic types and functions are statically distributed in the stage (each generic type or function will be compiled to zero or more times according to the number of the combinations of the generic parameters passed in when using it). The output of this stage is several ASTs.

### Atric-IR

Atric-IR is an intermediate representation of the verified Atric-AST, the built-in type and functions are reserved in this stage (for example the qubit type and quantum operations are still the same as normal built-in types and functions, respectively).

### Atric-IR to Quantric-IR Transformation

This transformation will transform all Atric-Operations to Quantric-Operations and the quantum functions are treated as independent Quantric-Operations in this process. This process will also verify if all the Atric-Operations used in the source are supported by Quantric (for example, references are supported by Atric-IR but not by Quantric-IR).

### Quantric-IR to QCIS-IR Transformation

This transformation will transform Quantric-IR to QCIS assembly. Since the QCIS assembly does not support syscalls and control flow, this transformation process also contains a verifier which is able to filter all invalid syscalls and control flow operations (which means that all the Atric standard libraries are not supported by Quantric). The first step of this process is function inlining, all non-built-in functions are inlined and all available built-in functions are reduced to operations defined in QCIS-IR. Notice that this transformation does not perform the classical operation of the IR in advance, therefore the branch predictions and the loop unfoldings are not done in this process. 

### QCIS-IR to QCIS assembly Transformation

TODO

Thinking: Adding a new layer of IR?

### Quantric-IR to eQASM Transformation

TODO

### Quantric-IR to OpenQASM Transformation

TODO



## Iteration Plan

TODO



## [Compiler Structure](https://textik.com/#2b514097f4367fef)

```
                                                                       
                          Quantric Source Code                         
                                   |                                   
                                   v                                   
                      +-------------------------+                      
                      |           CLI           |     Rust             
                      +-------------------------+                      
                                   |                                   
                       Source Code | Compile Options                   
                                   v                                   
                    +-----------------------------+                    
                    |          Front-end          |                    
                    |  +-----------------------+  |                    
                    |  |        Lexer          |  |                    
                    |  +-----------------------+  |                    
                    |  +-----------------------+  |                    
                    |  |     Preprocessor      |  |   C++/Antlr        
                    |  +-----------------------+  |                    
                    |  +-----------------------+  |                    
                    |  |        Parser         |  |                    
                    |  +-----------------------+  |                    
                    |  +-----------------------+  |                    
                    |  |   Semantic Analyzer   |  |                    
                    |  +-----------------------+  |                    
                    +-----------------------------+                    
                                   |                                   
                                   | Abstract Syntax Tree              
                                   v                                   
                    +-----------------------------+                    
                    |         Middle-end          |                    
                    |  +-----------------------+  |                    
                    |  |   Quantric Dialect    |  |                    
                    |  +-----------------------+  |                    
                    |  +-----------------------+  |   C++/MLIR         
                    |  |       Optimizer       |  |                    
                    |  +-----------------------+  |                    
                    |  +-----------------------+  |                    
                    |  |  MLIR Code Generator  |  |                    
                    |  +-----------------------+  |                    
                    +-----------------------------+                    
                                   |                                   
                                   | Quantric-IR                       
                                   |                                   
    +-------------------------------------------------------------+    
    |                  Back-end    v                              |    
    |                 +-------------------------+                 |    
    |                 | MLIR General Optimizer  |                 |    
    |                 +-------------------------+                 |    
    |                              |                              |    
    |          +-------------------|-------------------+          |    
    |          |                   |                   |          |    
    |          v                   v                   v          |    
    |  +---------------+   +---------------+   +---------------+  |    
    |  |     QCIS      |   |   OpenQASM    |   |      ...      |  |    
    |  +---------------+   +---------------+   +---------------+  |    
    |                                                             |    
    +-------------------------------------------------------------+    
                                                                       
```



## Learning Resources

### Compiler Frontend : [Antlr4](https://github.com/antlr/antlr4)

#### Tutorial

- [EECS4302 (Youtube)](https://youtube.com/playlist?list=PL5dxAmCmjv_4FGYtGzcvBeoS-BobRTJLq)
- [EECS4302 (Bilibili)](https://www.bilibili.com/video/BV1er4y1F7sa?p=1)

#### Example

- [Antlr Demo Project](resources/antlr-demo)


### Compiler Backend : [MLIR](https://mlir.llvm.org/)

#### Tutorial

- [MLIR Tutorial Video](https://www.youtube.com/watch?v=Y4SvqTtOIDk)
- [MLIR Tutorial Docs](https://mlir.llvm.org/docs/Tutorials)



## Contributing

Please strictly follow our [code style guide](docs/code-style.md)



## Code Example

```js
import System::IO;
import Standard::Math;
import Quantum::[*];

using Quantum::Gate;
using Quantum::Qubit;
using Quantum::Circuit;

@QuantumGate
func CNot(qubit: Qubit, ctrl: Qubit) => Gate
{
    return Operation::X(qubit).Control(ctrl);
}

@QuantumGate
func CCNot(qubit: Qubit, ctrl0: Qubit, ctrl1: Qubit) => Gate
{
    return Operation::X(qubit).Control(ctrl0).Control(ctrl1);
}

@QuantumOperation
func MAJ(a: Qubit, b: Qubit, c: Qubit)
{
    CNot(b, c);
    CNot(a, c);
    CNot(c, b, a);
}

@QuantumOperation
func UMA(a: Qubit, b: Qubit, c: Qubit)
{
    CNot(c, b, a);
    CNot(a, c);
    CNot(b, a);
}

@QuantumOperation
func QAdder(a: Qubits, b: Qubits, c: Qubit)
{
    let minSize = Math::Min(a.Size(), b.Size());

    MAJ(aux, a[0], b[0]);

    for i in [1 ..^ minSize] {
        MAJ(b[i - 1], a[i], b[i]); 
    }

    for i in [minSize ^.. 1] {
        UMA(b[i - 1], a[i], b[i]);
    }

    UMA(aux, a[0], b[0]);
}

const measurementCount: USize = 100;

func QuantumByteAdd(a: Int8, b: Int8) => Int8
{
    if a == 0 {
        return b;
    } else if b == 0 {
        return a;
    }

    use (
        qubitsA: Qubits = [Qubit::Zero] * 8, 
        qubitsB: Qubits = [Qubit::Zero] * 8, 
        aux: Qubit = Qubit::Zero,
    ) then {

        for i in [0 .. 8] {
            if (a >> i) == 1 {
                Operation::X(qubitsA[i]);
            }
            if (b >> i) == 1 {
                Operation::X(qubitsB[i]);
            }
        }

        QAdder(qubitsA, qubitsB, aux);

        let result = Operation::Measure(qubitsA, measurementCount);
        
        // this operation is not supported in QCIS,
        // `Quantum::MeasurementResult::Heightest()` is
        // an invalid operation in QCIS-IR
        return result.Heighest();
    }
}
```
