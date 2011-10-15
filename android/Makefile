include /home/barosl/.sdk/baroslized-sdk/android/global-config.mk
include config.mk

.PHONY: default release install bin/$(NAME)-unsigned.apk jni c clean r run re

default ?= release
all: $(default)

release: bin/$(NAME)-release.apk

install: release
	adb install -r bin/$(NAME)-release.apk

bin/$(NAME)-release.apk: bin/$(NAME)-unaligned.apk
	zipalign -f 4 '$<' '$@'

bin/$(NAME)-unaligned.apk: bin/$(NAME)-unsigned.apk
	jarsigner -keystore '$(KEY_STORE)' -signedjar '$@' $(KEY_FLAGS) '$<' '$(KEY_ALIAS)'

bin/$(NAME)-unsigned.apk:
	ant release

jni:
	cd '$(NDK_PATH)' && make APP='$(APP)' MAKELEVEL=0

c: clean
clean:
	rm -rf bin/* gen/*

r: run
run: install
	adb shell am start -n $(MAIN)

re:
	@+$(MAKE) clean MAKELEVEL=0
	@+$(MAKE) all MAKELEVEL=0
