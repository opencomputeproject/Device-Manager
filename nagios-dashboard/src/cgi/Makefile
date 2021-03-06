###############################
# Makefile for Nagios CGIs
#
###############################

# Source code directories
SRC_COMMON=../common
SRC_INCLUDE=../include
SRC_XDATA=../xdata
SRC_LIB=../lib

prefix=/usr/local/nagios
exec_prefix=/usr/local/nagios
LOGDIR=/usr/local/nagios/var
CFGDIR=/usr/local/nagios/etc
BINDIR=/usr/local/nagios/bin
CGIDIR=/usr/local/nagios/sbin
HTMLDIR=/usr/local/nagios/share
INSTALL=/usr/bin/install -c
INSTALL_OPTS=-o nagios -g nagios
COMMAND_OPTS=-o nagios -g nagios
STRIP=/usr/bin/strip

CGIEXTRAS= statuswrl.cgi statusmap.cgi trends.cgi histogram.cgi

CP=@CP@
CC=gcc
CFLAGS=-Wall -I.. -g -O2 -DHAVE_CONFIG_H -DNSCGI
JSONFLAGS=-DJSON_NAGIOS_4X

# Compiler flags for optimization (overrides default)
#CFLAGS=-O3 -Wall -Wshadow -Wpointer-arith -Wcast-qual -Wcast-align -Wstrict-prototypes -Wmissing-prototypes -Wnested-externs -DHAVE_CONFIG_H -DNSCGI

# Compiler flags for optimization (complements default)
#CFLAGS_WARN=-Wall -Wshadow -Wpointer-arith -Wcast-qual -Wcast-align -Wstrict-prototypes -Wmissing-prototypes -Wnested-externs
#CFLAGS_DEBUG=-ggdb3 -g3
#CFLAGS+=$(CFLAGS_WARN) $(CFLAGS_DEBUG)

LDFLAGS=
LIBS=

CGIS=archivejson.cgi avail.cgi cmd.cgi config.cgi extinfo.cgi history.cgi notifications.cgi objectjson.cgi outages.cgi showlog.cgi ec_host_status.cgi status.cgi statusjson.cgi statuswml.cgi summary.cgi tac.cgi $(CGIEXTRAS)

# Object functions
ODATALIBS=objects-cgi.o xobjects-cgi.o
ODATAHDRS=
ODATADEPS=$(ODATALIBS)

# Host, service, and program status functions
SDATALIBS=statusdata-cgi.o xstatusdata-cgi.o comments-cgi.o downtime-cgi.o
SDATAHDRS=
SDATADEPS=$(SDATALIBS)

# Host and service comment functions
CDATALIBS=
CDATAHDRS=
CDATADEPS=$(CDATALIBS)

# Host and service downtime functions
DDATALIBS=
DDATAHDRS=
DDATADEPS=$(DDATALIBS)

# Common CGI functions (includes object and status functions)
CGILIBS=$(SRC_COMMON)/shared.o getcgi.o cgiutils.o cgiauth.o macros-cgi.o $(SNPRINTF_O) $(ODATALIBS) $(SDATALIBS) $(SRC_LIB)/libnagios.a
CGIHDRS=$(SRC_INCLUDE)/config.h $(SRC_INCLUDE)/common.h $(SRC_INCLUDE)/locations.h
CGIDEPS=$(CGILIBS) $(ODATADEPS) $(SDATADEPS) $(SRC_LIB)/libnagios.a


MATHLIBS=-lm
GDLIBS=-lgd  -lpng -ljpeg -lz -lm


all cgis: $(CGIS)

$(CGILIBS): $(CGIHDRS)


######## REQUIRED LIBRARIES ##########

$(SRC_LIB)/libnagios.a:
	$(MAKE) -C $(SRC_LIB)

macros-cgi.o: $(SRC_COMMON)/macros.c $(SRC_INCLUDE)/macros.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_COMMON)/macros.c

objects-cgi.o: $(SRC_COMMON)/objects.c $(SRC_INCLUDE)/objects.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_COMMON)/objects.c

xobjects-cgi.o: $(SRC_XDATA)/xodtemplate.c $(SRC_XDATA)/xodtemplate.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_XDATA)/xodtemplate.c

statusdata-cgi.o: $(SRC_COMMON)/statusdata.c $(SRC_INCLUDE)/statusdata.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_COMMON)/statusdata.c

xstatusdata-cgi.o: $(SRC_XDATA)/xsddefault.c $(SRC_XDATA)/xsddefault.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_XDATA)/xsddefault.c

comments-cgi.o: $(SRC_COMMON)/comments.c $(SRC_INCLUDE)/comments.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_COMMON)/comments.c

downtime-cgi.o: $(SRC_COMMON)/downtime.c $(SRC_INCLUDE)/downtime.h
	$(CC) $(CFLAGS) -c -o $@ $(SRC_COMMON)/downtime.c

$(SRC_COMMON)/shared.o: $(SRC_COMMON)/shared.c
	$(CC) $(CFLAGS) -c -o $@ $<

########## CGIS ##########

avail.cgi: avail.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ avail.c $(CGILIBS) $(LIBS)

checksanity.cgi: checksanity.c $(CGIDEPS) $(CDATADEPS) $(DDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ checksanity.c $(CGILIBS) $(CDATALIBS) $(DDATALIBS) $(LIBS)

cmd.cgi: cmd.c $(CGIDEPS) $(CDATADEPS) $(DDATADEPS) extcmd_list.o
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ cmd.c extcmd_list.o $(CGILIBS) $(CDATALIBS) $(DDATALIBS) $(LIBS)

config.cgi: config.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ config.c  $(CGILIBS) $(LIBS)

extinfo.cgi: extinfo.c $(CGIDEPS) $(CDATADEPS) $(DDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ extinfo.c $(CGILIBS) $(CDATALIBS) $(DDATALIBS) $(LIBS)

history.cgi: history.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ history.c $(CGILIBS) $(LIBS)

ministatus.cgi: ministatus.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ ministatus.c $(CGILIBS) $(LIBS)

notifications.cgi: notifications.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ notifications.c $(CGILIBS) $(LIBS)

outages.cgi: outages.c $(CGIDEPS) $(CDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ outages.c $(CGILIBS) $(CDATALIBS) $(LIBS)

showlog.cgi: showlog.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ showlog.c $(CGILIBS) $(LIBS)

ec_host_status.cgi: ec_host_status.c $(CGIDEPS) $(CDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ ec_host_status.c $(CGILIBS) $(CDATALIBS) $(LIBS)

status.cgi: status.c $(CGIDEPS) $(CDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ status.c $(CGILIBS) $(CDATALIBS) $(LIBS)

statuswml.cgi: statuswml.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ statuswml.c $(CGILIBS) $(LIBS)

statusmap.cgi: statusmap.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ statusmap.c $(CGILIBS) $(GDLIBS) $(LIBS)

statuswrl.cgi: statuswrl.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ statuswrl.c $(CGILIBS) $(MATHLIBS) $(LIBS)

summary.cgi: summary.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ summary.c $(CGILIBS) $(LIBS)

tac.cgi: tac.c $(CGIDEPS) $(CDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ tac.c $(CGILIBS) $(CDATALIBS) $(LIBS)

tac-xml.cgi: tac-xml.c $(CGIDEPS) $(CDATADEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ tac-xml.c $(CGILIBS) $(CDATALIBS) $(LIBS)

trends.cgi: trends.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ trends.c $(CGILIBS) $(GDLIBS) $(LIBS)

histogram.cgi: histogram.c $(CGIDEPS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ histogram.c $(CGILIBS) $(GDLIBS) $(LIBS)

########## JSON CGIS ##########

jsonutils.o: jsonutils.c $(SRC_INCLUDE)/jsonutils.h
	$(CC) $(CFLAGS) $(JSONFLAGS) -c -o $@ jsonutils.c

archiveutils.o: archiveutils.c $(SRC_INCLUDE)/archiveutils.h
	$(CC) $(CFLAGS) $(JSONFLAGS) -c -o $@ archiveutils.c

archivejson.cgi: archivejson.c $(CGIDEPS) archiveutils.o jsonutils.o $(SRC_INCLUDE)/archivejson.h
	$(CC) $(CFLAGS) $(JSONFLAGS) $(LDFLAGS) -o $@ archivejson.c $(CGILIBS) archiveutils.o jsonutils.o $(LIBS)

objectjson.cgi: objectjson.c $(CGIDEPS) jsonutils.o $(SRC_INCLUDE)/objectjson.h
	$(CC) $(CFLAGS) $(JSONFLAGS) $(LDFLAGS) -o $@ objectjson.c $(CGILIBS) jsonutils.o $(LIBS)

statusjson.cgi: statusjson.c $(CGIDEPS) jsonutils.o $(SRC_INCLUDE)/statusjson.h
	$(CC) $(CFLAGS) $(JSONFLAGS) $(LDFLAGS) -o $@ statusjson.c $(CGILIBS) jsonutils.o $(LIBS)


clean:
	rm -f $(CGIS)
	rm -f *.o core gmon.out
	rm -f *~ *.*~

distclean: clean
	rm -f Makefile cgiutils.h

devclean: distclean

install:
	$(MAKE) install-basic

install-unstripped:
	$(INSTALL) -m 775 $(INSTALL_OPTS) -d $(DESTDIR)$(CGIDIR)
	for file in *.cgi; do \
		$(INSTALL) -m 775 $(INSTALL_OPTS) $$file $(DESTDIR)$(CGIDIR); \
	done

install-basic:
	$(INSTALL) -m 775 $(INSTALL_OPTS) -d $(DESTDIR)$(CGIDIR)
	for file in *.cgi; do \
		$(INSTALL) -s -m 775 $(INSTALL_OPTS) $$file $(DESTDIR)$(CGIDIR); \
	done

.PHONY: libnagios
