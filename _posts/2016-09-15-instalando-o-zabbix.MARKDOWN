---
layout: post
title: Instalando o Zabbix no Ubuntu Server 16.04
date: 2016-09-15 23:57:00
categories: Zabbix
tags: Zabbix
---

Oi pessoal!  
Como esse é o primeiro post do "blog", vamos começar com o Zabbix. Já que venho brincando com ele há semanas. Então, vamos lá!  

<!--more-->

### 1. Baixando o pacote e instalando as dependências
Antes de tudo, acesse a [página de download do Zabbix](http://www.zabbix.com/download.php) e baixe o arquivo de fontes. Depois de baixado, extraia o arquivo de fontes com o seguinte comando:  

```
$ tar -zxvf zabbix-3.0.3.orig.tar.gz  
```

Após descompactar o arquivo, é a hora de instalar as dependências para podermos compilar o pacote do Zabbix.  

```
sudo apt-get install make gcc gpp apache2 php php-mysql libapache2-mod-php php-gd libsnmp-dev libcurl4-openssl-dev vim mysql-server mysql-client libmysqld-dev libcurl-dev php-bcmath php-mbstring php-xml  
```

### 2. Criando a conta de usuário Zabbix  
Para todos os processos daemons do Zabbix, é necessário que exista um usuário comum (sem privilégios de root) para a sua execução.  
Para criar o usuário Zabbix, execute os seguintes comandos:  

```
groupadd zabbix  
useradd -g zabbix zabbix  
```

*Importante!*  
Se o Zabbix Server e o Zabbix Agent estiverem na mesma máquina, é recomendável que estejam rodando com usuários diferentes. Caso os dois sejam executados com o mesmo usuário, o agente poderá acessar o arquivo de configuração do Zabbix Server e informações sensíveis que poderão ser facilmente recuperadas.  

### 3. Criando o banco de dados do Zabbix  
Para os daemons do Zabbix Server e para a interface web, é necessário um banco de dados.  
Para criar o banco de dados do Zabbix Server, execute os seguintes comandos:  

```
shell> mysql -uroot -p<password>  
mysql> create database zabbix character set utf8 collate utf8_bin;  
mysql> grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';  
mysql> flush privileges;  
mysql> quit;  
shell> cd database/mysql  
shell> mysql -uzabbix -p<password> zabbix < schema.sql  
# stop here if you are creating database for Zabbix proxy  
shell> mysql -uzabbix -p<password> zabbix < images.sql  
shell> mysql -uzabbix -p<password> zabbix < data.sql  
```

Após criar o banco, procesa com para os passos de compilação do Zabbix.  

### 4. Configurando as fontes e compilando
Para configurar as fontes para a compilação, é necessário que seja definido qual o tipo de banco de dados será utilizado. Os binários serão compilados para um banco de dados específico.  
Como no nosso caso escolhemos utilizar o MySQL, utiizaremos o seguinte comando para configurar as fontes:  

```
./configure --enable-server --enable-agent --with-mysql --enable-ipv6 --with-net-snmp --with-libcurl --with-libxml2  
```

Após configurar as fontes, execute o seguinte comando para compilar o Zabbix Server: 
 
```
sudo make install  
```

### 5. Revisando e editando os arquivos de configuração  
Abra o arquivo de configuração do Zabbix com o seguinte comando: 
 
```
# vim /usr/local/etc/zabbix_server.conf  
```

Procure pelas as linhas:  
  
```
DBHost=localhost  
DBName=zabbix  
DBUser=zabbix  
DBPassword=zabbix  
```

Todas essas linhas existem no arquivo de configuração mas estão comentadas. Portanto, é só descomentar a linha e adicionar o parâmetro após o sinal de igualdade.  

Abra o arquivo de configuração do PHP com o seguinte comando: 
 
```
# vim /etc/php/7.0/apache2/php.ini  
```

Procure pelas seguintes linhas no arquivo e altere os valores.  

```
memory_limit=128M  
post_max_size=16M  
upload_max_filesize=2M  
max_execution_time=300  
max_input_time=600  
date.timezone=America/Sao_Paulo  
```

### 6. Instalando a interface web do Zabbix  
A interface web do Zabbix é escrita em PHP, portanto, é necessário um servidor web com suporte ao PHP. A instalação será concluída simplesmente copiando os arquivos PHP de frontends/php para o diretorio HTML do Apache.  
A localização do diretório HTML do Apache pode variar em função da distro utilizada.  
 
 - /srv/www/htdocs (OpenSUSE, SLES)
 - /var/www/html (Fedora, RHEL, CentOS)
 - /var/www/html (Debian, Ubuntu)
 
 Utilize os comandos a seguir para criar o diretório do zabbix e copiar os arquivos para o mesmo.  

```
# mkdir <htdocs>/zabbix  
# cd frontends/php  
# cp -a . <htdocs>/zabbix
```
 
 E... para terminar...
 
### 7. Iniciando o daemon do Zabbix Server e reinciando o Apache
 Após copiar os arquivos da interface web, é necessário reiniciar o serviço do Apache para que as alterações sejam aplicadas.  

 ```
 sudo service apache2 restart  
 ```
 
 Após reiniciar o serviço do Apache, é a hora de iniciar o daemon do Zabbix Server.  

 ```
 shel> zabbix_server  
 ```
 
 Agora é só abrir o navegador e digitar localhost/zabbix/ e finalizar a configuração pela interface web!  
