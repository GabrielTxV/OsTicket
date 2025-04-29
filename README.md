# OsTicket
Passo a passo para instalação do OsTicket
Instalação feita no Ubuntu 20.04
## Atualizar o sistema
```bash
sudo apt update && sudo apt upgrade -y
```
## Instalar o PHP 8.1, Apache e MariaDB
### Repositórios PHP 
```
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```
### Instalar Apache, MariaDB e PHP 8.1 com as extensões necessárias
```
sudo apt install apache2 mariadb-server mariadb-client -y
sudo apt install php8.1 php8.1-cli php8.1-mysql php8.1-imap php8.1-xml php8.1-mbstring php8.1-curl php8.1-gd php8.1-intl php8.1-common libapache2-mod-php8.1 unzip -y
```
### Configurar o Apache para usar PHP 8.1:
```
sudo a2dismod php7.4
sudo a2enmod php8.1
sudo update-alternatives --set php /usr/bin/php8.1
sudo systemctl restart apache2
```
## Configurar o Banco de Dados
```
sudo mysql
```
### Executar os comandos SQL
```
CREATE DATABASE osticket;
CREATE USER 'ostuser'@'localhost' IDENTIFIED BY 'senhaqualquer';
GRANT ALL PRIVILEGES ON osticket.* TO 'ostuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```


