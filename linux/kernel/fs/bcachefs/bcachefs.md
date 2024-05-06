# bcachefs

[来源连接](https://tinylab.org/bcachefs-intro-part1)
## btree
![[imgs/Pasted image 20240505182129.png]]

### bset
![[imgs/Pasted image 20240505182043.png]]

## 案例

### 一个文件
我们假设打开了一个文件，文件内容 —— 即开始和结束的 bpos 所囊括的区间，由一个 `BTREE_ID_extents` 类型 btree 所定位
- 假设这个 btree 目前只有两层，顶层是 level 1 节点
- level 1 是一个中间节点（interior node），它某个 bkey 的 value 指向了一个 level 0 的节点（叶子节点）
    - 换言之，指向的还是 btree 节点。其 bkey 类型为 `BKEY_TYPE_btree`（参见 `btree_types.h`，通过宏组合名称，定义的 bkey 类型）
    - 下表展示了 value 序列化后的结构

| 序号  | 内容                 | 说明                                                                                                      |
| --- | ------------------ | ------------------------------------------------------------------------------------------------------- |
| 0   | `bch_btree_ptr_v2` | 相当于一个 header，其中  <br>`bch_btree_ptr_v2::sectors_written`，留意和 `bkey::size` 区别。  <br>前者指向叶子节点的已用空间；后者是总空间 |
| 1   | `bch_extent_ptr`   | 其中 `bch_extent_ptr::offset` 指向了叶子节点的磁盘起始位置，  <br>以 sectors 计                                            |
| 2   | `bch_extent_ptr`   | 这是指向另一磁盘上副本的指针                                                                                          |
| 3   | `bch_extent_ptr`   | 同上                                                                                                      |
| 4   | `bch_extent_ptr`   | 同上，总计最多有四个副本                                                                                            |
我们再假设，上述文件内容被一个叶子节点中的某个 bkey 的 value 指向
- 换言之，指向的是一块 extent，而非 btree 节点。其 bkey 类型为 `BKEY_TYPE_extents`
- 下表展示了 value 序列化后的结构（参见 `bch_extent_entry`）

| 内容                      | 说明                  |
| ----------------------- | ------------------- |
| `bch_extent_ptr`        | 指向 extent           |
| `bch_extent_crc32`      | crc 和 stripe 不在本文展开 |
| `bch_extent_crc64`      |                     |
| `bch_extent_crc128`     |                     |
| `bch_extent_stripe_ptr` |                     |

下图展示了上述两种 bkey：

- level 1 中的一个 bset 中的一个 bkey，指向了磁盘某个 256KB 块
    - 它就是 level 0 的内容序列化
- level 0 中的一个 bset 中的一个 bkey，指向了磁盘某个块，内容即上述例中的文件内容
    - 图中还展示了另一种形式，即当文件内容很小时，可以直接变成 `bch_inline_data` 嵌在 bkey 中。
    

![[imgs/Pasted image 20240505182212.png]]