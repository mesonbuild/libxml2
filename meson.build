project('libxml2', 'c', version : '2.9.2', license : 'mit')

cdata = configuration_data()

cc = meson.get_compiler('c')

checked_headers = [
  ['ansidecl.h', 'HAVE_ANSIDECL_H'],
  ['ctype.h', 'HAVE_CTYPE_H'],
  ['dirent.h', 'HAVE_DIRENT_H'],
  ['dlfcn.h', 'HAVE_DLFCN_H'],
  ['dl.h', 'HAVE_DL_H'],
  ['errno.h', 'HAVE_ERRNO_H'],
  ['fcntl.h', 'HAVE_FCNTL_H'],
  ['float.h', 'HAVE_FLOAT_H'],
  ['ieeefp.h', 'HAVE_IEEEFP_H'],
  ['malloc.h', 'HAVE_MALLOC_H'],
  ['math.h', 'HAVE_MATH_H'],
  ['memory.h', 'HAVE_MEMORY_H'],
  ['nan.h', 'HAVE_NAN_H'],
  ['netdb.h', 'HAVE_NETDB_H'],
  ['netinet/in.h', 'HAVE_NETINET_IN_H'],
  ['poll.h', 'HAVE_POLL_H'],
  ['pthread.h', 'HAVE_PTHREAD_H'],
  ['resolv.h', 'HAVE_RESOLV_H'],
  ['signal.h', 'HAVE_SIGNAL_H'],
  ['stdarg.h', 'HAVE_STDARG_H'],
  ['stdint.h', 'HAVE_STDINT_H'],
  ['stdlib.h', 'HAVE_STDLIB_H'],
  ['strings.h', 'HAVE_STRINGS_H'],
  ['sys/dir.h', 'HAVE_SYS_DIR_H'],
  ['sys/mman.h', 'HAVE_SYS_MMAN_H'],
  ['sys/ndir.h', 'HAVE_SYS_NDIR_H'],
  ['sys/select.h', 'HAVE_SYS_SELECT_H'],
  ['sys/socket.h', 'HAVE_SYS_SOCKET_H'],
  ['sys/stat.h', 'HAVE_SYS_STAT_H'],
  ['sys/timeb.h', 'HAVE_SYS_TIMEB_H'],
  ['sys/time.h', 'HAVE_SYS_TIME_H'],
  ['sys/types.h', 'HAVE_SYS_TYPES_H'],
  ['time.h', 'HAVE_TIME_H'],
  ['unistd.h', 'HAVE_UNISTD_H'],
]

foreach h : checked_headers
  if cc.has_header(h.get(0))
    cdata.set(h.get(1), 1)
  endif
endforeach

cdata.set('ICONV_CONST', '')
cdata.set('SEND_ARG2_CAST', '')
cdata.set('GETHOSTBYNAME_ARG_CAST', '')

dl_lib = cc.find_library('dl', required: false)
if (dl_lib.found() and cc.has_function('dlopen', args: '-ldl'))
  cdata.set('HAVE_DLOPEN', 1)
endif

math_lib = cc.find_library('m', required: false)

iconv_lib = cc.find_library('iconv', required : false)
deps = [math_lib, iconv_lib, dl_lib]

if cc.compiles('''#include<stdarg.h>
va_list ap1, ap2;
void func() {
  va_copy(ap1, ap2);
}
''')
  cdata.set('HAVE_VA_COPY', 1)
elif cc.compiles('''#include<stdarg.h>
va_list ap1, ap2;
void func() {
  __va_copy(ap1, ap2);
}
''')
  cdata.set('HAVE___VA_COPY', 1)
endif

configure_file(input : 'config.h.meson',
  output : 'config.h',
  configuration : cdata)

sources = [
  'SAX.c',
  'entities.c',
  'encoding.c',
  'error.c',
  'parserInternals.c',
  'parser.c',
  'tree.c',
  'hash.c',
  'list.c',
  'xmlIO.c',
  'xmlmemory.c',
  'uri.c',
  'valid.c',
  'xlink.c',
  'HTMLparser.c',
  'HTMLtree.c',
  'debugXML.c',
  'xpath.c',
  'xpointer.c',
  'xinclude.c',
  'nanohttp.c',
  'nanoftp.c',
  'catalog.c',
  'globals.c',
  'threads.c',
  'c14n.c',
  'xmlstring.c',
  'buf.c',
  'xmlregexp.c',
  'xmlschemas.c',
  'xmlschemastypes.c',
  'xmlunicode.c',
  'xmlreader.c',
  'relaxng.c',
  'dict.c',
  'SAX2.c',
  'xmlwriter.c',
  'legacy.c',
  'chvalid.c',
  'pattern.c',
  'xmlsave.c',
  'xmlmodule.c',
  'schematron.c',
  'xzlib.c'
]

incdir = include_directories('include')

c_args = ['-D_REENTRANT']

xml2lib = library('xml2', sources,
		  c_args : c_args,
		  include_directories : incdir,
		  dependencies: deps)
xml2lib_dep = declare_dependency(link_with: xml2lib,
				 include_directories: incdir,
				 dependencies: deps)

dictexe = executable('testdict', 'testdict.c',
		     dependencies : xml2lib_dep)

test('dict', dictexe)
