#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.59])
AC_INIT([FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS])
AM_INIT_AUTOMAKE([manifold_mcp-cache], [1.0])
AC_CONFIG_SRCDIR([L1_cache.h])
#AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_RANLIB

# Checks for libraries.


# enable parallel simulation by default
AC_ARG_ENABLE([para_sim],
    [AS_HELP_STRING([--disable-para-sim],
        [disable parallel simulation @<:@default: no@:>@])],
    [para_sim=${enableval}], [para_sim=yes])

if test "x${para_sim}" = xyes ; then
    AC_CHECK_PROGS([MPICPP], [mpic++], [mpic++])
    if test "${MPICPP}" = "mpic++" ; then # para_sim enabled; mpic++ found
        AC_SUBST(CXX, mpic++)
    else # para_sim enabled; mpic++ not found
        AC_MSG_WARN([
	---------------------------------------------
	mpic++ not found! Building serial simulation!
	---------------------------------------------])
	AC_DEFINE(NO_MPI)
    fi
else # para_sim disabled
    AC_DEFINE(NO_MPI)
    AC_MSG_WARN([
    ---------------------------------------------------------
    Parallel simulation disabled! Building serial simulation!
    ---------------------------------------------------------])
fi


# configure variables: KERINC
# This can be used to let user tell us where to find kernel headers.
# For example: configure  KERINC="foo/include/kernel"
AC_ARG_VAR([KERINC], [The path where kernel headers can be found.])

# if KERINC not specified, first look in super-project, and if not found,
# then set to installation directory.
if test -z "$KERINC" ; then
    if test -d "$srcdir/../../../kernel"; then
	KERINC='$(top_srcdir)/../../..'
	AC_MSG_NOTICE([
    -----------------------------------------
    Use kernel header files in super-project.
    -----------------------------------------
	])
    else
	#check installation dir
	# unfortunately I can't use if test -f "$includedir/manifold/kernel/manifold.h"
	# because $includedir will only expand to ${prefix}/include

	# simply set the value. I can't check if it's really there.
	KERINC='$(includedir)/manifold'
	AC_MSG_NOTICE([
    --------------------------------------------------
    Use kernel header files in installation direcotry.
    --------------------------------------------------
	])
    fi
else # KERINC is specified
    AC_MSG_NOTICE([
    -----------------------------------------
    Use kernel header files in $KERINC
    -----------------------------------------
    ])
fi

AC_SUBST([KERNEL_INC], [$KERINC])


# disable FORECAST_NULL by default
AC_ARG_ENABLE([forecast_null],
    [AS_HELP_STRING([--enable-forecast-null],
        [enable Forecat Null Message Algorithm @<:@default: no@:>@])],
    [forecast_null=${enableval}], [forecast_null=no])

if test "x${forecast_null}" = xyes ; then
    AC_MSG_WARN([
    ----------------------
    FORECAST_NULL enabled!
    ----------------------])
    AC_DEFINE(FORECAST_NULL)
fi

# check kitfox integration
AC_ARG_WITH([kitfox],
    [AS_HELP_STRING([--without-kitfox],
        [do not use QSim @<:@default: no@:>@])],
    [kitfox=${enableval}], [kitfox=yes])

if test "x${kitfox}" = xyes ; then
    #check if QSim header files exist
    # We allow user to specify the location of the header files, e.g.,
    # configure KITFOXINC=/foo/kitfox
    if test -z "$KITFOXINC" ; then
	KITFOXINC='/usr/local/include'
    fi

    if test ! -e "$KITFOXINC/kitfox.h"; then
	AC_MSG_ERROR([
    -----------------------------------------
    ERROR: kitfox.h not found in $KITFOXINC!
    -----------------------------------------
	])
    else
	AC_DEFINE(LIBKITFOX)
	AC_SUBST([KITFOX_INC], [$KITFOXINC])
    fi
else # use_kitfox disabled
    AC_MSG_WARN([
    -----------------------------------------------------------------
    LIBKITFOX disabled! Building only processor models not using KitFox.
    -----------------------------------------------------------------])
	AC_SUBST([KITFOX_INC], ["."])
fi


# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL

# Checks for library functions.

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
