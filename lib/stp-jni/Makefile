# NOTE: you need to make libstp.a using -fPIC You will need to edit
# the CFLAGS inside stp's Makefile.common and inside stp/sat/core's
# Makefile
#
# NOTE: You must run the target that is suitable for your platform

JAVA_HOME_RAW := $(shell /usr/libexec/java_home -v 1.6) 
JAVA_HOME := $(strip $(JAVA_HOME_RAW))
UNAME := $(shell uname)

LONG_BITS := $(shell getconf LONG_BIT)

all: 
	@if test "$(LONG_BITS)" = "64"; \
	then \
		$(MAKE) all64; \
	else \
		$(MAKE) all32; \
	fi

clean:
	@if test "$(LONG_BITS)" = "64"; \
	then \
		$(MAKE) clean64; \
	else \
		$(MAKE) clean32; \
	fi


teststp:
	@if test "$(LONG_BITS)" = "64"; \
	then \
		$(MAKE) tests64; \
	else \
		$(MAKE) tests32; \
	fi

all32:
	g++ -Wall stp/STPJNI.c -o 32bit/libSTPJNI.so -shared -Iinclude -I/usr/lib/jvm/java-6-sun/include  -I/usr/lib/jvm/java-6-sun/include/linux -L32bit -lstp -fPIC  
	find . -name '*.java' -exec javac -nowarn -cp .:../junit.jar -g -source 1.5 {} \;
	jar -cvf STPJNI.jar stp/*.class stp/*.java
	@cp STPJNI.jar ../
	@cp 32bit/libSTPJNI.so ../
	@cp 32bit/libstp.a ../

all64:
	g++ -Wall stp/STPJNI.c -o 64bit/$(UNAME)/libSTPJNI.so -shared -Iinclude -I$(JAVA_HOME)/include  -I$(JAVA_HOME)/include/$(UNAME) -L64bit/$(UNAME) -lboost_system -lstp -fPIC  
	find . -name '*.java' -exec javac -nowarn -cp .:../junit.jar -g -source 1.5 {} \;
	jar -cvf STPJNI.jar stp/*.class stp/*.java
	@cp STPJNI.jar ../
	@cp 64bit/$(UNAME)/libSTPJNI.so ../
	@cp 64bit/$(UNAME)/libstp.a ../

clean32:
	rm -rf *~ stp/*.class tests/*.class stp/*.o tests/*.so 32bit/libSTPJNI.so 

clean64:
	rm -rf *~ stp/*.class tests/*.class stp/*.o tests/*.so 64bit/libSTPJNI.so

tests32:
	java -Djava.library.path=32bit -cp .:../junit.jar stp.examples.AllTests

tests64:
	java -Djava.library.path=64bit -cp .:../junit.jar stp.examples.AllTests
