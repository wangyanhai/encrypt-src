The tool is used to encrypt string constants in source code.

It is built on QT.

参考qt的tr机制：

1.函数：char* se(const char* str)，用于包裹需要加密的串，所有需要加密的串用这个函数包起来，这样源代码改动风险最小，而且还可读；

这个函数可以直接返回源字串，不影响本地编译和调试。

2.函数：char* dse(const char* str)，用于替换掉se函数，这个函数参数是加密的字串，返回解密字串，解密方法与步骤3的模块匹配。

3.一个预处理模块，在编译之前调用，作用是扫描cpp文件中所有被se()函数的地方，将其中被包裹的串提出来，加密（可以用国密或base64算法）其中的字串，然后把se(“源字串”)替换为dse(“加密字串”)。这样编译时的源码中没有了源字串，不会被扫描出来。

 

好处是不影响源代码可读性，改动风险低。预处理模块需要考虑健壮性、跨平台、在编译环境下如何执行等。

在不需要安全扫描的情况下，只要去掉预处理环节，不影响代码质量和性能。