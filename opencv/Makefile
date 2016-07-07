IMAGE_NAME := lpr-deps/opencv:latest


build: ## Build a new image and tag it as the latest
	docker build -t $(IMAGE_NAME) .


deb: ## Grab the deb file out the container.
	docker create --name build-container-opencv $(IMAGE_NAME)
	docker cp build-container-opencv:/root/deb/ ./
	docker rm build-container-opencv


shell: ## Run an interactive shell in a new container and delete it afterwards
	docker run -i -t --rm --entrypoint /bin/sh $(IMAGE_NAME)


.PHONY: build deb shell
