include /home/barosl/.java-keys/config.mk
include config.mk

SRC_DIR = src
BIN_DIR = bin

SRCES = $(shell cd $(SRC_DIR) && find)
JAVAS = $(filter %.java, $(SRCES))
CLSES = $(JAVAS:%.java=%.class)
CLSES_BIN = $(CLSES:%=$(BIN_DIR)/%)

.PHONY: all compile c clean r run jar re

DEFAULT ?= jar
all: $(DEFAULT)

compile: $(CLSES_BIN)

c: clean
clean:
	rm -rf $(BIN_DIR)/*

r: run
run: jar
	@java -cp $$CLASSPATH:$(BIN_DIR)/$(NAME).jar $(MAIN)

jar: $(BIN_DIR)/$(NAME).jar

re:
	@+make clean MAKELEVEL=0
	@+make all MAKELEVEL=0

.SECONDARY: $(CLSES_BIN)
%.jar: $(CLSES_BIN)
	@echo 'Main-Class: $(MAIN)' > $(BIN_DIR)/manifest
	cd $(BIN_DIR) && find -name '*.class' -print0 | xargs -0 jar cmf manifest ../$@
ifneq ($(strip $(SIGN_JAR)),)
	jarsigner -keystore $(KEY_STORE) $(KEY_FLAGS) $@ $(KEY_ALIAS)
endif

bin/%.class: src/%.java
	@mkdir -p $(BIN_DIR)
	javac -d $(BIN_DIR) -sourcepath $(SRC_DIR) $(JFLAGS) $<
