# DAW Plugins for Web Browsers
# Jari Kleimola 2014-15
# Dexed web port


# Copyright (c) 2013 The Chromium Authors. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

# GNU Makefile based on shared rules provided by the Native Client SDK.
# See README.Makefiles for more details.

VALID_TOOLCHAINS := pnacl

NACL_SDK_ROOT ?= $(abspath $(CURDIR)/../../..)

include $(NACL_SDK_ROOT)/tools/common.mk

TARGET = dexed
HOST = ../../host/cpp
PLUG = $(HOST)/plugformats
SRC = src
MSFA = src/msfa
INC = -I$(HOST) -I$(HOST)/plugformats -I$(HOST)/inc/JuceLibraryCode
LIBS = ppapi_cpp ppapi juce_core juce_audio juce_events

CFLAGS = -DWEB -DPNACL -DDLLstubs -DJUCE -Wall $(INC) -I$(SRC) -I$(MSFA)

SOURCES = $(HOST)/pdawhost.cc $(HOST)/dawhost.cc $(HOST)/dawbundle.cc $(HOST)/dawplugin.cc $(HOST)/utils.cc $(PLUG)/juce.cc \
$(SRC)/PluginProcessor.cpp $(SRC)/PluginParam.cpp $(SRC)/EngineMkI.cpp \
$(SRC)/PluginData.cpp $(SRC)/PluginFX.cpp $(SRC)/EngineOpl.cpp \
$(MSFA)/lfo.cc $(MSFA)/dx7note.cc $(MSFA)/freqlut.cc $(MSFA)/sin.cc $(MSFA)/exp2.cc \
$(MSFA)/fm_core.cc $(MSFA)/pitchenv.cc $(MSFA)/env.cc $(MSFA)/fm_op_kernel.cc

# Build rules generated by macros from common.mk:

$(foreach src,$(SOURCES),$(eval $(call COMPILE_RULE,$(src),$(CFLAGS))))

# The PNaCl workflow uses both an unstripped and finalized/stripped binary.
# On NaCl, only produce a stripped binary for Release configs (not Debug).
ifneq (,$(or $(findstring pnacl,$(TOOLCHAIN)),$(findstring Release,$(CONFIG))))
$(eval $(call LINK_RULE,$(TARGET)_unstripped,$(SOURCES),$(LIBS),$(DEPS)))
$(eval $(call STRIP_RULE,$(TARGET),$(TARGET)_unstripped))
else
$(eval $(call LINK_RULE,$(TARGET),$(SOURCES),$(LIBS),$(DEPS)))
endif

$(eval $(call NMF_RULE,$(TARGET),))
