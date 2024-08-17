# Homebrew Development Notes

My personal notes for using/building different libraries used in homebrew development.

## Tools/SDKs

## devkitPro (3DS, Wii, Gamecube, Wii U, Switch, GBA and a few more)

devkitPro comes with a lot of examples and libraries with the installation.

#### SDK Download:
https://github.com/devkitPro/installer/releases

---

### 3DS

#### Libraries:
citro2d - https://github.com/devkitPro/citro2d<br>
citro3d - https://github.com/devkitPro/citro3d<br>
SDL2 - https://github.com/libsdl-org/SDL<br>
- Building SDL2:
https://github.com/libsdl-org/SDL/blob/main/docs/README-n3ds.md

#### Emulator:
https://citra-emulator.com

---

### Wii & Gamecube
Almost same architecture so a lot of the libraries are compatible with both.

#### Libraries:
SDL2 - https://github.com/devkitPro/SDL/tree/ogc-sdl-2.28<br>
GRRLIB (2D/3D) - https://github.com/GRRLIB/GRRLIB<br>

#### Emulator:
https://dolphin-emu.org/

---

### Wii U
#### Libraries:
WUT - https://github.com/devkitPro/wut/releases<br>
SDL2 - https://github.com/devkitPro/SDL/tree/wiiu-sdl2-2.28<br>
RomFS - https://github.com/yawut/libromfs-wiiu<br>

#### Emulator:
https://cemu.info/

Resources:<br>
https://www.copetti.org/writings/consoles/wiiu/

---

### Switch
OpenGL 4 and OpenGL ES are supported, see devkitPro examples.

#### Libraries:
SDL2 - https://github.com/libsdl-org/SDL<br>

#### Emulator:
https://ryujinx.org/<br>
https://sudachi-emu.com/home/ (Yuzu fork)

---

## PSP

SDL2 is supported and many samples are available with the SDK.

#### SDK:
Follow the steps in https://pspdev.github.io/<br>
For Windows, WSL2 is recommended and there is a prebuilt SDK.<br>
https://learn.microsoft.com/en-us/windows/wsl/install

#### Windows (alternative):
This is is an older compiler/SDK and only comes with SDL1 out of the box.
https://darksectordds.github.io/html/MinimalistPSPSDK/index.html

#### Emulator:
https://www.ppsspp.org/

---

## PS Vita

SDL2 is supported and many samples are available with the SDK.

#### SDK:
https://vitasdk.org/

#### Emulator:
https://vita3k.org/

---

# Build info

Misc notes for myself.

## GRRLib 4.5.1 (Wii/GC)

Download:<br>
https://github.com/GRRLIB/GRRLIB

PNG support with libpngu is included in the source.
Optionally, remove TTF and JPEG support by edting these files:
- GRRLIB__lib.h - `#ifdef` the TTF functions
- GRRLIB_ttf.c - `#ifdef` the file
- GRRLIB_core.c - `#ifdef` the calls to `GRRLIB_ExitTTF and GRRLIB_InitTTF`
- GRRLIB_texEdit.c - `#ifdef GRRLIB_LoadTextureJPG and GRRLIB_LoadTextureJPGEx`<br>
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
---

## SDL 2.28 (Wii/GC)

Download:<br>
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
