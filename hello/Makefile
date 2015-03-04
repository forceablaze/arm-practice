PROJECT = hello

FLASH_IMAGE = flash.bin
EXECUTABLE = $(PROJECT).elf
BIN_IMAGE = $(PROJECT).bin
HEX_IMAGE = $(PROJECT).hex

VERBOSE_COMPILE = yes

# Toolchain configurations
CROSS_COMPILE ?= arm-none-eabi-
CC = $(CROSS_COMPILE)gcc
LD = $(CROSS_COMPILE)ld
AS = $(CROSS_COMPILE)as
OBJCOPY = $(CROSS_COMPILE)objcopy
OBJDUMP = $(CROSS_COMPILE)objdump
SIZE = $(CROSS_COMPILE)size

LDFLAGS = -Ttext=0x0

OBJS += hello.o

all: $(FLASH_IMAGE)

$(FLASH_IMAGE): $(BIN_IMAGE)
	@dd if=/dev/zero of=$@ count=4096 bs=4096
	@dd if=$^ of=$@ conv=notrunc bs=4096

$(BIN_IMAGE): $(EXECUTABLE)
	$(OBJCOPY) -O binary $^ $@
	$(OBJCOPY) -O ihex $^ $(HEX_IMAGE)
	$(OBJDUMP) -h -S -D $(EXECUTABLE) > $(PROJECT).lst
	$(SIZE) $(EXECUTABLE)

$(EXECUTABLE): $(OBJS)
ifeq ($(VERBOSE_COMPILE),yes)
	$(LD) $(LDFLAGS) -o $@ $(OBJS)
else
	@echo LD $@
	@$(LD) $(LDFLAGS) -o $@ $(OBJS)
endif

%.o: %.S
ifeq ($(VERBOSE_COMPILE),yes)
	$(AS) -c $< -o $@
else
	@echo CC $<
	@$(AS) -c $< -o $@
endif

clean:
ifeq ($(VERBOSE_COMPILE),yes)
	rm -f $(OBJS)
	rm -f $(EXECUTABLE) $(BIN_IMAGE) $(HEX_IMAGE) $(FLASH_IMAGE)
	rm -f $(PROJECT).lst
else
	@rm -f $(OBJS)
	@rm -f $(EXECUTABLE) $(BIN_IMAGE) $(HEX_IMAGE) $(FLASH_IMAGE)
	@rm -f $(PROJECT).lst
	@echo Objects deleted.
endif

flash:
	@echo flash

.PHONY: clean
