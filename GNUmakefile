
SDL3_VERSION = 3.2.16
SDL2_COMPAT_VERSION = 2.32.56
SDL12_COMPAT_VERSION = 1.2.68
SDL_GFX_VERSION = 2.0.27
MIKMOD_VERSION = 3.3.13
LIBOGG_VERSION = 1.3.6
LIBVORBIS_VERSION = 1.3.7
SDL_MIXER_VERSION = 1.2.12
ZLIB_VERSION = 1.3.1
LIBPNG_VERSION = 1.6.50
FREETYPE_VERSION = 2.13.3
SDL_TTF_VERSION = 2.0.11
GLEW_VERSION = 2.2.0

ARCH=x86_64
MINGW_TRIPLET = $(ARCH)-w64-mingw32
CMAKEFLAGS = -DCMAKE_SYSTEM_NAME=Windows -DCMAKE_SYSTEM_PROCESSOR=x86_64 -DCMAKE_C_COMPILER=$(MINGW_TRIPLET)-gcc -DCMAKE_CXX_COMPILER=$(MINGW_TRIPLET)-g++ -DCMAKE_INSTALL_PREFIX=$(CURDIR)/install -DCMAKE_PREFIX_PATH=$(CURDIR)/install
CONFIGUREFLAGS = --host=$(MINGW_TRIPLET) --prefix=$(CURDIR)/install
BUILDENV = WINEPREFIX=/dev/null PATH=$(CURDIR)/install/bin:$$PATH PKG_CONFIG_PATH=$(CURDIR)/install/lib/pkgconfig INCLUDES=-I$(CURDIR)/install/include LDFLAGS=-L$(CURDIR)/install/lib

WGET = wget
TARX = tar xvfm

# SDL3
SDL3_SOURCE_FILENAME = SDL3-$(SDL3_VERSION).tar.gz
SDL3_SOURCE_URL = https://github.com/libsdl-org/SDL/releases/download/release-$(SDL3_VERSION)/SDL3-$(SDL3_VERSION).tar.gz

SDL3_SOURCE = SDL3-$(SDL3_VERSION)

$(SDL3_SOURCE_FILENAME):
	$(WGET) $(SDL3_SOURCE_URL)

$(SDL3_SOURCE)/CMakeLists.txt: $(SDL3_SOURCE_FILENAME)
	$(TARX) $<

$(SDL3_SOURCE)/Makefile: $(SDL3_SOURCE)/CMakeLists.txt
	cd $(SDL3_SOURCE) && $(BUILDENV) cmake $(CMAKEFLAGS)

$(SDL3_SOURCE)/SDL3.dll: $(SDL3_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(SDL3_SOURCE)

$(CURDIR)/install/bin/SDL3.dll: $(SDL3_SOURCE)/SDL3.dll
	+$(BUILDENV) $(MAKE) -C $(SDL3_SOURCE) install
	touch $(CURDIR)/install/bin/SDL3.dll

# sdl2-compat
SDL2_COMPAT_SOURCE_FILENAME = sdl2-compat-$(SDL2_COMPAT_VERSION).tar.gz
SDL2_COMPAT_SOURCE_URL = https://github.com/libsdl-org/sdl2-compat/releases/download/release-$(SDL2_COMPAT_VERSION)/sdl2-compat-$(SDL2_COMPAT_VERSION).tar.gz

SDL2_COMPAT_SOURCE = sdl2-compat-$(SDL2_COMPAT_VERSION)

$(SDL2_COMPAT_SOURCE_FILENAME):
	$(WGET) $(SDL2_COMPAT_SOURCE_URL)

$(SDL2_COMPAT_SOURCE)/CMakeLists.txt: $(SDL2_COMPAT_SOURCE_FILENAME)
	$(TARX) $<

$(SDL2_COMPAT_SOURCE)/Makefile: $(SDL2_COMPAT_SOURCE)/CMakeLists.txt $(CURDIR)/install/bin/SDL3.dll
	cd $(SDL2_COMPAT_SOURCE) && $(BUILDENV) cmake $(CMAKEFLAGS)

$(SDL2_COMPAT_SOURCE)/SDL2.dll: $(SDL2_COMPAT_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(SDL2_COMPAT_SOURCE)
	touch $(SDL2_COMPAT_SOURCE)/SDL2.dll

$(CURDIR)/install/bin/SDL2.dll: $(SDL2_COMPAT_SOURCE)/SDL2.dll
	+$(BUILDENV) $(MAKE) -C $(SDL2_COMPAT_SOURCE) install
	touch $(CURDIR)/install/bin/SDL2.dll

# sdl12-compat
SDL12_COMPAT_SOURCE_FILENAME = release-$(SDL12_COMPAT_VERSION).tar.gz
SDL12_COMPAT_SOURCE_URL = https://github.com/libsdl-org/sdl12-compat/archive/refs/tags/$(SDL12_COMPAT_SOURCE_FILENAME)

SDL12_COMPAT_SOURCE = sdl12-compat-release-$(SDL12_COMPAT_VERSION)

$(SDL12_COMPAT_SOURCE_FILENAME):
	$(WGET) $(SDL12_COMPAT_SOURCE_URL)

$(SDL12_COMPAT_SOURCE)/CMakeLists.txt: $(SDL12_COMPAT_SOURCE_FILENAME)
	$(TARX) $<

$(SDL12_COMPAT_SOURCE)/Makefile: $(SDL12_COMPAT_SOURCE)/CMakeLists.txt $(CURDIR)/install/bin/SDL2.dll
	cd $(SDL12_COMPAT_SOURCE) && $(BUILDENV) cmake $(CMAKEFLAGS)

$(SDL12_COMPAT_SOURCE)/SDL.dll: $(SDL12_COMPAT_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(SDL12_COMPAT_SOURCE)
	touch $(SDL12_COMPAT_SOURCE)/SDL.dll

$(CURDIR)/install/bin/SDL.dll: $(SDL12_COMPAT_SOURCE)/SDL.dll
	+$(BUILDENV) $(MAKE) -C $(SDL12_COMPAT_SOURCE) install
	touch $(CURDIR)/install/bin/SDL.dll

# SDL_gfx
SDL_GFX_SOURCE_FILENAME = SDL_gfx-$(SDL_GFX_VERSION).tar.gz
SDL_GFX_SOURCE_URL = http://www.ferzkopp.net/Software/SDL_gfx-2.0/SDL_gfx-$(SDL_GFX_VERSION).zip
SDL_GFX_SOURCE = SDL_gfx-$(SDL_GFX_VERSION)

$(SDL_GFX_SOURCE_FILENAME):
	$(WGET) $(SDL_GFX_SOURCE_URL)

$(SDL_GFX_SOURCE)/configure: $(SDL_GFX_SOURCE_FILENAME)
	$(TARX) $<

$(SDL_GFX_SOURCE)/Makefile: $(SDL_GFX_SOURCE)/configure $(CURDIR)/install/bin/SDL.dll
	cd $(SDL_GFX_SOURCE) && $(BUILDENV) ./configure $(CONFIGUREFLAGS) --with-sdl=$(CURDIR)/install

$(SDL_GFX_SOURCE)/.libs/libSDL_gfx-16.dll: $(SDL_GFX_SOURCE)/Makefile $(CURDIR)/install/bin/SDL.dll
	+$(BUILDENV) $(MAKE) -C $(SDL_GFX_SOURCE)
	test -e $(SDL_GFX_SOURCE)/.libs/libSDL_gfx-16.dll && touch $(SDL_GFX_SOURCE)/.libs/libSDL_gfx-16.dll

$(CURDIR)/install/bin/libSDL_gfx-16.dll: $(SDL_GFX_SOURCE)/.libs/libSDL_gfx-16.dll
	+$(BUILDENV) $(MAKE) -C $(SDL_GFX_SOURCE) install
	touch $(CURDIR)/install/bin/libSDL_gfx-16.dll

# mikmod
MIKMOD_TAG = libmikmod-$(MIKMOD_VERSION)
MIKMOD_GIT_URL = https://git.code.sf.net/p/mikmod/mikmod
MIKMOD_SOURCE = mikmod

$(MIKMOD_SOURCE)/.git:
	git clone $(MIKMOD_GIT_URL)

$(MIKMOD_SOURCE)/libmikmod/CMakeLists.txt: $(MIKMOD_SOURCE)/.git patches/mikmod/*.patch
	(cd $(MIKMOD_SOURCE) && git fetch)
	(cd $(MIKMOD_SOURCE) && git checkout -f $(MIKMOD_TAG))
	(cd $(MIKMOD_SOURCE) && for i in ../patches/mikmod/*.patch; do (patch -p1 < $$i || exit); done)

$(MIKMOD_SOURCE)/libmikmod/Makefile: $(MIKMOD_SOURCE)/libmikmod/CMakeLists.txt
	cd $(MIKMOD_SOURCE)/libmikmod && $(BUILDENV) cmake $(CMAKEFLAGS)

$(MIKMOD_SOURCE)/libmikmod/libmikmod-3.dll: $(MIKMOD_SOURCE)/libmikmod/Makefile
	+$(BUILDENV) $(MAKE) -C $(MIKMOD_SOURCE)/libmikmod
	touch $(MIKMOD_SOURCE)/libmikmod/libmikmod-3.dll

$(CURDIR)/install/bin/libmikmod-3.dll: $(MIKMOD_SOURCE)/libmikmod/libmikmod-3.dll
	+$(BUILDENV) $(MAKE) -C $(MIKMOD_SOURCE)/libmikmod install
	touch $(CURDIR)/install/bin/libmikmod-3.dll

# libogg
LIBOGG_SOURCE_FILENAME = libogg-$(LIBOGG_VERSION).tar.xz
LIBOGG_SOURCE_URL = https://github.com/xiph/ogg/releases/download/v$(LIBOGG_VERSION)/$(LIBOGG_SOURCE_FILENAME)
LIBOGG_SOURCE = libogg-$(LIBOGG_VERSION)

$(LIBOGG_SOURCE_FILENAME):
	$(WGET) $(LIBOGG_SOURCE_URL)

$(LIBOGG_SOURCE)/configure: $(LIBOGG_SOURCE_FILENAME)
	$(TARX) $<
	(cd $(LIBOGG_SOURCE) && autoreconf -if)

$(LIBOGG_SOURCE)/Makefile: $(LIBOGG_SOURCE)/configure
	cd $(LIBOGG_SOURCE) && $(BUILDENV) ./configure $(CONFIGUREFLAGS)

$(LIBOGG_SOURCE)/src/.libs/libogg-0.dll: $(LIBOGG_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(LIBOGG_SOURCE)

$(CURDIR)/install/bin/libogg-0.dll: $(LIBOGG_SOURCE)/src/.libs/libogg-0.dll
	+$(BUILDENV) $(MAKE) -C $(LIBOGG_SOURCE) install
	touch $(CURDIR)/install/bin/libogg-0.dll

# libvorbis
LIBVORBIS_SOURCE_FILENAME = libvorbis-$(LIBVORBIS_VERSION).tar.xz
LIBVORBIS_SOURCE_URL = https://github.com/xiph/vorbis/releases/download/v$(LIBVORBIS_VERSION)/$(LIBVORBIS_SOURCE_FILENAME)
LIBVORBIS_SOURCE = libvorbis-$(LIBVORBIS_VERSION)

$(LIBVORBIS_SOURCE_FILENAME):
	$(WGET) $(LIBVORBIS_SOURCE_URL)

$(LIBVORBIS_SOURCE)/configure: $(LIBVORBIS_SOURCE_FILENAME)
	$(TARX) $<
	(cd $(LIBVORBIS_SOURCE) && autoreconf -if)

$(LIBVORBIS_SOURCE)/Makefile: $(LIBVORBIS_SOURCE)/configure $(LIBOGG_SOURCE)/src/.libs/libogg-0.dll
	cd $(LIBVORBIS_SOURCE) && $(BUILDENV) ./configure $(CONFIGUREFLAGS)

$(LIBVORBIS_SOURCE)/lib/.libs/libvorbis-0.dll: $(LIBVORBIS_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(LIBVORBIS_SOURCE)

$(CURDIR)/install/bin/libvorbis-0.dll: $(LIBVORBIS_SOURCE)/lib/.libs/libvorbis-0.dll
	+$(BUILDENV) $(MAKE) -C $(LIBVORBIS_SOURCE) install
	test -e $(CURDIR)/install/bin/libvorbis-0.dll && touch $(CURDIR)/install/bin/libvorbis-0.dll

# SDL_mixer
SDL_MIXER_SOURCE_FILENAME = SDL_mixer-$(SDL_MIXER_VERSION).tar.gz
SDL_MIXER_SOURCE_URL = https://www.libsdl.org/projects/SDL_mixer/release/$(SDL_MIXER_SOURCE_FILENAME)
SDL_MIXER_SOURCE = SDL_mixer-$(SDL_MIXER_VERSION)

$(SDL_MIXER_SOURCE_FILENAME):
	$(WGET) $(SDL_MIXER_SOURCE_URL)

$(SDL_MIXER_SOURCE)/configure: $(SDL_MIXER_SOURCE_FILENAME) patches/sdl_mixer/*.patch
	$(TARX) $<
	(cd $(SDL_MIXER_SOURCE) && for i in ../patches/sdl_mixer/*.patch; do (patch -p1 < $$i || exit); done)

$(SDL_MIXER_SOURCE)/Makefile: $(SDL_MIXER_SOURCE)/configure $(CURDIR)/install/bin/SDL.dll $(CURDIR)/install/bin/libmikmod-3.dll $(CURDIR)/install/bin/libvorbis-0.dll
	cd $(SDL_MIXER_SOURCE) && $(BUILDENV) ./configure $(CONFIGUREFLAGS) --with-sdl-prefix=$(CURDIR)/install

$(SDL_MIXER_SOURCE)/build/.libs/SDL_mixer.dll: $(SDL_MIXER_SOURCE)/Makefile $(CURDIR)/install/bin/SDL.dll
	+$(BUILDENV) $(MAKE) -C $(SDL_MIXER_SOURCE)
	test -e $(SDL_MIXER_SOURCE)/build/.libs/SDL_mixer.dll && touch $(SDL_MIXER_SOURCE)/build/.libs/SDL_mixer.dll

$(CURDIR)/install/bin/SDL_mixer.dll: $(SDL_MIXER_SOURCE)/build/.libs/SDL_mixer.dll
	+$(BUILDENV) $(MAKE) -C $(SDL_MIXER_SOURCE) install
	touch $(CURDIR)/install/bin/SDL_mixer.dll

# zlib

ZLIB_SOURCE_FILENAME = zlib-$(ZLIB_VERSION).tar.xz
ZLIB_SOURCE_URL = https://github.com/madler/zlib/releases/download/v$(ZLIB_VERSION)/$(ZLIB_SOURCE_FILENAME)
ZLIB_SOURCE = zlib-$(ZLIB_VERSION)

$(ZLIB_SOURCE_FILENAME):
	$(WGET) $(ZLIB_SOURCE_URL)

$(ZLIB_SOURCE)/win32/Makefile.gcc: $(ZLIB_SOURCE_FILENAME)
	$(TARX) $<

$(ZLIB_SOURCE)/zlib1.dll: $(ZLIB_SOURCE)/win32/Makefile.gcc
	+$(BUILDENV) $(MAKE) -C $(ZLIB_SOURCE) -f$(CURDIR)/$(ZLIB_SOURCE)/win32/Makefile.gcc PREFIX=$(MINGW_TRIPLET)-

$(CURDIR)/install/bin/zlib1.dll: $(ZLIB_SOURCE)/zlib1.dll
	+$(BUILDENV) $(MAKE) -C $(ZLIB_SOURCE) -f$(CURDIR)/$(ZLIB_SOURCE)/win32/Makefile.gcc install BINARY_PATH=$(CURDIR)/install/bin INCLUDE_PATH=$(CURDIR)/install/include LIBRARY_PATH=$(CURDIR)/install/lib SHARED_MODE=1
	test -e $(CURDIR)/install/bin/zlib1.dll && touch $(CURDIR)/install/bin/zlib1.dll

# libpng

LIBPNG_TAG = v$(LIBPNG_VERSION)
LIBPNG_GIT_URL = https://github.com/pnggroup/libpng.git
LIBPNG_SOURCE = libpng

$(LIBPNG_SOURCE)/.git:
	git clone $(LIBPNG_GIT_URL)

$(LIBPNG_SOURCE)/configure: $(LIBPNG_SOURCE)/.git
	(cd $(LIBPNG_SOURCE) && git fetch)
	(cd $(LIBPNG_SOURCE) && git checkout -f $(LIBPNG_TAG))
	touch $(LIBPNG_SOURCE)/configure

$(LIBPNG_SOURCE)/Makefile: $(LIBPNG_SOURCE)/configure $(CURDIR)/install/bin/zlib1.dll
	cd $(LIBPNG_SOURCE) && $(BUILDENV) ./configure $(CONFIGUREFLAGS)

$(LIBPNG_SOURCE)/.libs/libpng16-16.dll: $(LIBPNG_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(LIBPNG_SOURCE)
	touch $(LIBPNG_SOURCE)/.libs/libpng16-16.dll

$(CURDIR)/install/bin/libpng16-16.dll: $(LIBPNG_SOURCE)/.libs/libpng16-16.dll
	+$(BUILDENV) $(MAKE) -C $(LIBPNG_SOURCE) install
	test -e $(CURDIR)/install/bin/libpng16-16.dll && touch $(CURDIR)/install/bin/libpng16-16.dll

# freetype
FREETYPE_SOURCE_FILENAME = freetype-$(FREETYPE_VERSION).tar.xz
FREETYPE_SOURCE_URL = https://download-mirror.savannah.gnu.org/releases/freetype/$(FREETYPE_SOURCE_FILENAME)
FREETYPE_SOURCE = freetype-$(FREETYPE_VERSION)

$(FREETYPE_SOURCE_FILENAME):
	$(WGET) $(FREETYPE_SOURCE_URL)

$(FREETYPE_SOURCE)/configure: $(FREETYPE_SOURCE_FILENAME)
	$(TARX) $<

$(FREETYPE_SOURCE)/Makefile: $(FREETYPE_SOURCE)/configure $(CURDIR)/install/bin/libpng16-16.dll $(CURDIR)/install/bin/zlib1.dll
	cd $(FREETYPE_SOURCE) && $(BUILDENV) ./configure $(CONFIGUREFLAGS) --with-brotli=no --with-bzip2=no --with-harfbuzz=no
	touch $(FREETYPE_SOURCE)/Makefile

$(FREETYPE_SOURCE)/objs/libfreetype.la: $(FREETYPE_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(FREETYPE_SOURCE)

$(CURDIR)/install/lib/libfreetype.la: $(FREETYPE_SOURCE)/objs/libfreetype.la
	+$(BUILDENV) $(MAKE) -C $(FREETYPE_SOURCE) install
	touch $(CURDIR)/install/lib/libfreetype.la

# SDL_ttf
SDL_TTF_SOURCE_FILENAME = SDL_ttf-$(SDL_TTF_VERSION).tar.gz
SDL_TTF_SOURCE_URL = https://www.libsdl.org/projects/SDL_ttf/release/$(SDL_TTF_SOURCE_FILENAME)
SDL_TTF_SOURCE = SDL_ttf-$(SDL_TTF_VERSION)

$(SDL_TTF_SOURCE_FILENAME):
	$(WGET) $(SDL_TTF_SOURCE_URL)

$(SDL_TTF_SOURCE)/configure.in: $(SDL_TTF_SOURCE_FILENAME) patches/sdl_ttf/*.patch
	$(TARX) $<
	(cd $(SDL_TTF_SOURCE) && for i in ../patches/sdl_ttf/*.patch; do (patch -p1 < $$i || exit); done)

$(SDL_TTF_SOURCE)/Makefile: $(SDL_TTF_SOURCE)/configure.in $(CURDIR)/install/bin/SDL.dll $(CURDIR)/install/lib/libfreetype.la
	cd $(SDL_TTF_SOURCE) && ./autogen.sh && $(BUILDENV) ./configure $(CONFIGUREFLAGS) --with-sdl-prefix=$(CURDIR)/install

$(SDL_TTF_SOURCE)/.libs/SDL_ttf.dll: $(SDL_TTF_SOURCE)/Makefile $(CURDIR)/install/bin/SDL.dll
	+$(BUILDENV) $(MAKE) -C $(SDL_TTF_SOURCE)

$(CURDIR)/install/bin/SDL_ttf.dll: $(SDL_TTF_SOURCE)/.libs/SDL_ttf.dll
	+$(BUILDENV) $(MAKE) -C $(SDL_TTF_SOURCE) install
	touch $(CURDIR)/install/bin/SDL_ttf.dll

# glew

GLEW_SOURCE_FILENAME = glew-$(GLEW_VERSION).tgz
GLEW_SOURCE_URL = https://downloads.sourceforge.net/glew/$(GLEW_SOURCE_FILENAME)
GLEW_SOURCE = glew-$(GLEW_VERSION)

$(GLEW_SOURCE_FILENAME):
	$(WGET) $(GLEW_SOURCE_URL)

$(GLEW_SOURCE)/Makefile: $(GLEW_SOURCE_FILENAME)
	$(TARX) $<

$(GLEW_SOURCE)/lib/glew32.dll: $(GLEW_SOURCE)/Makefile
	+$(BUILDENV) $(MAKE) -C $(GLEW_SOURCE) SYSTEM=linux-mingw64

$(CURDIR)/install/bin/glew32.dll: $(GLEW_SOURCE)/lib/glew32.dll
	+$(BUILDENV) $(MAKE) -C $(GLEW_SOURCE) SYSTEM=linux-mingw64 DESTDIR=$(CURDIR)/install GLEW_DEST= install
	mv install/lib/glew32.dll install/bin/glew32.dll

$(CURDIR)/install/lib/libglew32.dll.a: $(GLEW_SOURCE)/lib/libglew32.dll.a
	cp $(GLEW_SOURCE)/lib/libglew32.dll.a $(CURDIR)/install/lib/libglew32.dll.a

# hyperrogue

hyperrogue/language-data.cpp: hyperrogue/langen.cpp
	+$(MAKE) -C hyperrogue language-data.cpp

hyperrogue/autohdr.h: hyperrogue/makeh.cpp hyperrogue/language-data.cpp hyperrogue/*.cpp
	+$(MAKE) -C hyperrogue autohdr.h

hyperrogue/hyper.res: hyperrogue/hyper.rc hyperrogue/hr-icon.ico
	cd hyperrogue && $(MINGW_TRIPLET)-windres hyper.rc -O coff -o hyper.res

hyperrogue/hyperrogue.exe: hyperrogue/language-data.cpp hyperrogue/autohdr.h hyperrogue/hyper.res $(CURDIR)/install/bin/SDL_ttf.dll $(CURDIR)/install/bin/libSDL_gfx-16.dll $(CURDIR)/install/bin/SDL_mixer.dll $(CURDIR)/install/bin/glew32.dll $(CURDIR)/install/lib/libglew32.dll.a
	+$(BUILDENV) $(MAKE) -C hyperrogue OS=mingw TOOLCHAIN=mingw CXXFLAGS_EARLY="-DWINDOWS -mwindows -D_A_VOLID8 -I$(CURDIR)/install/include -I$(CURDIR)/install/include/SDL" CXX=$(MINGW_TRIPLET)-g++ HYPERROGUE_USE_GLEW=1 HYPERROGUE_USE_PNG=1

files: hyperrogue/hyperrogue.exe
	rm -rfv hyperrogue-build
	mkdir -p hyperrogue-build
	cp install/bin/* hyperrogue-build
	for i in libgcc_s_seh-1.dll libwinpthread-1.dll libstdc++-6.dll; do cp /usr/$(MINGW_TRIPLET)/bin/$$i hyperrogue-build; done
	cp -r hyperrogue/hyperrogue.exe hyperrogue/*.ttf hyperrogue/honeycombs hyperrogue/sounds hyperrogue/music hyperrogue/hyperrogue-music.txt hyperrogue/*.dat hyperrogue-build

dist: files
	cd hyperrogue-build && 7z a ../hyperrogue.zip *

