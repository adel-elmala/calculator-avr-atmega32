SRC = 	main.c\
		APP/application.c\
		APP/calculator.c\
		MCAL/DIO/dio.c\
		MCAL/EXT_INT/External_INT.c\
		HAL/KeyPad/Keypad.c\
		HAL/LCD/LCD.c\


OBJ = $(subst .c,.o, $(SRC))

MCU = atmega32
FLASHER = usbasp
F_CPU = 1000000UL 

CFLAGS = -Og -ggdb -Wall -mmcu=$(MCU) -DF_CPU=$(F_CPU)

INCLUDE  = -I./utils
INCLUDE += -I./MCAL
INCLUDE += -I./HAL
INCLUDE += -I/lib/avr/include

LD_AVR = -L/lib/avr/lib/avr5 -latmega32a

all: compile build flash

	
compile:$(OBJ)
	
	@avr-gcc $(CFLAGS) $(INCLUDE) $(LD_AVR) $(OBJ)  -o ./bin/main.elf
%.o:%.c
	@echo "compiling $^"
	@avr-gcc $(CFLAGS) $(INCLUDE) $(LD_AVR) $^ -c  -o $@ 

build:
	@echo " building Hex file ... "

	@avr-objcopy  -j .text -j .data -O ihex ./bin/main.elf ./bin/main.hex

flash:build
	avrdude -c $(FLASHER) -p $(MCU)  -U flash:w:./bin/main.hex

clean:
	$(foreach file,$(OBJ),$(shell rm $(file)))


sim:
	simavr -g -m $(MCU) ./bin/main.elf

