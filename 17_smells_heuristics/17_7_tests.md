## 17.7 测试（Tests）

### T1: 测试不足（Insufficient Tests）

一套测试应该测到所有可能失败的东西。

只要还有没被测试探测过的条件，或是还有没被验证过的计算，那么测试就是不足的。

### T2: 使用覆盖率工具（Use a Coverage Tool!）

IDE支持。

### T3: 别略过小测试（Don’t Skip Trivial Tests）

小测试易于编写，其文档上的价值高于编写成本。

### T4: 被忽略的测试就是对不确定事物的疑问（An Ignored Test Is a Question about an Ambiguity）

有时因为需求不明，我们可以把测试表为`@Ignore`。

### T5: 测试边界条件（Test Boundary Conditions）

边界判断错误的情形很常见。

### T6: 全面测试相近的缺陷（Exhaustively Test Near Bugs）

80% 的软件缺陷常常生存在软件 20% 的空间里。

### T7: 测试失败的模式有启发性（Patterns of Failure Are Revealing）

有时可以通过测试用例失败的模式来诊断问题所在。

### T8: 测试覆盖率的模式有启发性（Test Coverage Patterns Can Be Revealing）

Looking at the code that is or is not executed by the passing tests gives clues to why the failing tests fail. 


### T9: 测试应该快速（Tests Should Be Fast）

A slow test is a test that won’t get run. When things get tight, it’s the slow tests that will be dropped from the suite. So *do what you must* to keep your tests fast. 

