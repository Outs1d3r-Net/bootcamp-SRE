# Código

---

## Instalar Docker

---

### Remover as versões antigas

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Instalar pacotes complementares

```bash
sudo apt-get install ca-certificates curl gnupg lsb-release
```

### Adicionar a chave GPG oficial do Docker

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o
/usr/share/keyrings/docker-archive-keyring.gpg
```

### Adicionar o repositório do docker na source list da distro Ubuntu

```bash
echo \
 "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]
https://download.docker.com/linux/ubuntu \
 $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Instalar

```bash
apt-get update && apt-get install docker-ce docker-ce-cli containerd.io
docker -v
```

![Untitled](Co%CC%81digo%20616781cc5beb4c9ea9204a7df4cad8bf/Untitled.png)

### Instalar em uma linha

```bash
wget -qO- https://get.docker.com/ | sh
```

### Adicione seu user no grupo do docker

```bash
sudo usermod -aG docker <nome usuario>
```

### Validar instalação

```bash
docker run hello-world
```

## Docker Pull e Images

---

```bash
docker pull ubuntu:20.04 - Download da imagem do registry publico(docker hub)
docker images - Lista as imagens baixadas
docker images --no-trunc → Lista os IDs das images completos
docker images --filter "dangling=true" → Lista as images sem tags
docker pull myregistry.local:5000/testing/test-image - Download da imagem do registry privado
Para remover as imagens rode docker rmi -f IMAGE ID
```

## Docker RUN

---

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
docker run --name elven-teste ubuntu:20.04
docker run --name elven-teste2 ubuntu:20.04 /bin/echo 'Bem vindo!!'
docker ps
docker ps -a
docker run --name elven-test -it ubuntu:20.04
docker run --name elven-test -it -d ubuntu:20.04
docker ps
```

## Docker PS e RM

---

```bash
docker ps ou docker container ls → mostra os containers que estão em execução
docker ps -a → Mostra todos os containers sem exceção
docker rm CONTAINER ID → Remove o container
docker rmi IMAGE ID → Remove a imagem
docker rm -f → Força a remoção do container
```

## Docker Portas

---

```bash
docker run --name nginx3 -d -p80:80 nginx
```

## Docker Volumes

---

```bash
#Volume nomeado
–mount 'type=volume,source=nginx-vol,target=/etc/nginx,readonly'
-v ou --volume nginx-vol:/etc/nginx:ro
docker volume ls
docker volume create nginx-vol
docker run --name nginx4 -v nginx-vol:/usr/share/nginx/html/:ro -d -p81:80 nginx

#Bind
-v $(pwd)/index.html:/usr/share/nginx/html/index.html

docker container cp nginx2:/usr/share/nginx/html/index.html .
docker run --name nginx3 -v $(pwd)/index.html:/usr/share/nginx/html/index.html -d -p80:80 nginx

docker volume prune
```

## Docker Commit e Push

---

```bash
docker commit CONTAINER ID userdockerhub/imagename:version1
docker commit b08b4a5269ba userdockerhub/newimage:version1 → Gera uma nova versão de 
sua imagem

docker login → Autenticação com o docker hub

docker tag userdockerhub/newimage:version1 userdockerhub/newimage:version1 → Tag em sua nova imagem

docker image push userdockerhub/newimage:version1 → Envio para o Registry docker

docker pull userdockerhub/newimage:version1 → Baixando a nova imagem
```

## Docker Inspect

---

```bash
docker inspect CONTAINER ID
```