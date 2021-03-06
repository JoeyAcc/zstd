# ################################################################
# Copyright (c) 2016-present, Yann Collet, Facebook, Inc.
# All rights reserved.
#
# This source code is licensed under both the BSD-style license (found in the
# LICENSE file in the root directory of this source tree) and the GPLv2 (found
# in the COPYING file in the root directory of this source tree).
# ################################################################

# This Makefile presumes libzstd is installed, using `sudo make install`

LDFLAGS += -lzstd

.PHONY: default all clean test

default: all

all: simple_compression simple_decompression \
	dictionary_compression dictionary_decompression \
	streaming_compression streaming_decompression \
	multiple_streaming_compression

simple_compression : simple_compression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

simple_decompression : simple_decompression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

dictionary_compression : dictionary_compression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

dictionary_decompression : dictionary_decompression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

streaming_compression : streaming_compression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

multiple_streaming_compression : multiple_streaming_compression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

streaming_decompression : streaming_decompression.c
	$(CC) $(CPPFLAGS) $(CFLAGS) $^ $(LDFLAGS) -o $@

clean:
	@rm -f core *.o tmp* result* *.zst \
        simple_compression simple_decompression \
        dictionary_compression dictionary_decompression \
        streaming_compression streaming_decompression \
        multiple_streaming_compression
	@echo Cleaning completed

test: all
	cp README.md tmp
	cp Makefile tmp2
	@echo -- Simple compression tests
	./simple_compression tmp
	./simple_decompression tmp.zst
	./streaming_decompression tmp.zst > /dev/null
	@echo -- Streaming compression tests
	./streaming_compression tmp
	./streaming_decompression tmp.zst > /dev/null
	@echo -- Edge cases detection
	! ./streaming_decompression tmp    # invalid input, must fail
	! ./simple_decompression tmp       # invalid input, must fail
	! ./simple_decompression tmp.zst   # unknown input size, must fail
	touch tmpNull                      # create 0-size file
	./simple_compression tmpNull
	./simple_decompression tmpNull.zst # 0-size frame : must work
	@echo -- Multiple streaming tests
	./multiple_streaming_compression *.c
	@echo -- Dictionary compression tests
	./dictionary_compression tmp2 tmp README.md
	./dictionary_decompression tmp2.zst tmp.zst README.md
	$(RM) tmp* *.zst
	@echo tests completed
