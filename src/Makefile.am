EXTRA_DIST = \
  wscript

ldflags = -module -avoid-version -export-symbols-regex init_cairo
if PLATFORM_WIN32
ldflags += -no-undefined
endif

pycairoexecdir = $(pyexecdir)/cairo

pkgincludedir = $(includedir)/pycairo
pkginclude_HEADERS = pycairo.h

pycairoexec_PYTHON = __init__.py

pycairoexec_LTLIBRARIES = _cairo.la
_cairo_la_LDFLAGS = $(ldflags)
_cairo_la_LIBADD = $(PYTHON_LIBS) $(CAIRO_LIBS)
_cairo_la_CPPFLAGS = $(PYTHON_INCLUDES) $(CAIRO_CFLAGS)
_cairo_la_SOURCES = \
  cairomodule.c \
  context.c \
  font.c \
  path.c \
  pattern.c \
  matrix.c \
  private.h \
  pycairo.h \
  surface.c
