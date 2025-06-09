BUILDER_IMAGE_NODE=node:22.13.1-bullseye-slim

define docker-inside-node
	docker run \
		--rm -t \
		-u node \
		-v $(CURDIR):/app \
		-w /app \
		$(BUILDER_IMAGE_NODE) \
		sh -x -c "$(strip $(1))"
endef

################################################################################
.PHONY: test
test:
	$(call docker-inside-node, JEST_JUNIT_OUTPUT_DIR='./test/' JEST_JUNIT_OUTPUT_NAME='results.xml' yarn run test --coverage --ci --reporters=default --reporters=jest-junit)
	@echo "test check passed"

################################################################################
.PHONY: lint
lint:
	$(call docker-inside-node, yarn eslint --quiet)
	@echo "lint check passed"

################################################################################
.PHONY: build-app
build-app:
	$(call docker-inside-node, yarn install)
	$(call docker-inside-node, yarn build)
	@echo "build app passed"

################################################################################
.PHONY: build-image
build-image:
	@docker build -t ${app_image} \
		--build-arg BUILDER_IMAGE_NODE=$(BUILDER_IMAGE_NODE) \
		-f ${docker_file} .
