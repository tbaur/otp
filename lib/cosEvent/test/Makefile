#
# %CopyrightBegin%
# 
# Copyright Ericsson AB 1999-2011. All Rights Reserved.
# 
# The contents of this file are subject to the Erlang Public License,
# Version 1.1, (the "License"); you may not use this file except in
# compliance with the License. You should have received a copy of the
# Erlang Public License along with this software. If not, it can be
# retrieved online at http://www.erlang.org/.
# 
# Software distributed under the License is distributed on an "AS IS"
# basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See
# the License for the specific language governing rights and limitations
# under the License.
# 
# %CopyrightEnd%
#
#
include $(ERL_TOP)/make/target.mk
include $(ERL_TOP)/make/$(TARGET)/otp.mk

# ----------------------------------------------------
# Application version
# ----------------------------------------------------
include ../vsn.mk
VSN=$(COSEVENT_VSN)
# ----------------------------------------------------
# Release directory specification
# ----------------------------------------------------
RELSYSDIR = $(RELEASE_PATH)/cosEvent_test

# ----------------------------------------------------
# Target Specs
# ----------------------------------------------------
TEST_SPEC_FILE = cosEvent.spec
COVER_FILE = cosEvent.cover


IDL_FILES = \
	event_test_server.idl \

IDLOUTDIR = idl_output

MODULES =  \
	event_test_PushC_impl \
	event_test_PullC_impl \
	event_test_PushS_impl \
	event_test_PullS_impl \
	event_channel_SUITE \
	generated_SUITE

GEN_MOD_COS = \
	event_test_PullC \
	event_test_PushS \
	event_test_PullS \
	oe_event_test_server \
	event_test_PushC

GEN_HRL_COS = \
	event_test.hrl \
	event_test_PushC.hrl \
	event_test_PullC.hrl \
	event_test_PushS.hrl \
	event_test_PullS.hrl \
	oe_event_test_server.hrl


GEN_MODULES = $(GEN_MOD_COS) 

ERL_FILES = $(MODULES:%=%.erl)

HRL_FILES = 

GEN_HRL_FILES = $(GEN_HRL_COS)

GEN_FILES = \
	$(GEN_HRL_FILES:%=$(IDLOUTDIR)/%) \
	$(GEN_MODULES:%=$(IDLOUTDIR)/%.erl) 

GEN_TARGET_FILES = $(GEN_MODULES:%=$(IDLOUTDIR)/%.$(EMULATOR))

SUITE_TARGET_FILES = $(MODULES:%=%.$(EMULATOR))

TARGET_FILES = \
	$(GEN_TARGET_FILES) \
	$(SUITE_TARGET_FILES)

# ----------------------------------------------------
# PROGRAMS
# ----------------------------------------------------
# ----------------------------------------------------
# FLAGS
# ----------------------------------------------------
ERL_IDL_FLAGS += -pa $(ERL_TOP)/lib/orber/ebin -pa $(ERL_TOP)/lib/ic/ebin

ERL_COMPILE_FLAGS += $(ERL_IDL_FLAGS) \
	-pa $(ERL_TOP)/lib/test_server/ebin \
	-pa $(ERL_TOP)/lib/cosEvent/ebin \
	-pa $(ERL_TOP)/lib/cosEvent/test/idl_output \
	-I$(ERL_TOP)/lib/cosEvent \
	-I$(ERL_TOP)/lib/cosEvent/test/$(IDLOUTDIR) \
	-I$(ERL_TOP)/lib/test_server/include

# ----------------------------------------------------
# Targets
# ----------------------------------------------------
tests debug opt: $(TARGET_FILES) 

clean:
	rm -f idl_output/*
	rm -rf java_initial_reference_idl java_cos_naming_idl 
	rm -rf java_iiop_module_idl java_output/*
	rm -f $(TARGET_FILES) 
	rm -f errs core *~


docs:

# ----------------------------------------------------
# Special Targets
# ----------------------------------------------------

IDL-GENERATED: event_test_server.idl
	erlc $(ERL_IDL_FLAGS) -o$(IDLOUTDIR) event_test_server.idl
	>IDL-GENERATED

$(GEN_FILES): IDL-GENERATED

$(TARGET_FILES): IDL-GENERATED

# ----------------------------------------------------
# Release Targets
# ---------------------------------------------------- 
# We don't copy generated intermediate erlang and hrl files

include $(ERL_TOP)/make/otp_release_targets.mk

release_spec: 

release_docs_spec:

release_tests_spec: tests
	$(INSTALL_DIR) $(RELSYSDIR)
	$(INSTALL_DATA) $(IDL_FILES)  $(TEST_SPEC_FILE) \
		$(COVER_FILE) $(ERL_FILES) $(RELSYSDIR)
	$(INSTALL_DATA) $(SUITE_TARGET_FILES) $(RELSYSDIR)
	$(INSTALL_DIR) $(RELSYSDIR)/$(IDLOUTDIR)
	$(INSTALL_DATA) $(GEN_TARGET_FILES) $(GEN_FILES) \
		$(RELSYSDIR)/$(IDLOUTDIR)

