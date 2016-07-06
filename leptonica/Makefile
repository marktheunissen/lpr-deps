IMAGE_NAME := lpr-deps/leptonica:latest


build: ## Build a new image and tag it as the latest
	docker build -t $(IMAGE_NAME) .


deb: ## Run the service in a container
	docker run --rm $(IMAGE_NAME)


shell: ## Run an interactive shell in a new container and delete it afterwards
	docker run -i -t --rm --entrypoint /bin/sh $(IMAGE_NAME)


.PHONY: build deb shell
