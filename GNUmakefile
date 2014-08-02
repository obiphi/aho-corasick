-include rules.mk
export acism ?= .

#---------------- PRIVATE VARS:
# acism.pgms: test programs requiring no args

acism.c          = $(patsubst %,$(acism)/%, acism.c acism_create.c acism_dump.c acism_file.c)
acism.pgms       = $(acism)/acism_x $(acism)/acism_mmap_x
ACISM_SIZE       ?= 4   # size of transition table elements.

#---------------- PUBLIC VARS (see rules.mk)
acism.lib       = $(acism)/libacism.a
acism.include   = $(acism)/acism.h
clean           += $(acism)/*.tmp

#---------------- PUBLIC TARGETS (see rules.mk):
all             : $(acism.lib)
test            : $(acism)/acism_t.pass

# Activate default actions for (clean,install):
install .PHONY  : acism.install

#---------------- PRIVATE RULES:
$(acism.lib)	: $(acism.c:c=o)

# ACISM by default defines TRAN as a 4-byte int. This has about a 10MB pattern string limit.
#   An 8-byte TRAN has no practical limit. It is also faster on a 64bit processor,
#   since its fields are const size.

$(acism)/acism_t.pass : $(acism.pgms) $(acism)/words

$(acism.pgms)   : CPPFLAGS := -I$(acism) -DACISM_SIZE=$(ACISM_SIZE) $(CPPFLAGS) 
$(acism.pgms)   : $(acism.lib) $(acism)/msutil.o $(acism)/tap.o

$(acism.c:c=[isI]) : CPPFLAGS := -I$(acism) -DACISM_SIZE=$(ACISM_SIZE) $(CPPFLAGS) 
# Include auto-generated depfiles (gcc -MMD):
-include $(acism)/*.d

# vim: set nowrap :
