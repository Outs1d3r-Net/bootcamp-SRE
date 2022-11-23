# Procedimentos

---

[O que é o Amazon EC2?](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/concepts.html)

[Amazon RDS for MySQL - Amazon Web Services (AWS)](https://aws.amazon.com/pt/rds/mysql/)

[O que é Amazon VPC?](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/what-is-amazon-vpc.html)

[Advanced Load Balancer, Web Server, & Reverse Proxy - NGINX](https://www.nginx.com/)

[memcached - a distributed memory object caching system](https://memcached.org/)

[Ferramenta de blog, plataforma de publicação e CMS - WordPress.org Brasil](https://br.wordpress.org/)

## **Criar banco de dados como Serviço - MySQL**

---

- [ ]  Grupo de segurança da VPC: Novo
- [ ]  Nome do grupo de segurança: Blog-estudo-<seu-nome>-db
- [ ]  Zona de disponibilidade: AZ A
- [ ]  Provisione | Criar banco de dados
- [ ]  Vá até o security group, e libere para que o security group de WEB possa entrar com requisições na porta 3306 (mysql)
- [ ]  Teste com telnet: telnet <endpoint do RDS criado> 3306
- [ ]  Em seu servidor web, instale o agent de MySQL: yum install mysql
- [ ]  Através do seu servidor de WEB criado anteriormente, conecte-se ao banco de dados MySQL
- [ ]  mysql -h <endpoint do RDS> -u admin -p
- [ ]  Crie sua base de dados de nome blog: create database blog;
- [ ]  Se utiliza Azure busque os mesmos passos no Azure SQL
- [ ]  Se utiliza Google Cloud busque os mesmos passos no Cloud SQL
- [ ]  Comando para sair do banco de dados: quit

## **Instalar o Wordpress em sua VM/EC2 e instância RDS criados**

---

- [ ]  Na sua VM/EC2, entre na pasta /var/www/html: cd /var/www/html
- [ ]  Mova a sua pasta blog para blog_old: mv blog blog_old
- [ ]  Baixe o wordpress no site oficial: [https://br.wordpress.org/download/](https://br.wordpress.org/download/) ou
- [ ]  Baixe direto na instância EC2/VM: wget [https://br.wordpress.org/latest-pt_BR.tar.gz](https://br.wordpress.org/latest-pt_BR.tar.gz)
- [ ]  Descompacte a pasta do Wordpress: tar -zvxf latest-pt_BR.tar.gz
- [ ]  Mova a pasta de wordpress para blog: mv wordpress blog
- [ ]  Transforme o usuário nginx como owner da pasta blog: chown nginx:nginx blog -R
- [ ]  Abre no seu navegador o endereço IP do seu servidor WEB: Exemplo: http://<seu endereço da VM criada>/
- [ ]  Nas configurações, coloque os dados do nosso banco de dados gerenciado (RDS)
- [ ]  Exemplo:

```markdown
Nome do banco de dados: blog
Endpoint: endpoint do RDS
Usuário: adminSenha: DEFINIDA ANTERIORMENTE
```

- [ ]  Defina o nome do seu blog
- [ ]  Acesse ele ainda como HTTP, e pronto, você tem o seu primeiro blog no ar

## **Configurar sessões em memcached**

---

- [ ]  Copiar o info.php da pasta blog_old para blog: cp blog_old/info.php blog/.
- [ ]  Abrir página do phpinfo: http://<SEU IP>/info.php
- [ ]  Instale o servidor de memcached localmente em seu servidor WEB: yum install memcached -y
- [ ]  Inicialize o serviço de memcached localmente: service memcached start
- [ ]  Veja se o serviço esta UP: netstat -tplun | grep mem
- [ ]  Adicione a biblioteca de memcached ao php: yum install php-pecl-memcache
- [ ]  Edite o arquivo php.ini: vim ou nano /etc/php.ini

```markdown
session.save_handler = memcached
session.save_path = "127.0.0.1:11211
```