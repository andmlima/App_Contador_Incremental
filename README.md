# App_Contador_Incremental
That application has Node, database Redis and Ngnix. Aplicação tem um contador de acessos.

### Building aplication
## Prerequirement
Docker installed

## Download o código fonte
````
$ mkdir /path/
$ cd /path/
$ git clone https://github.com/andmlima/App_Contador_Incremental
$ cd App_Contador_Incremental
````
#### Container=REDIS
 Redis é um banco de dados não relacional
 
Iremos fazer o build da imagem do Redis para a nossa aplicação.
```sh
$ cd redis
$ docker build -t andmlima/redis:devops .
$ docker run -d --name redis -p 6379:6379 andmlima/redis:devops
$ docker ps
$ docker logs redis
```
Com isso temos o container do Redis rodando na porta 6379.

#### Container=NODEJS
Iremos fazer o build do container do NodeJs, que contém a nossa aplicação.
```sh
$ cd ../node
$ docker build -t andmlima/node:devops .
```
Agora iremos rodar a imagem do node, fazendo a ligação dela com o container do Redis.
```sh
$ docker run -d --name node -p 8080:8080 --link redis andmlima/node:devops
$ docker ps 
$ docker logs node
```
Com isso temos nossa aplicação rodando, e conectada no Redis. A api para verificação pode ser acessada em /redis.

#### Container=NGINX
Iremos fazer o build do container do nginx, que será nosso balanceador de carga.
Criando o container do nginx a partir da imagem
```sh
$ cd ../nginx
$ docker build -t andmlima/nginx:devops .
```
Fazendo a ligação com o container do Node
```sh
$ docker run -d --name nginx -p 80:80 --link node andmlima/nginx:devops
$ docker ps
```
Podemos acessar então nossa aplicação nas portas 80 e 8080 no ip da nossa instância.
```sh
$ http://localhost:8080/
```  
Iremos acessar a api em /redis varias vezes para validar o contador de acessos.
```sh
$ http://localhost:8080/redis
```




