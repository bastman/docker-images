SERVICE_NAME=java8-buildtools
SERVICE_VERSION_FILE=./version.txt

GIT_BRANCH_NAME=$(shell git rev-parse --abbrev-ref HEAD)
GIT_COMMIT_HASH=$(shell git rev-parse HEAD | cut -c 1-8)
BUILD_TIME=$(shell date -u +"%Y-%m-%dT%H.%M.%SZ")

SERVICE_VERSION ?=$(shell cat $(SERVICE_VERSION_FILE))

DOCKER_FILE=./Dockerfile
DOCKER_TAG_LOCAL=local/$(SERVICE_NAME):$(SERVICE_VERSION)

DOCKERHUB_REGISTRY_HOST=docker.io
DOCKERHUB_IMAGE_NAME=$(SERVICE_NAME)
DOCKERHUB_TAG_REMOTE=$(DOCKERHUB_REGISTRY_HOST)/bastman77/$(DOCKERHUB_IMAGE_NAME):$(SERVICE_VERSION)

ifeq ($(shell cat $(SERVICE_VERSION_FILE)),)
SERVICE_VERSION := $(BUILD_TIME)-$(GIT_COMMIT_HASH)-$(GIT_BRANCH_NAME)
$(shell echo $(SERVICE_VERSION) > $(SERVICE_VERSION_FILE))
@echo "(xnew) service.version: $(SERVICE_VERSION)"
endif

print-%: ; @echo $*=$($*)
guard-%:
	@test ${${*}} || (echo "FAILED! Environment variable $* not set " && exit 1)
	@echo "-> use env var $* = ${${*}}";

.PHONY : help
help : Makefile
	@sed -n 's/^##//p' $<

version.show: guard-SERVICE_VERSION
	@echo "service.version: $(SERVICE_VERSION)"

version.clean:
	rm -f -v $(SERVICE_VERSION_FILE)
	$(eval SERVICE_VERSION := "")
	@echo "service.version: $(SERVICE_VERSION)"

version.set: guard-NEW_SERVICE_VERSION
	@echo "$(NEW_SERVICE_VERSION)" > $(SERVICE_VERSION_FILE)


## clean   : clean
clean: version.clean
## build   : build docker-image "build-tools"
build: guard-SERVICE_VERSION
	docker build -t $(DOCKER_TAG_LOCAL) -f $(DOCKER_FILE) .
## build-tools.build   : build docker-image "build-tools"
run: guard-SERVICE_VERSION
	docker run -it $(DOCKER_TAG_LOCAL) bash

## dockerhub.push   : push to dockerhub
dockerhub.push: guard-SERVICE_VERSION
	@echo "docker-tag: $(SERVICE_NAME) version: $(SERVICE_VERSION) tag-local: $(DOCKER_TAG_LOCAL) -> tag-remote: $(DOCKERHUB_TAG_REMOTE) ..."
	docker tag $(DOCKER_TAG_LOCAL) $(DOCKERHUB_TAG_REMOTE)
	@echo "docker-push: tag-remote: $(DOCKERHUB_TAG_REMOTE) ..."
	docker push $(DOCKERHUB_TAG_REMOTE)

## dockerhub.pull   : pull from dockerhub
dockerhub.pull: guard-SERVICE_VERSION
	@echo "docker-pull: tag-remote: $(DOCKERHUB_TAG_REMOTE) ..."
	docker pull $(DOCKERHUB_TAG_REMOTE)