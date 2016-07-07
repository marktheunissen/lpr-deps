IMAGE_NAME := lpr-deps/openalpr:latest


build: ## Build a new image and tag it as the latest
	docker build -t $(IMAGE_NAME) .


deb: ## Grab the deb file out the container.
	docker create --name build-container-openalpr $(IMAGE_NAME)
	docker cp build-container-openalpr:/root/deb/ ./
	docker rm build-container-openalpr


shell: ## Run an interactive shell in a new container and delete it afterwards
	docker run -i -t --rm --entrypoint /bin/sh $(IMAGE_NAME)


.PHONY: build deb shell
