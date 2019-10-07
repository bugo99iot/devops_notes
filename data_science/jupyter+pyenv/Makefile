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
