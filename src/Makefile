INCDIR =./include
CC=gcc
CFLAGS=-I$(INCDIR) -fPIC -ftest-coverage
AR=ar

ODIR=obj
LDIR = ./lib

LIBS=-lm -linputlib -lgcov

_OBJ = main.o input1.o
OBJ = $(patsubst %,$(ODIR)/%,$(_OBJ))

LIBOBJ = inputlib.so

$(ODIR)/%.o: %.c
	mkdir -p $(ODIR)
	$(CC) -c -o $@ $< $(CFLAGS)

inputlib_link.c:
	ln -s inputlib.c inputlib_link.c

$(ODIR)/inputlib_link.o: inputlib_link.c
	mkdir -p $(ODIR)
	$(CC) -c -o $(ODIR)/inputlib_link.o inputlib_link.c $(CFLAGS)

asm:
	mkdir -p $(ODIR)
	$(CC) $(CFLAGS) -S inputasm.c -o inputasm.S
	$(CC) $(CFLAGS) -c inputasm.S -o $(ODIR)/inputasm.o

lib: $(ODIR)/inputlib_link.o
	mkdir -p $(LDIR)
	mv $(ODIR)/inputlib_link.o $(ODIR)/inputlib.o
	$(CC) -shared -fPIC -o $(LDIR)/libinputlib.so $(ODIR)/inputlib.o
	$(AR) rcs $(LDIR)/libinputlib.a $(ODIR)/inputlib.o

bin: $(OBJ)
	$(CC) -o testbillofmaterials $^ $(ODIR)/inputasm.o -L $(LDIR) $(LIBS)

all: asm lib bin

.PHONY: clean all

clean:
	rm -f $(ODIR)/*.o *~ core $(INCDIR)/*~ $(LDIR)/* testbillofmaterials
