# This is a makefile that builds the XXX morphological parser
# *****************************************************************
# This is a dummy file. XXX refers to the name of the language, tel to the 
# ISO code of the language, for use in file names.

# This is a preliminary file that builds a XXX parser based upon
# xfst. You may also comment in the ## lines and get twolc instead.

# Here we build the final generator , an inverted transducer of the analyzer.
# It is dependent upon tel.save

XFST = xfst
LEXC = lexc
TWOLC = twolc

ifeq (victorio.uit.no, $(shell hostname))
XFST = /opt/sami/xerox/c-fsm/ix86-linux2.6-gcc3.4/bin/fst -utf8
LEXC = /opt/sami/xerox/bin/lexc -utf8
TWOLC = /opt/sami/xerox/bin/twolc -utf8
endif

scripts=$(GTHOME)/gt/script

LEX = tel-lex.txt 
#	  noun-tel-morph.txt \
#	  noun-tel-lex.txt \
#	  adj-tel-lex.txt \
#	  verb-tel-lex.txt \
#	  verb-tel-morph.txt \
#	  pron-tel-lex.txt \
#	  adv-tel-lex.txt \
#	  pp-tel-lex.txt \
#	  punct-tel-lex.txt

all: itel.fst

itel.fst: ../bin/itel.fst
../bin/itel.fst: ../bin/tel.fst
	@echo
	@echo "*** Building the inverse itel.fst ***"
	@echo
	@printf "load ../bin/tel.fst \n\
	invert net \n\
	save stack ../bin/itel.fst \n\
	quit \n" > ../../tmp/itel-fst-script
	@(XFST) < ../../tmp/itel-fst-script
	@rm -f ../../tmp/itel-fst-script



# The first goal is to build tel-lex.save
# This goal depends on a bunch of lexicon files

tel.fst: ../bin/tel.fst
../bin/tel.fst: ../bin/twol-tel.bin $(LEX)
	@echo
	@echo "*** Building tel-lex.save ***" ;
	@echo
	printf "compile-source $(LEX) \n\
	read-rules ../bin/twol-tel.bin \n\
	compose-result \n\
	save-result ../bin/tel.fst \n\
	quit \n" > ../../tmp/tel-lex-save-script
	$(LEXC) < ../../tmp/tel-lex-save-script
	rm -f ../../tmp/tel-lex-save-script


# The fourth goal is to build twol-tel.bin
# This goal depends on twol-tel.txt
twol-tel.bin: ../bin/twol-tel.bin
../bin/twol-tel.bin: twol-tel.txt
	@echo
	@echo "*** Building twol-tel.bin ***" ;
	@echo
	@printf "read-grammar twol-tel.txt \n\
	compile \n\
	save-binary ../bin/twol-tel.bin \n\
	quit \n" > ../tmp/twol-tel-script
	$(TWOLC)  < ../tmp/twol-tel-script
	@rm -f ../tmp/twol-tel-script
	
clean:
	@rm -f ../bin/*.bin ../bin/*.fst ../bin/*.save

