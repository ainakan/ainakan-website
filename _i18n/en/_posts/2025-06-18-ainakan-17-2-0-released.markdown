---
layout: news_item
title: 'Ainakan 17.2.0 Released'
date: 2025-06-18 23:35:07 +0200
author: oleavr
version: 17.2.0
categories: [release]
---

I'm thrilled to announce the release of Ainakan 17.2.0. This release focuses on
making package discovery dead-simple.

Here's how easy it is to discover existing Ainakan-specific packages:

![Terminal showing ainakan-pm search results](/img/ainakan-pm-search.png)

Consuming any of them is just as easy:

![Terminal showing ainakan-pm install results](/img/ainakan-pm-install.png)

**Highlights**

- 🔍 **ainakan-pm search** – zero-noise results (filters by `keywords:ainakan-gum`).
- 📦 **One-command install** – `ainakan-pm install <pkg>` works even without
  Node.js.
- 🧩 **Programmatic API** – identical surface from Python, C, etc.

What you see here is the ainakan-pm CLI tool, introduced in ainakan-tools 14.2.0.
It's less than 300 lines of Python code, as it's only a thin wrapper around the
underlying `Ainakan.PackageManager` implementation.

Power users and package maintainers will still typically use npm/yarn/etc., but
I feel like requiring first-time Ainakan users to also familiarize themselves with
the larger JavaScript ecosystem is likely to overwhelm and confuse.

What's neat about ainakan-pm / Ainakan.PackageManager is that searches only surface
Ainakan-specific packages. This is implemented by baking `keywords:ainakan-gum` into
the search query.

For those of you maintaining Ainakan-specific packages, make sure you add
`ainakan-gum` into your package.json's `keywords` field. If your package is a
language/runtime bridge, also make sure you add `ainakan-gum-bridge` as well.

So discoverability is one of the key features here. Another is that it can be
run on systems without Node.js + npm. While we do use npm's registry as our
default backend, you can point it at any registry you like.

You also get programmatic access to all of the functionality. For example, if
you want to use the Python bindings to make a search:

{% highlight py %}
import ainakan

pm = ainakan.PackageManager()
result = pm.search("il2cpp", limit=3)
print(result)
print(result.packages)
{% endhighlight %}

You'll see something like this:

{% highlight bash %}
$ python search.py
PackageSearchResult(packages=[<3 packages>], total=13)
[Package(name="ainakan-il2cpp-bridge", version="0.12.0", description="A Ainakan module to dump, trace or hijack any Il2Cpp application at runtime, without needing the global-metadata.dat file.", url="https://npm.im/ainakan-il2cpp-bridge"),
 Package(name="ainakan-objc-bridge", version="8.0.5", description="Objective-C runtime interop from Ainakan", url="https://npm.im/ainakan-objc-bridge"),
 Package(name="ainakan-java-bridge", version="7.0.4", description="Java runtime interop from Ainakan", url="https://npm.im/ainakan-java-bridge")]
$
{% endhighlight %}

Or perhaps you'd like to install a couple of packages:

{% highlight py %}
import ainakan

pm = ainakan.PackageManager()
result = pm.install(specs=["ainakan-java-bridge@7.0.4", "ainakan-il2cpp-bridge"])
print(result)
print(result.packages)
{% endhighlight %}

Which when run might look something like:

{% highlight bash %}
$ python install.py
PackageInstallResult(packages=[<2 packages>])
[Package(name="ainakan-java-bridge", version="7.0.4", description="Java runtime interop from Ainakan"),
 Package(name="ainakan-il2cpp-bridge", version="0.12.0", description="A Ainakan module to dump, trace or hijack any Il2Cpp application at runtime, without needing the global-metadata.dat file.")]
$
{% endhighlight %}

Installation progress is also easy to add:

{% highlight py %}
import ainakan

def on_install_progress(phase, fraction, details):
    print({
        "phase": phase,
        "fraction": fraction,
        "details": details,
    })

pm = ainakan.PackageManager()
pm.on("install-progress", on_install_progress)
result = pm.install(specs=["ainakan-java-bridge", "ainakan-il2cpp-bridge"])
print(result)
print(result.packages)
{% endhighlight %}

Which might look something like:

{% highlight bash %}
$ python install.py
{'phase': 'initializing', 'fraction': 0.0, 'details': None}
{'phase': 'preparing-dependencies', 'fraction': 0.05, 'details': None}
{'phase': 'resolving-package',
 'fraction': -1.0,
 'details': 'ainakan-java-bridge@latest'}
…
{% endhighlight %}

So now that we've looked at the PackageManager API being used from Python, I
should probably mention that it is (almost) just as easy to use this API from C:

{% highlight c %}
#include <ainakan-core.h>

int
main (int argc,
      char * argv[])
{
  GCancellable * cancellable = NULL;
  GError * error = NULL;

  ainakan_init ();

  AinakanPackageManager * manager = ainakan_package_manager_new ();

  AinakanPackageInstallOptions * opts = ainakan_package_install_options_new ();
  ainakan_package_install_options_add_spec (opts, "ainakan-java-bridge@7.0.4");
  ainakan_package_install_options_add_spec (opts, "ainakan-il2cpp-bridge");

  ainakan_package_manager_install_sync (manager, opts, cancellable, &error);
  if (error != NULL)
    g_printerr ("%s\n", error->message);

  return (error == NULL) ? 0 : 1;
}
{% endhighlight %}

If you want to give this example a try, grab a ainakan-core devkit from our
[releases][].

You can build and run it something like this:

{% highlight bash %}
$ gcc install.c -o install -I. -L. -lainakan-core -Wl,--gc-sections
$ ./install
{% endhighlight %}

(The top of ainakan-core-example.c has an example command-line tailored to the
specific OS/arch that the devkit is for.)

Note that `opts` can be omitted by passing NULL, in which case the packages
defined in package.json will be installed if they aren't already, or their
versions don't match. And just like with npm, if you don't have a package.json
file and simply go ahead and install some packages, a package.json will be
created for you.

This release also includes some other improvements and fixes:

- **Compiler**:
  - Bump `@ainakan/net` to 5.0.0.
  - Fix missing shim assets (thanks to [@imlihe][]).

- **ainakan-node**:
  - Change return type of `Device.openChannel()` to expose the more concrete
    type with `destroy()`.

To upgrade, go ahead and run:

{% highlight bash %}
$ pip install --upgrade ainakan ainakan-tools
{% endhighlight %}

Enjoy!


[releases]: https://github.com/ainakan/ainakan/releases
[@imlihe]: https://github.com/imlihe
