IMAGE_NAME := lpr-deps/tesseract:latest


build: ## Build a new image and tag it as the latest
	docker build -t $(IMAGE_NAME) .


deb: ## Grab the deb file out the container.
	docker create --name build-container-tesseract $(IMAGE_NAME)
	docker cp build-container-tesseract:/root/deb/ ./
	docker rm build-container-tesseract


shell: ## Run an interactive shell in a new container and delete it afterwards
	docker run -i -t --rm --entrypoint /bin/sh $(IMAGE_NAME)


.PHONY: build deb shell
