#
# Copyright 2017-2018 NVIDIA Corporation. All rights reserved
# 
INCLUDES=-I../../include -I../extensions/include/profilerhost_util -I../extensions/include/c_util

ifndef OS
 OS   := $(shell uname)
 HOST_ARCH := $(shell uname -m)
endif

ifeq ($(OS),Windows_NT)
    LIB_PATH ?= ..\..\lib64
else
    LIB_PATH ?= ../../lib64
endif

ifeq ($(OS),Windows_NT)
    LIBS= -lcuda -L $(LIB_PATH) -lcupti -lnvperf_host -lnvperf_target -L ..\extensions\src\profilerhost_util -lprofilerHostUtil
    OBJ = obj
    LIBEXT = lib
    LIBPREFIX =
    BINEXT = .exe
else
    ifeq ($(OS), Darwin)
        export DYLD_LIBRARY_PATH := $(DYLD_LIBRARY_PATH):$(LIB_PATH)
        LIBS= -Xlinker -framework -Xlinker cuda -L $(LIB_PATH) -lcupti -lnvperf_host -lnvperf_target -L ../extensions/src/profilerhost_util -lprofilerHostUtil
    else
        export LD_LIBRARY_PATH := $(LD_LIBRARY_PATH):$(LIB_PATH)
        LIBS= -lcuda -L $(LIB_PATH) -lcupti -lnvperf_host -lnvperf_target -L ../extensions/src/profilerhost_util -lprofilerHostUtil
    endif
    OBJ = o
    LIBEXT = a
    LIBPREFIX = lib
    BINEXT = 
endif

userRangeSample: simplecuda.$(OBJ)
	nvcc -o $@ $^ $(LIBS)

simplecuda.$(OBJ): simplecuda.cu
	nvcc -c $(INCLUDES) $<

run: userRangeSample
	./$<

clean:
ifeq ($(OS),Windows_NT)
	del userRangeSample.exe userRangeSample.lib userRangeSample.exp simplecuda.$(OBJ)
else
	rm -f userRangeSample simplecuda.$(OBJ)
endif
