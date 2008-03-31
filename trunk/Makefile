.DELETE_ON_ERROR:

############
## Verbosity
ifeq ($(VERBOSE),1)
        Q =
else
        Q = @
endif

UT_DIR := unittests
RESULT_DIR := results
XSLTPROC := xsltproc
ECHO := /bin/echo
DIFF := diff
X2JSON   := xml2json.xslt
X2JS     := xml2js.xslt

JSON_TESTS := $(patsubst $(UT_DIR)/%.json.expected, $(RESULT_DIR)/%.json.passed, $(wildcard $(UT_DIR)/*.json.expected))
JS_TESTS   := $(patsubst $(UT_DIR)/%.js.expected, $(RESULT_DIR)/%.js.passed, $(wildcard $(UT_DIR)/*.js.expected))

TESTS := $(JSON_TESTS) $(JS_TESTS)
OUTPUTS := $(patsubst %.passed,%.output,$(TESTS))
# Outputs are intermediate files, but should be saved if
# the unittests failed.
.SECONDARY: $(OUTPUTS)

.PHONY: default
default: alltests

.PHONY: alltests
alltests:
	$(Q)-$(MAKE) -k clean tests VERBOSE=$(VERBOSE)
	$(Q)$(ECHO)
	$(Q)$(ECHO) " ** Results:"
	$(Q)$(ECHO) " ** -----------"
	$(Q)$(ECHO) -n " ** Passed: "
	$(Q)ls -1 $(RESULT_DIR)/*.passed | wc -l
	$(Q)$(ECHO) -n " ** Failed: "
	$(Q)ls -1 $(RESULT_DIR)/*.failed | wc -l

.PHONY: tests
tests: $(TESTS)
	$(info All tests passed!)

$(RESULT_DIR)/.stamp:
	$(Q)mkdir -p $(@D)
	$(Q)touch $@

$(RESULT_DIR)/%.json.passed: $(RESULT_DIR)/%.json.output $(UT_DIR)/%.json.expected
	$(Q)$(DIFF) -u $^ > $(RESULT_DIR)/$*.json.failed
	$(Q)mv $(RESULT_DIR)/$*.json.failed $@
#   Don't save the output if it passed.
	$(Q)rm $(RESULT_DIR)/$*.json.output

$(RESULT_DIR)/%.js.passed: $(RESULT_DIR)/%.js.output $(UT_DIR)/%.js.expected
	$(Q)$(DIFF) -u $^ > $(RESULT_DIR)/$*.js.failed
	$(Q)mv $(RESULT_DIR)/$*.js.failed $@
#   Don't save the output if it passed.
	$(Q)rm $(RESULT_DIR)/$*.js.output

$(RESULT_DIR)/%.json.output: $(UT_DIR)/%.xml $(RESULT_DIR)/.stamp
	$(Q)$(XSLTPROC) $(X2JSON) $(filter %.xml, $^) > $@
	$(Q)perl -pi -e 's!\n?$$!\n!' $@

$(RESULT_DIR)/%.js.output: $(UT_DIR)/%.xml $(RESULT_DIR)/.stamp
	$(Q)$(XSLTPROC) $(X2JS) $(filter %.xml, $^) > $@
	$(Q)perl -pi -e 's!\n?$$!\n!' $@

.PHONY: clean
clean:
	$(Q)rm -rf $(RESULT_DIR)
