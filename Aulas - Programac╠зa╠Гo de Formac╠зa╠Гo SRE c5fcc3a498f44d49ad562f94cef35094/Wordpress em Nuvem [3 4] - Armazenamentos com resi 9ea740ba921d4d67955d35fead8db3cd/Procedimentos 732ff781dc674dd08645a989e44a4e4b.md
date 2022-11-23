# Procedimentos

---

[O que é o Amazon EC2?](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/concepts.html)

[Amazon RDS for MySQL - Amazon Web Services (AWS)](https://aws.amazon.com/pt/rds/mysql/)

[O que é Amazon VPC?](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/what-is-amazon-vpc.html)

[Advanced Load Balancer, Web Server, & Reverse Proxy - NGINX](https://www.nginx.com/)

[memcached - a distributed memory object caching system](https://memcached.org/)

[Ferramenta de blog, plataforma de publicação e CMS - WordPress.org Brasil](https://br.wordpress.org/)

[O que é o Amazon ElastiCache for Memcached?](https://docs.aws.amazon.com/pt_br/AmazonElastiCache/latest/mem-ug/WhatIs.html)

[O que é o Amazon Elastic File System?](https://docs.aws.amazon.com/pt_br/efs/latest/ug/whatisefs.html)

## **Provisionando Elasticache**

---

- [ ]  Crie um Security Group na parte de EC2 com o nome: Blog-estudo-seu-nome-sessoes
- [ ]  Libere para que o Grupo de WEB possa entrar nele através da porta 11211
- [ ]  Crie um novo Grupo de sub-rede na parte de Elasticache
- [ ]  Nome: Cache-sessoes-estudos-<seu-nome>
- [ ]  Selecione sua VPC
- [ ]  Selecione suas 2 sub-redes privadas (caso precise, vá ao painel de VPC para consultar quais IDs são privados)
- [ ]  Crie um Elasticache com o nome: Blog-estudo-seu-nome-sessoes
- [ ]  Tecnologia: memcached
- [ ]  Tamanho do nó, selecione: cache.t3.micro
- [ ]  Quantidade de nós, selecione: 2
- [ ]  Posicionamento de zonas de disponibilidade: Nó 1 no AZ-A e Nó 2 no AZ-B
- [ ]  Security Group: Blog-estudo-seu-nome-sessoes
- [ ]  Criar

## **Alterando ponto de sessões**

---

- [ ]  Copie o "Endpoint de configuração”
- [ ]  De dentro do EC2 (VM), dê um telnet para testar a conexão: telnet endpoint 11211
- [ ]  Comando quit para sair
- [ ]  Edite no seu servidor de web o arquivo php.ini e ajuste agora a linha de 127.0.0.1 para o novo endpoint
- [ ]  Obs.: Ao copiar o endpoint, vem junto a porta, exemplo: [blog-estudo-almeida-sessoes.bpcpx7.cfg.usw2.cache.amazonaws.com:11211](http://blog-estudo-almeida-sessoes.bpcpx7.cfg.usw2.cache.amazonaws.com:11211/), no comando telnet isso pode dar falha, pois usa-se o espaço
- [ ]  Pronto, suas sessões não são mais armazenadas localmente

## **Provisionando Elastic File System(EFS)**

---

- [ ]  Altere sua VPC para suportar nomes DNSs: Vá em VPC e edite a opção "Editar nomes de host DNS", marcando como habilitado
- [ ]  Crie um Security Group com o nome: Blog-estudo-seu-nome-storage
- [ ]  Libere que o Grupo de WEB possa entrar com tráfego total
- [ ]  Vá no console de EFS e crie um sistema de arquivos
- [ ]  Nome: Blog-estudo-seu-nome-storage
- [ ]  Selecione sua VPC e deixe a opção de disponibilidade e durabilidade como: Regional
- [ ]  Vá em opções avançadas / Personalizar e desmarque "Habilitar backups automáticos”
- [ ]  Transição para IA e Transição fora do IA: nenhum
- [ ]  Criptografia desabilitado
- [ ]  Selecione as sub-redes e o security groups e crie

## **Montando pasta compartilhada**

---

- [ ]  Dentro da EC2/VM
- [ ]  Instale o pacote complementar de EFS no Linux Amazon: yum install amazon-efs-utils
- [ ]  Crie uma pasta /imagens: mkdir /imagens
- [ ]  No console do EFS, vá em anexar e copie o comando de montagem, altere o destino de efs para /imagens
- [ ]  Desmonte seu disco: umount /imagens
- [ ]  Coloque a montagem do disco em auto-inicialização: edite o arquivo /etc/fstab e inclua a linha abaixo
- [ ]  id do seu filesystem:/ /imagens efs defaults,nofail,_netdev 0 0
- [ ]  fs-0e8b1fa07d439a8e2:/ /imagens efs defaults,nofail,_netdev 0 0
- [ ]  Agora teste se a inicialização ocorre com sucesso: mount -a

## **Usando a pasta compartilhada**

---

- [ ]  Faça um link simbólico da sua pasta de imagens do wordpress
- [ ]  Vá no caminho: /var/www/html/blog e crie um link simbólico apontando para a /images
- [ ]  ln -s /imagens
- [ ]  Agora, suas imagens deverão ser salvas em uma pasta compartilhada entre todos os servidores

## **Multi-AZ do RDS**

---

- [ ]  Vá em RDS e selecione seu banco de dados criado para estudos
- [ ]  Vá na opção de modificação e na parte de “Implantação Multi-AZ” marque a opção “Criar uma instância em espera (recomendado para uso de produção)”
- [ ]  Selecione aplicar e ajuste para ser aplicado imediatamente