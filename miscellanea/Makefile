UNAME := $(shell uname)

ifeq ($(UNAME), Linux)
	B64_FLAG := w
endif

ifeq ($(UNAME), Darwin)
	B64_FLAG := b
endif


.PHONY: serveo-server
serveo-server: ## run local service on some_name.serveo.net
	ssh -R some_name:80:localhost:8990 serveo.net
