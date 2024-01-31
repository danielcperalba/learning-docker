# Docker Overview

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
