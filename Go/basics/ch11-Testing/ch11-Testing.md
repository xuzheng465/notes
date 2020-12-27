* Useful Community Projects
  * Testify
  * Ginkgo
  * GoConvey
  * httpexpect
  * gomock
  * go-sqlmock



* go test
  * Run all tests in current directory
* go test {pkg1} {pkg2}
  * Test specified packages
* `go test ./...`
  * Run tests in current package and descendants
* `go test -v`
  * Generate verbose output
* go test -run {regexp}
  * Run Only tests matching {regexp}



```bash
go test -coverprofile cover.out

go tool cover -func cover.out

go tool cover -html cover.out

go test -coverprofile count.out -covermode count

go tool cover -html count.out
```



### Other useful functions



Log 

Logf

Helper

Skip, Skipf, and SkipNow

Run

Parallel



## Benchmark

### Key testing.B members

* b.N
* b.StartTimer, b.StopTimer, b.ResetTimer
* b.RunParallel



go test -benchmem

go test -trace {trace.out}

go test -{type}profile {file}



```bash
go test -bench Alloc -memprofile profile.out

go tool pprof profile.out
```



