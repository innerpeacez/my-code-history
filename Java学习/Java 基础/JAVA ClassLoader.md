#### Java 的类加载机制

![1570671735168](../images/1570671735168.png)

从 .class 文件到机器可执行的二进制机器码的过程中，JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，有的热点代码，通过JIT运行时编译器首次编译后保留机器码，JDK 9引入了一种新的编译模式AOT(Ahead of Time Compilation)，它是直接将字节码编译成机器码，这样就避免了JIT预热等各方面的开销。

- **Bootstrap ClassLoader**
- **Extention ClassLoader**
- **Appclass Loader**

