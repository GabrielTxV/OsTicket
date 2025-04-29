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
## Baixar e Configurar o osTicket
```
cd /tmp
wget https://github.com/osTicket/osTicket/releases/download/v1.18.1/osTicket-v1.18.1.zip
unzip osTicket-v1.18.1.zip
sudo mv upload /var/www/html/osticket
```
### Configurar permissões
```
cd /var/www/html/osticket/include
sudo cp ost-sampleconfig.php ost-config.php
sudo chown -R www-data:www-data /var/www/html/osticket
sudo chmod 0666 /var/www/html/osticket/include/ost-config.php
```
## Configurar Virtual Host do Apache
```
sudo nano /etc/apache2/sites-available/osticket.conf
```
```
<VirtualHost *:80>
    ServerAdmin admin@seudominio.com
    DocumentRoot /var/www/html/osticket
    ServerName osticket.local

    <Directory /var/www/html/osticket>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/osticket_error.log
    CustomLog ${APACHE_LOG_DIR}/osticket_access.log combined
</VirtualHost>
```
### Habilitar site e módulos
```
sudo a2ensite osticket.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```
## Finalizar instalação pelo navegador
### Acessar pelo navegador
```
http://SEU_IP/osticket
```
## Após instalação
### Remover pasta de instalação e ajustar permissões
```
sudo rm -rf /var/www/html/osticket/setup
sudo chmod 0644 /var/www/html/osticket/include/ost-config.php
```
### Para acesso do admin com usuário e senha cadastrados
```
http://SEU-IP/osticket/scp
```
### Para usuário final
```
http://SEU-IP/osticket
```

