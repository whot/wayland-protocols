AC_PREREQ([2.64])

m4_define([wayland_protocols_major_version], [0])
m4_define([wayland_protocols_minor_version], [1])
m4_define([wayland_protocols_micro_version], [0])
m4_define([wayland_protocols_version],
          [wayland_protocols_major_version.wayland_protocols_minor_version.wayland_protocols_micro_version])

AC_INIT([wayland-protocols],
        [wayland_protocols_version],
        [https://bugs.freedesktop.org/enter_bug.cgi?product=Wayland&component=protocols&version=wayland-protocols_version],
        [wayland-protocols],
        [http://wayland.freedesktop.org/])

AC_SUBST([WAYLAND_PROTOCOLS_VERSION_MAJOR], [wayland_protocols_major_version])
AC_SUBST([WAYLAND_PROTOCOLS_VERSION_MINOR], [wayland_protocols_minor_version])
AC_SUBST([WAYLAND_PROTOCOLS_VERSION_MICRO], [wayland_protocols_micro_version])
AC_SUBST([WAYLAND_PROTOCOLS_VERSION], [wayland_protocols_version])

AM_INIT_AUTOMAKE([1.11 foreign no-dist-gzip dist-xz])

AC_PATH_PROG([wayland_scanner], [wayland-scanner])
if test x$wayland_scanner = x; then
        PKG_CHECK_MODULES(WAYLAND_SCANNER, [wayland-scanner])
        wayland_scanner=`$PKG_CONFIG --variable=wayland_scanner wayland-scanner`
fi

AC_ARG_ENABLE([documentation],
	      [AC_HELP_STRING([--disable-documentation],
		              [Disable building the documentation])],
	      [],
	      [enable_documentation=yes])
AM_CONDITIONAL(BUILD_DOCS, [test x$enable_documentation = xyes])
if test "x$enable_documentation" = "xyes"; then
	AC_PATH_PROG(DOXYGEN, doxygen)

	if test "x$DOXYGEN" = "x"; then
		AC_MSG_ERROR([Documentation build requested but doxygen not found. Install doxygen or disable the documentation using --disable-documentation])
	fi

	AC_MSG_CHECKING([for compatible doxygen version])
	doxygen_version=`$DOXYGEN --version`
	AS_VERSION_COMPARE([$doxygen_version], [1.6.0],
	                   [AC_MSG_RESULT([no])
	                    AC_MSG_ERROR([Doxygen $doxygen_version too old. Doxygen 1.6+ required for documentation build. Install required doxygen version or disable the documentation using --disable-documentation])],
	                   [AC_MSG_RESULT([yes])],
	                   [AC_MSG_RESULT([yes])])

	AC_CONFIG_FILES([
	wayland.doxygen
	])
fi

AM_SILENT_RULES([yes])

PKG_PROG_PKG_CONFIG()

AC_CONFIG_FILES([
	Makefile
	wayland-protocols.pc
	])
AC_OUTPUT

AC_MSG_RESULT([
	Prefix			${prefix}
	])
