The Go bindings make it easy to use Ainakan's APIs from [Go][].

Some of the provided functionality includes:

* Listing devices/applications/processes
* Attaching to applications/processes
* Fetching information about devices/applications/processes

For the full documentation please visit [pkg.go.dev][].

## Example

{% highlight golang %}
package main

import (
	"fmt"

	"github.com/ainakan/ainakan-go/ainakan"
)

func main() {
	manager := ainakan.NewDeviceManager()
	devices, err := manager.EnumerateDevices()
	if err != nil {
		panic(err)
	}

	fmt.Printf("[*] Ainakan version: %s\n", ainakan.Version())
	fmt.Println("[*] Devices: ")
	for _, device := range devices {
		fmt.Printf("[*] %s => %s\n", device.Name(), device.ID())
	}
}
{% endhighlight %}

Example above should output something like:

{% highlight bash %}
$ go build main.go && ./main
[*] Ainakan version: 16.0.3
[*] Devices:
[*] Local System => local
[*] Local Socket => socket
{% endhighlight %}


[Go]: https://go.dev/
[pkg.go.dev]: https://pkg.go.dev/github.com/ainakan/ainakan-go/ainakan
