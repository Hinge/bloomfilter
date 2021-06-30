# bloomfilter

![Build](https://github.com/OldPanda/bloomfilter/workflows/Build/badge.svg?event=push)
[![codecov](https://codecov.io/gh/OldPanda/bloomfilter/branch/master/graph/badge.svg?token=FCV788SCL7)](https://codecov.io/gh/OldPanda/bloomfilter)
[![Go Reference](https://pkg.go.dev/badge/github.com/OldPanda/bloomfilter.svg)](https://pkg.go.dev/github.com/OldPanda/bloomfilter)
[![Go Report Card](https://goreportcard.com/badge/github.com/OldPanda/bloomfilter)](https://goreportcard.com/report/github.com/OldPanda/bloomfilter)
[![Mentioned in Awesome Go](https://awesome.re/mentioned-badge-flat.svg)](https://github.com/avelino/awesome-go)

## Overview

Yet another Bloomfilter implementation in Go, compatible with Java's Guava library. This library borrows how [Java's Guava libraray](https://guava.dev/) implements Bloomfilter hashing strategies to achieve the serialization compatibility.

## Installing

First pull the latest version of the library:

```
go get github.com/OldPanda/bloomfilter
```

Then import the this library in your code:

```
import "github.com/OldPanda/bloomfilter"
```

## Usage Examples

### Basic Usage

```Go
package main

import (
	"fmt"

	"github.com/OldPanda/bloomfilter"
)

func main() {
	// create bloomfilter with expected insertion=500, error rate=0.01
	bf, _ := bloomfilter.NewBloomFilter(500, 0.01)
	// add number 0~199 into bloomfilter
	for i := 0; i < 200; i++ {
		bf.Put(i)
	}

	// check if number 100 and 200 are in bloomfilter
	fmt.Println(bf.MightContain(100))
	fmt.Println(bf.MightContain(200))
}
```

### Serialization

```Go
package main

import "github.com/OldPanda/bloomfilter"

func main() {
	// expected insertion=500, error rate=0.01
	bf, _ := bloomfilter.NewBloomFilter(500, 0.01)
	// add 0~199 into bloomfilter
	for i := 0; i < 200; i++ {
		bf.Put(i)
	}

	// serialize bloomfilter to byte array
	bytes := bf.ToBytes()
	// handling the bytes ...
}
```

### Deserialization

```Go
package main

import (
	"fmt"

	"github.com/OldPanda/bloomfilter"
)

func main() {
	// create bloomfilter from byte array
	bf, _ := bloomfilter.FromBytes(bytes)
	// check whether number 100 is in bloomfilter
	fmt.Println(bf.MightContain(100))
}
```

## Benchmark

The benchmark testing runs on element insertion and query separately.

```Bash
» go test -bench . -benchmem ./...
# github.com/OldPanda/bloomfilter.test
goos: darwin
goarch: amd64
pkg: github.com/OldPanda/bloomfilter
cpu: Intel(R) Core(TM) i7-4870HQ CPU @ 2.50GHz
BenchmarkBloomfilterInsertion-8                  4923142               387.1 ns/op            17 B/op          1 allocs/op
BenchmarkBloomfilterQuery-8                      4678299               259.6 ns/op            15 B/op          1 allocs/op
BenchmarkBloomfilterDeserialization-8             162871              7110 ns/op           13200 B/op         52 allocs/op
PASS
ok      github.com/OldPanda/bloomfilter 4.880s
```
