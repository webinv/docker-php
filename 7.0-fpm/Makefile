BUILD_NAME = webinventions/php:7.0-fpm
BUILD_ARGS ?= 

ifeq ($(shell uname -s),Darwin)
XARGS_OPTS =
else
XARGS_OPTS = -r
endif

.PHONY: clean dist-clean image commit
.IGNORE: dist-clean
.DEFAULT: image

login:
	docker login 

image:
	docker build $(BUILD_ARGS) -t $(BUILD_NAME) $(CURDIR)/.

bash:
	docker run --rm -it $(BUILD_NAME) bash

commit: image
	docker push $(BUILD_NAME)

dist-clean:
	- docker images "$(BUILD_NAME)" -q | xargs $(XARGS_OPTS) docker rmi