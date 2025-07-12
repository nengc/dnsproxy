NAME = dnsproxy

.PHONY: default
default: build

clean: cleanfast
	rm -rf build

cleanfast:
	rm -f $(NAME).aar
	rm -f $(NAME)-sources.jar
	rm -rf $(NAME).framework
	rm -rf $(NAME).xcframework

# 设置全局环境变量
export GO111MODULE = on
export GOPROXY = https://proxy.golang.org,direct

android: check-env-android
	go install golang.org/x/mobile/cmd/gomobile@latest
	
	gomobile init -ndk $(ANDROID_NDK)
	gomobile bind -target=android -o $(NAME).aar github.com/AdguardTeam/dnsproxy/mobile

ios:
	go install golang.org/x/mobile/cmd/gomobile@latest
	go install golang.org/x/mobile/cmd/gobind@latest
	go env
	ls /Users/runner/go/pkg/mod/golang.org/x
	ls /Users/runner/go/pkg/mod/golang.org/x/mobile@v0.0.0-20250711185624-d5bb5ecc55c0

    	#go get golang.org/x/mobile/bind/objc@latest
	which gomobile
	ls /Users/runner/go/bin/
	gomobile init
	gomobile bind -ldflags="-s -w" -target=ios,iossimulator/arm64,iossimulator/amd64 -o $(NAME).xcframework github.com/AdguardTeam/dnsproxy/mobile

build: android ios

check-env-android:
	@ if [ "$(ANDROID_HOME)" = "" ]; then \
		echo "Environment variable ANDROID_HOME not set"; \
		exit 1; \
	fi
