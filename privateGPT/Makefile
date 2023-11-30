# Makefile for building and running privateGPT container

DOCKER=/usr/local/bin/docker
GIT=/usr/bin/git
REMOVEDIR=/bin/rm -rf

DOCKERFILEDIR=.

PRIVATEGPTGITHUB=https://github.com/imartinez/privateGPT.git
PRIVATEGPTLOCAL=privateGPT

VERSION=0

DEFAULT_IMAGE_NAME=private_gpt_v$(VERSION)

IMAGE_NAME?=$(DEFAULT_IMAGE_NAME)

CLIENTPORT=8001

build:
	$(DOCKER) build -t $(IMAGE_NAME) $(DOCKERFILEDIR)

run: 
	$(DOCKER) run -p 8001:$(CLIENTPORT) $(IMAGE_NAME)

init:
	$(GIT) clone --depth=1 $(PRIVATEGPTGITHUB) $(PRIVATEGPTLOCAL) && $(REMOVEDIR) .git

clean: 
	$(REMOVEDIR) $(PRIVATEGPTLOCAL)
	$(DOCKER) rmi `($(DOCKER) images '$(IMAGE_NAME)' -a -q`

