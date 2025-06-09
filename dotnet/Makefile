DOTNET_BUILDER_IMAGE=mcr.microsoft.com/dotnet/sdk:8.0
DOTNET_RUNTIME_IMAGE=mcr.microsoft.com/dotnet/aspnet:8.0

define docker-inside-core
	docker run \
		--rm -t \
		-v $(CURDIR):/app \
		-w /app \
		$(DOTNET_BUILDER_IMAGE) \
		sh -x -c "$(strip $(1))"
endef

################################################################################
.PHONY: test
test:
	$(call docker-inside-core, dotnet test tests/Tests.Unit --filter Type!~Applied --logger:\"junit;LogFilePath=/app/test/results.xml\")
	@echo "test check passed"

################################################################################
.PHONY: lint
lint:
	@echo "mock lint check"

################################################################################
.PHONY: build-app
build-app:
	$(call docker-inside-core, dotnet publish src/Api -c Release -o /app/out)
	$(call docker-inside-core, dotnet publish src/Migrator -c Release -o /app/migrator)
	@echo "build app passed"

################################################################################
.PHONY: build-image
build-image:
	@docker build -t ${app_image} --build-arg DOTNET_RUNTIME_IMAGE=$(DOTNET_RUNTIME_IMAGE) --build-arg APP_DLL="Api.dll" -f Dockerfile .
	@echo "build image passed"

################################################################################
