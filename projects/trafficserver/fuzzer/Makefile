TARGET=Fuzzing

#Binary
EsiEXE=FuzzEsi
HttpEXE=FuzzHTTP

#Flags
INCDIR=-I..
LIBDIR=-L..
MANFLAGS=-DHAVE_CONFIG_H 
CPPFLAGS=-D_GNU_SOURCE -DOPENSSL_NO_SSL_INTERN -DOPENSSL_API_COMPAT=10002 -DOPENSSL_IS_OPENSSL3
AM_CFLAGS=-std=gnu99 -g -pipe -Wall -Wno-deprecated-declarations -Qunused-arguments -Wextra -Wno-ignored-qualifiers -Wno-unused-parameter -fno-strict-aliasing -mcx16
AM_CXXFLAGS=-std=c++17 -g -pipe -Wall -Wno-deprecated-declarations -Qunused-arguments -Wextra -Wno-ignored-qualifiers -Wno-unused-parameter -fno-strict-aliasing -Wno-invalid-offsetof -mcx16
AM_CPPFLAGS=-Dlinux -D_LARGEFILE64_SOURCE=1 -D_COMPILE64BIT_SOURCE=1 -D_REENTRANT -D__STDC_LIMIT_MACROS=1 -D__STDC_FORMAT_MACROS=1
MACROS=$(MANFLAGS) $(CPPFLAGS) $(AM_CPPFLAGS)

#EsiEXE
EsiINC=\
	$(INCDIR)/include \
	$(INCDIR)/plugins/esi \
	$(INCDIR)/plugins/esi/common \
	$(INCDIR)/plugins/esi/lib \
	$(INCDIR)/plugins/esi/fetcher 

EsiLIB=$(LIB_FUZZING_ENGINE) \
	$(LIBDIR)/plugins/esi/.libs/ \
	-lesicore

#HttpEXE
HttpINC=\
	$(INCDIR)/include \
	$(INCDIR)/iocore/aio \
	$(INCDIR)/iocore/cache \
	$(INCDIR)/iocore/dns \
	$(INCDIR)/iocore/eventsystem \
	$(INCDIR)/iocore/hostdb \
	$(INCDIR)/iocore/net \
	$(INCDIR)/iocore/net/quic \
	$(INCDIR)/iocore/utils \
	$(INCDIR)/lib \
	$(INCDIR)/lib/swoc/include \
	$(INCDIR)/proxy/api \
	$(INCDIR)/proxy/hdrs \
	$(INCDIR)/proxy/http

CoreLib=$(LIB_FUZZING_ENGINE) ../proxy/hdrs/libhdrs.a ../proxy/http/libhttp.a ../proxy/shared/libdiagsconfig.a ../iocore/eventsystem/libinkevent.a ../src/records/librecords_p.a ../src/tscore/libtscore.a \
	$(LIBDIR)/lib/swoc/.libs/ -ltsswoc \
	$(LIBDIR)/src/api/.libs/ -ltsapi \
	$(LIBDIR)/src/tscpp/util/.libs/ -ltscpputil \
	-lpcre -lssl -lcrypto -lresolv \
	$(LIBDIR)/src/tscpp/util/.libs/ -ltscpputil

all: $(TARGET)

$(TARGET):
	$(CXX) $(CXXFLAGS) $(AM_CXXFLAGS) $(MACROS) $(EsiINC) -c $(EsiEXE).cc
	$(CXX) $(CXXFLAGS) $(AM_CXXFLAGS) -o $(EsiEXE) $(EsiEXE).o $(EsiLIB)

	$(CXX) $(CXXFLAGS) $(AM_CXXFLAGS) $(MACROS) $(HttpINC) -c $(HttpEXE).cc
	$(CXX) $(CXXFLAGS) $(AM_CXXFLAGS) -o $(HttpEXE) $(HttpEXE).o $(CoreLib)

clean:
	rm $(EsiEXE) $(HttpEXE) *.o

.PHONY: all clean
