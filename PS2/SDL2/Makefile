EE_BIN = SDLTest.elf
EE_OBJS = main_sprites.o testutils.o
EE_LIBS = -L$(PS2SDK)/ports/lib -L$(PS2DEV)/gsKit/lib/\
		-Lmodules/ds34bt/ee/ -Lmodules/ds34usb/ee/ \
		-lSDL2 \
		-lpatches -lfileXio -lps2_drivers -ldebug -lmath3d -lpacket2 -ldma -ljpeg -lfreetype -lgskit_toolkit -lgskit \
		-ldmakit -lpng -lz -lmc -laudsrv -lelf-loader -lpad -lc \

EE_INCS += -I$(PS2DEV)/gsKit/include -I$(PS2SDK)/ports/include -I$(PS2SDK)/ports/include/freetype2 \
		   -I$(PS2SDK)/ports/include/zlib -I$(PS2SDK)/ports/include/SDL2

EE_CFLAGS = -I$(PS2DEV)/gsKit/include
EE_LDFLAGS = -L$(PS2DEV)/gsKit/lib

all: $(EE_BIN) audsrv.irx

audsrv.irx:
	cp $(PS2SDK)/iop/irx/audsrv.irx $@

clean:
	rm -f $(EE_BIN) $(EE_OBJS)

run: $(EE_BIN)
	ps2client execee host:$(EE_BIN)

reset:
	ps2client reset

include $(PS2SDK)/samples/Makefile.pref
include $(PS2SDK)/samples/Makefile.eeglobal


#The GS Enviroment Variable and path had to be removed, so that the pad script would function.