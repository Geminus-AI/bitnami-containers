# Images to build and push
COMPONENTS := keycloak keycloak-config-cli minio postgresql redis

# Latest versions for each image
KEYCLOAK_VERSION := 26
KEYCLOAK_CONFIG_CLI_VERSION := 6
MINIO_VERSION := 2025
POSTGRESQL_VERSION := 17
REDIS_VERSION := 7.4

# Operating system
OS := debian-12

.PHONY: all build push clean

# Default target
all: build

# Build all images by calling their respective Makefiles
build:
	@echo "Building all images..."
	@$(MAKE) -C keycloak/$(KEYCLOAK_VERSION)/$(OS) build
	@$(MAKE) -C keycloak-config-cli/$(KEYCLOAK_CONFIG_CLI_VERSION)/$(OS) build
	@$(MAKE) -C minio/$(MINIO_VERSION)/$(OS) build
	@$(MAKE) -C postgresql/$(POSTGRESQL_VERSION)/$(OS) build
	@$(MAKE) -C redis/$(REDIS_VERSION)/$(OS) build
	@echo "All images built successfully!"

# Push all images by calling their respective Makefiles
push:
	@echo "Pushing all images..."
	@$(MAKE) -C keycloak/$(KEYCLOAK_VERSION)/$(OS) push
	@$(MAKE) -C keycloak-config-cli/$(KEYCLOAK_CONFIG_CLI_VERSION)/$(OS) push
	@$(MAKE) -C minio/$(MINIO_VERSION)/$(OS) push
	@$(MAKE) -C postgresql/$(POSTGRESQL_VERSION)/$(OS) push
	@$(MAKE) -C redis/$(REDIS_VERSION)/$(OS) push
	@echo "All images pushed successfully!"
