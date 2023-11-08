# cmd

```shell
0  0x000000000301215a in github.com/minio/minio/cmd.(*streamingBitrotWriter).Write
   at ./cmd/bitrot-streaming.go:51
1  0x000000000366d557 in github.com/minio/minio/cmd.testBitrotReaderWriterAlgo
   at ./cmd/bitrot_test.go:45
2  0x000000000366e614 in github.com/minio/minio/cmd.TestAllBitrotAlgorithms
   at ./cmd/bitrot_test.go:86
3  0x00000000005ce74a in testing.tRunner
   at /usr/lib/go-1.20/src/testing/testing.go:1576
4  0x00000000005d02d9 in testing.(*T).Run.func1
   at /usr/lib/go-1.20/src/testing/testing.go:1629
5  0x000000000047c461 in runtime.goexit
   at /usr/lib/go-1.20/src/runtime/asm_amd64.s:1598

up 3

down 2
```
