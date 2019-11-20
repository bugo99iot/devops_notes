##
## deploy nuvoschedule crontabs
##

.PHONY: deploy-crontabs
deploy-crontabs: ## collect crontab files in machine and run
	cat $(PWD)/containers/nuvoschedule/data_schedules > vm_crontab
	crontab vm_crontab
	crontab -l