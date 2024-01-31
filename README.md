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
Aqui está a saída que o comando de build gera:

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

Você não consegue remover uma imagem se um contêiner ainda estiver usando a imagem, caso tente, é retornado uma mensagem de erro que lista o contêiner que depende da imagem.

## Como gerenciar contêineres do Docker?
Um contêiner do Docker tem um ciclo de vida que você pode utilizar para gerenciar e rastrear o estado do contêiner.

###Comandos:

`run` — Para colocar um contêiner em estado de execução. Também é possível reiniciar um contêiner que já esteja em execução. Ao reiniciar um contêiner, ele recebe um sinal de encerramento para habilitar qualquer processo em execução a ser desligado suavemente antes do encerramento do kernel do contêiner.
`pause` — Para pausar um contêiner em execução. Esse comando suspende todos os processos no contêiner.
`stop` — Para interromper um contêiner em execução. O comando stop permite desligar o processo de trabalho normalmente, enviando um sinal de encerramento à ele. O kernel do contêiner será encerrado após o desligamento do processo.
`kill` — Para enviar um sinal de encerramento. O kernel do contêiner captura o sinal de encerramento, mas o processo em execução não. Esse comando ecerra de maneira forçada o processo de trabalho no contêiner.
`remove` — Para remover os contêineres que estejam em um estado interrompido. Após remover um contêiner, todos os dados armazenados nele serão destruídos.

##Como exibir os contêineres disponíveis?
Para listar os contêineres em execução, utilize o comando ‘docker ps’. Use o argumento ‘-a’ para conferir contêineres em todos os estados.

```bash
docker ps -a
```
A saída deve parecer com esta:

```bash
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS                  PORTS                   NAMES
3a7bf214e93d   nome_da_imagem     "nginx -g 'daemon of…"   2 hours ago     Up 2 hours              0.0.0.0:8080->80/tcp    web_app
58f5c2cd7d22   ubuntu             "/bin/bash"              3 days ago      Exited (0) 2 days ago                           ubuntu_container
eac4f179b750   nginx              "nginx -g 'daemon of…"   3 weeks ago     Exited (137) 2 weeks ago                         nginx_container
```

Existem três itens que devem ser analisados na saída anterior:

O nome da imagem listado na columa IMAGE; neste exemplo, tmp-ubuntu:latest. Observe como é permitido criar mais de um contêiner da mesma imagem. Esse é um poderoso recurso de gerenciamento que você pode utilizar para habilitar a colocação em escala em suas soluções.

O status do contêiner listado na coluna STATUS. Neste exemplo, você tem um contêiner em execução e um contêiner encerrado.

O nome do contêiner listado na coluna NOMES. Neste exemplo, não fornecemos de modo explícito um nome para cada contêiner. Como resultado, o Docker deu um nome aleatório para o contêiner. Para dar um nome explícito a um contêiner usando o sinalizador ‘ — name’, utilize o comando ‘run’.

# Referências

- [Introdução aos contêineres do Docker — Training | Microsoft Learn]([https://link1](https://learn.microsoft.com/pt-br/training/modules/intro-to-docker-containers/))
- [Docker Cheat Sheet: Guia de Referência de Códigos e Comandos (hostinger.com.br)]([https://link2](https://www.hostinger.com.br/tutoriais/docker-cheat-sheet)https://www.hostinger.com.br/tutoriais/docker-cheat-sheet)
- [O que é o Docker? | IBM]([https://link3](https://www.ibm.com/br-pt/topics/docker)https://www.ibm.com/br-pt/topics/docker)
