include config.mk

.PHONY: default release install bin/$(name)-unsigned.apk jni clean

default ?= release
default: $(default)

release: bin/$(name)-release.apk

bin/$(name)-release.apk: bin/$(name)-unaligned.apk
	zipalign -f 4 '$<' '$@'

bin/$(name)-unaligned.apk: bin/$(name)-unsigned.apk
	jarsigner -keystore '$(key_store)' -signedjar '$@' $(key_flags) '$<' '$(key_alias)'

bin/$(name)-unsigned.apk:
	ant release

install: release
	adb install -r bin/$(name)-release.apk

jni:
	cd '$(NDK_PATH)' && make APP='$(APP)' MAKELEVEL=0

clean:
	rm -rf bin/* gen/*

run: install
	adb shell am start -n $(main)

re: clean default
