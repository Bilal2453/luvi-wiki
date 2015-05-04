Luvi is currently built using a semi-automated set of build bots in @creationix's office.

If you wish to setup your own, the process is fairly simple.

 - Install [ninja](https://martine.github.io/ninja/) onto your system if it's not windows.  It's `ninja-build` on some debian based systems.  Also you'll probably need GCC or clang, whichever is native to your platform.
 - If it's windows, install the community edition of visual studio and make sure to get the version made for desktop apps as that's the one that comes with a C compiler.
 - Install [cmake](http://www.cmake.org/).  This is pretty easy cross-platform
 - Install [github-release](https://github.com/aktau/github-release).  They have binaries for most platforms, but for others, you need to first install [go](https://golang.org/) and then build it using go.  On raspberry PIs I use pre-built [go binaries](http://dave.cheney.net/unofficial-arm-tarballs).
 - Do a recursive clone of luvi and run `git describe` in the checkout to make sure you have the version you expect to be building.
 - Create en environment variable `GITHUB_TOKEN` that contains an api token capable of pushing to the luvi project. (Tokens can be created at <https://github.com/settings/applications/new>)
 - If using ninja, create an environment variable `GENERATOR` that holds the string `Ninja`.

Then to make a release:

 - do the normal changelog and push an annotated and signed tag to the luvi repo.
 - Go to the [luvi releases page](https://github.com/luvit/luvi/releases) and edit the release.  I copy the text from the tag containing the changelog and paste it into the description of the release you're creating.
 - run `make publish-src` and `make publish` from one machine (usually my macbook that's also building the darwin binary)
 - then run `make publish` on the other bots (make sure to `git pull` and `git describe` to make sure you're building what you expect to be building).  On some systems like solaris or freebsd, you'll need `gmake publish` instead of `make publish`.

Occasionally github will timeout while uploading the binaries, especially the larger sized ones with debug symbols.  If this happens, edit your release again on the github page.  Delete the failed uploads and save the changes to the release.  Then you can retry the publish.  Running `make publish-tiny` or `make publish-regular` directly will skip `make clean` and in the case of a failed upload, will jump right back to uploading the binary again.  Since this usually happens on the larger binary, I highly recommend this route.

## Supported platforms:

Currently @creationix builds every luvit release on the following platforms.  The desktop class machines will finish the publish in under 10 minutes, but armv6 on the older PI will take up to an hour.

 - Windows 8.1 amd64
 - Ubuntu 14.04 x86_64
 - Ubuntu 14.04 i686
 - OSX 10.10 x86_64
 - Raspbian armv6l (raspberry pi A+)
 - Raspbian armv7l (raspberry pi 2 B+)
 - FreeBSD 10.1 amd64