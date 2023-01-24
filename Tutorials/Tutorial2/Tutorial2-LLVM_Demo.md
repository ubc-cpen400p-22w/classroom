
# Tutorial 2

## Objectives:

1. Get familiar with the LLVM programming model
2. Write a basic LLVM [analysis] pass


## Prerequisites

1. LLVM should be built and setup.


## Compiling C/C++ Source Code to LLVM IR

```shell
clang++ -O0 -S -emit-llvm -o HelloWorld.ll -c HelloWorld.cpp
```

## Visualization

For visualization (optional but helpful):
```shell
sudo apt-get install graphviz
```

#### Visualizing the Call Graph
```shell
clang++ -O0 -S -emit-llvm -Xclang -disable-O0-optnone -o HelloWorld.ll -c HelloWorld.cpp
opt -disable-output --dot-callgraph -enable-new-pm=0 HelloWorld.ll
dot -Tpng HelloWorld.ll.callgraph.dot -o HelloWorld.callgraph.png
```

#### Visualizing the CFG of a Function
```shell
opt -disable-output --dot-cfg -enable-new-pm=0 HelloWorld.ll
dot -Tpng ._Z3bari.dot -o bar.png
```

## Building Your First LLVM Pass

1. First navigate to your LLVM build folder. Then, rerun the build command.
```shell
cd ~/llvm-project/build/
ninja
```
2. Navigate to the directory containing the source file you want to analyze.

3. Finally, use the `opt` tool to run your LLVM pass on the source file to be analyzed.
```shell
opt -enable-new-pm=0 -load $PATH_TO_PASS -[RegisteredNameOfPass] < HelloWorld.ll > /dev/null
```
Example command if the built pass is called `Tutorial2.so` and the pass is registered as `p1function`:
```shell
opt -enable-new-pm=0 -load ~/llvm-project/build/lib/Tutorial2.so -p1function < HelloWorld.ll > /dev/null
```

## Task 1
Analyze [HelloWorld.cpp](HelloWorld.cpp)

Goal: Print all the functions' name...
1. As a function pass
2. As a module pass

## Task 2
Analyze [CompFunc.cpp](CompFunc.cpp)

Goal: Count...
1. All comparison instructions (i.e., ==, >, <)
2. All the `MUL` instructions

## Task 3
Analyze [CompFunc.cpp](CompFunc.cpp)

Goal: For each comparison instruction, print out the...
1. Instruction itself
2. $1^{st}$ source operand
3. $2^{nd}$ source operand

