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
