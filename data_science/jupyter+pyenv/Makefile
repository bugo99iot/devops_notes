.PHONY: help
help:
	@awk 'BEGIN {FS = ":.*?## "} /^[0-9a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)


.PHONY: jupyter-setup
jupyter-setup: ## setup juopyter to work with a specific venv
	virtualenv -p python3.6 py_36_env
	source py_36_env/bin/activate
	pip install ipykernel
	python -m ipykernel install --user --name=py_36_env
	jupyter notebook
	# Installing packages thereafter
	source py_36_env/bin/activate && pip install some_package
	# or stright in notebook (just !pip install won't work, won't find the right venv)
	# https://jakevdp.github.io/blog/2017/12/05/installing-python-packages-from-jupyter/
	import sys
	!{sys.executable} -m pip install some_package

.PHONY: install-pyenv-macos-11
install-pyenv-macos-11:
	#!/bin/bash
	SYSTEM_VERSION_COMPAT=1
	brew install zlib
	brew install sqlite
	brew install bzip2
	brew install libiconv
	brew install libzip
	CFLAGS="-I$(brew --prefix zlib)/include -I$(brew --prefix bzip2)/include -I$(brew --prefix openssl)/include -I$(brew --prefix readline)/include -I$(brew --prefix ncurses)/include -I$(xcrun --show-sdk-path)/usr/include" \
	LDFLAGS="-L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib -L$(brew --prefix openssl)/lib -L$(brew --prefix readline)/lib -L$(brew --prefix ncurses)/lib -L$(xcrun --show-sdk-path)/usr/lib" \
	pyenv install --patch 3.6.6 < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)
