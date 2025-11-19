This is a command-line tool for listing processes, which is very useful
when interacting with a remote system.

You can acquire device id from [ainakan-ls-devices](/docs/ainakan-ls-devices) tool.
{% highlight bash %}
# Connect Ainakan to an iPad over USB and list running processes
$ ainakan-ps -U

# List running applications
$ ainakan-ps -Ua

# List installed applications
$ ainakan-ps -Uai

# Connect Ainakan to the specific device
$ ainakan-ps -D 0216027d1d6d3a03

{% endhighlight %}
