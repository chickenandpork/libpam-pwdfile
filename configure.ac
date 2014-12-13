dnl ***********************************************************************
dnl Process this file with autoconf (autoreconf -vfi) to produce a configure script.
dnl ***********************************************************************
AC_INIT(libpam-pwdfile, 1.0)
AC_CONFIG_MACRO_DIR([m4])

AC_CANONICAL_HOST

AM_INIT_AUTOMAKE([foreign dist-bzip2])

dnl ***********************************************************************
dnl Checks for programs used during the build
dnl ***********************************************************************
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_MAKE_SET
LT_INIT

dnl ***********************************************************************
dnl Checks for programs used during test, markup, diagrams. etc.
dnl ***********************************************************************
AC_PATH_PROGS(RPMBUILD, [rpmbuild], [missing rpmbuild])
AM_CONDITIONAL(HAVE_RPMBUILD, [test "x$RPMBUILD" != "xmissing rpmbuild"])

AC_PATH_PROGS(GIT, [git], [missing])
AC_MSG_CHECKING([version agreement with git tag])
BUILDID=$( (git describe; cat gitdescribe) 2>/dev/null |head -1| cut -d "-" -f 2)
CHKVER=$( (git describe; cat gitdescribe) 2>/dev/null |head -1| cut -d "-" -f 1)
if test "${CHKVER}" = "${VERSION}"
then 
  AC_MSG_RESULT([ok])
else
  AC_MSG_WARN([version mismatch])
fi
AC_SUBST(BUILDID)

AM_CONDITIONAL(HAVE_GIT, test "$GIT" != "missing git" -a -f .git/config)
AC_PATH_PROGS(ECHO, [echo], [missing])


AC_ARG_WITH(sudopostmove,
[AS_HELP_STRING([--with-sudopostmove],[Enable sudopostmove of a built RPM; default=disabled])],
[
    case "$withval" in
        "no")  WITH_SUDOPOSTMOVE=no  ;;
        *) WITH_SUDOPOSTMOVE="$withval" ;;
    esac
],
[
    WITH_SUDOPOSTMOVE=no
])
AM_CONDITIONAL(DO_SUDOPOSTMOVE, test "$WITH_SUDOPOSTMOVE" != no)
AC_SUBST([SUDOPOSTMOVE], ${WITH_SUDOPOSTMOVE})

dnl AC_CHECK_LIB(rpm, rpmReadPackageFile,,AC_MSG_ERROR(rpm lib not found))
AC_CHECK_LIB(crypt, rpmHeaderGetEntry)
AC_CHECK_LIB(pam, headerGetEntry)
AC_CHECK_HEADERS(crypt.h security/pam_appl.h)



AC_CONFIG_FILES(GNUmakefile
)

if test "x$RPMBUILD" != "xmissing rpmbuild"
then
  AC_CONFIG_FILES(libpam-pwdfile.spec:pkg/rpm/libpam-pwdfile.spec.in)
fi

AC_OUTPUT
