## Process this file with automake to produce Makefile.in
#====================================================================
#  NeXus - Neutron & X-ray Common Data Format
#  
#  $Id$
#  
#  Makefile for NeXus SWIG bindings
#
#  Automake version Copyright (C) 2004 Freddie Akeroyd
#  (based on original Makefile by Mark Koennecke, October 2002)
#  
#  This library is free software; you can redistribute it and/or
#  modify it under the terms of the GNU Lesser General Public
#  License as published by the Free Software Foundation; either
#  version 2 of the License, or (at your option) any later version.
# 
#  This library is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
#  Lesser General Public License for more details.
# 
#  You should have received a copy of the GNU Lesser General Public
#  License along with this library; if not, write to the Free 
#  Software Foundation, Inc., 59 Temple Place, Suite 330, Boston, 
#  MA  02111-1307  USA
#             
#  For further information, see <http://www.neutron.anl.gov/NeXus/>
#
#====================================================================
AM_CPPFLAGS=-I$(top_srcdir)/include
lib_LTLIBRARIES=$(LIBNXTCL) $(LIBNXPYTHON) $(LIBNXSCHEME) $(LIBNXGUILE)
bin_PROGRAMS=$(NXINTER) $(NXGUILE)

# python wrapper
if HAVE_SWIG
PYTHONWRAP=nxpython_wrap.c
nxpython_wrap.c: nxinter.i nxdataset.i
	$(SWIG) -o $@ -python nxinter.i 
endif
if HAVE_PYTHON
LIBNXPYTHON=libnxpython.la
libnxpython_la_SOURCES=nxpython_wrap.c $(COMMON_SRC)
libnxpython_la_CFLAGS=-I$(PYTHONROOT)/include/python$(PYTHON_VERSION) $(CFLAGS)
libnxpython_la_LDFLAGS=$(top_builddir)/src/libNeXus.la $(LDFLAGS)
endif

# tcl wrapper
if HAVE_SWIG
TCLWRAP=nxinter_wrap.c
nxinter_wrap.c: nxinter.i nxdataset.i
	$(SWIG) -o $@ -tcl8 -ltclsh.i nxinter.i 
endif
if HAVE_TCL
LIBNXTCL=libnxtcl.la
NXINTER=nxinter
TCLLIB=-L$(TCLROOT)/lib -ltcl
libnxtcl_la_SOURCES=nxinter_wrap.c $(COMMON_SRC)
libnxtcl_la_CFLAGS=-I$(TCLROOT)/include
libnxtcl_la_LDFLAGS=$(top_builddir)/src/libNeXus.la $(TCLLIB) $(LDFLAGS)

nxinter_SOURCES=libnxtcl.la
nxinter_LDFLAGS=-static libnxtcl.la $(top_builddir)/src/libNeXus.la $(TCLLIB) $(LDFLAGS)
endif

# guile wrapper
if HAVE_SWIG
GUILEWRAP=nxguile_wrap.c
nxguile_wrap.c: nxinter.i nxdataset.i
	$(SWIG) -o $@ -guile -lguilemain.i $(SWGUILEFLAGS) nxinter.i
endif
if HAVE_GUILE
LIBNXGUILE=libnxguile.la
NXGUILE=nxguile
GGLIB=-L$(GUILEROOT)/lib -lguile
libnxguile_la_SOURCES=nxguile_wrap.c $(COMMON_SRC)
libnxguile_la_CFLAGS=-I$(GUILEROOT)/include/guile \
  -DSWIGINIT="SCM scm_init_nxinter_module(void); scm_init_nxinter_module();"\
  $(CFLAGS)
libnxguile_la_LDFLAGS=$(top_builddir)/src/libNeXus.la $(GGLIB) $(LDFLAGS)

nxguile_SOURCES=libnxguile.la
nxguile_LDFLAGS=-static libnxguile.la $(top_builddir)/src/libNeXus.la $(GGLIB) $(LDFLAGS)
endif

# mzscheme wrapper
if HAVE_SWIG
SCHEMEWRAP=nxscheme_wrap.c
nxscheme_wrap.c: nxinter.i nxdataset.i
	$(SWIG) -o $@ -mzscheme nxinter.i 
endif
if HAVE_MZSCHEME
LIBNXSCHEME=libnxscheme.la
MZINC=/data/koenneck/opt/plt
MZLIB=-L$(MZINC)/lib -lmzscheme -lmzgc $(MZINC)/lib/mzdyn.o
libnxscheme_la_SOURCES=nxscheme_wrap.c $(COMMON_SRC)
libnxscheme_la_CFLAGS=-I$(MZINC)/include $(CFLAGS)
libnxscheme_la_LDFLAGS=$(top_builddir)/src/libNeXus.la $(MZLIB) $(LDFLAGS)
endif

DOC_SRC		= nxinter.tex
swigdocdir 	= $(NXDOCDIR)
swigdoc_DATA	= $(DOC_OUTPUT)
if HAVE_LATEX
DOC_OUTPUT=nxinter.ps nxinter.pdf
nxinter.ps: $(DOC_SRC)
	latex nxinter.tex
	latex nxinter.tex
	dvips -f nxinter.dvi > nxinter.ps
	ps2pdf12 nxinter.ps nxinter.pdf
endif

include $(top_srcdir)/build_rules.am

# if we have XML support, we already have nxdataset
if HAVE_XML
else
NXDATASET=nxdataset.c
endif

COMMON_SRC=nxinterhelper.c $(NXDATASET)

EXTRA_DIST=nxinter.i nxdataset.i nxdataset.h nxinterhelper.h \
		$(TCLWRAP) $(GUILEWRAP) $(PYTHONWRAP) $(SCHEMEWRAP) \
		$(DOC_OUTPUT) $(DOC_SRC)
