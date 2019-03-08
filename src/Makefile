INCDIR =./include
CC=gcc
CFLAGS=-I$(INCDIR) -fPIC

ODIR=obj
LDIR = ./lib

LIBS=-lm -linputlib

_OBJ = main.o input1.o
OBJ = $(patsubst %,$(ODIR)/%,$(_OBJ))

LIBOBJ = inputlib.so

$(ODIR)/%.o: %.c
	mkdir -p $(ODIR)
	$(CC) -c -o $@ $< $(CFLAGS)

asm:
	mkdir -p $(ODIR)
	$(CC) $(CFLAGS) -S inputasm.c -o inputasm.S
	$(CC) $(CFLAGS) -c inputasm.S -o $(ODIR)/inputasm.o

lib: $(ODIR)/inputlib.o
	mkdir -p $(LDIR)
	$(CC) $(CFLAGS) -shared -fPIC -o $(LDIR)/libinputlib.so $(ODIR)/inputlib.o

bin: $(OBJ)
	$(CC) -o testbillofmaterials $^ $(ODIR)/inputasm.o $(CFLAGS) -L $(LDIR) $(LIBS)

all: asm lib bin

.PHONY: clean all

clean:
	rm -f $(ODIR)/*.o *~ core $(INCDIR)/*~ $(LDIR)/*