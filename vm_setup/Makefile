###############################################################################
# HELP / DEFAULT COMMAND
###############################################################################
.PHONY: help
help:
	@awk 'BEGIN {FS = ":.*?## "} /^[0-9a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)


.PHONY: crate-user
create-user: ## create user in new VM
	ssh root@someip
	adduser deploy
	usermod -aG sudo deploy
	su - deploy
	mkdir .ssh
	cd .ssh
	touch authorized_keys
	# add ssh key from remote to authorized_keys


.PHONY: set-up-docker
set-up-docker: ## set up docker in new VM
	ssh deploy@someip
	curl -fsSL https://get.docker.com -o get-docker.sh
	sh get-docker.sh
	sudo groupadd docker
	sudo usermod -aG docker $USER
	# log out and log in again
	exit
	ssh deploy@someip
	sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
	sudo chmod +x /usr/local/bin/docker-compose


.PHONY: set-up-zerotier
set-up-zerotier: # set up zerotier in new vm
	ssh deploy@someip
	curl -s https://install.zerotier.com | sudo bash
	sudo cat /var/lib/zerotier-one/authtoken.secret >>.zeroTierOneAuthToken
	chmod 0600 .zeroTierOneAuthToken
	zerotier-cli join somenetworkid

.PHONY: add-entropy
add-entropy: # on digital ocean server, metabase startup takes 30m, due to issue https://www.digitalocean.com/community/tutorials/how-to-setup-additional-entropy-for-cloud-servers-using-haveged
	apt-get install haveged
	update-rc.d haveged defaults


.PHONY: set-up-drive
set-up-drive: # set up drives in new vm
	# add disks through (Azure) portal
	ssh deploy@someip
	mkdir /data
	sudo chmod 777 /data
	sudo fdisk -l
	# find new disk with location, here we assume disk is in /dev/sdc
	sudo mkfs.ext4 /dev/sdc
	sudo mount /dev/sdc /data
	# check that disk is now mounted
	sudo lsblk


.PHONY: set-up-vm
set-up-vm: ## set up new vm
	# create new vm with deply user and port 22 open
	ssh deploy@someip
	make set-up-docker
	make set-up-zerotier
	set-up-drive
	docker network create core-infra
	sudo apt install make
	make set-up-docker-postgres
	make set-up-db-and-user
	# ask devops to add server authentication on docker registry
	docker login -p SOMEPASS -u SOMEUSEER https://somedomain.com
	# ask devops to connect vm to ssl certificate authority let's encrypt