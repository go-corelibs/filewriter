[![godoc](https://img.shields.io/badge/godoc-reference-blue.svg)](https://pkg.go.dev/github.com/go-corelibs/filewriter)
[![codecov](https://codecov.io/gh/go-corelibs/filewriter/graph/badge.svg?token=GZgRVDKZlr)](https://codecov.io/gh/go-corelibs/filewriter)
[![Go Report Card](https://goreportcard.com/badge/github.com/go-corelibs/filewriter)](https://goreportcard.com/report/github.com/go-corelibs/filewriter)

# go-corelibs/filewriter - file writer that does not keep open file handles

filewriter is a package for writing things to files that other processes may
also be writing to at the same time so it does not keep any open file handles
longer than absolutely necessary.

# Installation

``` shell
> go get github.com/go-corelibs/filewriter@latest
```

# Examples

## 

``` go
func main() {
    // create a new writer using defaults and a temp file
    var err error
    var fw filewriter.FileWriter
    if fw, err = filewriter.New().Make(); err != nil {
        panic(err)
    }
    defer fw.Remove() // cleanup the file when we're done
    // write things to the file
    if err = fw.Write([]byte("two lines\nof text")); err != nil {
        panic(err)
    }
    // need to read the file? fw.ReadFile gives the whole contents,
    // however for large files perhaps the WalkFile is better!
    stopped := fw.WalkFile(func(line string) (stop bool) {
        // do something with this line of file contents and
        // to stop the walking process, return true, otherwise
        // the walk continues to the end of the file
        return
    })
    // stopped == false because the func did not return true
}
```

# Go-CoreLibs

[Go-CoreLibs] is a repository of shared code between the [Go-Curses] and
[Go-Enjin] projects.

# License

```
Copyright 2023 The Go-CoreLibs Authors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use file except in compliance with the License.
You may obtain a copy of the license at

 http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[Go-CoreLibs]: https://github.com/go-corelibs
[Go-Curses]: https://github.com/go-curses
[Go-Enjin]: https://github.com/go-enjin
