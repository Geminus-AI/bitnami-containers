SHORT_HASH = $(shell git rev-parse --short HEAD)
NAME=ubuntu-keycloak
ARGS ?=

build:
	@echo "Building the Docker image..."
	docker build -t $(IMAGE_REPO)/$(NAME):$(SHORT_HASH) -f Dockerfile . --progress=plain $(ARGS)
	docker tag $(IMAGE_REPO)/$(NAME):$(SHORT_HASH) $(IMAGE_REPO)/$(NAME):latest

push: build
	@echo "Pushing the Docker image..."
	docker push $(IMAGE_REPO)/$(NAME):$(SHORT_HASH)
	docker push $(IMAGE_REPO)/$(NAME):latest
