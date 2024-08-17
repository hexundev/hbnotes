# Homebrew notes

My personal notes for using/building different libraries used in homebrew development 

## GRRLib 4.5.1 (Wii/GC)

Download:\
https://github.com/GRRLIB/GRRLIB

Optionally, remove TTF and JPEG support:
- GRRLIB_ttf.c
- GRRLIB_texEdit.c
- GRRLIB_core.c 

Edit makefile to support different build folders for GC/Wii.

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

If needed, edit cmakelists.txt to remove opengl, build error happened for me:
```bash
set_option(SDL_OPENGL              "Include OpenGL support" OFF)
set_option(SDL_OPENGLES            "Include OpenGL ES support" OFF)
```

From root dir:
```bash
cmake -S. -Bbuild -DCMAKE_TOOLCHAIN_FILE="$DEVKITPRO/cmake/Wii.cmake" -DCMAKE_BUILD_TYPE=Release
cmake --build build
cmake --install build
```
