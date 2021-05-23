<p align=center><a href=""><img src=scrapligo.svg?sanitize=true/></a></p>

[![Go Report](https://img.shields.io/badge/go%20report-A%2B-blue?style=flat-square&color=00c9ff&labelColor=bec8d2)](https://goreportcard.com/report/github.com/scrapli/scrapligo)
[![License: MIT](https://img.shields.io/badge/License-MIT-blueviolet.svg?style=flat-square)](https://opensource.org/licenses/MIT)


---

**Source Code**: <a href="https://github.com/scrapli/scrapligo" target="_blank">https://github.com/scrapli/scrapligo</a>

**Examples**: <a href="https://github.com/scrapli/scrapligo/tree/master/examples" target="_blank">https://github.com/scrapli/scrapligo/tree/master/examples</a>

---

scrapligo -- scrap(e c)li (but in go!) --  is a Go library focused on connecting to devices, specifically network devices
(routers/switches/firewalls/etc.) via SSH and NETCONF.

**NOTE** this is a work in progress, use with caution!


#### Key Features:

- __Easy__: It's easy to get going with scrapligo -- if you are familiar with go and/or scrapli you are already most of 
  the way there! Check otu the examples linked above to get started! 
- __Fast__: Do you like to go fast? Of course you do! All of scrapli is built with speed in mind, but this port of 
  scrapli to go is of course even faster than its python sibling! 
- __But wait, there's more!__: Have NETCONF devices in your environment, but love the speed and simplicity of
  scrapli? You're in luck! NETCONF support is built right into scrapligo!


## A simple Example

```go
package main

import (
	"fmt"

	"github.com/scrapli/scrapligo/driver/base"

	"github.com/scrapli/scrapligo/driver/core"
)

func main() {
	d, err := core.NewCoreDriver(
		"localhost",
		"cisco_iosxe",
		base.WithPort(21022),
		base.WithAuthStrictKey(false),
		base.WithAuthUsername("vrnetlab"),
		base.WithAuthPassword("VR-netlab9"),
		base.WithAuthSecondary("VR-netlab9"),
	)

	if err != nil {
		fmt.Printf("failed to create driver; error: %+v\n", err)
		return
	}

	err = d.Open()
	if err != nil {
		fmt.Printf("failed to open driver; error: %+v\n", err)
		return
	}

	// send some configs
	configs := []string{
		"interface loopback0",
		"interface loopback0 description tacocat",
		"no interface loopback0",
	}

	_, err = d.SendConfigs(configs)
	if err != nil {
		fmt.Printf("failed to send configs; error: %+v\n", err)
		return
	}

	err = d.Close()
	if err != nil {
		fmt.Printf("failed to close driver; error: %+v\n", err)
	}
}
```

<small>* gopher artwork by [@egonelbre](https://github.com/egonelbre/gophers)</small>