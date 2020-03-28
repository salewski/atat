dnl Process this file with autoconf to produce a configure script. -*-Autoconf-*-
dnl SPDX-License-Identifier: GPL-2.0-or-later

dnl +-------------------------+
dnl |  Autotools Boilerplate  |
dnl +-------------------------+

AC_INIT([atat], [0.0.1-snapshot], [salewski@att.net])

dnl Ensure that a recent-enough version of Autoconf is being used
AC_PREREQ([2.69])

AC_CONFIG_AUX_DIR([autotools])

dnl Indicate dir where our custom m4 macros may be found (future
dnl proofing; as of may not be used yet...)
AC_CONFIG_MACRO_DIR([autotools])

dnl Turn on all Automake warnings and report them as errors
AM_INIT_AUTOMAKE([-Wall -Werror foreign])


DFLT_MAINTAINER="Alan D. Salewski <salewski@att.net>"
AC_SUBST([DFLT_MAINTAINER])


dnl +------------------------+
dnl |  Checks for programs.  |
dnl +------------------------+

dnl makes $(LN_S) available in our Makefile.am files
AC_PROG_LN_S

# The POSIX/SUS at(1) command:
#     https://pubs.opengroup.org/onlinepubs/9699919799/utilities/at.html
#
# At the time of writing (2020-03), the 'at' program used on Debian systems
# (and presumably on derived systems) is that which was originally written and
# packaged by Thomas Koenig, and currently maintained by Jose M. Calhariz. It
# can be found here:
#
#     http://software.calhariz.com/at/
#     http://blog.calhariz.com/index.php/tag/at
#
# The 'AUTHORS' section of the at(1) man page on OpenBSD, FreeBSD, NetBSD, and
# Dragonfly BSD all indicate a common lineage with Koenig's implementation.
#
AC_PATH_PROG([AT], [at])
if test -z "$AT"; then
    AC_MSG_ERROR([at not found])
fi

AC_PATH_PROG([CAT], [cat])
if test -z "$CAT"; then
    AC_MSG_ERROR([cat not found])
fi

AC_PATH_PROG([XARGS], [xargs])
if test -z "$XARGS"; then
    AC_MSG_ERROR([xargs not found])
fi


# pod2man comes with Perl, but it's not reliable to just check for the
# presence of the 'perl' program (distribution packaging may provide it in an
# optional package, for instance), so we check for it explicitly here. We use
# pod2man at build time to create some of our man pages.
AC_PATH_PROG([POD2MAN], [pod2man])
if test -z "$POD2MAN"; then
    AC_MSG_ERROR([pod2man program not found])
fi

AC_PATH_PROG([POD2HTML], [pod2html])
if test -z "$POD2HTML"; then
    AC_MSG_ERROR([pod2html program not found])
fi


dnl +------------------------+
dnl |  Checks for Libraries  |
dnl +------------------------+
dnl (None)


dnl +----------------------+
dnl |  Checks for Headers  |
dnl +----------------------+
dnl (None)


dnl +------------------------------------------------------------------+
dnl |  Checks for typedefs, structures, and compiler characteristics.  |
dnl +------------------------------------------------------------------+
dnl (None)


dnl +---------------------------------+
dnl |  Checks for library functions.  |
dnl +---------------------------------+
dnl (None)


dnl +---------------------+
dnl |  Autotools Output.  |
dnl +---------------------+


# Note that htmldir defaults to ${docdir}, which already includes the package
# name:
#
#     <DATAROOTDIR>/doc/<PACKAGE>
#
# Our pkghtmldir is oddly named in that is not introducing the package name as
# a new component; it just creates an 'html' subdirectory.
#
AC_SUBST([pkghtmldir],  ["\${htmldir}/html"])


AC_SUBST([manhtmldir],  ["\${pkghtmldir}/man"])
AC_SUBST([man1htmldir], ["\${pkghtmldir}/man/man1"])dnl executable programs
dnl AC_SUBST([man2htmldir], ["\${pkghtmldir}/man/man2"])dnl system calls
dnl AC_SUBST([man3htmldir], ["\${pkghtmldir}/man/man3"])dnl library calls
dnl AC_SUBST([man4htmldir], ["\${pkghtmldir}/man/man4"])dnl special files (usually found in /dev)
AC_SUBST([man5htmldir], ["\${pkghtmldir}/man/man5"])dnl file formats and conventions
dnl AC_SUBST([man6htmldir], ["\${pkghtmldir}/man/man6"])dnl games
AC_SUBST([man7htmldir], ["\${pkghtmldir}/man/man7"])dnl misc (packages, conventions, intro pages, ...)
dnl AC_SUBST([man8htmldir], ["\${pkghtmldir}/man/man8"])dnl sysadmin commands (e.g., for root only)
dnl AC_SUBST([man9htmldir], ["\${pkghtmldir}/man/man9"])dnl kernel routines (non-standard)




## The 'do_subst' Makefile var is the conventional name for manual
## filtering process that needs to be done at build time.
##
## Basically, any file we generate that is not a Makefile should be
## filtered at build time rather than at configure time; at build
## time, autotools variables will be fully expanded (well, expanded
## recursively, which amounts to the same thing) and allows any of the
## make vars to be overridden on the 'make' command line. This also
## avoids other kludgey hacks, such as eval'ing shell variables to
## expand them at configure time).
##
## Note that we replace all of the autoconf installation directory
## vars (see "Installation Directory Variables" in the autoconf
## manual) and most of the autoconf preset output variables (see
## "Preset Output Variables" in the autoconf manual). The exceptions
## to that latter are flag vars to the C/C++/Fortran compilers,
## linkers, etc. (CFLAGS and friends), and the 'configure_input' var,
## which we have to provide our own variation of. We also omit
## 'builddir' (which is always '.'); in this last case, what you
## probably want instead is 'abs_builddir', which we provide.
##
## The SHELL variable is somewhat special in that it is set based on
## CONFIG_SHELL (see the documentation for AS_INIT in the autoconf manual). We
## use it directly in shell scripts that should be invoked by something that
## looks like a capable POSIX-ish shell (that is not necessarily bash, ksh,
## etc.).
[
do_subst_command='do_subst = sed \
  -e '"'"'s,[@]PACKAGE[@],$(PACKAGE),g'"'"' \
  -e '"'"'s,[@]VERSION[@],$(VERSION),g'"'"' \
  -e '"'"'s,[@]PATH_SEPARATOR[@],$(PATH_SEPARATOR),g'"'"' \
  -e '"'"'s,[@]configure_input[@],Generated from $@.in; do not edit by hand.,g'"'"' \
  -e "s,[@]BUILD_DATE[@],$$(date '"'"'+%Y-%m-%d %H:%M:%S'"'"'),g" \
  \
  -e '"'"'s,[@]SHELL[@],$(SHELL),g'"'"' \
  \
  -e '"'"'s,[@]bindir[@],$(bindir),g'"'"' \
  -e '"'"'s,[@]datadir[@],$(datadir),g'"'"' \
  -e '"'"'s,[@]exec_prefix[@],$(exec_prefix),g'"'"' \
  -e '"'"'s,[@]includedir[@],$(includedir),g'"'"' \
  -e '"'"'s,[@]infodir[@],$(infodir),g'"'"' \
  -e '"'"'s,[@]libdir[@],$(libdir),g'"'"' \
  -e '"'"'s,[@]libexecdir[@],$(libexecdir),g'"'"' \
  -e '"'"'s,[@]localstatedir[@],$(localstatedir),g'"'"' \
  -e '"'"'s,[@]mandir[@],$(mandir),g'"'"' \
  -e '"'"'s,[@]oldincludedir[@],$(oldincludedir),g'"'"' \
  -e '"'"'s,[@]prefix[@],$(prefix),g'"'"' \
  -e '"'"'s,[@]sbindir[@],$(sbindir),g'"'"' \
  -e '"'"'s,[@]sharedstatedir[@],$(sharedstatedir),g'"'"' \
  -e '"'"'s,[@]sysconfdir[@],$(sysconfdir),g'"'"' \
  \
  -e '"'"'s,[@]abs_builddir[@],$(abs_builddir),g'"'"' \
  -e '"'"'s,[@]abs_srcdir[@],$(abs_srcdir),g'"'"' \
  -e '"'"'s,[@]abs_top_builddir[@],$(abs_top_builddir),g'"'"' \
  -e '"'"'s,[@]srcdir[@],$(srcdir),g'"'"' \
  -e '"'"'s,[@]top_builddir[@],$(top_builddir),g'"'"' \
  -e '"'"'s,[@]top_srcdir[@],$(top_srcdir),g'"'"' \
  \
  -e '"'"'s,[@]pkghtmldir[@],$(pkghtmldir),g'"'"'   \
  \
  -e '"'"'s,[@]manhtmldir[@],$(manhtmldir),g'"'"'   \
  -e '"'"'s,[@]man1htmldir[@],$(man1htmldir),g'"'"' \
  -e '"'"'s,[@]man5htmldir[@],$(man5htmldir),g'"'"' \
  -e '"'"'s,[@]man7htmldir[@],$(man7htmldir),g'"'"' \
  \
  -e '"'"'s,[@]DFLT_MAINTAINER[@],$(DFLT_MAINTAINER),g'"'"' \
  \
  -e '"'"'s,[@]POD2MAN[@],$(POD2MAN),g'"'"' \
  -e '"'"'s,[@]POD2HTML[@],$(POD2HTML),g'"'"' \
  \
  -e '"'"'s,[@]AT[@],$(AT),g'"'"'   \
  -e '"'"'s,[@]CAT[@],$(CAT),g'"'"' \
  -e '"'"'s,[@]XARGS[@],$(XARGS),g'"'"' \
'
]
AC_SUBST([do_subst_command])


# It's common for makefile scripts to need to "sed sanitize" the value of a
# variable in order to use it on the left side of a sed 's' (substitution)
# command. To obtain a "sed sanitized" copy of any variable, echo its value to:
#     $(sed_slsanitize)
#
# Example:
#     vers_sedslsanitized=$$(printf "%s\n" "-$(VERSION)" | $(sed_slsanitize)) ;\
#     nonvers=$$(echo progwithvers | sed -e "s/$${vers_sedslsanitized}$$//")
[
sed_slsanitize_command='sed_slsanitize = sed \
  -e '"'"'s!\([]^\*\$\/&[]\)!\\\1!g'"'"' \
  -e '"'"'s![-]![-]!g'"'"' \
'
]
AC_SUBST([sed_slsanitize_command])


# Similar to 'sed_slsanitize' above, but for the replacement text on the right
# side of a sed 's' (substitution) command. On the right side of a sed 's'
# substitution we only need to escape backslashes and amperdoodle chars.
[
sed_srsanitize_command='sed_srsanitize = sed \
  -e '"'"'s!\([\/&]\)!\\\1!g'"'"' \
'
]
AC_SUBST([sed_srsanitize_command])


dnl Declare our output Makefiles
AC_CONFIG_FILES(
        [Makefile]
        [src/Makefile]
        [src/main/Makefile]
        [src/main/sh/Makefile]
        [src/main/sh/bin/Makefile]
)

dnl Actually output the declared files
AC_OUTPUT


# XXX: Shell functions are not considered portable to old ancient *nices...
#
# Prints on stdout the fully expanded representation of the value of the shell
# variable named by the SHELL_VAR_NAME param. The variable will be expanded
# repeatedly until it cannot be expanded any further; that final value is what
# is printed.
#
# Does /not/ assign the expanded value to the named shell var.
#
# __expand_fully( SHELL_VAR_NAME )
#
# @param SHELL_VAR_NAME should be set to the /name/ of an existing shell
#                       variable whose value contains a value that may or may
#                       not need expansion of the configure shell variables
#                       ($prefix, and the like).
#
__expand_fully () {
    _tmp_name=$1
    _tmp_last="`eval echo '${'"${_tmp_name}"'}'`"
    if test -z "${_tmp_last}"; then
        printf "__expand_fully() (WARNING): var \"%s\" is empty\n" \
          "${_tmp_name}" 1>&2
        return 0;  # nothing to do
    fi
    _tmp_last_expanded="`eval echo ${_tmp_last}`"
    while test "${_tmp_last_expanded}" != "${_tmp_last}"; do

        # Set 'last' hold var to most recently expanded version...
        _tmp_last="${_tmp_last_expanded}"

        # ...and try to expand further.
        _tmp_last_expanded="`eval echo ${_tmp_last_expanded}`"
    done
    printf "%s\n" "${_tmp_last_expanded}"
    unset _tmp_last_expanded
    unset _tmp_last
    unset _tmp_name
    return 0;
}


dnl show user a summary of the configured options
printf "\n"
printf "${PACKAGE} has been configured with the following options:\n"
printf "                        Prefix: %s\n" "`__expand_fully prefix`"
printf "                 User binaries: %s\n" "`__expand_fully bindir`"
printf "               System binaries: %s\n" "`__expand_fully sbindir`"
printf "           Configuration files: %s\n" "`__expand_fully sysconfdir`"
printf "                  Manual pages: %s\n" "`__expand_fully mandir`"
printf "\n"
printf "                  Manual pages: %s\n" "`__expand_fully manhtmldir`"
printf "                  Manual pages: %s\n" "`__expand_fully man1htmldir`"
printf "                  Manual pages: %s\n" "`__expand_fully man5htmldir`"
printf "                  Manual pages: %s\n" "`__expand_fully man7htmldir`"
printf "\n"
printf "                         SHELL: %s\n" "`__expand_fully SHELL`"
printf "                            AT: %s\n" "`__expand_fully AT`"
printf "                           CAT: %s\n" "`__expand_fully CAT`"
printf "                         XARGS: %s\n" "`__expand_fully XARGS`"
printf "\n"


## DEBUG: Show all variables set in the shell at the end of 'configure' run
## set | grep -v '^[ {}]' | sort
