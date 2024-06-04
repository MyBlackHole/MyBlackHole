# radix

## 定义

radix tree(也被称为radix trie，或者compact prefix tree)用于表示一种空间优化的trie(prefix tree)数据结构

例如:

1. romane
2. romanus
3. romulus
4. rubens
5. ruber

                       r
                     /   \
                    /     \
                   /       \
                  /         \
                 /           \
                /             \
               /               \
              om               ube
             /   \              | \
            /     \             |  \
           /       \            |   \
          /         \           |    \
         /           \          |     \
       an           ulus        ns     r
      /  \            \         |       \
     e    us           3        4        5
    /      \
   1        2

