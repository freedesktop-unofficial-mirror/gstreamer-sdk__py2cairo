-*- mode: m4 -*-
AC_PREREQ(2.59)

# the pycairo version number
m4_define(pycairo_version_major, 1)
m4_define(pycairo_version_minor, 10)
m4_define(pycairo_version_micro, 0)
m4_define(pycairo_version, pycairo_version_major.pycairo_version_minor.pycairo_version_micro)

# versions of packages we require
m4_define(cairo_required_version, 1.10.0)
m4_define(xpyb_required_version, 1.3)

AC_INIT([py2cairo],
	[pycairo_version],
	[http://bugs.freedesktop.org/enter_bug.cgi?product=pycairo])

AC_DEFINE(PYCAIRO_VERSION_MAJOR, pycairo_version_major, [pycairo major version])
AC_DEFINE(PYCAIRO_VERSION_MINOR, pycairo_version_minor, [pycairo minor version])
AC_DEFINE(PYCAIRO_VERSION_MICRO, pycairo_version_micro, [pycairo macro version])

AC_CONFIG_SRCDIR([src/pycairo.h])
AC_CONFIG_HEADERS(src/config.h)

AM_INIT_AUTOMAKE([1.9.6 -Wall])

# Options ---

# put the ACLOCAL flags in the makefile
ACLOCAL="$ACLOCAL $ACLOCAL_FLAGS"

# Checks for programs ---
AC_DISABLE_STATIC
AC_PROG_LIBTOOL dnl required version (1.4) DON'T REMOVE - used by autogen.sh
dnl when using libtool 2.x create libtool early, because it's used in configure
m4_ifdef([LT_OUTPUT], [LT_OUTPUT])

AM_PATH_PYTHON(2.6)

# Checks for libraries ---

# get rid of the -export-dynamic stuff from the configure flags ...
export_dynamic=`(./libtool --config; echo eval echo \\$export_dynamic_flag_spec) | sh`

# cairo
PKG_CHECK_MODULES(CAIRO, cairo >= cairo_required_version)
if test -n "$export_dynamic"; then
  CAIRO_LIBS=`echo $CAIRO_LIBS | sed -e "s/$export_dynamic//"`
fi

# xpyb
PKG_CHECK_MODULES(XPYB, xpyb >= xpyb_required_version,[have_xpyb=yes],[have_xpyb=no])
AS_IF([test "x$have_xpyb" = "xyes"],
      [AC_DEFINE([HAVE_XPYB], [1], [Define to 1 if xpyb is available])])

AC_MSG_CHECKING([for some Win32 platform])
case "$host" in
  *-*-mingw*|*-*-cygwin*)
    platform_win32=yes
    ;;
  *)
    platform_win32=no
    ;;
esac
AC_MSG_RESULT([$platform_win32])

AM_CONDITIONAL(PLATFORM_WIN32, test "$platform_win32" = "yes")
# Checks for header files ---
if test "x$cross_compiling" = "xyes" -a "x$platform_win32" = "xyes" ; then
  # skip python cheks if cross-compiling to windows
  # $PYTHON_INCLUDES and PYTHON_LIBS should be set properly
  save_CPPFLAGS="$CPPFLAGS"
  CPPFLAGS="$CPPFLAGS $PYTHON_INCLUDES"
  cat confdefs.h - <<_ACEOF >conftest.$ac_ext
  /* end confdefs.h.  */
  #include <Python.h>
_ACEOF
  if ac_fn_c_try_cpp "$LINENO"; then :
    { $as_echo "$as_me:${as_lineno-$LINENO}: result: found" >&5
      $as_echo "found" >&6; }

  else
    { $as_echo "$as_me:${as_lineno-$LINENO}: result: not found" >&5
      $as_echo "not found" >&6; }
    as_fn_error $? "could not find Python headers" "$LINENO" 5
   fi
   rm -f conftest.err conftest.i conftest.$ac_ext
   CPPFLAGS="$save_CPPFLAGS"
   AC_SUBST(PYTHON_LIBS)
else
  AM_CHECK_PYTHON_HEADERS(,[AC_MSG_ERROR(could not find Python headers)])
fi

# checks for types and compilers ---
AC_C_CONST

# add gcc debugging options
changequote(,)dnl
if test "x$GCC" = xyes; then
  case " $CFLAGS " in
  *[\	\ ]-std=c99[\	\ ]*) ;;
  *[\	\ ]-std=c9x[\	\ ]*) ;;
  *) CFLAGS="$CFLAGS -std=c99" ;;
  esac

  case " $CFLAGS " in
  *[\	\ ]-Wall[\	\ ]*) ;;
  *) CFLAGS="$CFLAGS -Wall" ;;
  esac
fi
changequote([,])dnl

case $GCC in
yes)
  # code using Py_True, Py_False will receive
  # "warning: dereferencing type-punned pointer will break strict-aliasing rules"
  # -fno-strict-aliasing (as used in Python build) switches warnings off
    AC_MSG_CHECKING(whether $CC accepts -fno-strict-aliasing)
     ac_save_cc="$CC"
     CC="$CC -fno-strict-aliasing"
     AC_TRY_RUN([int main() { return 0; }],
     ac_cv_no_strict_aliasing_ok=yes,
     ac_cv_no_strict_aliasing_ok=no,
     ac_cv_no_strict_aliasing_ok=no)
     CC="$ac_save_cc"
    AC_MSG_RESULT($ac_cv_no_strict_aliasing_ok)
    if test $ac_cv_no_strict_aliasing_ok = yes
    then
      CFLAGS="$CFLAGS -fno-strict-aliasing"
    fi
esac

# Checks for library functions ---
# Checks for system services ---

# Output ---
AC_CONFIG_FILES([
Makefile
pycairo.pc
pycairo-uninstalled.pc
src/Makefile
doc/Makefile
examples/Makefile
test/Makefile
])

AC_OUTPUT

echo "
Configuration:
Installation prefix             ${prefix}
"
