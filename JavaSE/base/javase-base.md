# javaSE基础

## Java运行机制
源文件（*.java） --> 

```mermaid
graph LR
A[源文件 *.java] --> B[java编译器]
B --> C[字节码文件 *.class]
C --> D(类装载器)
D --> E(字节码校验器)
E --> F(解释器)
F --> G(系统平台)
```

==在笔记本上==