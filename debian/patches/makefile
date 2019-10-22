Description: Makefile updates for Debian and binary renaming
 This patch includes the Debian-related changes with respect re-factoring
 to resolve Debian RC due to naming conflict with another package. This
 package is currently unmaintained upstream.
Author: Anuradha Weeraman <aweeraman@gmail.com>
Origin: other
Last-Update: 2019-10-22

diff --git a/Makefile b/Makefile
index 545b445..4e915ab 100644
--- a/Makefile
+++ b/Makefile
@@ -2,43 +2,45 @@
 # these are set by config
 CC = g++
 DESTDIR ?= /usr
-LCFLAGS = -g -O2
+LCFLAGS = -g -O2 -Wno-write-strings
 BINDIR = ${DESTDIR}/bin
 MANDIR = ${DESTDIR}/share/man
 INCLUDEDIR = ${DESTDIR}/include
-NOGNU = /usr/include/nognu
+NOGNU = /usr/include/ncc.h
+SHAREDIR = ${DESTDIR}/share/ncc
 
 #
 
 CFLAGS = $(LCFLAGS) -c
+LDFLAGS = $(shell dpkg-buildflags --get LDFLAGS)
 
-
-tout: objdir/ncc nccnav/nccnav
+tout: objdir/nccgen nccnav/nccnav
 	@echo Salut.
 
 install: tout
-	cp objdir/ncc $(BINDIR)/ncc
-	cp scripts/nccstrip2.py $(BINDIR)/nccstrip2.py
-	ln -sf $(BINDIR)/ncc $(BINDIR)/nccar
-	ln -sf $(BINDIR)/ncc $(BINDIR)/nccld
-	ln -sf $(BINDIR)/ncc $(BINDIR)/nccc++
-	ln -sf $(BINDIR)/ncc $(BINDIR)/nccg++
+	cp objdir/nccgen $(BINDIR)/nccgen
+	mkdir $(SHAREDIR)
+	cp scripts/nccstrip2.py $(SHAREDIR)/nccstrip2.py
+	ln -sf $(BINDIR)/nccgen $(BINDIR)/nccar
+	ln -sf $(BINDIR)/nccgen $(BINDIR)/nccld
+	ln -sf $(BINDIR)/nccgen $(BINDIR)/nccc++
+	ln -sf $(BINDIR)/nccgen $(BINDIR)/nccg++
 	cp nccnav/nccnav $(BINDIR)/nccnav
 	ln -fs $(BINDIR)/nccnav $(BINDIR)/nccnavi
-	cp ncc.1 $(MANDIR)/man1
+	cp ncc.1 $(MANDIR)/man1/nccgen.1
 	cp nccnav/nccnav.1 $(MANDIR)/man1
 	cp doc/nognu $(INCLUDEDIR)
 
 uninstall:
-	rm -f $(BINDIR)/ncc $(BINDIR)/nccnav $(BINDIR)/nccnavi $(MANDIR)/man1/ncc.1 $(INCLUDEDIR)/nognu
+	rm -f $(BINDIR)/nccgen $(BINDIR)/nccnav $(BINDIR)/nccnavi $(MANDIR)/man1/nccgen.1 $(INCLUDEDIR)/nognu
 	rm -f $(BINDIR)/nccar $(BINDIR)/nccld $(BINDIR)/nccc++ $(BINDIR)/nccg++
 
 nccnav/nccnav: nccnav/nccnav.C
 	@echo Compiling nccnav viewer.
 	@cd nccnav && make
 
-objdir/ncc: objdir/dbstree.o objdir/inttree.o objdir/lex.o objdir/space.o objdir/cexpand.o objdir/cdb.o objdir/parser.o objdir/ccexpr.o objdir/preproc.o objdir/usage.o main.C
-	$(CC) $(LCFLAGS) main.C objdir/*.o -o objdir/ncc 
+objdir/nccgen: objdir/dbstree.o objdir/inttree.o objdir/lex.o objdir/space.o objdir/cexpand.o objdir/cdb.o objdir/parser.o objdir/ccexpr.o objdir/preproc.o objdir/usage.o main.C
+	$(CC) $(LCFLAGS) $(LDFLAGS) main.C objdir/*.o -o objdir/nccgen
 
 objdir/cexpand.o: cexpand.C
 	$(CC) $(CFLAGS) cexpand.C
@@ -87,5 +89,5 @@ clean:
 	rm -f objdir/*.o
 
 distclean:
-	rm -f objdir/* objdir/ncc
+	rm -f objdir/* objdir/nccgen
 	@cd nccnav && make clean
diff --git a/config.h b/config.h
index e5722b5..35511b4 100644
--- a/config.h
+++ b/config.h
@@ -14,7 +14,7 @@
 #define GNU_VIOLATIONS
 #define LABEL_VALUES
 #define OUTPUT_EXT ".nccout"
-#define NOGNU "/usr/include/nognu"
+#define NOGNU "/usr/include/ncc.h"
 #define NCC_INFILE_KEY "ncc-key"
 #define FAKE_VARIABLE_ARRAYS
 #define NCC_ISOC99
diff --git a/nccnav/Makefile b/nccnav/Makefile
index 37a4055..c89f310 100644
--- a/nccnav/Makefile
+++ b/nccnav/Makefile
@@ -1,12 +1,13 @@
 
 CC = g++
 LCFLAGS = -Wall -g
-CFLAGS = $(LCFLAGS) -c -g
+CFLAGS = $(LCFLAGS) -c -g -Wno-write-strings
+LDFLAGS = $(shell dpkg-buildflags --get LDFLAGS)
 
 OBJDIR = ../objdir
 
 $(OBJDIR)/nccnav: nccnav.C $(OBJDIR)/dbstree.o $(OBJDIR)/inttree.o
-	$(CC) -g nccnav.C -I.. -lncurses -o nccnav $(OBJDIR)/dbstree.o $(OBJDIR)/inttree.o
+	$(CC) $(LDFLAGS) -g -Wno-write-strings nccnav.C -I.. -lncurses -o nccnav $(OBJDIR)/dbstree.o $(OBJDIR)/inttree.o
 
 $(OBJDIR)/dbstree.o: ../dbstree.[Ch]
 	$(CC) $(CFLAGS) ../dbstree.C -c
