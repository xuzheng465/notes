* How to add your workspace in your GOPATH
  * export GOPATH=\$(go env GOPATH)
  * `export GOPATH=$(pwd)`

* How to  add the workspace's bin subdirectory to your PATH
		 `export PATH=$PATH:$(go env GOPATH)/bin`



## OR

Config at `.zshrc` file

```bash
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
export GOPATH=/Users/xuzheng/golib
export PATH=$PATH:$GOPATH/bin
export GOPATH=$GOPATH:/Users/xuzheng/Projects/gocode
```

 

