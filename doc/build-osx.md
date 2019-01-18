Mac OS X Build Instructions and Notes
====================================
This guide will show you how to build adcoind (headless client) for OSX.

Notes
-----

* Tested on OS X 10.7 through 10.10 on 64-bit Intel processors only.

* All of the commands should be executed in a Terminal application. The
built-in one is located in `/Applications/Utilities`.

Preparation
-----------

You need to install XCode with all the options checked so that the compiler
and everything is available in /usr not just /Developer. XCode should be
available on your OS X installation media, but if not, you can get the
current version from https://developer.apple.com/xcode/. If you install
Xcode 4.3 or later, you'll need to install its command line tools. This can
be done in `Xcode > Preferences > Downloads > Components` and generally must
be re-done or updated every time Xcode is updated. Xcode version 10.0 already
has command line tools pre-installed. To check if command line tools are working
use the following line in your terminal:
```bash
xcode-select --install
```

There's also an assumption that you already have `git` installed. If
not, it's the path of least resistance to install [Github for Mac](https://mac.github.com/)
(OS X 10.7+) or
[Git for OS X](https://code.google.com/p/git-osx-installer/). It is also
available via Homebrew using 
```bash
brew install git
```

You will also need to install [Homebrew](http://brew.sh) in order to install library
dependencies.

The installation of the actual dependencies is covered in the Instructions
sections below.

Instructions: Homebrew
----------------------

#### Install dependencies using Homebrew

        brew install autoconf automake berkeley-db4 libtool boost miniupnpc openssl pkg-config protobuf qt5 zmq gmp libevent

### Building `adcoind`

1. Clone the github tree to get the source code and go into the directory.

        git clone https://github.com/AdCoin-Project/AdCoin.git
        cd AdCoin

2.  Make the Homebrew OpenSSL headers visible to the configure script  (do ```brew info openssl``` to find out why this is necessary, or if you use Homebrew with installation folders different from the default).

        export LDFLAGS+=-L/usr/local/opt/openssl/lib
        export CPPFLAGS+=-I/usr/local/opt/openssl/include

3.  Build adcoind:

        ./autogen.sh
        ./configure --with-gui=qt5
        make

4.  It is also a good idea to build and run the unit tests:

        make check

5.  (Optional) You can also install adcoind to your path:

        make install

Use Qt Creator as IDE
------------------------
You can use Qt Creator as IDE, for debugging and for manipulating forms, etc.
Download Qt Creator from http://www.qt.io/download/. Download the "community edition" and only install Qt Creator (uncheck the rest during the installation process).

1. Make sure you installed everything through homebrew mentioned above
2. Do a proper ./configure --with-gui=qt5 --enable-debug
3. In Qt Creator do "New Project" -> Import Project -> Import Existing Project
4. Enter "adcoin-qt" as project name, enter src/qt as location
5. Leave the file selection as it is
6. Confirm the "summary page"
7. In the "Projects" tab select "Manage Kits..."
8. Select the default "Desktop" kit and select "Clang (x86 64bit in /usr/bin)" as compiler
9. Select LLDB as debugger (you might need to set the path to your installtion)
10. Start debugging with Qt Creator

Creating a release build
------------------------
You can ignore this section if you are building `adcoind` for your own use.

adcoind/adcoin-cli binaries are not included in the adcoin-Qt.app bundle.

If you are building `adcoind` or `adcoin-qt` for others, your build machine should be set up
as follows for maximum compatibility.

To create a build for others install the following dependency:
```bash        
brew install librsvg
```
After the dependcy is installed use the command:
```bash
make deploy
```

All dependencies should be compiled with these flags:

 -mmacosx-version-min=10.7
 -arch x86_64
 -isysroot $(xcode-select --print-path)/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.7.sdk

Once dependencies are compiled, see release-process.md for how the ADCOIN-Qt.app
bundle is packaged and signed to create the .dmg disk image that is distributed.

Running
-------

It's now available at `./adcoind`, provided that you are still in the `src`
directory. We have to first create the RPC configuration file, though.

Run `./adcoind` to get the filename where it should be put, or just try these
commands:

    echo -e "rpcuser=adcoinrpc\nrpcpassword=$(xxd -l 16 -p /dev/urandom)" > "/Users/${USER}/Library/Application Support/AdCoin/adcoin.conf"
    chmod 600 "/Users/${USER}/Library/Application Support/AdCoin/adcoin.conf"

The next time you run it, it will start downloading the blockchain, but it won't
output anything while it's doing this. This process may take several hours;
you can monitor its process by looking at the debug.log file, like this:

    tail -f $HOME/Library/Application\ Support/ADCOIN/debug.log

Other commands:
-------

    ./adcoind -daemon # to start the adcoin daemon.
    ./adcoin-cli --help  # for a list of command-line options.
    ./adcoin-cli help    # When the daemon is running, to get a list of RPC commands
