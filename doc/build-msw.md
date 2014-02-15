Original credit for this guide: https://bitcointalk.org/index.php?topic=149479.0

You must be able to:
* Read
* Follow instructions
* Type
* Use a mouse
* Install software
* Use Windows Explorer to copy files around

If you cannot do all of those things, (especially the second) do not ask for help.  I'll just make fun of you.

Some operations have steps that are obviously repeated.  I do not go into gory detail every single time, only the first time.

It must be said that this is HARD.  You can and will get seriously frustrated.  When that happens, I walk away for a while and then come back and find the solution.  

* DO NOT be intimidated by the technical jargon.  
* DO NOT be intimidated by the difficulty.  
* DO ask others for help.  
* DO NOT expect anyone to care more about this than you.
* DO READ THE DIRECTIONS AND FOLLOW THEM AND DO NOT ASK QUESTIONS IF YOU HAVEN'T.
* DO NOT ANSWER QUESTIONS FOR ANYONE WHO CANNOT READ AND FOLLOW DIRECTIONS.

This isn't a coin so much as an MBTI.  I digress.

1.0 Install 7z
--------------
a) http://downloads.sourceforge.net/sevenzip/7z920.msi (32-bit)

OR

http://downloads.sourceforge.net/sevenzip/7z920-x64.msi (64-bit)

1.0.1 Install Notepad ++
--------------
a) http://download.tuxfamily.org/notepadplus/6.5.3/npp.6.5.3.Installer.exe

1.0.2 Install Git
--------------
a) http://git-scm.com/downloads

b) Select the following options during installation:
* Run Git From Windows Command Prompt
* Check out Windows-style, commit Unix-style

1.1 Install msys:
--------------
a) http://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download

b) When the installer loads, click "Install" and/or "Continue" until you're presented with a list on the left hand side of the screen that starts with "Basic Setup". Click Basic Setup and on the list to the right scroll all the way down and click the checkbox for ONLY the "msys-base" package.  Select "Mark for installation", then click "Installation" from the top menu and "Apply Changes".

c) Then click "Install" or "Continue" or "Close" until you're back to the lists again and then exit the Installation Manager by clicking "Installation" and "Quit".

1.2 Install Perl:
--------------
a) http://downloads.activestate.com/ActivePerl/releases/5.18.1.1800/ActivePerl-5.18.1.1800-MSWin32-x64-297570.msi

b) Run the installer ("Next", "Next", "Next", "Install", blah blah blah).  If it asks to make changes to your computer, say "Yes".
You can exclude Perl Script, PPM, documentation and examples if you don't need them for other purposes.

1.3 Install Python:
--------------
a) http://www.python.org/ftp/python/3.3.3/python-3.3.3.amd64.msi

b) Same deal, install the software.  However, make sure the "Add Python to Path" option is selected.

1.4 Install GCC from MinGW-builds project:
--------------
a) Download and unpack i686-4.8.2-release-posix-dwarf-rt_v3-rev2.7z
http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/4.8.2/threads-posix/dwarf/i686-4.8.2-release-posix-dwarf-rt_v3-rev2.7z/download

b) Open Explorer and go to the download location, right click on the file and select "7-Zip" -> "Extract to i686-4.8.2-release-posix-dwarf-rt_v3-rev2/"

c) Double click on the i686-4.8.2-release-posix-dwarf-rt_v3-rev2 directory and copy the folder "mingw32" to Local Disk (C:) 

d) Click on Local Disk (C:) 

e) Copy the "mingw32" folder into the "MinGW" folder

1.5 Verify your paths are correct:
--------------
a) In Explorer, right click on "Computer" and select "Properties". 

b) Select "Advanced System Settings"

c) Click "Environment Variables"

d) In the box labeled "System Variables", locate "Path" and double click it.

e) In the box that comes up, scroll through the "Variable Value" and make sure you see the words "Perl" and "Python" and "MinGW"
* IF YOU DON'T SEE "PERL":
* * Go to the beginning of the text and ADD TO THE BEGINNING "C:\Perl64\site\bin;C:\Perl64\bin;"
* IF YOU DON'T SEE "PYTHON":
* * Go to the beginning of the text and ADD TO THE BEGINNING "C:\Python33;"
* IF YOU DON'T SEE "MINGW":
* * Go to the beginning of the text and ADD TO THE BEGINNING "C:\MinGW\mingw32\bin;"

Click "Ok", "Ok", "Ok", etc to close the Control Panel.			

Having fun yet?  We haven't even gotten started.

2. Download, unpack and build required dependencies.
--------------
a) Create a folder called "deps" on Local Disk (C:) [C:\deps]
b) In Explorer, navigate to C:\MinGW\msys\1.0\ and find "msys.bat".  Double click it to run it.  A window will open that looks like a command prompt.
c) In this section, download all of the files to the C:\deps folder.

2.1 Download OpenSSL: http://www.openssl.org/source/openssl-1.0.1e.tar.gz

a) From a MinGw shell (that window we opened), unpack the source archive with tar (this will avoid symlink issues) then configure and make (one command per line):
			cd /c/deps/

			tar xvfz openssl-1.0.1e.tar.gz

			cd openssl-1.0.1e

			Configure mingw

			make

Ta da!  You just compiled OpenSSL!

IF THIS FAILS... IF YOU SEE THE WORD "ERROR" OR "WHATEVER OF THE SORT... GO BACK TO STEP 1.5 AND TRY AGAIN.

If it works it doesn't pat you on the back.  It just stops without saying "ERROR".  Welcome to software development.

2.2 Berkeley DB: http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz

I used Berkeley DB 4.8.30 because it worked.  No other reason. 

a)From the MinGW shell, unpack the source archive, configure and make:

	cd /c/deps/

	tar xvfz db-4.8.30.NC.tar.gz

	cd db-4.8.30.NC/build_unix

	../dist/configure --disable-replication --enable-mingw --enable-cxx

	make

2.3 Boost: http://sourceforge.net/projects/boost/files/boost/1.55.0/
--------------
Some of the coddling stops here.  If you've made it this far successfully, you don't need me to hold your hand.

a) Unpack boost inside your C:\deps folder.
b) Then open a command prompt (CMD.EXE), and:

		cd C:\deps\boost_1_55_0\

		bootstrap.bat mingw

		b2 --build-type=complete --with-chrono --with-filesystem --with-program_options --with-system --with-thread toolset=gcc stage

This will compile the required boost libraries and put them into the stage folder (C:\deps\boost_1_55_0\stage).

2.4 Miniupnpc: http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.8.tar.gz
--------------
a) From the MSYS window (remember that?), type:

		tar xvfz miniupnpc-1.8.tar.gz

b) Using Explorer, rename containing folder from "miniupnpc-1.8" to "miniupnpc"
c) Change to the "miniupnpc" directory and find the file named "Makefile.mingw" and open in it Notepad ++
d) Go to line 61 and change "-enable-stdcall-fixup" to "--enable-stdcall-fixup"
e) Save the file
f) then from a Windows command prompt:

		cd C:\deps\miniupnpc

		mingw32-make -f Makefile.mingw init upnpc-static

IF THIS STEP FAILS, REPEAT STEP 1.5!

3.0 Download the PetroDollar source code
--------------
a) From a MSYS type:

		cd /c/

		git clone https://github.com/bryceweiner/petrodollar

		cd /c/petrodollar/src/leveldb

		TARGET_OS=NATIVE_WINDOWS make libleveldb.a libmemenv.a

b) Switch to the command prompt and type:

		cd C:\petrodollar\src

		mingw32-make -f makefile.mingw

		strip petrodollard.exe

CONGRATULATIONS!  YOU'VE JUST COMPILED A HEADLESS VERSION OF PETRODOLLAR!

Now... for the graphical wallet... and we're going to go really fast because you're a f'in pro now.

4. Compiling Qt 5.2.0 libraries and PetroDollar-Qt 
--------------
4.1 Download and unpack Qt base and tools sources:

a) http://download.qt-project.org/official_releases/qt/5.2/5.2.0/submodules/qtbase-opensource-src-5.2.0.7z
* Make a directory called C:\Qt
* Unpack the contents of the file into C:\Qt
* Rename the directory from qtbase-opensource-src-5.2.0 to 5.2.0

b) http://download.qt-project.org/official_releases/qt/5.2/5.2.0/submodules/qttools-opensource-src-5.2.0.7z
* Unpack the contents of the file into C:\Qt

c) Open a command prompt and type:

	cd\Qt\5.2.0

	configure.bat -release -opensource -confirm-license -static -make libs -no-sql-sqlite -no-opengl -qt-zlib -qt-pcre -no-icu -no-gif -qt-libpng -qt-libjpeg -no-angle -no-vcproj -no-openssl -no-dbus -no-audio-backend -no-wmf-backend -no-qml-debug

	mingw32-make

e) Remember when we had to check the Path variable before?  We need to do that again.  Reference the instructions above on how to access it.  Add the following path to the beginning:

	C:\Qt\5.2.0\bin;

f) close and reopen the command prompt window so the new path will take effect.

g) Type:

	cd C:\Qt\qttools-opensource-src-5.2.0

	qmake qttools.pro

	mingw32-make

5. Compiling the PetroDollar-Qt client
--------------
5.1 In Notepad ++, open the file C:\petrodollar\petrodollar-qt.pro

a) Go to line 119

b) Add a hashtag to the start of the line so it looks like this:

	#genleveldb.commands = cd $$PWD/src/leveldb && CC=$$QMAKE_CC CXX=$$QMAKE_CXX TARGET_OS=OS_WINDOWS_CROSSCOMPILE $(MAKE) OPT=\"$$QMAKE_CXXFLAGS $$QMAKE_CXXFLAGS_RELEASE\" libleveldb.a libmemenv.a && $$QMAKE_RANLIB $$PWD/src/leveldb/libleveldb.a && $$QMAKE_RANLIB $$PWD/src/leveldb/libmemenv.a

* This is needed for Windows compiles.  For Linux compiles, remove the hashtag.

5.2 In the command prompt type:

	cd\petrodollar

	qmake petrodollar-qt.pro

	mingw32-make -f Makefile.Release

YOU DID IT!

Don't forget to create your .conf files!
