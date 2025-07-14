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
	go mod init github.com/nengc/dnsproxy/mobile
	go get github.com/AdguardTeam/dnsproxy@v0.24.0
	go mod tidy

	go install golang.org/x/mobile/cmd/gomobile@latest
	go install golang.org/x/mobile/cmd/gobind@latest
	go env

	gomobile init
	go get golang.org/x/mobile/cmd/gomobile
	gomobile bind -ldflags="-s -w" -target=ios,iossimulator/arm64,iossimulator/amd64 -o $(NAME).xcframework github.com/nengc/dnsproxy/mobile	

build: android ios

check-env-android:
	@ if [ "$(ANDROID_HOME)" = "" ]; then \
		echo "Environment variable ANDROID_HOME not set"; \
		exit 1; \
	fi
