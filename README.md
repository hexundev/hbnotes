# Homebrew notes

My personal notes for using/building different libraries used in homebrew development 

## GRRLib 4.5.1 (Wii/GC)

Download:\
https://github.com/GRRLIB/GRRLIB

PNG support with libpngu is included in the source.
Optionally, remove TTF and JPEG support by edting these files:
- GRRLIB__lib.h - `#ifdef` the TTF functions
- GRRLIB_ttf.c - `#ifdef` the file
- GRRLIB_core.c - `#ifdef` the calls to `GRRLIB_ExitTTF and GRRLIB_InitTTF`
- GRRLIB_texEdit.c - `#ifdef GRRLIB_LoadTextureJPG and GRRLIB_LoadTextureJPGEx`\
and the relevant calls in `GRRLIB_LoadTexture`

Edit makefile to support different builds for GC/Wii.

```Makefile
ifeq ($(PLATFORM),cube)
TARGET		:=	grrlib_gc
BUILD		:=	build_gc
else
TARGET		:=	grrlib_wii
BUILD		:=	build_wii
endif
```

Edit makefile to remove freetype
```Makefile
CFLAGS	= -g -O2 -Wall $(MACHDEP) $(INCLUDE) `$(PREFIX)pkg-config freetype2 --cflags`
# to
CFLAGS = -g -O2 -Wall $(MACHDEP) $(INCLUDE)
```

Build:
```Makefile
# GC
make PLATFORM=cube clean all install
# Wii
make clean all install
```

## SDL 2.28 (Wii/GC)

Download:\
https://github.com/devkitPro/SDL/tree/ogc-sdl-2.28

If needed, edit cmakelists.txt to remove opengl, build errors happened for me:
```bash
set_option(SDL_OPENGL              "Include OpenGL support" OFF)
set_option(SDL_OPENGLES            "Include OpenGL ES support" OFF)
```

From SDL folder:
```bash
cmake -S. -Bbuild -DCMAKE_TOOLCHAIN_FILE="$DEVKITPRO/cmake/Wii.cmake" -DCMAKE_BUILD_TYPE=Release
cmake --build build
cmake --install build
```
