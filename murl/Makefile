CC = gcc
CFLAGS+=-I. -I.. -g -O0 -fPIC -Wall
LDFLAGS+=
INCDIRS+=

SOURCES=http_parser.c murl.c murl_http.c 
OBJECTS=$(SOURCES:.c=.o)

TEST_SOURCES=test/ut_main.c test/ut_tls.c test/ut_get.c test/ut_post.c test/ut_util.c ../parson.c
TEST_OBJECTS=$(TEST_SOURCES:.c=.o)

all: murl test libmurl.a libmurl.so

libmurl.a: $(OBJECTS)
	ar rcs libmurl.a $(OBJECTS)

.c.o:
	$(CC) $(INCDIRS) $(CFLAGS) -c $< -o $@

libmurl.so: $(OBJECTS)
	$(CC) $(INCDIRS) $(CFLAGS) -shared -Wl,-soname,libmurl.so.1.0.0 -o libmurl.so.1.0.0 $(OBJECTS) $(LDFLAGS) -lcrypto -lssl
	ln -fs libmurl.so.1.0.0 libmurl.so

murl:	libmurl.so
	$(CC) $(INCDIRS) -I.. $(CFLAGS) murl_cli.c -o murl $(LDFLAGS) -L. -lmurl -lcrypto -lssl 

test:	$(TEST_OBJECTS) libmurl.so
	$(CC) $(INCDIRS) -I.. $(CFLAGS) $(TEST_OBJECTS) -o ut-murl $(LDFLAGS) -L. -lmurl -lcrypto -lssl -lpthread


clean:
	rm -f *.[ao]
	rm -f libmurl.so.1.0.0
	rm -f libmurl.so
	rm -f murl
	rm -f ut-murl
