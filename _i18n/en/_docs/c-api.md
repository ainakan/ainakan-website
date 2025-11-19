## Getting Started

The functionality that provides you the JavaScript API for injection, function
manipulation, memory reading, and more is also available from C.

Ainakan is broken down into several modules, which we will discuss below.

These can each be compiled individually and are also available on
[the releases page](https://github.com/ainakan/ainakan/releases).

The devkit downloads come with an example on how to use each module.
Using the devkits is the best way to learn how to utilize each module.

## core

ainakan-core contains the main injection code.  From ainakan-core, you can inject
into a process, create a thread running QuickJS, and run your JavaScript.

See the [ainakan-core](https://github.com/ainakan/ainakan-core) repository for the source.

## gum

ainakan-gum allows you to augment and replace functions using C.

The example in the devkit shows you how to augment `open` and `close` from C only.

See the [ainakan-gum](https://github.com/ainakan/ainakan-gum) repository for the source.

## gumjs

ainakan-gumjs contains the JavaScript bindings.

## gadget

Similar to ainakan-agent except to either DYLD_INSERT_LIBRARIES, bundle with an
app, etc. and it can run either in a remote mode where it listens and looks just
like ainakan-server.

_Please click "Improve this page" above and add an example. Thanks!_
