CXX		?= gcc
CXX_FLAGS 	:= -ansi -pedantic -Werror -Wall -O3 -std=c++17 -fext-numeric-literals -ffast-math -flto -lpitch_detection -lffts

all: test

test:
	$(CXX) ./$@*.cpp ./util.cpp -o $@ $(CXX_FLAGS) -lpthread -lgtest -lgtest_main

bench:
	$(CXX) ./$@.cpp ./util.cpp -o $@ $(CXX_FLAGS) -lpthread -lbenchmark

benchmem:
	$(CXX) ./$@.cpp ./util.cpp -o $@ $(CXX_FLAGS) -lgflags

clean:
	-rm bench benchmem test

.PHONY: clean
