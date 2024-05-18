# elf

[推荐连接](https://www.cnblogs.com/linhaostudy/p/8855238.html)

- Linking View
    Linking 链接时：需要Section Header Table，不需要Program Header Table
- Execution View
    Execution 执行时：需要Program Header Table，不需要Section Header Table

```shell

              +-----------------+
        +----| ELF File Header |----+
        |    +-----------------+    |
        v                           v
+-----------------+      +-----------------+
| Program Headers |      | Section Headers |
+-----------------+      +-----------------+
      ||                               ||
      ||                               ||
      ||                               ||
      ||   +------------------------+  ||
      +--> | Contents (Byte Stream) |<--+
          +------------------------+

+-------------------------------+
| ELF File Header               |
+-------------------------------+
| Program Header for segment #1 |
+-------------------------------+
| Program Header for segment #2 |
+-------------------------------+
| ...                           |
+-------------------------------+
| Contents (Byte Stream)        |
| ...                           |
+-------------------------------+
| Section Header for section #1 |
+-------------------------------+
| Section Header for section #2 |
+-------------------------------+
| ...                           |
+-------------------------------+
| ".shstrtab" section           |
+-------------------------------+
| ".symtab"   section           |
+-------------------------------+
| ".strtab"   section           |
+-------------------------------+
```
