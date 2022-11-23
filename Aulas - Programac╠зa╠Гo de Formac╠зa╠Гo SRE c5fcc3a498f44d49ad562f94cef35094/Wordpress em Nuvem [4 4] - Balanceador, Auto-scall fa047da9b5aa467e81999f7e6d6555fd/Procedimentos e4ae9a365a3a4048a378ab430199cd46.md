# Procedimentos

---

[O que é o Amazon EC2?](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/concepts.html)

[Amazon RDS for MySQL - Amazon Web Services (AWS)](https://aws.amazon.com/pt/rds/mysql/)

[O que é Amazon VPC?](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/what-is-amazon-vpc.html)

[Advanced Load Balancer, Web Server, & Reverse Proxy - NGINX](https://www.nginx.com/)

[memcached - a distributed memory object caching system](https://memcached.org/)

[Ferramenta de blog, plataforma de publicação e CMS - WordPress.org Brasil](https://br.wordpress.org/)

[https://docs.aws.amazon.com/pt_br/AmazonElastiCache/latest/mem-ug/WhatIs.html](https://docs.aws.amazon.com/pt_br/AmazonElastiCache/latest/mem-ug/WhatIs.html)

[O que é o Amazon Elastic File System?](https://docs.aws.amazon.com/pt_br/efs/latest/ug/whatisefs.html)

## **Criando um certificado SSL de testes**

---

- [ ]  Entre na pasta de configuração do nginx (/etc/nginx) e crie uma pasta de nome ssls: mkdir /etc/nginx/ssls
- [ ]  Entre nesta pasta: cd /etc/nginx/ssls
- [ ]  Utilize o openssl para criar seu certificado
- [ ]  Preencha com as informações necessários como teste
- [ ]  Converta sua chave privada de KEY para PEM
- [ ]  Converta sua chave pública
- [ ]  Entre na pasta de configurações de vhosts do nginx
- [ ]  Agora copie o arquivo de configuração do nginx de blog.conf para blog-ssl.conf
- [ ]  Edite o arquivo blog-ssl.conf, ajustando os itens abaixo
- [ ]  Reinicialize o nginx
- [ ]  Para testar, requisite sua página de info.php URL nos endereços HTTP e HTTPS, ambos devem responder [https://IP-PÚBLICO/info.php](https://xn--ip-pblico-3za/info.php)

## **Criando um balanceador de cargas**

---

- [ ]  Vá na área de EC2 e selecione a opção de balanceador de cargas (load balancers)
- [ ]  Agora vá em balanceadores
- [ ]  Crie um balanceador do tipo: Classic Load Balancer
- [ ]  Use o nome: Blog-estudo-seu-nome-lb
- [ ]  Libere entrada HTTP enviando para HTTP
- [ ]  Libere entrada TCP (443) enviando para TCP (443)
- [ ]  Selecione suas 2 sub-redes públicas
- [ ]  Crie um grupo de segurança com o nome: Blog-estudo-seu-nome-lb
- [ ]  Regras: Entrada de 0.0.0.0/0 em HTTP e HTTPS
- [ ]  Ping Path: /info.php
- [ ]  Adicionar EC2/VM atual no balanceador
- [ ]  Crie
- [ ]  Para validar o funcionamento, chame a URL pública do balanceador em HTTP e HTTPS
- [ ]  [https://endpoint](https://endpoint/) do balanceador/info.php (Demora uns minutos para abrir a página por conta do DNS do Brasil)

## **Requisição HTTPS e ajuste wordpress**

---

- [ ]  É necessário ajustar no wordpress, a definição de qual será o endereço da home do blog, para isso, vamos ajustar no arquivo de configurações do wordpress.
- [ ]  Entre na pasta: /var/www/html/blog
- [ ]  Edite o arquivo: wp-config.php (sugiro fazer um backup do arquivo antes)
- [ ]  Inclua as 2 linhas abaixo

```php
define('WP_HOME','https://endpoint do balanceador');
define('WP_SITEURL','https://endpoint do balanceador');
```

- [ ]  Agora sim, seu blog está abrindo do modo correto.

## **Auto-scaling - Template**

---

- [ ]  Primeiro passo é gerarmos um template da nossa EC2
- [ ]  Vá em EC2 e nas opções na parte de imagens e modelos, selecione a opção “criar imagem”. Defina um nome como Blog-estudo-seu-nome-template-web-data
- [ ]  Crie sua imagem
- [ ]  Vá em EC2 e depois na parte de Auto-scaling
- [ ]  Crie um novo grupo de auto-scaling
- [ ]  Defina o nome como: Blog-estudo-seu-nome-ASG
- [ ]  Crie um novo modelo de execução com o nome Blog-estudo-seu-nome-modelo-web
- [ ]  Em Imagem de máquina da Amazon (AMI), selecione a sua imagem criada anteriormente (AMI)
- [ ]  Em Tipo de instância selecione t3a.micro
- [ ]  Em Par de chaves é opcional ser ativado (se ativar use sua mesma chave PEM)
- [ ]  Em Grupo de segurança, selecione o Blog-estudo-seu-nome-web
- [ ]  Inclua Tag Name = Blog-estudo-seu-nome-web-escalavel
- [ ]  Agora com o modelo criado, selecione em seu auto-scaling
- [ ]  Selecione sua VPC e suas 2 Sub-redes privadas (AZ-A e AZ-B)
- [ ]  Selecione um balanceador de cargas existente
- [ ]  Escolha a opção classic e selecione o balanceador anteriormente criado

```markdown
Capacidade desejada: 2
Capacidade mínima: 2
Capacidade máxima: 4
```

- [ ]  Em TAG Inclua abaixo

```markdown
Chave: Name
Valor: Blog-estudo-seu-nome-web-escalavel
```

- [ ]  Criar
- [ ]  Confira se você possui novos servidores de WEB provisionados (+2)

## **Conferindo e testando**

---

- [ ]  Veja se os servidores WEB estão conectados ao balanceador de cargas, todos devem estar como InService
- [ ]  Agora nós temos uma arquitetura Multi-AZ

## **DELETE TUDO**

---

- [ ]  Balanceador
- [ ]  Auto-Scaling
- [ ]  EFS
- [ ]  RDS
- [ ]  Elasticache
- [ ]  NAT Gateway em VPC