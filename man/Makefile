#
# Makefile for ./man path, used by configure
#

PROGRAM = gocr

mandir = ${prefix}/share/man

INSTALL=/usr/bin/install -c
DESTDIR=/usr/local

default: all

all:

# PHONY = don't look at file clean, -rm = start rm and ignore errors
.PHONY : clean proper install uninstall
install: all
	$(INSTALL) -d $(DESTDIR)$(mandir)/man1
	$(INSTALL) ../man/man1/$(PROGRAM).1 $(DESTDIR)$(mandir)/man1

uninstall:
	-rm -f $(DESTDIR)$(mandir)/man1/$(PROGRAM).1

clean:
	-rm -f *.o *~

proper: clean
	
