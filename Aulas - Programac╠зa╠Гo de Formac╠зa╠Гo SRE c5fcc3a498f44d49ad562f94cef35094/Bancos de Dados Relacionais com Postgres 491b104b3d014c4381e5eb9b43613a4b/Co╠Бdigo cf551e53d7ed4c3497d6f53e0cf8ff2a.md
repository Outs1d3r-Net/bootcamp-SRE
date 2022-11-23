# Código

---

## Instalar Postgres no Linux

---

```bash
#Adicionando repositório de pacotes:
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" >
/etc/apt/sources.list.d/pgdg.list'
#Importando o certificado
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
#Atualizando índice de pacotes
sudo apt-get update
#Instalando a versão mais recente do Postgres
sudo apt-get install postgresql
#Inicie o serviço do Postgres
sudo /etc/init.d/postgresql start
```

## Conectar Postgres com psql

---

```bash
#Instale o netstat para validar que o Postgres está em execução:
sudo apt install net-tools
#Pesquisando pela porta 5432:
netstat -an | grep LISTEN
#Mudando p/ usuário postgres e conectando ao banco pela linha de comando
sudo -i -u postgres
psql
```

```sql
#Execute um comando SELECT simples para validar o funcionamento:
postgres=# select 1;
```

## Restaurar a base de exemplo

---

```bash
#Saia do shell do postgres e do usuário postgres e baixe a base de dados de exemplo:
postgres=# exit
exit
logout
curl -L "https://elven.works/wp-content/uploads/2022/03/dvdrental.zip" > /tmp/dvdrental.zip
Adicione permissão de leitura ao arquivo para que o usuário postgres consiga processá-lo:
chmod a+r /tmp/dvdrental.zip
```

```bash
#Descompactando a base e conectando ao postgres pela linha de comando:
 sudo apt-get install unzip
 sudo -i -u postgres
 unzip /tmp/dvdrental.zip
 psql
```

```sql
#Crie uma base chamada dvdrental:
postgres=# create database dvdrental;
#Saia do terminal do postgres para restaurar a base:
postgres=# \q

```

```bash
#Restaure a base com o pg_restore:
pg_restore --dbname=dvdrental --verbose dvdrental.tar
```

## Algumas operações na base de exemplo

---

```sql
#Acessar console postgres com psql:
psql
#Mudar para a base dvdrental:
\c dvdrental
#You are now connected to database "dvdrental" as user "postgres".
dvdrental=#
#Veja as tabelas existentes:
dvdrental=# \dt
```

![Untitled](Co%CC%81digo%20cf551e53d7ed4c3497d6f53e0cf8ff2a/Untitled.png)

## Consultas

---

```sql
#Consulte o número de atores:
dvdrental=# select count(1) from actor;
#Consultar os primeiros 2 atores da lista:
dvdrental=# select * from actor limit 2;
#Consultar os primeiros 2 atores da lista em ordem alfabética:
dvdrental=# select * from actor order by first_name limit 2;
#Consultar os atores com primeiro nome começando com A:
dvdrental=# select * from actor where first_name like 'A%';
#Consulte as colunas da tabela de atores:
dvdrental=# \d actor
#Consulte as colunas da tabela de filmes:
dvdrental=# \d film;
#Consulte as colunas da tabela de relacionamento entre atores e filmes:
dvdrental=# \d film_actor;
#Consulte os filmes com um determinado ator:
dvdrental=# select f.title, concat(a.first_name,' ',a.last_name) as "actor",f.release_year, f.length from film f, actor a, film_actor fa where f.film_id =
fa.film_id and fa.actor_id = a.actor_id and a.actor_id = 1;
```

## Funções de agregação

---

```sql
#COUNT (contando filmes de um ator):
dvdrental=# select count(1) from film_actor where actor_id = 10;

#SUM (duração total dos filmes de um ator):
dvdrental=# select sum(f.length) from film f, actor a, film_actor fa where f.film_id = fa.film_id and fa.actor_id = a.actor_id and a.actor_id = 1;

#AVG (duração média dos filmes de um ator):
dvdrental=# select avg(f.length) from film f, actor a, film_actor fa where f.film_id = fa.film_id and fa.actor_id = a.actor_id and a.actor_id = 1;

#MAX/MIN (duração máxima e mínima dos filmes de um ator):
dvdrental=# select max(f.length), min(f.length) from film f, actor a, film_actor fa where f.film_id = fa.film_id and fa.actor_id = a.actor_id and a.actor_id =
1;
```

## Atualizações e Inserções

---

```sql
#Atualizando nome de um ator:
dvdrental=# update actor set first_name = 'Edward' where actor_id = 3;
#Consultando os dados atualizados do ator:
dvdrental=# select * from actor where actor_id = 3;
#Cadastrando um novo ator:
dvdrental=# insert into actor (first_name, last_name) values ('Bruno', 'Pereira');
dvdrental=# select * from actor where first_name = 'Bruno';
dvdrental=# \d actor;
```

## Deleções

---

```sql
#Removendo um ator:
dvdrental=# delete from actor where actor_id = 200;
ERROR: update or delete on table "actor" violates foreign key constraint "film_actor_actor_id_fkey" on table "film_actor"
DETAIL: Key (actor_id)=(200) is still referenced from table "film_actor".

#Verificar a integridade referencial
dvdrental=# \d film_a

#Removendo primeiro os filmes do ator e depois o ator:
dvdrental=# delete from film_actor where actor_id = 200;
dvdrental=# delete from actor where actor_id = 200;
```

## Transações

---

![Untitled](Co%CC%81digo%20cf551e53d7ed4c3497d6f53e0cf8ff2a/Untitled%201.png)

## Liberar conexões de rede

---

```bash
#Procurando o arquivo postgresql.conf:
sudo find / -name "postgresql.conf"
/etc/postgresql/14/main/postgresql.conf

#O nome da opção é listen_addresses, que provavelmente está comentada/desabilitada (linha iniciando com #)
#Com esta opção desabilitada, o servidor do Postgres aceita conexões somente do próprio servidor local onde está em execução.
#Remova o # do começo da linha e coloque a opção listen_addresses = ‘*’. Com isso o postgres aceitará conexões em todos os endereços de rede do servidor
#Salve o arquivo e reinicie o postgres:
sudo /etc/init.d/postgresql restart

#Confirme que o postgres está escutando em todos os endereços:
netstat -an | grep LISTEN

```

![Untitled](Co%CC%81digo%20cf551e53d7ed4c3497d6f53e0cf8ff2a/Untitled%202.png)

## Conectar com o pgAdmin

---

```bash
#Faça download do pgAdmin para seu sistema operacional em: https://www.pgadmin.org/download/
#Defina uma senha para o usuário postgres no banco de dados:
sudo -i -u postgres
psql
postgres=# ALTER USER postgres WITH PASSWORD 'postgres';
ALTER ROLE

#Configure o postgres para permitir conexões com senha:
logout
sudo vim /etc/postgresql/14/main/pg_hba.conf
#Modifique a linha do IPv4 local connections para que fique conforme a seguir:
host all all 0.0.0.0/0 md5

#Reinicie o servidor do postgres e conecte pelo pgAdmin no endereço de rede local
sudo /etc/init.d/postgresql restart
```

![Untitled](Co%CC%81digo%20cf551e53d7ed4c3497d6f53e0cf8ff2a/Untitled%203.png)

## Fazer backup da base

---

```bash
#Acesse o usuário postgres:
sudo -i -u postgres
pg_dump dvdrental > dvd-rental-04042022.sql
```