.PHONY: help
help:
	@awk 'BEGIN {FS = ":.*?## "} /^[0-9a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)


.PHONY: postgres-start
postgres-start: ## start analytics database
	## this creates a default database postgres with default user postgres
	docker create network postgres-network || true
	docker container stop postgres-db || true
	docker container rm postgres-db || true
	docker run \
	        --restart unless-stopped \
	        --name postgres-db \
			--network postgres-network \
	        -v /data/postgres:/var/lib/postgresql/data \
	        -e POSTGRES_PASSWORD=somecomplexpassword \
	        -p 5432:5432 \
	        -d postgres:11.5


.PHONY: set-up-db-and-user
set-up-db-and-user: # set up database and user
	## get a shell in container and log in
	docker exec -it docker-postgres bash -c 'psql -d postgres -U postgres'
	## create user and db
	CREATE ROLE someusername WITH LOGIN ENCRYPTED PASSWORD 'somecomplexpassword';
	CREATE DATABASE somedbname;
	GRANT ALL PRIVILEGES ON somedbname TO someusername;
	\q;
	## verify
	psql -d somedbname -U someuser # log in to db with user
	\q;
	exit


.PHONY: postgres-build-with-postgis
postgres-build-with-postgis: ## screate analytics database with postgis
	docker build -t postgres-postgis .


.PHONY: postgres-start-with-postgis
postgres-start-with-postgis: ## start analytics database with postgis
	docker create network postgres-network || true
	docker container stop postgres-db-postgis || true
	docker container rm postgres-db-postgis || true
	docker run \
		--restart unless-stopped \
		--name postgres-db-postgis \
		--network postgres-network \
		-p 5434:5432 \
		-v /data/postgres:/var/lib/postgresql/data \
		postgres-postgis
