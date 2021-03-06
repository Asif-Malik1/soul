# Choose your word size
WSIZE = 64
OPTLEVEL = 1
# WSIZE = 32
CC = gcc
GFLAGS = -pg 
INC = ../include
MCFLAGS = -g -Wall -O$(OPTLEVEL) -I. -I$(INC) -msse4 -fno-inline -fno-optimize-sibling-calls -lrt -mfpmath=sse
CFLAGS = $(MCFLAGS) -m$(WSIZE)
F386 = -march=i386
F686 = -march=i686
LIB = ../lib
LIBCSAPP = $(LIB)/libcsapp$(WSIZE).a
GETASM = ../getasmfun.pl
RUNPOS = 0.8
MULTIRUN = ./multirun.pl -n 50 -p $(RUNPOS)
BXTR = ./bxtr.pl
VXTR = ./vxtr.pl
TABULATE = ./tabulate.pl
# Choose your machine
# MACHINE = p4
# MACHINE = athlon
MACHINE=machine
RESULTS=results

PROGS = isbench\
	ipbench\
	fsbench\
	fpbench\
	dsbench\
	dpbench\
	fpb.64s\
	fpb-O2.64s\
	div\
	dmult\
	lower\
	list\
	test_cpe\
	copy\
	twiddle.o\
	opt-meas\
	swap.o\
	func.o\
	fragments.o\
	deref.o\


#	lsbench\
#	lpbench\
#	csbench\
#	cpbench\
#	mem \


all: $(PROGS)

bench: isbench ipbench fsbench fpbench dsbench dpbench isb.32s isb.64s fpb.64s fpb-O2.64s test_cpe.64s

fragments.o: fragments.c
	$(CC) -c -DSUM -DINT fragments.c

isbench:   vec.h combine.h combine.c benchmark.c \
	vec.c $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DINT -o isbench benchmark.c combine.c vec.c \
	  $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DINT -S combine.c -o isb.s
	$(GETASM) < isb.s > isb-s.txt
	$(MULTIRUN) -c ./isbench > $(RESULTS)/isb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/isb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/isb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/isb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/isb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

ipbench: $(LIBCSAPP)   vec.h combine.h combine.c benchmark.c vec.c
	$(CC) $(CFLAGS) -DPROD -DINT -o ipbench benchmark.c combine.c vec.c \
	 $(LIBCSAPP) 
	$(MULTIRUN) -c  ./ipbench > $(RESULTS)/ipb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/ipb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/ipb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/ipb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/ipb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

ipb.32s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DPROD -DINT -m32 -S combine.c -o ipb.s
	$(GETASM) < ipb.s > ipb.32s

ipb.64s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DPROD -DINT -m64 -S combine.c -o ipb.s
	$(GETASM) < ipb.s > ipb.64s
	cp -p combine.c ipb.c

isb.32s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DPROD -DINT -m32 -S combine.c -o isb.s
	$(GETASM) < isb.s > isb.32s

isb.64s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DPROD -DINT -m64 -S combine.c -o isb.s
	$(GETASM) < isb.s > isb.64s
	cp -p combine.c isb.c

fsbench:   vec.h combine.h combine.c benchmark.c \
	vec.c $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DFLOAT -o fsbench benchmark.c combine.c vec.c \
	  $(LIBCSAPP)
	#
	$(CC) $(CFLAGS) -DSUM -DFLOAT -S combine.c -o fsb.s
	$(MULTIRUN) -c ./fsbench > $(RESULTS)/fsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/fsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/fsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/fsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/fsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

fsb.32s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DSUM -DFLOAT -m32 -S combine.c -o fsb.s
	$(GETASM) < fsb.s > fsb.32s

fsb.64s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DSUM -DFLOAT -m64 -S combine.c -o fsb.s
	$(GETASM) < fsb.s > fsb.64s
	cp -p combine.c fsb.c

fpbench: $(LIBCSAPP)   vec.h combine.h combine.c benchmark.c vec.c
	$(CC) $(CFLAGS) -DPROD -DFLOAT -o fpbench benchmark.c combine.c vec.c \
	 $(LIBCSAPP) 
	#
	$(CC) $(CFLAGS) -DPROD -DFLOAT -S combine.c -o fpb.s
	$(MULTIRUN) -c  ./fpbench > $(RESULTS)/fpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/fpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/fpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/fpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/fpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

fpb.32s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DPROD -DFLOAT -m32 -S combine.c -o fpb.s
	$(GETASM) < fpb.s > fpb.32s
	cp -p combine.c fpb.c

fpb.64s: vec.h combine.h combine.c
	$(CC) $(MCFLAGS) -DPROD -DFLOAT -m64 -S combine.c -o fpb.s
	$(GETASM) < fpb.s > fpb.64s
	cp -p combine.c fpb.c

fpb-O2.64s: vec.h combine.h combine.c
	$(CC) -O2 -DPROD -DFLOAT -m64 -S combine.c -o fpb.s
	$(GETASM) < fpb.s > fpb-O2.64s
	cp -p combine.c fpb-O2.c

dsb-O2.64s: vec.h combine.h combine.c
	$(CC) -O2 -DSUM -DDOUBLE -m64 -S combine.c -o dsb.s
	$(GETASM) < dsb.s > dsb-O2.64s
	cp -p combine.c dsb-O2.c

dsbench:   vec.h combine.h combine.c benchmark.c \
	vec.c $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DDOUBLE -o dsbench benchmark.c combine.c vec.c \
	  $(LIBCSAPP)
	#
	$(CC) $(CFLAGS) -DSUM -DDOUBLE -S combine.c -o dsb.s
	$(MULTIRUN) -c ./dsbench > $(RESULTS)/dsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/dsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/dsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/dsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/dsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

dpbench: $(LIBCSAPP)   vec.h combine.h combine.c benchmark.c vec.c
	$(CC) $(CFLAGS) -DPROD -DDOUBLE -o dpbench benchmark.c combine.c vec.c \
	 $(LIBCSAPP) 
	#
	$(CC) $(CFLAGS) -DPROD -DDOUBLE -S combine.c -o dpb.s
	$(MULTIRUN) -c ./dpbench > $(RESULTS)/dpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/dpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/dpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/dpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/dpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab
	$(TABULATE) -w $(WSIZE) -d $(RESULTS) -m $(MACHINE)-O$(OPTLEVEL) > $(RESULTS)/summary-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

lsbench:   vec.h combine.h combine.c benchmark.c \
	vec.c $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DLONG -o lsbench benchmark.c combine.c vec.c \
	  $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DLONG -S combine.c -o lsb.s
	$(GETASM) < lsb.s > lsb-s.txt
	$(MULTIRUN) -c ./lsbench > $(RESULTS)/lsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/lsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/lsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/lsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/lsb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

lpbench: $(LIBCSAPP)   vec.h combine.h combine.c benchmark.c vec.c
	$(CC) $(CFLAGS) -DPROD -DLONG -o lpbench benchmark.c combine.c vec.c \
	 $(LIBCSAPP) 
	$(CC) $(CFLAGS) -DPROD -DLONG -S combine.c -o lpb.s
	$(MULTIRUN) -c  ./lpbench > $(RESULTS)/lpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/lpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/lpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/lpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/lpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

csbench:   vec.h combine.h combine.c benchmark.c \
	vec.c $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DCHAR -o csbench benchmark.c combine.c vec.c \
	  $(LIBCSAPP)
	$(CC) $(CFLAGS) -DSUM -DCHAR -S combine.c -o csb.s
	$(GETASM) < csb.s > csb-s.txt
	$(MULTIRUN) -c ./csbench > $(RESULTS)/csb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/csb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/csb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/csb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/csb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

cpbench: $(LIBCSAPP)   vec.h combine.h combine.c benchmark.c vec.c
	$(CC) $(CFLAGS) -DPROD -DCHAR -o cpbench benchmark.c combine.c vec.c \
	 $(LIBCSAPP) 
	$(CC) $(CFLAGS) -DPROD -DCHAR -S combine.c -o cpb.s
	$(MULTIRUN) -c  ./cpbench > $(RESULTS)/cpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(BXTR) < $(RESULTS)/cpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/cpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).tab
	$(VXTR) < $(RESULTS)/cpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt > $(RESULTS)/cpb-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).vtab

bench-ncpe:	 $(LIBCSAPP)   vec.h combine.h combine.c benchmark-ncpe.c vec.c
	$(CC) $(CFLAGS) -DSUM -DINT -DUNROLL_ONLY -DCNT=31 -o bench-ncpe31 benchmark-ncpe.c combine.c vec.c $(LIBCSAPP) 
	$(CC) $(CFLAGS) -DSUM -DINT -DUNROLL_ONLY -DCNT=8192 -o bench-ncpe8192 benchmark-ncpe.c combine.c vec.c $(LIBCSAPP) 
	$(MULTIRUN) -c ./bench-ncpe31 > $(RESULTS)/ncpe-31-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(MULTIRUN) -c  ./bench-ncpe8192 > $(RESULTS)/ncpe-8192-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(TABULATE) -r ncpe-31:ncpe-8192 -w $(WSIZE) -d $(RESULTS) -m $(MACHINE)-O$(OPTLEVEL) > $(RESULTS)/summary-ncpe-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

div: div.c
	$(CC) $(CFLAGS)  div.c $(LIBCSAPP) -o div
	$(CC) $(CFLAGS)  -S div.c
	sleep 1 ; ./div > $(RESULTS)/div-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

lower: lower.c
	$(CC) $(CFLAGS)  lower.c $(LIBCSAPP) -o lower
	$(CC) $(CFLAGS)  -S lower.c

overhead: overhead.c
	$(CC) $(CFLAGS)  overhead.c $(LIBCSAPP) -o overhead

lower-bench:
	sleep 1 ; ./lower > $(RESULTS)/lower-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	sleep 1 ; ./lower 1 > $(RESULTS)/lower-exp-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

dmult: dmult.c
	$(CC) $(CFLAGS)  dmult.c $(LIBCSAPP) -o dmult
	$(CC) $(CFLAGS)  -S dmult.c
	sleep 1 ; ./dmult > $(RESULTS)/dmult-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

mem: mem.c
	$(CC) $(CFLAGS)  mem.c $(LIBCSAPP) -o mem
	$(CC) $(CFLAGS)  -S mem.c
	sleep 1 ; ./mem > $(RESULTS)/mem-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

list: list.c
	$(CC) $(CFLAGS)  list.c $(LIBCSAPP) -o list
	$(CC) $(CFLAGS)  -S list.c
	$(GETASM) < list.s > list.$(WSIZE)s
	sleep 1 ; ./list > $(RESULTS)/list-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

fact: fact.c
	$(CC) $(CFLAGS)  fact.c $(LIBCSAPP) -o fact
	$(CC) $(CFLAGS)  -S fact.c
	$(GETASM) < fact.s > fact.$(WSIZE)s

fact-opt: fact.c
	$(CC) -Wall -O$(OPTLEVEL) -I. -I$(INC) -lrt  fact.c $(LIBCSAPP) -o fact-opt
	$(CC) -Wall -O$(OPTLEVEL) -I. -I$(INC) -S fact.c -o fact-opt.s
	$(GETASM) < fact-opt.s > fact-opt.$(WSIZE)s

test_cpe: test_cpe.c $(LIBCSAPP)
	$(CC) $(CFLAGS)  test_cpe.c $(LIBCSAPP) -o test_cpe
	sleep 1 ; ./test_cpe -r -l 200 -s 30 -b 0.2 > $(RESULTS)/cpe-u1-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	sleep 1 ; ./test_cpe -r -u -l 200 -s 30 -b 0.2 > $(RESULTS)/cpe-u2-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt

test_cpe.64s: test_cpe.c
	$(CC) $(MCFLAGS) -m64 -S test_cpe.c 
	$(GETASM) < test_cpe.s > test_cpe.64s

cond-eg: $(LIBCSAPP) cond-eg.c
	$(CC) $(CFLAGS)  cond-eg.c $(LIBCSAPP) -o cond-eg
	sleep 1 ; ./cond-eg > $(RESULTS)/cond-eg-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(CC) $(CFLAGS) -S cond-eg.c 
	$(GETASM) < cond-eg.s > cond-eg-s.txt

merge: $(LIBCSAPP) merge.c
	$(CC) $(CFLAGS)  merge.c $(LIBCSAPP) -o merge
	sleep 1 ; ./merge > $(RESULTS)/merge-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(CC) $(CFLAGS) -S merge.c 
	$(GETASM) < merge.s > merge-s.txt


copy: $(LIBCSAPP) copy.c
	$(CC) $(CFLAGS)  copy.c $(LIBCSAPP) -o copy
	sleep 1 ; ./copy > $(RESULTS)/copy-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(CC) $(CFLAGS) -S copy.c 
	$(GETASM) < copy.s > copy-s.txt

opt-meas: $(LIBCSAPP) opt-meas.c
	$(CC) $(CFLAGS)  opt-meas.c $(LIBCSAPP) -o opt-meas
	sleep 1 ; ./opt-meas > $(RESULTS)/opt-meas-$(MACHINE)-O$(OPTLEVEL)-$(WSIZE).txt
	$(CC) $(CFLAGS) -S opt-meas.c 
	$(GETASM) < opt-meas.s > opt-meas-s.txt

dictionary: dictionary.c options.c
	$(CC) $(CFLAGS) dictionary.c options.c -o dictionary
	$(CC) $(CFLAGS) $(GFLAGS) dictionary.c options.c -o dictionary-pg
	$(CC) $(CFLAGS) -S dictionary.c

copy2d: copy2d.c $(LIBCSAPP)
	$(CC) -O2 -Wall -I$(INC) copy2d.c $(LIBCSAPP) -o copy2d

clean:
	rm -f $(PROGS) *.o *~ *.s *.exe



