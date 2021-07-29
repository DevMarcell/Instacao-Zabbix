~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Instalando Zabbix no Ubuntu 20.04 LTS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

a. Instale o repositório Zabbix
wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+focal_all.deb
dpkg -i zabbix-release_5.0-1+focal_all.deb
apt update

b. Instale o servidor, o frontend e o agente Zabbix
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent

c. Criar banco de dados inicial
apt-get install mysql-server mysql-client
mysql –u root -p
password
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> quit;

No servidor do Zabbix, importe o esquema inicial e os dados. Será solicitado a inserir a senha que foi criada anteriormente.
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix

d. Configure o banco de dados para o servidor Zabbix
Edite o arquivo /etc/zabbix/zabbix_server.conf ~
DBPassword=password

e. Configure o PHP para o frontend Zabbix
Editar arquivo /etc/zabbix/apache.conf, descomente e defina o fuso horário correto.
# php_value date.timezone America/Sao_Paulo

f. Inicie o servidor Zabbix e os processos do agente
Inicie o servidor Zabbix e os processos do agente e configure-os para que sejam iniciados durante o boot do sistema. ~
# systemctl restart zabbix-server zabbix-agent apache2
# systemctl enable zabbix-server zabbix-agent apache2

g. Acesse o Zabbix
No navegador, digite o endereço IP da máquina onde foi instalado/zabbix ~
Exemplo: http://10.10.1.49/zabbix/
