SOCKS
=====

[![GoDoc](https://godoc.org/h12.io/socks?status.svg)](https://godoc.org/h12.io/socks)

SOCKS is a SOCKS4, SOCKS4A and SOCKS5 proxy package for Go.

## Quick Start
### Get the package

    go get -u "github.com/rhomber/socks"

### Import the package

    import "github.com/rhomber/socks"

### Create a SOCKS proxy dialing function

    dialSocksProxy := socks.Dial("socks5://127.0.0.1:1080?timeout=5s")
    tr := &http.Transport{Dial: dialSocksProxy}
    httpClient := &http.Client{Transport: tr}

## Example

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"

	"github.com/rhomber/socks"
)

func main() {
	dialSocksProxy := socks.Dial("socks5://127.0.0.1:1080?timeout=5s")
	tr := &http.Transport{Dial: dialSocksProxy}
	httpClient := &http.Client{Transport: tr}
	resp, err := httpClient.Get("http://www.google.com")
	if err != nil {
		log.Fatal(err)
	}
	defer resp.Body.Close()
	if resp.StatusCode != http.StatusOK {
		log.Fatal(resp.StatusCode)
	}
	buf, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(string(buf))
}
```
