ACLOCAL_AMFLAGS = -I m4

bin_PROGRAMS = osm2pgsql nodecachefilereader

osm2pgsql_SOURCES = build_geometry.cpp input.c middle.h middle-ram.h output-gazetteer.h output-pgsql.c rb.c sanitizer.h text-tree.h build_geometry.h input.h middle-pgsql.c osm2pgsql.c output.h output-pgsql.h rb.h sprompt.c UTF8sanitizer.c expire-tiles.c keyvals.c middle-pgsql.h osmtypes.h output-null.c parse-o5m.c parse-o5m.h parse-primitive.c parse-primitive.h parse-xml2.c parse-xml2.h pgsql.c reprojection.c sprompt.h expire-tiles.h keyvals.h middle-ram.c output-gazetteer.c output-null.h pgsql.h reprojection.h text-tree.c node-ram-cache.c wildcmp.c node-ram-cache.h node-persistent-cache.c node-persistent-cache.h binarysearcharray.c binarysearcharray.h tagtransform.c 

nodecachefilereader_SOURCES = node-persistent-cache-reader.c node-persistent-cache.c node-ram-cache.c binarysearcharray.c


if READER_PBF
osm2pgsql_SOURCES += parse-pbf.c parse-pbf.h fileformat.pb-c.c fileformat.pb-c.h osmformat.pb-c.c osmformat.pb-c.h

fileformat.pb-c.c: protobuf/fileformat.proto
	 $(AM_V_GEN) $(PROTOC_C) --proto_path=protobuf --c_out=. $<

fileformat.pb-c.h: fileformat.pb-c.c
	@if test ! -f $@; then \
	  rm -f $<; \
	  $(MAKE) $(AM_MAKEFLAGS) $<; \
	else :; fi

osmformat.pb-c.c: protobuf/osmformat.proto
	 $(AM_V_GEN) $(PROTOC_C) --proto_path=protobuf --c_out=. $<

osmformat.pb-c.h: osmformat.pb-c.c
	@if test ! -f $@; then \
	  rm -f $<; \
	  $(MAKE) $(AM_MAKEFLAGS) $<; \
	else :; fi

BUILT_SOURCES = \
  fileformat.pb-c.c fileformat.pb-c.h \
  osmformat.pb-c.c osmformat.pb-c.h

CLEANFILES = \
  fileformat.pb-c.c fileformat.pb-c.h \
  osmformat.pb-c.c osmformat.pb-c.h

endif

osm2pgsqldir = $(datadir)/osm2pgsql

AM_CFLAGS = @PTHREAD_CFLAGS@ @LFS_CFLAGS@ @POSTGRESQL_CFLAGS@ @XML2_CFLAGS@ @BZIP2_CFLAGS@ @GEOS_CFLAGS@ @PROJ_CFLAGS@ @PROTOBUF_C_CFLAGS@ @ZLIB_CFLAGS@ -DOSM2PGSQL_DATADIR='"$(osm2pgsqldir)"' -DVERSION='"@PACKAGE_VERSION@"' @LUA_INCLUDE@
AM_CPPFLAGS = @PTHREAD_CFLAGS@ @POSTGRESQL_CFLAGS@ @XML2_CFLAGS@ @BZIP2_CFLAGS@ @GEOS_CFLAGS@ @PROJ_CFLAGS@ -DOSM2PGSQL_DATADIR='"$(osm2pgsqldir)"' -Igeos-fallback @LUA_INCLUDE@

AM_LDFLAGS = @PTHREAD_CFLAGS@ @ZLIB_LDFLAGS@ @ZLIB_LIBS@ @POSTGRESQL_LDFLAGS@ @POSTGRESQL_LIBS@ @XML2_LDFLAGS@ @BZIP2_LDFLAGS@ @BZIP2_LIBS@ @GEOS_LDFLAGS@ @GEOS_LIBS@ @PROJ_LDFLAGS@ @PROJ_LIBS@ @PROTOBUF_C_LDFLAGS@ @PROTOBUF_C_LIBS@ -L/usr/lib/x86_64-linux-gnu @LUA_LIB@

osm2pgsql_DATA = default.style 900913.sql

man1_MANS = docs/osm2pgsql.1 docs/nodecachefilereader.1

EXTRA_DIST = osm2pgsql.spec.in \
             osm2pgsql.spec \
             protobuf/fileformat.proto \
             protobuf/osmformat.proto \
             debian \
             $(osm2pgsql_DATA)

$(PACKAGE).spec: $(PACKAGE).spec.in
	sed -e "s/@""PACKAGE""@/$(PACKAGE)/g; s/@""VERSION""@/$(VERSION)/g; s/@""SVN""@/`svnversion`/g;" $^ > $@

rpm: dist-gzip
	rpmbuild -ta $(distdir).tar.gz

distclean-local:
	@rm -f $(PACKAGE).spec
	@rm -f config.nice

test:
	tests/regression-test.py -f tests/liechtenstein-2013-08-03.osm.pbf
	tests/regression-test.py -f tests/liechtenstein-2013-08-03.osm.bz2
