DOCKER_IMAGE_NAME=gnomad-readviz
TAG1 = gcr.io/broad-mpg-gnomad/${DOCKER_IMAGE_NAME}:v3.1
TAG2 = gcr.io/broad-mpg-gnomad/${DOCKER_IMAGE_NAME}:latest

PWD=`pwd`

all: build push update_scripts

build:
	docker build -t $(TAG1) -t $(TAG2) .

push:
	docker push $(TAG1)
	docker push $(TAG2) | tee /dev/tty | grep sha256 | grep latest | cut -d ' ' -f 3 > sha256.txt

update_scripts:
	cat sha256.txt && [ ! -z "`cat sha256.txt`" ] && sed -i.bak "s/${DOCKER_IMAGE_NAME}@sha256:[^\"]*/"${DOCKER_IMAGE_NAME}@`cat sha256.txt`"/"  ../*run_haplotype_caller*.py
	rm  ../*run_haplotype_caller*.py.bak
