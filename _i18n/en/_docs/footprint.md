We put a lot of effort into making sure that Ainakan can scale from desktops all
the way down to embedded systems.

While our prebuilt binaries ship with all features enabled, building Ainakan
yourself means you can tailor it to your needs and build significantly smaller
binaries. The way this is done is by tweaking the available options in
`config.mk`:

{% highlight make %}
# Features ordered by binary footprint, from largest to smallest
AINAKAN_V8 ?= enabled
AINAKAN_CONNECTIVITY ?= enabled
AINAKAN_DATABASE ?= enabled
AINAKAN_JAVA_BRIDGE ?= auto
AINAKAN_OBJC_BRIDGE ?= auto
AINAKAN_SWIFT_BRIDGE ?= auto
{% endhighlight %}

If working on embedded systems, all the aforementioned features may be disabled.

Specifically, they are only required in the following cases:
- AINAKAN_V8: default Javascript runtime is QuickJS so it can safely disabled if
  not used. Required if the V8 runtime is needed, for example when specifically
  requested via the API `create_script(..., runtime='v8')` or through the
  ainakan-tools CLI with `--runtime=v8`.
- AINAKAN_CONNECTIVITY: required if using certificates to enable TLS, or if using
  `setup_peer_connection()` (API) or `--p2p` (CLI). Note that it is not required
  for network connectivity. For example, it is not required when using
  ainakan-server like this: `ainakan-server -l 0.0.0.0`.
- AINAKAN_DATABASE: required if using [SqliteDatabase](/docs/javascript-api/#sqlitedatabase)
  and related APIs, can be safely disabled if not.
- AINAKAN_JAVA_BRIDGE: required when wanting to call or instrument Java APIs
  inside processes with a Java Virtual Machine or Android Runtime environment.
  Note that there are other languages apart from Java which run either on the
  JVM or the Android Runtime, such as Kotlin and Scala.
- AINAKAN_OBJC_BRIDGE and AINAKAN_SWIFT_BRIDGE: required when wanting to call or
  instrument Objective-C or Swift code. Useful on Apple OSes, like i/macOS, may
  be safely disabled outside the Apple ecosystem.
 
Let's run through these and look at how the different options impact footprint
size on Linux/armhf (32-bit ARM).

To make the following a bit clearer, we have added `-Dassets=installed` to the
ainakan-core Meson options. This means that ainakan-agent.so is not embedded into
the ainakan-server/ainakan-inject binary, but is instead loaded from the filesystem.

This is also what you typically want on embedded systems, as writing out the
agent to /tmp is somewhat wasteful, whether it's backed by flash or tmpfs.

## All config.mk features enabled on linux-armhf

{% highlight bash %}
3.8M ainakan-inject
3.2M ainakan-server
 15M ainakan-agent.so
 15M ainakan-gadget.so
{% endhighlight %}

## Step 1: Disable V8

{% highlight bash %}
3.8M ainakan-inject
3.2M ainakan-server
5.2M ainakan-agent.so
5.3M ainakan-gadget.so
{% endhighlight %}

Agent reduced by 9.8M.

## Step 2: Disable connectivity features (TLS and ICE), eliminating OpenSSL

{% highlight bash %}
2.6M ainakan-inject
2.0M ainakan-server
3.6M ainakan-agent.so
3.7M ainakan-gadget.so
{% endhighlight %}

Agent reduced by 1.6M.

## Step 3: Disable the GumJS Database API, eliminating SQLite

{% highlight bash %}
2.6M ainakan-inject
2.0M ainakan-server
3.2M ainakan-agent.so
3.3M ainakan-gadget.so
{% endhighlight %}

Agent reduced by 0.4M.

## Step 4: Disable the GumJS bridges: ObjC, Swift, Java

{% highlight bash %}
2.6M ainakan-inject
2.0M ainakan-server
2.8M ainakan-agent.so
2.9M ainakan-gadget.so
{% endhighlight %}

Agent reduced by 0.4M.

Let's look at what we're left with:

![ainakan-agent.so footprint](/img/ainakan-agent-footprint.png "ainakan-agent.so footprint")

And to sate our curiosity, let's have a closer look at three of the components
that stand out:

![libcapstone.a footprint](/img/capstone-breakdown.png "libcapstone.a footprint")

![libglib-2.0.a footprint](/img/glib-breakdown.png "libglib-2.0.a footprint")

![libgio-2.0.a footprint](/img/gio-breakdown.png "libgio-2.0.a footprint")

