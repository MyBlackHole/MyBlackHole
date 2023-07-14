# revert
![[imgs/Pasted image 20230714205728.png]]

如上图所示，撤销 C2 这个commit，但是保留C3、C4的改动。在这种情况下，可以使用git revert命令。
git revert命令并不会删除C2，而是创建一个新的commit，将C2中的改动再改回去。如下图所示。

在这个例子中，在revert之前是没有C4 commit的，假设C2的commit id为 0ad5a7a6，执行git revert 0ad5a7a6则会创建C4 commit。
![[imgs/Pasted image 20230714205557.png]]


