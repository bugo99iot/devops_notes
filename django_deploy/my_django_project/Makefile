.PHONY: help
help:
	@awk 'BEGIN {FS = ":.*?## "} /^[0-9a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.PHONY: start-local
start-local: ## run app on local environment, hit http://0.0.0.0:8990/
	. venv/bin/activate && cd src && python manage.py runserver 0.0.0.0:8990

.PHONY: freeze
freeze: ## pip freeze
	. venv/bin/activate && pip freeze > requirements.txt

.PHONY: create-superuser
create-superuser: ## create django_deploy superuser
	. venv/bin/activate && cd src && python manage.py createsuperuser

.PHONY: migrate-apply
migrate-apply: ## run Django migrations and apply
	. venv/bin/activate && cd src && python manage.py makemigrations api && python manage.py migrate

.PHONY: test
test: ## run Django tests
	. venv/bin/activate && cd src && python manage.py test api

.PHONY: build
build: ## Build the acdc service
	docker build -t acdc_app .

.PHONY: start
start: ## Bring up the acdc service
	docker container stop acdc_app || true
	docker container rm acdc_app || true
	docker run \
		--rm \
		--network core-infra \
		--name acdc_app \
		--hostname acdc_app \
		--env-file env \
		-v $$PWD/src:/app \
		-v /data/acdc/:/data/acdc/ \
        -p 8990:8990 \
        -d acdc_app python manage.py runserver 0.0.0.0:8990

.PHONY: stop
stop: ## Bring down the acdc service
	docker container stop acdc_app

.PHONY: logs
logs: ## Get a running tail on the logs
	docker container logs acdc_app -f

.PHONY: shell
shell: ## Get a shell inside Docker contaier
	docker exec -it acdc_app bash

.PHONY: clean
clean: ## clean py cache
	find . -type f -name '*.py[co]' -delete -o -type d -name __pycache__ -delete


.PHONY: compose-up
compose-up: ## builds image if doesn't exist and runs containers
	docker container stop acdc_app || true
	docker container rm acdc_app || true
	docker-compose up -d

.PHONY: compose-up-build-run
compose-up-build-run: ## builds image in any case and runs container, whereas docker-compose build builds only without running
	docker container stop acdc_app || true
	docker container rm acdc_app || true
	docker-compose up -d --build

.PHONY: compose-up-build-only
compose-up-build-only: ## builds image only
	docker-compose build

.PHONY: compose-up-stop
compose-up-stop: ## stops containers
	docker-compose stop

.PHONY: compose-up-logs
compose-up-logs: ## shows logs
	docker-compose logs -f

.PHONY: compose-up
compose-up: ## builds image if doesn't exist and runs containers
	docker container stop acdc_app || true
	docker container rm acdc_app || true
	docker-compose up -d

.PHONY: compose-up-build-run
compose-up-build-run: ## builds image in any case and runs container, whereas docker-compose build builds only without running
	docker container stop acdc_app || true
	docker container rm acdc_app || true
	docker-compose up -d --build

.PHONY: compose-up-build-only
compose-up-build-only: ## builds image only
	docker-compose build

.PHONY: compose-up-stop
compose-up-stop: ## stops containers
	docker-compose stop

.PHONY: compose-up-logs
compose-up-logs: ## shows logs
	docker-compose logs -f

.PHONY: serveo-server
serveo-server: ## run serveo local server to remote
	ssh -R acdc:80:localhost:8990 serveo.net