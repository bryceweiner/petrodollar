UNIX BUILD NOTES
====================
Ubuntu 13.10 build instructions
-------------------------------
Build instructions assume a CLEAN DEVELOPMENT ENVIRONMENT FOR COMPILING.  This is 2014.  If you don't develop complex software in VMs you need to get with the program.  This does not give you compiling options or explain them.  That's up to you to learn.  This makes it work.

	sudo apt-get upgrade

	sudo apt-get install git build-essential libssl-dev libboost-all-dev libdb-dev libdb++-dev libminiupnpc-dev

	git clone http://github.com/bryceweiner/petrodollar

	cd petrodollar/src

	make clean

	cd petrodollar/src/leveldb

	sudo sh ./build_detect_platform build_config.mk .

	make -f Makefile libleveldb.a libmemenv.a

	cd ..

	make -f makefile.unix

	strip petrodollard
