CROSS= arm-unknown-eabi-

CC= $(CROSS)gcc
LD= $(CROSS)ld
OBJCOPY= $(CROSS)objcopy
OBJDUMP= $(CROSS)objdump
SIZE= $(CROSS)size
READELF= $(CROSS)readelf

# libgcc.a
LIBS = -L/home/jitomesky/cross/rpi/arm-unknown-eabi/lib/gcc/arm-unknown-eabi/4.8.1/
# newlib
LIBS += -L/home/jitomesky/cross/rpi/arm-unknown-eabi/arm-unknown-eabi/lib/
STARTUP= startup.o
OBJS= main.o

# systemcall
OBJS += syscalls.o

# RaspberryPi library
OBJS+= rpi_lib/rpi_init.o rpi_lib/gpio/gpio.o rpi_lib/timer/timer.o rpi_lib/delay/delay.o
OBJS+= rpi_lib/serial/serial.o
OBJS+= rpi_lib/bss/clearbss.o
OBJS+= rpi_lib/spi/spi0.o
OBJS+= rpi_lib/i2c/i2c1.o

# device
OBJS+= rpi_lib/device/ADT7310/ADT7310.o
OBJS+= rpi_lib/device/AQM0802/AQM0802.o

CFLAGS= -mfloat-abi=soft -mlittle-endian
CFLAGS+= -nostartfiles
CFLAGS+= -ffreestanding
CFLAGS+= -O0 -g3

########################
all:	rpi-micon.img

rpi-micon.elf:	$(STARTUP) $(OBJS)
	$(LD) -static -nostartfiles -T rpiMicon.lds $^ -o $@ $(LIBS) -lc -lgcc
	$(OBJDUMP) -D rpi-micon.elf > rpi-micon.disas
	$(SIZE) rpi-micon.elf > rpi-micon.size
	$(READELF) -a rpi-micon.elf > rpi-micon.readelf

.SUFFIXES: .elf .img

.elf.img:
	$(OBJCOPY) -O binary $< $@
.c.o:
	$(CC) $(CFLAGS) -c $< -o $@
.S.o:
	$(CC) $(CFLAGS) -c $< -o $@

clean::
	$(RM) -f *.o *.img *.elf */*.o */*/*.o 
	$(RM) -f tags *~

tags::
	ctags *.[chS]
