Oráculo BOT Telegram com ELK
=========================

Este BOT é para finalidade de estudo de auditoria de LOG usando a suite do ELK. Está em uso na ferramenta os seguintes aplicações:
    • Elasticsearch
    • Logstash
    • Kibana
    • Filebeat
    • Python3

OBS: Antes de executar o projeto siga todos os passos abaixo, observando o ambiente que esta executando o DOCKER

# Trocar o token do BOT do Telegram

#### /oraculo/bot.py
	
		TOKEN='868879441:AAE0BXeFe9eW4QOKKA_ZxhijiNdg-FB_jTY'

https://core.telegram.org/bots

#### Coloque o seu ID no BOT
##### caso não coloque o BOT ficará lhe ignorando mandando frases aleatórias  	
	lauro=67993868

# Alterar os paramentos nos seguintes arquivos para ajustar ao ambiente de testes

#### /filebeat/filebeat.yml
	paths:
		- "/var/log/shared/oraculo.log"

#### /kibana/config/kibana.yml
	elasticsearch.username: elastic
	elasticsearch.password: PheiKeephop1oomo1di9

#### /logstash/config/logstash.yml
	xpack.monitoring.elasticsearch.username: elastic
	xpack.monitoring.elasticsearch.password: PheiKeephop1oomo1di9

#### /logstash/pipeline/logstash.conf
	filter {
		if [log][file][path] == "/var/log/shared/oraculo.log" {

	user => "elastic"
	password => "PheiKeephop1oomo1di9"			

#### .env
	Colocar a versão desejada da Elastic Stack
	https://www.elastic.co/pt/products/

	ELK_VERSION=7.2.0

# Criar e mapear a pasta de VOLUME do HOST (onde será armazenado os logs do BOT):

#### mkdir /var/log/shared

	docker-compose.yml
		volumes:
		   log_statico:
		      driver: local
		      driver_opts:
		         type: none
		         o: bind
		         device: "/var/log/shared"

#### Mapear o volume dos PODs do Filebeat e no Oráculo
	
	- log_statico:/var/log/shared		         

#### Ajustar a senha do Elasticsearch no docker-compose.yml
	
	ELASTIC_PASSWORD: PheiKeephop1oomo1di9

#### Comandos para preparar o host DOCKER

##### sdocker é o hostname da máquina
##### Usei o ANSIBLE para realizar a instalação 

	->  ansible -m ping sdocker
	->  ansible -m shell -a "hostnamectl set-hostname sdocker" sdocker
	->  ansible -m shell -a "sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux" sdocker
	->  ansible -m shell -a "setenforce 0" sdocker
	->  ansible -m yum -a "name=* state=latest" sdocker
	->  ansible -m yum -a "name=yum-utils state=latest" sdocker
	->  ansible -m yum -a "name=device-mapper-persistent-data state=latest" sdocker
	->  ansible -m yum -a "name=lvm2 state=latest" sdocker
	->  ansible -m sysctl -a "name=vm.max_map_count value=262144 state=present sysctl_set=yes reload=yes" sdocker
	->  ansible -m get_url -a "url=https://get.docker.com dest=/tmp/get-docker.sh" sdocker
	->  ansible -m shell -a "sh /tmp/get-docker.sh" sdocker
	->  ansible -m systemd -a "name=docker enabled=yes state=restarted" sdocker
	->  ansible -m yum -a "name=docker-compose.noarch state=latest" sdocker
	->  ansible -m yum -a "name=rsync state=latest" sdocker

##### Copiar a pasta raiz para o servidor DOCKER
	rsync --delete -avzh -e "ssh -p3423 -i id_rsa.openssh" /oraculo_bot_telegram/ root@sdocker:/root/oraculo_bot_telegram/	

# LISTA DE COMANDOS do BOT #

	-> "/help"
	-> "/frases"
	-> "/frases_amor"
	-> "/frases_dev"
	-> "/frases_inteligentes"
	-> "/bop"
	-> "/lero_lero"
	-> "/teste"
	-> "/teste_diretorio"
	-> "/versao"
	-> "/liga"
	-> "/ping"
	-> "/hostname"