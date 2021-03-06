#############################
# librairies make file
#############################
NAME=stm8ef_disco
SDAS=sdasstm8
SDCC=sdcc
SDAR=sdar
CFLAGS=-mstm8 -lstm8 -I../inc
INC=../inc/
INCLUDES=$(INC)stm8s105.inc
BUILD=build/
SYMBOLS=$(OBJECTS:.rel=.sym)
LISTS=$(OBJECTS:.rel=.lst)
FLASH=stm8flash
BOARD=stm8s105c6
PROGRAMMER=stlinkv2

.PHONY: all

all: clean $(NAME).rel $(NAME).ihx 

$(NAME).rel:
	@echo
	@echo "**********************"
	@echo "compiling $(NAME)       "
	@echo "**********************"
	$(SDAS) -g -l -o $(BUILD)$(NAME).rel $(NAME).asm

$(NAME).ihx: $(NAME).rel $(INC)/$(INCLUDES)
	$(SDCC) $(CFLAGS) -Wl-u -o $(BUILD)$(NAME).ihx  $(BUILD)$(NAME).rel


.PHONY: clean 
clean: build
	@echo
	@echo "***************"
	@echo "cleaning files"
	@echo "***************"
	rm -f $(BUILD)*

clear_eevars:
	@echo
	@echo "***************"
	@echo "erase EEPROM"
	@echo "***************"
	$(FLASH) -c $(PROGRAMMER) -p $(BOARD) -s eeprom -b 128 -w /dev/zero

build:
	mkdir build

.PHONY: test 

flash: clear_eevars $(LIB)
	@echo
	@echo "***************"
	@echo "flashing device"
	@echo "***************"
	$(FLASH) -c $(PROGRAMMER) -p $(BOARD) -w $(BUILD)$(NAME).ihx 

eforth: $(NAME).asm 
	-rm build/* 
	$(SDAS) -g -l -o $(BUILD)$(NAME).rel $(NAME).asm 
	$(SDCC) $(CFLAGS) -Wl-u -o $(BUILD)$(NAME).ihx  $(BUILD)$(NAME).rel
	$(FLASH) -c $(PROGRAMMER) -p $(BOARD) -w $(BUILD)$(NAME).ihx

eevars_read:
	$(FLASH) -c $(PROGRAMMER) -p $(BOARD) -s eeprom -b 16 -r eevars.bin
	@hexdump -C eevars.bin 
