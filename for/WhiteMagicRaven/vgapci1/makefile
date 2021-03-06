# Open Watcom makefile.
# Do NOT use with GNU make.
#
# make sure Open Watcom is in your environment block and
# tools are available.
DOSLIB_REL=../../../doslib
CFLAGS=-i$(DOSLIB_REL) -e=2 -zq -mf -bt=dos -oilrtfm -wx -fp3 -3r -dTARGET_MSDOS=32 -dMSDOS=1 -dTARGET86=3 -DMMODE=f -q -fo=.obj

# for simplicity we only focus on 32-bit MS-DOS
TARGET=test.exe

# default ALL target
all: $(TARGET)

# we need to know where "HERE" is
HERE = $+$(%cwd)$-

# DOSLIB libraries (386f target)
LIBCPU=$(DOSLIB_REL)/hw/cpu/dos386f/cpu.lib

# how to build it
$(LIBCPU):
	@cd $(DOSLIB_REL)/hw/cpu
	./make.sh build lib dos386f
	@cd $(HERE)

# PCI libraries (386f target)
LIBPCI=$(DOSLIB_REL)/hw/pci/dos386f/pci.lib

# how to build it
$(LIBPCI):
	@cd $(DOSLIB_REL)/hw/pci
	./make.sh build lib dos386f
	@cd $(HERE)

# how to build test.exe
test.exe: $(LIBCPU) $(LIBPCI) test.obj
	%write tmp.cmd option quiet
	%write tmp.cmd system dos4g
	%write tmp.cmd library $(LIBCPU)
	%write tmp.cmd library $(LIBPCI)
	%write tmp.cmd file test.obj
	%write tmp.cmd name test.exe
	%write tmp.cmd option map=test.map
	@wlink @tmp.cmd

test.obj: test.c
	%write tmp.cmd $(CFLAGS) $[@
	@wcc386 @tmp.cmd

# NOTE: we assume Linux host
clean: .symbolic
	rm -fv $(TARGET) *.obj *.lib *.map tmp.cmd

