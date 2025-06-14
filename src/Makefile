#
# Makefile for ./src path, used by configure
#

EXEEXT = 
PROGRAM = gocr$(EXEEXT)
# lib removed for simplification (v0.46)
# but Igor from OSRA an optical chemical structure recognition software 
#  wants it (v0.47 Mar09)
PGMASCLIB = Pgm2asc
#LIBPGMASCLIB = lib$(PGMASCLIB).a
# ToDo: need a better pgm2asc.h for lib users 
#INCLUDEFILES = gocr.h
# avoid german compiler messages
LANG=C

LIBOBJS=pgm2asc.o \
	box.o \
	database.o \
	detect.o \
	barcode.o \
	lines.o \
	list.o \
	ocr0.o \
	ocr0n.o \
	ocr1.o \
	otsu.o \
	output.o \
	pixel.o \
	unicode.o \
	remove.o \
	pnm.o \
	pcx.o \
	progress.o \
	job.o

# these two lines are for cross-compiling, not tested
#srcdir = .
#VPATH = .
bindir = ${exec_prefix}/bin
#  lib removed for simplification
#libdir = ${exec_prefix}/lib
#includedir = ${prefix}/include

CC=gcc
# lib removed for simplification
# ar,ranlib detection removed from configure (simplicity)
#  but needed by some developpers (linux only) for for (make libs)
# RANLIB = @RANLIB@
# AR = @AR@
RANLIB = ranlib
AR = ar
INSTALL=/usr/bin/install -c
# shell is needed for OS/2 to let if test -r $(PROGRAM) ... work
SHELL=/bin/sh

DEFS=-DHAVE_CONFIG_H
CPPFLAGS=
# to see the config.h
CFLAGS=-g -O2 $(CPPFLAGS) -I../include $(DEFS)
LDFLAGS=-lm 
LIBS=-lnetpbm 
DESTDIR=/usr/local

.SUFFIXES: .s .o .c .h
# do not look for files if help (etc) is given
.PHONY : doc clean install libs default

.c.o: gocr.h pgm2asc.h ../include/config.h
	$(CC) $(CFLAGS) -c -o $*.o $<

default: all

# all: $(PROGRAM) lib$(PGMASCLIB).a
all: $(PROGRAM)

gocr.o: gocr.h Makefile ../include/version.h

.c.h:

# Aug2010
unicode.o: unicode_defs.h
list.o pgm2asc.o: list.h

#$(PROGRAM): lib$(PGMASCLIB).a gocr.o
$(PROGRAM): $(LIBOBJS) gocr.o
	# make it conform to ld --as-needed
	#$(CC) -o $@ $(LDFLAGS) gocr.o ./lib$(PGMASCLIB).a $(LIBS)
	$(CC) -o $@ $(LDFLAGS) gocr.o $(LIBOBJS) $(LIBS)
	# if test -r $(PROGRAM); then cp $@ ../bin; fi

libs: lib$(PGMASCLIB).a lib$(PGMASCLIB).0.54.so

#lib$(PGMASCLIB).0.54.so: $(LIBOBJS)
#	$(CC) -fPIC -shared -Wl,-h$@ -o $@ $(LIBOBJS)
lib$(PGMASCLIB).0.54.so: $(LIBOBJS:%.o=%.c) Makefile
	# .o must be build with -fPIC too (Aug2010 JS)
	$(CC) $(CFLAGS) -fPIC -shared -Wl,-h$@ -o $@ $(LIBOBJS:%.o=%.c)
	-rm -f lib$(PGMASCLIB).so  # set new link
	-ln -s $@ lib$(PGMASCLIB).so

lib$(PGMASCLIB).a: $(LIBOBJS)
	#	-rm -f $@
	$(AR) cru $@ $(LIBOBJS)
	$(RANLIB) $@

$(LIBOBJS): Makefile

# PHONY = don't look at file clean, -rm = start rm and ignore errors
.PHONY : clean proper install uninstall
install: all
	#$(INSTALL) -d $(DESTDIR)$(bindir) $(DESTDIR)$(libdir) $(DESTDIR)$(includedir)
	$(INSTALL) -d $(DESTDIR)$(bindir)
	$(INSTALL) $(PROGRAM) $(DESTDIR)$(bindir)
	$(INSTALL) ../bin/gocr.tcl   $(DESTDIR)$(bindir)  # better X11/bin?
	if test -f lib$(PGMASCLIB).a; then\
	 $(INSTALL) lib$(PGMASCLIB).a $(DESTDIR)$(libdir);\
	 $(INSTALL) lib$(PGMASCLIB).0.54.so $(DESTDIR)$(libdir);\
	 $(INSTALL) lib$(PGMASCLIB).so $(DESTDIR)$(libdir);\
	fi
	# ToDo: not sure that the link will be installed correctly
	#$(INSTALL) $(INCLUDEFILES) $(DESTDIR)$(includedir)

# directories are not removed
uninstall:
	-rm -f $(DESTDIR)$(bindir)/$(PROGRAM)
	-rm -f $(DESTDIR)$(bindir)/gocr.tcl
	-rm -f $(DESTDIR)$(libdir)/lib$(PGMASCLIB).a
	-rm -f $(DESTDIR)$(libdir)/lib$(PGMASCLIB).0.54.so
	-rm -f $(DESTDIR)$(libdir)/lib$(PGMASCLIB).so
	# ToDo: set to old version.so ?
	#for X in $(INCLUDEFILES); do rm -f $(DESTDIR)$(includedir)/$$X; done

clean:
	-rm -f *.o *~

proper: clean
	-rm -f gocr libPgm2asc.*
	-rm -f gocr
	
