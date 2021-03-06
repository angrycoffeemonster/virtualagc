# Copyright 2003-2005,2009 Ronald S. Burkey <info@sandroid.org>
# 
# This file is part of yaAGC.
#
# yaAGC is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# yaAGC is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with yaAGC; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
#
# Filename:	Makefile
# Purpose:	Makefile for the yaYUL assembler.
# Mods:		04/11/03 RSB.	Began. 
#		10/22/03 RSB.	Added the "install" target.
#		05/14/04 RSB.	Added PREFIX.
#		04/30/05 RSB	Added CFLAGS
#		03/06/09 RSB	Added the 'all-archs' target and made 
#				various relevant adjustments.
#		2012-10-03 JL	Add YAYUL_TRACE define to turn on debug
#				messages.
#		2016-08-18 RSB	Changed the way YAYUL_TRACE works, since
#				(at least for the moment) I don't want to
#				use it, so I don't want it hard-coded into
#				CFLAGS.  It can still be used, though, just
#				invoke the make as
#					YAYUL_TRACE=-DYAYUL_TRACE make
#				(from Linux, anyway).
#		2016-08-19 RSB	Didn't do that quite right.

ifndef PREFIX
PREFIX=/usr/local
endif

ifdef NATIVE
PREFIX_WIN=
BIN_WIN=
else
PREFIX_WIN=i386-mingw32-
PREFIX_MAC=powerpc-apple-darwin9-
BIN_WIN=${HOME}/IMCROSS/i386-mingw32/bin/
BIN_MAC=${HOME}/IMCROSS/mac/bin/
INC_MAC=${HOME}/IMCROSS/mac/include
SDK_MAC=${HOME}/IMCROSS/mac/SDKs/MacOSX10.4u.sdk
endif

ifeq ($(NATIVE),WIN32)
   EXENAME=yaYUL.exe
else
   EXENAME=yaYUL
endif

CFLAGS += ${YAYUL_TRACE}

.PHONY: default
default: $(EXENAME)

.PHONY: all-archs
all-archs: default yaYUL.exe yaYUL-macosx

CFILES:=$(wildcard *.c)

yaYUL:	$(CFILES:%.c=%.o)
	gcc ${CFLAGS} -o $@ $^ -lm

yaYUL.exe: ${CFILES}
	$(PREFIX_WIN)gcc ${CFLAGS} -DNVER=${NVER} \
		-Wall -o $@ $^ -lm

yaYUL-macosx: ${CFILES}
	$(PREFIX_MAC)gcc -arch ppc -arch i386 ${CFLAGS} -DNVER=${NVER} -Wall -o $@ $^ -lm

clean:
	-rm -f yaYUL *.o *~ *.exe *-macosx Utilities/SplitInterp Utilities/*.exe

install: yaYUL
	cp  yaYUL ${PREFIX}/bin
	chmod ugo+x ${PREFIX}/bin/yaYUL

%.o: %.c yaYUL.h
	gcc ${CFLAGS} -DNVER=${NVER} -DINSTALLDIR=${PREFIX} -Wall -c -o $@ $<
	

