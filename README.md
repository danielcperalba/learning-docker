# Uma visão sobre Docker: Conceitos, Arquitetura e Utilizações

Este artigo oferece uma visão sobre Docker, criado com o objetivo de me auxiliar nos estudos e servir de apoio para outros interessados nessa tecnologia. Todo o conteúdo foi feito a partir de resumos e anotações que achei relevantes nas documentações referenciadas ao final do texto.

## Pré-requisitos
- Conhecimento dos conceitos de virtualização do sistema operacional em um nível iniciante
- Conhecimento de aplicativos baseados em linha de comando em um nível iniciante

## Início
O processo de desenvolvimento e gerenciamento de aplicativos em uma empresa normalmente inclui uma ou mais equipes. Há uma equipe de desenvolvimento que cria o software e uma equipe de operações responsável por implantar esses aplicativos.

...

## O que é o Docker?
O Docker é uma plataforma de transporte em contêineres usada para desenvolver, enviar e executar contêineres. Ele não usa um hipervisor e pode ser executado na área de trabalho ou no laptop se estiver desenvolvendo e testando aplicativos.

...

## Objetos do Docker
O Docker Hub é um registro de contêiner de SaaS do Docker. Os registros são repositórios que usamos para armazenar e distribuir as imagens de contêiner que criamos. O Docker Hub é o registro público padrão usado pelo Docker para o gerenciamento de imagens.

...

## Como compilar uma imagem?
Usamos o comando ‘docker build’ para compilar imagens do Docker. Vamos supor que usamos a definição de Dockerfile anterior para compilar uma imagem. Aqui está um exemplo que mostra o comando build:

```bash
docker build -t nome_da_imagem:tag .
```

```bash
Step 1/7 : FROM ubuntu:latest
 ---> f643c72bc252
Step 2/7 : RUN apt-get update
 ---> Using cache
 ---> 1c1aef9e5483
Step 3/7 : RUN apt-get install -y nginx
 ---> Using cache
 ---> 6dc565a9404f
Step 4/7 : COPY ./app /app
 ---> Using cache
 ---> 7c4873a3d447
Step 5/7 : WORKDIR /app
 ---> Using cache
 ---> 027b7595dbf8
Step 6/7 : EXPOSE 80
 ---> Using cache
 ---> 32f18b5d9615
Step 7/7 : CMD ["nginx", "-g", "daemon off;"]
 ---> Using cache
 ---> 4e5021f88eba
Successfully built 4e5021f88eba
Successfully tagged nome_da_imagem:tag
```
Observe as etapas listadas na saída. Quando cada etapa é executada, uma nova camada é adicionada à imagem que estamos compilando.

Na etapa 2, executamos os comandos ‘apt -y update’ e ‘apt install -y’ para atualizar o sistema operacional.

## Como listar imagens?
O software do Docker configura automaticamente um Registro de imagem local no computador. Para listar as imagens desse registro use o comando ‘docker images’.

```bash
docker images
```

A saída deve se parecer com o seguinte exemplo:
```bash
REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
nome_da_imagem       tag       4e5021f88eba   2 hours ago    256MB
ubuntu               latest    f643c72bc252   2 weeks ago    72.9MB
nginx                latest    ae2feff98a0c   2 months ago   133MB
```

## Como remover uma imagem?
Para remover uma imagem usamos o comando ‘docker rmi’ e especifique o nome ou ID da imagem a ser removida. Isso nos ajuda a economizar espaço no disco do host do contêiner, pois as camadas de imagem de contêiner são somadas ao espaço total disponível.

```bash
docker rmi temp-ubuntu:version-1.0
```
