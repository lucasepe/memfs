# memfs: A simple in-memory io/fs.FS filesystem

memfs is an in-memory implementation of Go's io/fs.FS interface.
The goal is to make it easy and quick to build an fs.FS filesystem
when you don't have any complex requirements.


## Usage

```go
package main

import (
	"fmt"
	"io/fs"

	"github.com/lucasepe/memfs"
)

func main() {
	rootFS := memfs.New()

	err := rootFS.MkdirAll("dir1/dir2", 0777)
	if err != nil {
		panic(err)
	}

	err = rootFS.WriteFile("dir1/dir2/f1.txt", []byte("lorem-ipsum"), 0755)
	if err != nil {
		panic(err)
	}

	err = fs.WalkDir(rootFS, ".", func(path string, d fs.DirEntry, err error) error {
		fmt.Println(path)
		return nil
	})
	if err != nil {
		panic(err)
	}

	content, err := fs.ReadFile(rootFS, "dir1/dir2/f1.txt")
	if err != nil {
		panic(err)
	}
	fmt.Printf("%s\n", content)
}
```
