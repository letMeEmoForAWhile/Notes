**在死代码区域变异存在以下局限：**

1. 可变异范围小，只局限于死代码区域
2. 死代码区域可能被编译器消除，导致所有变异无效
3. 只在死代码区域显示的编译错误是不可见的，因为错误代码没有被执行（注，编译错误表现为应用程序失败）

# 主要的挑战：

- 确保合成的语句没有未定义的行为。

  解决方法：提出一种自下而上的方法来逐渐构造表达式（4.3.2）

- 确保EMI的有效性：变异用例的执行结果与原始用例保持相同

  解决方法：

# 贡献：

- 提出一种新的EMI技术，让变异可以发生在整个程序中（**活代码区域**+死代码区域）。

- 提出一种自下而上的方法来合成正确的表达式
- 实现工具Hermes来验证C编译器。

# **算法：**

FCB（Always False Conditional Block）：恒为false的if/while语句 	

TG（Always True Guard）：恒为True的if语句

==TCB==（Always True Conditional Block）：恒为True的if/while语句 使用模板

==第30张==

