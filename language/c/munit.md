### 描述
**c 的小测试框架**
```
https://github.com/nemequ/munit/blob/master/example.c
```
### 用法
```
用法：main [选项...] [测试...]

  --种子种子
            用于播种 PRNG 的值。 必须是十进制的 32 位整数
            没有分隔符（逗号、小数、空格等）的符号，或
            前缀为“0x”的十六进制。
  --迭代 N
            每个测试运行 N 次。 0 表示默认编号。
  --参数名称值
            将传递给任何测试的参数键/值对
            采用该名称的参数。 如果没有提供，测试将是
            为每个可能的参数值运行一次。
  --list 写下所有可用测试的列表。
  --list-参数
            写下所有可用测试及其可能参数的列表。
  --single 在单个配置中运行每个参数化测试而不是
            每一种可能的组合
  --log-visible debug|info|warning|error
  --log-fatal debug|info|warning|error
            设置不同严重级别的消息可见的级别，
            或导致测试终止。
  --no-fork 不要在子进程中执行测试。 如果提供此选项
            并且测试崩溃（包括断言失败），不再继续
            将进行测试。
  --致命失败
            一旦发现故障，立即停止执行测试。
  --show-stderr
            显示测试写入 stderr 的数据，即使测试成功。
  --color auto|always|never
            着色（或不着色）输出。
  --help 打印此帮助信息并退出。

µnit 0.4.1
完整文档位于：https://nemequ.github.io/munit/
```
