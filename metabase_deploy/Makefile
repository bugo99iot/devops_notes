###############################################################################
# HELP / DEFAULT COMMAND
###############################################################################
.PHONY: help
help:
	@awk 'BEGIN {FS = ":.*?## "} /^[0-9a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)


.PHONY: metabase-postgres-start
metabase-postgres-start: ## start metabase database
		docker container stop metabase-postgres || true
		docker container rm metabase-postgres || true
		docker run \
				--restart always \
				--name metabase-postgres \
				--network web \
				-v /data/metabase/postgres:/var/lib/postgresql/data \
				-e POSTGRES_USER=metabase \
				-e POSTGRES_PASSWORD=somecomplexsecretkey \
				-e POSTGRES_DB=metabase \
				-p 5433:5432 \
				-d postgres


.PHONY: metabase-app-start
metabase-app-start:: ## start a metabase container
		docker container stop metabase-app ||true
		docker container rm metabase-app || true
		docker run \
				--restart always \
				--name metabase-app \
				--hostname metabase-app \
				--network web  \
				--label-file $(PWD)/containers/metabase/labels \
				--env-file $(PWD)/containers/metabase/env \
				-p 3000:3000 \
				-d metabase/metabase:v0.28.5


.PHONY: metabase-logs
metabase-logs: ## check metabase logs
	docker container logs -f metabase-app


.PHONY: metabase-migrate
metabase-migrate: ## migrate metabase h2 to postgres
		## postgres should be running (make metabase-postgres-start)
		## make sure network exists and is shared with postgres container
		## depending on version, you might need to use /metabase-data/metabase.db/metabase.db
		## pin the metabase version to the same as was last used with h2 db, newer and older might collide
		docker container rm metabase-migrate || true
		docker run \
				-it \
				--name metabase-migrate \
				--network web \
				-v /data/metabase/legacy/metabase-data:/metabase-data \
				--env-file $(PWD)/containers/metabase/env \
				-e MB_DB_FILE=/metabase-data/metabase.db \
				metabase/metabase:v0.28.5 load-from-h2
