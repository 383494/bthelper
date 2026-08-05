AC_PREREQ(2.61)
AC_INIT(bthelper, 0.01, thomas@habets.se)
AC_CANONICAL_SYSTEM
AC_CONFIG_SRCDIR([src/bt-connecter.cc])
AM_INIT_AUTOMAKE([foreign subdir-objects])
AM_MAINTAINER_MODE
AC_CONFIG_HEADER(src/config.h)

# Check for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_MAKE_SET
AC_ENABLE_SHARED
AC_DISABLE_STATIC
AC_PROG_LIBTOOL(libtool)
PKG_PROG_PKG_CONFIG
AC_CONFIG_MACRO_DIRS([m4])

# Check for libraries.
AC_LANG_CPLUSPLUS
AC_CHECK_LIB([util], [forkpty])

# Add -std=c++17 iff needed.
AC_LANG_PUSH([C++])
AC_MSG_CHECKING([whether std::string_view works by default])
AC_COMPILE_IFELSE(
    [AC_LANG_PROGRAM(
        [[#include <string_view>]],
        [[
            std::string_view s = "test";
            return s.size() != 4;
        ]]
    )],
    [
        AC_MSG_RESULT([yes])
    ],
    [
        AC_MSG_RESULT([no])

        save_CXXFLAGS=$CXXFLAGS
        CXXFLAGS="$CXXFLAGS -std=c++17"

        AC_MSG_CHECKING([whether std::string_view works with -std=c++17])
        AC_COMPILE_IFELSE(
            [AC_LANG_PROGRAM(
                [[#include <string_view>]],
                [[
                    std::string_view s = "test";
                    return s.size() != 4;
                ]]
            )],
            [
                AC_MSG_RESULT([yes])
                dnl Keep -std=c++17 in CXXFLAGS.
            ],
            [
                AC_MSG_RESULT([no])
                CXXFLAGS=$save_CXXFLAGS
                AC_MSG_ERROR(
                    [a compiler and standard library supporting std::string_view are required]
                )
            ]
        )
    ]
)

AC_LANG_POP([C++])

# Output
AC_CONFIG_FILES([Makefile])
AC_OUTPUT

# Status
echo "
  $PACKAGE_NAME version $PACKAGE_VERSION
  Prefix.........: $prefix
  Debug Build....: $debug
  C++ Compiler...: $CXX $CXXFLAGS $CPPFLAGS
  Linker.........: $LD $LDFLAGS $LIBS
"
