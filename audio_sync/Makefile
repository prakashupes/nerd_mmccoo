

# for this makefile to work, you'll need these:
#
# sudo apt-get install libboost-all-dev
# sudo apt-get install libcgal-dev
#

# Add .d to Make's recognized suffixes.
SUFFIXES += .d

CXX = g++


CFLAGS = -g -I. -std=c++14 -I../dependencies/tinyxml2

# to get the dependencies, cd to ../dependencies and do make all
SNDFILE_PATH=../dependencies/libsndfile-1.0.28/install/lib/pkgconfig
CFLAGS += $(shell export PKG_CONFIG_PATH=$(SNDFILE_PATH) && pkg-config --cflags sndfile)
LFLAGS =  $(shell export PKG_CONFIG_PATH=$(SNDFILE_PATH) && pkg-config --libs sndfile)

FFTW_PATH=../dependencies/fftw-3.3.6-pl2/install/lib/pkgconfig
CFLAGS += $(shell export PKG_CONFIG_PATH=$(FFTW_PATH) && pkg-config --cflags fftw3)
LFLAGS += $(shell export PKG_CONFIG_PATH=$(FFTW_PATH) && pkg-config --libs fftw3)


TARGETS = xmltest sync_wavs varianceexample

all: $(TARGETS)


$(TARGETS): %: %.o ../dependencies/libsndfile-1.0.28/install/lib/libsndfile.a ../dependencies/fftw-3.3.6-pl2/install/lib/libfftw3.a ../dependencies/tinyxml2/libtinyxml2.a
	$(CXX) -o $@ $^  $(LFLAGS)


%.o: %.cxx
	$(CXX) -M $(CFLAGS) $< > $*.d
	$(CXX) -c $(CFLAGS) $< -o $@

clean:
	rm -f *.o $(TARGETS) *.d

test: $(TARGETS)
	./xmltest sample_video/sample.kdenlive


# pull in dependency info for *existing* .o files
# got this hint here: http://scottmcpeak.com/autodepend/autodepend.html
# except I'm using wildcard instead of a hard list of obj/d files.
# it's important for this to be after the first rule. Otherwise,
# make with no arguments will use one of these.
-include $(wildcard *.d)