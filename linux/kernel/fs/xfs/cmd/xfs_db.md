# xfs_db

```shell
sudo xfs_db /dev/zd0
xfs_db> help
ablock filoff -- set address to file offset (attr fork)
addr [field-expression] -- set current address
agf [agno] -- set address to agf header
agfl [agno] -- set address to agfl block
agi [agno] -- set address to agi header
agresv -- print AG reservation stats
back -- move to the previous location in the position ring
blockfree -- free block usage information
blockget [-s|-v] [-n] [-t] [-b bno]... [-i ino] ... -- get block usage and check consistency
blockuse [-n] [-c blockcount] -- print usage for current block(s)
bmap [-ad] [block [len]] -- show block map for current file
btdump [-a] [-i] -- dump btree
btheight [-b blksz] [-n recs] [-w max|-w min] btree types... -- compute btree heights
convert type num [type num]... type -- convert from one address form to another
crc [-i|-r|-v] -- manipulate crc values for V5 filesystem structures
daddr [d] -- set address to daddr value
dblock filoff -- set address to file offset (data fork)
debug [flagbits] -- set debug option bits
dquot [-g|-p|-u] id -- set current address to a group, project or user quota block for given ID
echo [args]... -- echo arguments
forward -- move forward to next entry in the position ring
frag [-a] [-d] [-f] [-l] [-q] [-R] [-r] [-v] -- get file fragmentation data
freesp [-bcdfs] [-A alignment] [-a agno]... [-e binsize] [-h h1]... [-m binmult] -- summarize free space for filesystem
fsblock [fsb] -- set address to fsblock value
fsmap [start_fsb] [end_fsb] -- display reverse mapping(s)
hash string -- calculate hash value
help [command] -- help for one or all commands
info -- pretty-print superblock info
inode [inode#] -- set current inode
label [label] -- write/print FS label
log [stop|start <filename>] -- start or stop logging to a file
logres -- dump log reservations
ls [-i] [paths...] -- list directory contents
metadump [-a] [-e] [-g] [-m max_extent] [-w] [-o] filename -- dump metadata to a file
ncheck [-s] [-i ino] ... -- print inode-name pairs
path  -- navigate to an inode by path
pop -- pop location from the stack
print [value]... -- print field values
push [command] -- push location to the stack
quit -- exit xfs_db
ring -- show position ring or move to a specific entry
sb [agno] -- set current address to sb header
source source-file -- get commands from source-file
stack -- view the location stack
timelimit [--classic|--bigtime] [--pretty] -- display timestamp limits
type [newtype] -- set/show current data type
uuid [uuid] -- write/print FS uuid
version [feature | [vnum fnum]] -- set feature bit(s) in the sb version field
```

## 例子

- 碎片化状态
```shell
sudo xfs_db -c frag -r /dev/xfs_test/xfs_test_1
actual 0, ideal 0, fragmentation factor 0.00% (碎片化)
Note, this number is largely meaningless.
Files on this filesystem average -nan extents per file
```
