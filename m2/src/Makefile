#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM ?= HOST 
MAP=c1m2.map
OUTPUT = c1m2.out
OBJS = $(SOURCES:.c=.o)
# Architectures Specific Flags

# Compiler Flags and Defines

CC = gcc
LD = gcc
LDFLAGS = -Wl,-Map=$(MAP)
CFLAGS = -Wall -Werror -g -std=c99 -O0
CPPFLAGS = $(INCLUDES)
	
ifeq ($(PLATFORM), HOST)
    CPPFLAGS += -DHOST
else ifeq ($(PLATFORM), MSP432)
    CC = arm-none-eabi-gcc
    LD = arm-none-eabi-ld
    LDFLAGS += -T msp432p401r.lds
    CPU = -mcpu=cortex-m4
    ARCH = -mthumb -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16
    SPECS = --specs=nosys.specs
    CFLAGS += $(CPU) $(ARCH) $(SPECS)
    CPPFLAGS += -DMSP432
else
    $(error "Platform provided is not supported in this Build System")
endif

# Compiler Flags and Defines
LDFLAGS = -Wl,-Map=c1m2.map -T
CFLAGS = -Wall -Werror -g -std=c99 -O0
CPPFLAGS = $(INCLUDES)

.PHONY: compile-all build clean

%.i : %.c
	$(CC) $(CPPFLAGS) $(CFLAGS) -E $< -o $@

%.asm : %.c
	$(CC) $(CPPFLAGS) $(CFLAGS) -S $< -o $@

%.o : %.c 
	$(CC) $(CPPFLAGS) $(CFLAGS) -S $< -o $@

compile-all : $(OBJS)
	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@


$(OUTPUT): $(OBJS)
	$(CC) $(CPPFLAGS) $(CFLAGS) $(OBJS) $(LDFLAGS) $(LINKER_FILE) -o $(OUTPUT)
	size $(OUTPUT)

build: $(OUTPUT)




clean:
	rm -f *.o *.i *.asm *.d $(OUTPUT) $(MAP)
