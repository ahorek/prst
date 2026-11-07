# Makefile for macOS on Apple Silicon (arm64)

EXE       = prst
LIB_GWNUM = ../../framework/gwnum/mac-arm64/gwnum.a

COMPOBJS_COMMON = md5.o arithmetic.o group.o giant.o lucas.o config.o inputnum.o integer.o logging.o file.o container.o task.o exp.o fermat.o order.o pocklington.o lucasmul.o morrison.o proof.o testing.o support.o batch.o abc_parser.o
COMPOBJS        = $(COMPOBJS_COMMON) prst.o

# Oldest macOS that runs on Apple Silicon
MINVER ?= 11.0
export MACOSX_DEPLOYMENT_TARGET := $(MINVER)

# GMP built for $(MINVER); defaults to Homebrew's copy
GMP_PREFIX ?= $(shell brew --prefix gmp 2>/dev/null || echo /opt/homebrew)

CC  = clang
CXX = clang++

VPATH = ..:../../framework:../../framework/arithmetic

LFLAGS =
LIBS2  = -lm -lpthread $(GMP_PREFIX)/lib/libgmp.a  # static link to gmp

# -DX86_64 matches the linux-aarch64 build with gwnum_c1
COMMON_CFLAGS = -arch arm64 -mmacosx-version-min=$(MINVER) \
                -I$(GMP_PREFIX)/include -I.. -I../../framework -I../../framework/arithmetic -I../../framework/gwnum \
                -DGMP -DX86_64 -O2 -g -Wall -Wextra

CFLAGS   += -std=c99
CXXFLAGS += -std=gnu++17 -stdlib=libc++
# the code is too noisy for all warnings
CXXFLAGS += -Wno-sign-compare -Wno-unused-parameter -Wno-unused-private-field

COMMON_LDFLAGS = -arch arm64 -stdlib=libc++ -mmacosx-version-min=$(MINVER)

all: $(EXE)

%.o: %.c
	$(CC)  $(CPPFLAGS) $(COMMON_CFLAGS) $(CFLAGS)   -c $< -o $@

%.o: %.cpp
	$(CXX) $(CPPFLAGS) $(COMMON_CFLAGS) $(CXXFLAGS) -c $< -o $@

$(EXE): $(COMPOBJS)
	$(CXX) $(LFLAGS) $(COMMON_LDFLAGS) -o $@ $(COMPOBJS) $(LIB_GWNUM) $(LIBS2)

clean::
	rm -f $(EXE) $(COMPOBJS)