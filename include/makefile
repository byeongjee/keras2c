# Path to the WASI SDK installation
WASI_SDK_PATH=/Users/byeongje/wasm/wasi-sdk-22.0

# Use the clang compiler from the WASI SDK
CC=$(WASI_SDK_PATH)/bin/clang
# Use the wasm-ld linker from the WASI SDK
LD=$(WASI_SDK_PATH)/bin/wasm-ld

# WASI sysroot path
WASI_SYSROOT=$(WASI_SDK_PATH)/share/wasi-sysroot

# Optimization flags
OPTFLAGS = -O3

# Compiler flags
ifeq ($(origin CI),undefined)
	CCFLAGS = $(OPTFLAGS) -std=c99 -I./include/
else
	CCFLAGS = -g -Og -std=c99 --coverage -I./include/
endif

# Object files to compile
OBJ = \
	k2c_activations.o \
	k2c_convolution_layers.o \
	k2c_core_layers.o \
	k2c_embedding_layers.o \
	k2c_helper_functions.o \
	k2c_merge_layers.o \
	k2c_normalization_layers.o \
	k2c_pooling_layers.o \
	k2c_recurrent_layers.o

# Header dependencies
DEPS = \
	k2c_include.h \
	k2c_tensor_include.h

# Default target
all: libkeras2c.o

# Building the WASM library
libkeras2c.o: $(OBJ)
	$(LD) --relocatable -o libkeras2c.o $(OBJ) 

# Compiling individual object files
%.o: %.c $(DEPS)
	$(CC) -c -o $@ $< $(CCFLAGS)

# Clean target
clean:
	rm -f *.o *.wasm