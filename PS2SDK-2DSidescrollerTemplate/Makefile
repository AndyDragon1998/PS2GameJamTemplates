EE_BIN = ProjectName.elf
EE_BIN_COMPRESSED = compressed_$(EE_BIN)

EE_OBJS = main.o \
	Data/pad.o Data/stateManager.o Data/introState.o Data/menuState.o Data/gameState.o Data/overState.o Data/soundEffects.o Data/musicManager.o Data/textToGraphics.o audsrv.o libsd.o padman.o
	
EE_LIBS = -L$(PS2SDK)/ports/lib -L$(PS2DEV)/gsKit/lib/  -lpatches -ldebug -lmath3d -ljpeg -lfreetype -lgskit_toolkit -lgskit -ldmakit -lpng -lz -lmc -lelf-loader -lc

EE_INCS += -I$(PS2DEV)/gsKit/include -I$(PS2SDK)/ports/include -I$(PS2SDK)/ports/include/freetype2 -I$(PS2SDK)/ports/include/zlib
#EE_CFLAGS is passed to GCC when compiling
#EE_LDFLAGS is passed to GCC/ld when linking

EE_CFLAGS = -I$(PS2DEV)/gsKit/include
EE_LDFLAGS = -L$(PS2DEV)/gsKit/lib

EE_LDFLAGS += -L$(PS2SDK)/ports/lib
EE_LIBS += -lps2_drivers -I$(PS2SDK)/ports/include

BIN2S = @bin2s


all: $(EE_BIN)

strip: $(EE_BIN)
	echo "Stripping $(EE_BIN)..."
	$(EE_STRIP) $(EE_BIN)
	
compress: strip
	echo "Compressing to $(EE_BIN_COMPRESSED)...\n"
	ps2-packer $(EE_BIN) $(EE_BIN_COMPRESSED) > /dev/null

clean:
	rm -f $(EE_BIN) $(EE_BIN_COMPRESSED) $(EE_OBJS)

run: $(EE_BIN)
	ps2client execee host:$(EE_BIN)

reset:
	ps2client reset

#The GS Enviroment Variable and path had to be removed, so that the pad script would function.

#embedded stuff
audsrv.s: $(PS2SDK)/iop/irx/audsrv.irx
	echo "Embedding AUDSRV Driver..."
	$(BIN2S) $< $@ audsrv_irx

padman.s: $(PS2SDK)/iop/irx/padman.irx
	echo "Embedding PADMAN Driver..."
	$(BIN2S) $< $@ padman_irx
	
libsd.s: $(PS2SDK)/iop/irx/libsd.irx
	echo "Embedding LIBSD Driver..."
	$(BIN2S) $< $@ libsd_irx

#includes
include $(PS2SDK)/samples/Makefile.pref
include $(PS2SDK)/samples/Makefile.eeglobal
