ifeq ($(wildcard ../mk/platform.mk),)
  $(error platform.mk not found! Please run configure script first)
endif

include ../mk/platform.mk

SOURCES := $(wildcard src/*.cpp)
OBJS_FILENAMES := $(patsubst src/%.cpp,Obj/%.o,$(SOURCES))

COMMONPP_HOME := ../Common++

ifdef WIN32
DEPS := -DWPCAP -DHAVE_REMOTE
endif
ifdef LINUX
DEPS := -DLINUX
endif
ifdef MAC_OS_X
DEPS := -DMAC_OS_X
endif

INCLUDES := -I"./src" \
			-I"./header" \
			-I"$(COMMONPP_HOME)/header"
			
ifdef WIN32
INCLUDES += -I$(MINGW_HOME)/include/ddk \
			-I$(WINPCAP_HOME)/Include
endif
ifdef LINUX
INCLUDES += -I/usr/include/netinet
endif
			
Obj/%.o: src/%.cpp
	@echo 'Building file: $<'
	@echo 'Invoking: GCC C++ Compiler'
	$(G++) $(DEPS) $(INCLUDES) -O2 -g -Wall -c -fmessage-length=0 -MMD -MP -MF"$(@:Obj/%.o=Obj/%.d)" -MT"$(@:Obj/%.o=Obj/%.d)" -o "$@" "$<"
	@echo 'Finished building: $<'
	@echo ' '

# All Target
all: Packet++

create-directories:
	$(MKDIR) -p Obj
	$(MKDIR) -p Lib

# Tool invocations
Packet++: create-directories $(OBJS_FILENAMES)
	@echo 'Building target: $@'
	@echo 'Invoking: GCC Archiver'
	$(AR) -r  "Lib/$(LIB_PREFIX)Packet++$(LIB_EXT)" $(OBJS_FILENAMES)
	@echo 'Finished successfully building target: $@'
	@echo ' '

# Other Targets
clean:
	$(RM) -rf ./Obj/*
	$(RM) -rf ./Lib/*
	@echo 'Clean finished'