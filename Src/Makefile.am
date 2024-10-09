datadir = @datadir@
localedir = $(datadir)/locale
DEFS = -DLOCALEDIR=\"$(localedir)\" @DEFS@

bin_PROGRAMS = dasher

SUBDIRS = Common DasherCore Gtk2 rust
dasher_SOURCES = main.cc

AM_CXXFLAGS = \
	-I$(srcdir)/Gtk2 \
	-I$(srcdir)/DasherCore \
	-I$(srcdir)/rust/rust_test_lib/target/include \
	$(GTKBUILD_CFLAGS) \
	$(SPEECH_CPPFLAGS) \
	-DPREFIX=\"$(PREFIX)\" \
	-DLIBDIR=\"$(libdir)\" \
	-DDATADIR=\"$(datadir)\" \
	-DPROGDATA=\"$(pkgdatadir)\" \
	-DSYSCONFDIR=\"$(sysconfdir)\" \
	-DPACKAGE_LOCALE_DIR=\"$(datadir)/locale\" \
	-L$(srcdir)/rust/rust_test_lib/target/debug

dasher_LDADD = \
	Gtk2/libdashergtk.la \
	Gtk2/libdashercontrol.la \
	DasherCore/libdashercore.la \
	DasherCore/libdasherprefs.la \
	DasherCore/LanguageModelling/libdasherlm.la \
	$(srcdir)/rust/rust_test_lib/target/debug/librust_test_lib.a \
	$(GTKBUILD_LIBS) \
	$(SPEECH_LIBS) \
	-lexpat
