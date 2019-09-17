.PHONY: help
help:
	@awk 'BEGIN {FS = ":.*?## "} /^[0-9a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)


.PHONY: traefik-logs
traefik-logs: ## check traefik logs
	docker container logs -f traefik


.PHONY: traefik-stop
traefik-stop: ## stop traefik
	docker container stop traefik || true
	docker container rm traefik || true


.PHONY: traefik-start
traefik-start: ## start traefik
	## make sure you are the owner of your domain (DSN A-target pointing at this VM)
	## when setting up first time, do 'touch acme.json && sudo chmod 600 acme.json'
	## if you are changing email credentials, do 'rm acme.json && touch acme.json && sudo chmod 600 acme.json'
	make traefik-stop
	docker run \
		--restart always \
		--name traefik \
		-p 80:80 \
		-p 443:443 \
		-p 8080:8080 \
		--network core-infra \
		-v /var/run/docker.sock:/var/run/docker.sock \
		-v ${PWD}/traefik.toml:/traefik.toml \
		-v ${PWD}/acme.json:/acme.json \
		-d traefik
