This is a command-line tool for killing processes.

You can acquire PIDs from [ainakan-ps](/docs/ainakan-ps/) tool.
{% highlight bash %}
$ ainakan-kill -D <DEVICE-ID> <PID>
# List active applications
$ ainakan-ps -D 1d07b5f6a7a72552aca8ab0e6b706f3f3958f63e  -a

PID  Name                Identifier
----  ------------------  -----------------------------------------------------
4433  Camera              com.apple.camera
4001  Cydia               com.saurik.Cydia
4997  Filza               com.tigisoftware.Filza
4130  IPA Installer       com.slugrail.ipainstaller
3992  Mail                com.apple.mobilemail
4888  Maps                com.apple.Maps
6494  Messages            com.apple.MobileSMS
5029 Safari              com.apple.mobilesafari
4121  Settings            com.apple.Preferences

# Connect Ainakan to the device and kill running process
$ ainakan-kill -D 1d07b5f6a7a72552aca8ab0e6b706f3f3958f63e 5029

# Check if process has been killed
$ ainakan-ps -D 1d07b5f6a7a72552aca8ab0e6b706f3f3958f63e  -a

PID  Name                Identifier
----  ------------------  -----------------------------------------------------
4433  Camera              com.apple.camera
4001  Cydia               com.saurik.Cydia
4997  Filza               com.tigisoftware.Filza
4130  IPA Installer       com.slugrail.ipainstaller
3992  Mail                com.apple.mobilemail
4888  Maps                com.apple.Maps
6494  Messages            com.apple.MobileSMS
4121  Settings            com.apple.Preferences

{% endhighlight %}
