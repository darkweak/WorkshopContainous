.PHONY: down help install reset reset-acme reset-fail2ban-db reset-traefik run up update

DC=docker-compose
DC_UP=$(DC) up -d
TRAEFIK_LOG=log/traefik/access.log
TO_600=chmod 600

down: ## Down containers
	$(DC) down --remove-orphans

help: ## Show commands
	@grep -E '(^[0-9a-zA-Z_-]+:.*?##.*$$)|(^##)' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[32m%-25s\033[0m %s\n", $$1, $$2}' | sed -e 's/\[32m##/[33m/'

install: update ## Install, prepare and build project
	docker network create api_platform_1

reset: reset-acme reset-traefik reset-fail2ban-db up  ## Reset all files for acme, traefik and fail2ban

reset-acme: down ## Reset acme file
	rm acme.json
	touch acme.json
	$(TO_600) acme.json

reset-fail2ban-db: down ## Reset fail2ban db
	rm data/db/fail2ban.sqlite3

reset-traefik: down ## Reset traefik logs
	rm $(TRAEFIK_LOG)
	touch $(TRAEFIK_LOG)
	$(TO_600) acme.json

run: up ## Run containers and show you logs
	$(DC) logs -f

up: ## Up containers
	$(DC_UP)

update: ## Update project
	git pull
	$(DC) down --remove-orphans
	$(DC) pull
	$(DC) build
	$(MAKE) up
