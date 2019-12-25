AC_PREREQ(2.64)
AC_INIT([futurerestore], [1.0], [https://github.com/s0uthwest/futurerestore/issues])

AC_CANONICAL_SYSTEM
AC_CANONICAL_HOST

# Check for operating system
AC_MSG_CHECKING([whether we need platform-specific build settings])
case $host_os in
  darwin* )
        CXXFLAGS+=" -stdlib=libc++"
        ;;
esac

AM_INIT_AUTOMAKE

CFLAGS="-DIDEVICERESTORE_NOMAIN=1 -DIMG4TOOL_NOMAIN=1 -DTSSCHECKER_NOMAIN=1"

AC_DEFUN([AX_UNPRECIOUS], [
    m4_define([_AC_PRECIOUS_VARS], m4_bpatsubst(_AC_PRECIOUS_VARS, [$1
], []))
])
AX_UNPRECIOUS([CFLAGS])
export CFLAGS

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIRS([m4])
AC_CONFIG_SUBDIRS(external/idevicerestore external/img4tool external/tsschecker)
AC_CONFIG_SRCDIR([futurerestore])

# Check for libraries
AC_SEARCH_LIBS([pthread_create], [pthread])
PKG_CHECK_MODULES(libplist, libplist >= 2.0.0)
PKG_CHECK_MODULES(libzip, libzip >= 0.10)
PKG_CHECK_MODULES(libimobiledevice, libimobiledevice-1.0 >= 1.2.1)
PKG_CHECK_MODULES(libfragmentzip, libfragmentzip >= 1.0)
PKG_CHECK_MODULES(libirecovery, libirecovery >= 0.2.0)

AC_PROG_CC
AC_PROG_CXX

# Optional module libipatcher
AC_ARG_WITH([libipatcher],
            [AS_HELP_STRING([--without-libipatcher],
            [build with bundle-less odysseus support (default is yes)])],
            [build_libipatcher=false],
            [build_libipatcher=true])

PKG_PROG_PKG_CONFIG
LIBIPATCHER_FLAGS=$($PKG_CONFIG --cflags libipatcher)
if [test -z "$LIBIPATCHER_FLAGS"]; then
    do_libipatcher=no
else
    AC_MSG_CHECKING([for futurerestore libipatcher])
    if test "$build_libipatcher" = "true"; then
            PKG_CHECK_MODULES(libipatcher, libipatcher >= 47)
            do_libipatcher=yes    
    else
            do_libipatcher=no
    fi
fi
AM_CONDITIONAL([HAVE_LIBIPATCHER],[test "x$do_libipatcher" = "xyes"])

AC_MSG_CHECKING([whether to enable WIN32 specific flags])
case "$host_os" in
*mingw*)
        win32=true
        AC_MSG_RESULT([yes])
	AC_CHECK_TOOL(WINDRES, windres)
  ;;
*)
        win32=false
        AC_MSG_RESULT([no])
  ;;
esac
AM_CONDITIONAL(WIN32, test x$win32 = xtrue)

LT_INIT

AC_OUTPUT([
Makefile
futurerestore/Makefile
])

echo "
Configuration for $PACKAGE $VERSION:
-------------------------------------------

  Install prefix ..........: $prefix
  With libipatcher ........: $do_libipatcher
  Now type 'make' to build $PACKAGE $VERSION,
  and then 'make install' for installation.
"
