CXX     = g++
AOCXX   = aoc
CXXFLAGS= -std=c++11 -g
BOARD 	= s5_ref
AOCXXFLAGS  = -march=emulator -board=$(BOARD) 
# OpenCL compile and link flags.
AOCL_COMPILE_CONFIG := $(shell aocl compile-config )
AOCL_LINK_CONFIG := $(shell aocl link-config )

.PHONY: all clean

all: report

report: Example2.cl 
	$(AOCXX) -v -board=$(BOARD) -c $< -report
	rm -rf reports || true
	cp -r Example2/reports .

report_buffered: Example2_Buffered.cl
	$(AOCXX) -v -board=$(BOARD) -c $< -report
	rm -rf reports_buffered || true
	cp -r Example2_Buffered/reports ./reports_buffered


emulated: Example2.cl
	$(AOCXX) $(AOCXXFLAGS) -o EmulatedExample2.aocx Example2.cl 

test: Test.cpp Example2.cl
	$(CXX) Test.cpp $(CXXFLAGS) -I../../intel_ocl_utils $(AOCL_COMPILE_CONFIG) $(AOCL_LINK_CONFIG) -o TestExample2

run_test: test emulated
	LD_LIBRARY_PATH=$(INTELFPGAOCLSDKROOT)/board/s5_ref/linux64/lib:$(INTELFPGAOCLSDKROOT)/host/linux64/lib:$(LD_LIBRARY_PATH) \
	env CL_CONTEXT_EMULATOR_DEVICE_INTELFPGA=1 ./TestExample2 EmulatedExample2.aocx

clean:
	rm -rf Example2*/ || true
	rm -rf EmulatedExample2/ || true
	rm -f *Example2*.ao* || true
	rm -rf Example2*.temp || true
	rm -rf report* || true
	rm -rf TestExample2 || true



