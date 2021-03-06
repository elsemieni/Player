#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([easyrpg-player],[0.6.2],[https://github.com/EasyRPG/Player/issues],[easyrpg-player],[https://easyrpg.org])

AC_CONFIG_AUX_DIR([builds/autoconf/aux])
AM_INIT_AUTOMAKE([1.11.4 foreign subdir-objects tar-ustar -Wall dist-xz])
AM_MAINTAINER_MODE([enable])
AM_SILENT_RULES([yes])

AC_CONFIG_MACRO_DIR([builds/autoconf/m4])
AC_CONFIG_SRCDIR([src/main.cpp])
AC_CONFIG_HEADERS([config.h])
AC_LANG([C++])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CXXCPP
AM_PROG_AR
AC_PROG_RANLIB
PKG_PROG_PKG_CONFIG

# Options
AC_ARG_VAR([EM_GAME_URL], [Game URL/directory (only used for the Emscripten port)])
AS_IF([test "x$EM_GAME_URL" != "x"],[
	AC_DEFINE_UNQUOTED([EM_GAME_URL], ["$EM_GAME_URL"], [Game URL (Emscripten)])
])
AC_ARG_ENABLE([fmmidi],
	AS_HELP_STRING([--enable-fmmidi@<:@=fallback@:>@],[use internal MIDI sequencer/as fallback @<:@default=no@:>@]))
AS_IF([test "x$enable_fmmidi" = "xyes"],[want_fmmidi=1],[test "x$enable_fmmidi" = "xfallback"],[want_fmmidi=2])
AS_IF([test "x$want_fmmidi" != "x"],
	AC_DEFINE_UNQUOTED([WANT_FMMIDI],[$want_fmmidi],[Enable internal MIDI sequencer(1)/as fallback(2)]))
AM_CONDITIONAL([WANT_FMMIDI],[test "x$want_fmmidi" != "x"])

# Checks for libraries.
PKG_CHECK_MODULES([LCF],[liblcf])
PKG_CHECK_MODULES([PIXMAN],[pixman-1])
AC_ARG_WITH([freetype], AS_HELP_STRING([--without-freetype], [Disable FreeType font rendering @<:@default=auto@:>@]))
AS_IF([test "x$with_freetype" != "xno"], [
	PKG_CHECK_MODULES([FREETYPE],[freetype2],[AC_DEFINE(HAVE_FREETYPE,[1],[Enable FreeType font rendering])],[auto_freetype=0])
	AC_ARG_WITH([harfbuzz], AS_HELP_STRING([--without-harfbuzz], [Disable HarfBuzz text shaping @<:@default=auto@:>@]))
	AS_IF([test "x$with_harfbuzz" != "xno"], [
		PKG_CHECK_MODULES([HARFBUZZ],[harfbuzz],[AC_DEFINE(HAVE_HARFBUZZ,[1],[Enable HarfBuzz text shaping])],[auto_harfbuzz=0])
	])
])
PKG_CHECK_MODULES([SDL],[sdl2 >= 2.0.5],[AC_DEFINE(USE_SDL,[2],[Enable SDL2])],[
	PKG_CHECK_MODULES([SDL],[sdl],[AC_DEFINE(USE_SDL,[1],[Enable SDL])])
])
AC_ARG_WITH([audio],[AS_HELP_STRING([--without-audio], [Disable audio support. @<:@default=on@:>@])])
AS_IF([test "x$with_audio" != "xno"],[
	AC_DEFINE(SUPPORT_AUDIO,[1],[Enable Audio Support])
	AC_ARG_WITH([sdlmixer],[AS_HELP_STRING([--without-sdlmixer],
		[Disable audio playback through SDL2 mixer. Audio is passed through SDL2 mixer when none of the provided audio libraries support the format. @<:@default=auto@:>@])])
	AS_IF([test "x$with_sdlmixer" != "xno"],[
		PKG_CHECK_MODULES([SDLMIXER],[SDL2_mixer],[AC_DEFINE(HAVE_SDL_MIXER,[1],[Enable SDL2_Mixer])],[
			PKG_CHECK_MODULES([SDLMIXER],[SDL_mixer],[AC_DEFINE(HAVE_SDL_MIXER,[1],[Enable SDL_Mixer])],[auto_sdlmixer=0])
		])
	])
])
PKG_CHECK_MODULES([PNG],[libpng])
PKG_CHECK_MODULES([ZLIB],[zlib])
AC_ARG_WITH([libmpg123],[AS_HELP_STRING([--without-libmpg123],
	[Disable improved MP3 support provided by libmpg123. Uses SDL_mixer instead which results in noise or crashes for some MP3s. @<:@default=auto@:>@])])
AS_IF([test "x$with_libmpg123" != "xno"],[
	PKG_CHECK_MODULES([MPG123],[libmpg123],[AC_DEFINE(HAVE_MPG123,[1],[Enable improved MP3 support provided by libmpg123])],[auto_mpg123=0])
])
AC_ARG_WITH([libwildmidi],[AS_HELP_STRING([--without-libwildmidi],
	[Disable Midi support provided by libwildmidi. @<:@default=auto@:>@])])
AS_IF([test "x$with_libwildmidi" != "xno"],[
	PKG_CHECK_MODULES([WILDMIDI],[wildmidi],[AC_DEFINE(HAVE_WILDMIDI,[1],[Enable Midi support provided by libwildmidi])],[auto_wildmidi=0])
])
AC_ARG_WITH([oggvorbis],[AS_HELP_STRING([--without-oggvorbis],
	[Disable Ogg Vorbis support @<:@default=auto@:>@])])
AS_IF([test "x$with_oggvorbis" != "xno"],[
	PKG_CHECK_MODULES([OGGVORBIS],[vorbisfile],[AC_DEFINE(HAVE_OGGVORBIS,[1],[Enable Ogg Vorbis support])],[auto_oggvorbis=0])
])
AC_ARG_WITH([opus],[AS_HELP_STRING([--without-opus],
	[Disable Opus support @<:@default=auto@:>@])])
AS_IF([test "x$with_opus" != "xno"],[
	PKG_CHECK_MODULES([OPUS],[opusfile],[AC_DEFINE(HAVE_OPUS,[1],[Enable Opus support])],[auto_opus=0])
])
AC_ARG_WITH([libsndfile],[AS_HELP_STRING([--without-libsndfile],
	[Disable improved WAV support provided by libsndfile.  @<:@default=auto@:>@])])
AS_IF([test "x$with_libsndfile" != "xno"],[
	PKG_CHECK_MODULES([SNDFILE],[sndfile],[AC_DEFINE(HAVE_LIBSNDFILE,[1],[Disable improved WAV support provided by libsndfile])],[auto_sndfile=0])
])
AC_ARG_WITH([libxmp],[AS_HELP_STRING([--without-libxmp],
	[Disable tracker module support provided by libxmp. @<:@default=auto@:>@])])
AS_IF([test "x$with_libxmp" != "xno"],[
	PKG_CHECK_MODULES([XMP],[libxmp],[AC_DEFINE(HAVE_XMP,[1],[Enable tracker module support provided by libxmp])],[auto_xmp=0])
])
AC_ARG_WITH([libspeexdsp],[AS_HELP_STRING([--without-libspeexdsp],
	[Disable resampling support provided by libspeexdsp. Uses SDL_mixer instead which results in noise. @<:@default=auto@:>@])])
AS_IF([test "x$with_libspeexdsp" != "xno"],[
	PKG_CHECK_MODULES([SPEEXDSP],[speexdsp],[AC_DEFINE(HAVE_LIBSPEEXDSP,[1],[Disable resampling support provided by libspeexdsp])],[auto_speexdsp=0])
])
PKG_CHECK_MODULES([FMT],[fmt >= 5.3])

# bash completion
AC_ARG_WITH([bash-completion-dir],[AS_HELP_STRING([--with-bash-completion-dir@<:@=DIR@:>@],
	[Install the parameter auto-completion script for bash in DIR. @<:@default=auto@:>@])],
	[],[with_bash_completion_dir=yes])
AS_IF([test "x$with_bash_completion_dir" = "xyes"],[
	BASHCOMPLETION_DIR="`$PKG_CONFIG --silence-errors --define-variable=prefix="\${prefix}" --define-variable=datadir="\${datadir}" --variable=completionsdir bash-completion`"
],[
	BASHCOMPLETION_DIR=$with_bash_completion_dir
])
AS_IF([test "x$BASHCOMPLETION_DIR" = "x"],[
	BASHCOMPLETION_DIR="${datadir}/bash-completion/completions"
])
AC_SUBST([BASHCOMPLETION_DIR])
AM_CONDITIONAL([HAVE_BASHCOMPLETION], [test "x$with_bash_completion_dir" != "xno"])

# C++14 is mandatory
AX_CXX_COMPILE_STDCXX(14, noext)

# Checks for header files.
AC_CHECK_HEADERS([cstdint cstdlib string iostream unistd.h wchar.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_BIGENDIAN
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT8_T
AC_TYPE_SIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_CHECK_FUNCS([malloc floor getcwd memset putenv strerror])

# manual page
AC_CHECK_PROGS([A2X], [a2x a2x.py], [no])
AM_CONDITIONAL([HAVE_A2X], [test x"$A2X" != "xno"])
AC_SUBST([A2X])
AS_IF([test x"$A2X" = "xno" && test ! -f "${srcdir}/resources/easyrpg-player.6"],
  AC_MSG_WARN([a2x is required to create the manual page]))
AM_CONDITIONAL([HAVE_MANUAL], [test -f "${srcdir}/resources/easyrpg-player.6"])

# Doxygen source documentation
m4_include([builds/autoconf/m4/ax_prog_doxygen.m4])
DX_DOXYGEN_FEATURE(OFF)
DX_HTML_FEATURE(ON)
DX_DOT_FEATURE(ON)
DX_INIT_DOXYGEN(easyrpg-player, resources/Doxyfile, doc)

# Files to generate
AC_CONFIG_FILES([Makefile
                 resources/Doxyfile])

AC_OUTPUT
