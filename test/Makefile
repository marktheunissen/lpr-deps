IMAGE_NAME := lpr-deps/test:latest


build: ## Build a new image and tag it as the latest
	docker build -t $(IMAGE_NAME) .


run: ## Run the test plate detection
	docker run --rm $(IMAGE_NAME)


shell: ## Run an interactive shell in a new container and delete it afterwards
	docker run -i -t --rm --entrypoint /bin/bash $(IMAGE_NAME)


.PHONY: build deb shell
