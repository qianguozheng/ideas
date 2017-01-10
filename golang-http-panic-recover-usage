#Golang http post panic recover usage

###Brief Introduction
In golang, some abnormal case would cause program crash, but if you want it recover, you need to handle it by hand.

Below is the simple example of http function use recover to recover from panic caused by peer-end server crash.

```
package main

import (
	"errors"
	"fmt"
	"net/http"
	"strings"
	"time"
)

var err error

func httppost() {
	for {
		fmt.Println("Cycle...")
		defer func() {

			if r := recover(); r != nil {

				fmt.Println("Recovered in testPanic2Error", r)

				//check exactly what the panic was and create error.
				switch x := r.(type) {
				case string:
					err = errors.New(x)
				case error:
					err = x
				default:
					err = errors.New("Unknow panic")
				}
			}
			fmt.Println(err)
			httppost()

		}()
		resp, err := http.Post("http://127.0.0.1/v3/api/device/vpn",
			"application/x-www-form-urlencoded",
			strings.NewReader("name=qgz"))

		if err != nil {
			fmt.Println(err)
		}

		resp.Body.Close()

		time.Sleep(time.Second * 5)
		fmt.Println("Cycle...")
	}
}

func main() {
	httppost()
}

```
