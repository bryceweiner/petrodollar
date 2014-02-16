AmKoin-Qt build instructions for Mac OS X
==========================================

This document build and configuration instructions for building AmKoin-Qt (Qt4 GUI for AmKoin) for OS X.  A amkoind binary is not included in the AmKoin-Qt.app bundle but can be built using the instructions in doc/build-osx.md.

Tested on OS X 10.5 through 10.9 on Intel processors only. PPC is not supported because it is big-endian.

All of the commands should be executed in a Terminal application. The built-in one is located in /Applications/Utilities.

Preparation
-----------

You need to install XCode with all the options checked so that the compiler and everything is available in /usr not just /Developer. XCode is available from the Mac App Store or from https://developer.apple.com/xcode/. If you install Xcode 4.3 or later, you'll need to install its command line tools. This can be done in `Xcode > Preferences > Downloads > Components` and generally must be re-done or updated every time Xcode is updated.

There's an assumption that you already have `git` installed, as well. If not, it's the path of least resistance to install [Github for Mac](https://mac.github.com/) (OS X 10.7+) or [Git for OS X](https://code.google.com/p/git-osx-installer/). It is also available via Homebrew or MacPorts.

You will also need to install [Homebrew](http://brew.sh) or [MacPorts](https://www.macports.org/) in order to install library dependencies. It's largely a religious decision which to choose, but, as of December 2012, MacPorts is a little easier because you can just install the dependencies immediately - no other work required. If you're unsure, read the instructions through first in order to assess what you want to do. Homebrew is a little more popular among those newer to OS X.

The rest of this guide will use Homebrew.

The installation of the actual dependencies is covered in the Instructions sections below.

Instructions
------------


#### Install dependencies using Homebrew

        brew install autoconf automake berkeley-db4 boost miniupnpc openssl pkg-config protobuf qt qrcode

Note: After you have installed the dependencies, you should check that the Homebrew installed version of OpenSSL is the one available for compilation. You can check this by typing

        openssl version

into Terminal. You should see OpenSSL 1.0.1f 6 Jan 2014.

If not, you can ensure that the Homebrew OpenSSL is correctly linked by running

        brew link openssl --force

Rerunning "openssl version" should now return the correct version.

### Install MacDeploy dependencies

You will need the appscript package for the disk image creation to work:

        easy_install appscript

For Snow Leopard (which uses [Python 2.6](http://www.python.org/download/releases/2.6/)), you will need the param_parser package:
        
        easy_install argparse

### Modify amkoin-qt.pro

Modify amkoin-qt.pro since it has options set to build for a Windows environment.

Comment out the follow lines by adding a # in front of them.

        windows:LIBS += -lshlwapi
        LIBS += $$join(BOOST_LIB_PATH,,-L,) $$join(BDB_LIB_PATH,,-L,) $$join(OPENSSL_LIB_PATH,,-L,) $$join(QRENCODE_LIB_PATH,,-L,)
        LIBS += -lssl -lcrypto -ldb_cxx$$BDB_LIB_SUFFIX
        windows:LIBS += -lws2_32 -lole32 -loleaut32 -luuid -lgdi32
        LIBS += -lboost_system-mgw48-mt-sd-1_55 -lboost_filesystem-mgw48-mt-sd-1_55 -lboost_program_options-mgw48-mt-sd-1_55 -lboost_thread-mgw48-mt-sd-1_55
        BOOST_LIB_SUFFIX=-mgw48-mt-sd-1_55
        BOOST_INCLUDE_PATH=C:/deps/boost_1_55_0
        BOOST_LIB_PATH=C:/deps/boost_1_55_0/stage/lib
        BDB_INCLUDE_PATH=c:/deps/db-4.8.30.NC/build_unix
        BDB_LIB_PATH=c:/deps/db-4.8.30.NC/build_unix
        OPENSSL_INCLUDE_PATH=c:/deps/openssl-1.0.1e/include
        OPENSSL_LIB_PATH=c:/deps/openssl-1.0.1e
        MINIUPNPC_LIB_PATH=c:/deps/miniupnpc
        MINIUPNPC_INCLUDE_PATH=c:/deps/

Perform Mac build
-----------------

The official Bitcoin OSX binaries are created on an older 32-bit, OSX 10.6 machine for maximum compatibility.

If you have an older machine, feel free to set the 'RELEASE=1' flag which add some extra compilation flags that aren't available in later versions of OS X.

Generate a Qt Makefile

        qmake USE_UPNP=1 USE_QRCODE=1 amkoin-qt.pro

Run the compilation

        make

### Build using MacDeploy

During the process, the disk image window will pop up briefly where the settings are applied. This is normal, please do not interfere with it and allow the window to disappear.

        export QTDIR=/usr/local/opt/qt  # needed to find translations/qt_*.qm files
        T=$(contrib/qt_translations.py $QTDIR/translations src/qt/locale)
        /usr/local/bin/python share/qt/clean_mac_info_plist.py
        /usr/local/bin/python contrib/macdeploy/macdeployqtplus AmKoin-Qt.app -add-qt-tr $T -dmg -fancy contrib/macdeploy/fancy.plist

You may receive this error:

       112:116: execution error: Finder got an error: Can’t get disk "AmKoin-Qt". (-1728)
       Error running osascript.

If you do, remove the temporarily created dmg file and run the last command again

       rm AmKoin-Qt.temp.dmg

When finished, it will produce a beautifully packaged Apple Disk Image `AmKoin-Qt.dmg`.

