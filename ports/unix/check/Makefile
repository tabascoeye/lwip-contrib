#
# Copyright (c) 2001, 2002 Swedish Institute of Computer Science.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without modification,
# are permitted provided that the following conditions are met:
#
# 1. Redistributions of source code must retain the above copyright notice,
#    this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution.
# 3. The name of the author may not be used to endorse or promote products
#    derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE AUTHOR ``AS IS'' AND ANY EXPRESS OR IMPLIED
# WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT
# SHALL THE AUTHOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT
# OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING
# IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY
# OF SUCH DAMAGE.
#
# This file is part of the lwIP TCP/IP stack.
#
# Author: Adam Dunkels <adam@sics.se>
#

CODEDIR=../../../../lwip
LWIPDIR=$(CODEDIR)/src
CONTRIBDIR=../../..
LWIPARCH=$(CONTRIBDIR)/ports/unix

LDFLAGS:=$(LDFLAGS) -lcheck -lm -lpthread -lrt -lgcov -coverage
CFLAGS:=$(CFLAGS) -DLWIP_UNIX_EMPTY_ASSERT -I/usr/include/check -I$(LWIPDIR)/../test/unit \
	-I$(LWIPDIR)/include \
	-I$(LWIPDIR)/include/lwip \
	-I$(LWIPDIR)/include/netif \
	-Iunix/include -I. -I../include \
	-fprofile-arcs -ftest-coverage

include $(LWIPDIR)/Filelists.mk

ARCHFILES=$(wildcard $(LWIPARCH)/*.c)

# LWIPFILES: All the above.
LWIPFILES=$(LWIPNOAPPSFILES) $(ARCHFILES)
LWIPOBJS=$(LWIPFILES:.c=.o)

TESTDIR=$(CODEDIR)/test/unit
TESTFILES=$(TESTDIR)/lwip_unittests.c $(TESTDIR)/udp/test_udp.c $(TESTDIR)/etharp/test_etharp.c $(TESTDIR)/tcp/tcp_helper.c \
	$(TESTDIR)/tcp/test_tcp_oos.c $(TESTDIR)/tcp/test_tcp.c $(TESTDIR)/core/test_mem.c $(TESTDIR)/dhcp/test_dhcp.c $(TESTDIR)/core/test_pbuf.c
TESTOBJS=$(TESTFILES:.c=.o)

%.o: %.c Makefile $(TESTDIR)/lwipopts.h
	$(CC) $(CFLAGS) -c $< -o $@

all: lwip_unittests
.PHONY: all clean check

clean:
	@rm -f lwip_unittests $(LWIPOBJS) $(TESTOBJS)
	@rm -f $(LWIPDIR)/api/*.gc*
	@rm -f $(LWIPDIR)/core/*.gc*
	@rm -f $(LWIPDIR)/core/ipv4/*.gc*
	@rm -f $(LWIPDIR)/core/ipv6/*.gc*
	@rm -f $(LWIPDIR)/core/snmp/*.gc*
	@rm -f $(LWIPDIR)/netif/*.gc*
	@rm -f $(LWIPDIR)/netif/ppp/*.gc*
	@rm -f $(LWIPDIR)/netif/ppp/polarssl/*.gc*
	@rm -f $(CODEDIR)/test/unit/*.gc*
	@rm -f $(CODEDIR)/test/unit/core/*.gc*
	@rm -f $(CODEDIR)/test/unit/dhcp/*.gc*
	@rm -f $(CODEDIR)/test/unit/etharp/*.gc*
	@rm -f $(CODEDIR)/test/unit/tcp/*.gc*
	@rm -f $(CODEDIR)/test/unit/udp/*.gc*

lwip_unittests: $(TESTOBJS) $(LWIPOBJS)
	$(CC) $(TESTOBJS) $(LWIPOBJS) -o lwip_unittests $(LDFLAGS)

check: lwip_unittests
	@./lwip_unittests
