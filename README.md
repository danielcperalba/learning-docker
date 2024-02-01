# Uma visão sobre Docker: Conceitos, Arquitetura e Utilizações

Este artigo oferece uma visão sobre Docker, criado com o objetivo de me auxiliar nos estudos e servir de apoio para outros interessados nessa tecnologia. Todo o conteúdo foi feito a partir de resumos e anotações que achei relevantes nas documentações referenciadas ao final do texto.

## Pré-requisitos
- Conhecimento dos conceitos de virtualização do sistema operacional em um nível iniciante
- Conhecimento de aplicativos baseados em linha de comando em um nível iniciante

## Início
O processo de desenvolvimento e gerenciamento de aplicativos em uma empresa normalmente inclui uma ou mais equipes. Há uma equipe de desenvolvimento que cria o software e uma equipe de operações responsável por implantar esses aplicativos. A equipe de operações também é responsável por gerenciar a infraestrutura de hospedagem de aplicativos.

Por exemplo, suponha que estejamos desenvolvendo um portal de acompanhamento de pedidos que será usado pelas várias lojas de nossa empresa. Vários ambientes hospedam nossos aplicativos durante o processo de desenvolvimento e publicação do aplicativo. Primeiro, a equipe de desenvolvimento desenvolve e testa o software em um ambiente de desenvolvimento. Daqui, o software é então implantado em um ambiente de QA (garantia de qualidade), seguido por pré-produção e um ambiente de produção final.

Existem vários desafios que precisaremos considerar no cenário anterior:

###Gerenciar ambientes de hospedagem
Todos os diferentes ambientes exigem o gerenciamento de software e hardware. Precisamos garantir que o software instalado e o hardware configurado em cada um deles sejam os mesmos. Também precisamos configurar aspectos como acesso à rede, armazenamento de dados e segurança por ambiente de maneira consistente e facilmente reproduzível.

###Continuidade na entrega de software
A implantação de aplicativos em nossos ambientes deve acontecer de forma consistente. Cada pacote de implantação precisa incluir todos os pacotes do sistema, binários, bibliotecas, arquivos de configuração e outros itens que garantem um aplicativo totalmente funcional. Também precisamos garantir que todas essas dependências sejam correspondentes às versões e à arquitetura de software.

###Uso eficiente de hardware
Cada aplicativo implantado precisa ser executado de modo que seja isolado de outros aplicativos em execução no mesmo hardware. Nosso objetivo é executar mais de um aplicativo por servidor para fazer o melhor uso dos recursos sem comprometer uns aos outros.

###Portabilidade do aplicativo
Há várias razões pelas quais a portabilidade do aplicativo é essencial. Um ambiente de hospedagem pode falhar ou talvez precisamos expandir nosso aplicativo. Em ambos os casos, o resultado potencial é a reimplantação do nosso software em um novo ambiente.

Desejamos mover o software de um host para outro, mesmo se a infraestrutura subjacente for diferente. Essa movimentação precisa ocorrer o mais rápido possível, a fim de reduzir o tempo de inatividade para nossos clientes.

## O que é o Docker?
Docker é um software livre desenvolvido pela Docker Inc. Ele foi apresentado ao público em geral em 13 de março de 2013 e se tornou desde então algo necessário no mundo do desenvolvimento em tecnologia.

Ele permite que os usuários criem ambientes de desenvolvimento independentes e isolados para lançar e implantar suas aplicações. Esses ambientes são, então, chamados de contêineres (containers, em inglês).

Isso permitirá ao desenvolvedor executar um contêiner em qualquer máquina.

Como você pode ver, com o Docker, não há mais problemas de dependências ou de compilação. Tudo o que você precisa é lançar seu contêiner e a aplicação iniciará imediatamente.

## O que é um contêiner?
Um contêiner é uma instância executável de uma imagem. Você pode criar, iniciar, parar, mover ou excluir um contêiner usando a API ou a CLI do Docker. Você pode conectar um contêiner para uma ou mais redes, anexar armazenamento a ele ou até mesmo criar um novo imagem baseada em seu estado atual.

Um contêiner é definido por sua imagem, bem como quaisquer opções de configuração que você forneça-o quando você criá-lo ou iniciá-lo. Quando um contêiner é removido, quaisquer alterações para seu estado que não é armazenado no armazenamento persistente desaparece.

...

## O que é uma imagem?
As imagens do Docker contêm um código-fonte de aplicativo executável, além de todas as ferramentas, bibliotecas e estrutura que esse código precisa para ser executado como um contêiner. Ao executar a imagem do Docker, ela se torna uma instância (ou várias instâncias) do contêiner.

As imagens do Docker são compostas de camadas que correspondem individualmente a uma versão da imagem. Sempre que um desenvolvedor faz mudanças na imagem, uma nova camada superior é criada, substituindo a anterior pela versão atual da imagem. As camadas anteriores são salvas caso seja necessário retroceder para versões anteriores ou reutilização em outros projetos.

## O que é um Dockerfile?
Todo contêiner do Docker começa com um arquivo de texto simples que contém instruções sobre como desenvolver a imagem do contêiner do Docker. O DockerFile automatiza o processo de criação de imagem do Docker. Ele consiste essencialmente em uma lista de instruções da interface da linha de comandos (CLI) que são executadas pelo Docker Engine com o intuito de montar a imagem. A lista de comandos do Docker é extensa, mas padronizada: as operações do Docker funcionam sempre da mesma forma independentemente do conteúdo, da infraestrutura ou outras variáveis de ambiente.

O Dockerfile pode ser semelhante ao exemplo a seguir:

```bash
# Step 1: Sepecify the parent image for the new image
From ubuntu:18.04
# Step 2: Update OS packages and install additional software
RUN apt -y update &&  apt install -y wget nginx software-properties-common apt-transport-https \
	&& wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
	&& dpkg -i packages-microsoft-prod.deb \
	&& add-apt-repository universe \
	&& apt -y update \
	&& apt install -y dotnet-sdk-3.0
# Step 3: Configure Nginx environment
CMD service nginx start
# Step 4: Configure Nginx environment
COPY ./default /etc/nginx/sites-available/default
# STEP 5: Configure work directory
WORKDIR /app
# STEP 6: Copy website code to container
COPY ./website/. .
# STEP 7: Configure network requirements
EXPOSE 80:8080
# STEP 8: Define the entry point of the process that runs in the container
ENTRYPOINT ["dotnet", "website.dll"]
```

## Arquitetura Docker
O Docker usa uma arquitetura cliente-servidor. O cliente do Docker conversa com o Docker daemon, que faz o trabalho pesado de construção, execução e distribuindo seus contêineres do Docker. O cliente e o daemon do Docker podem executar no mesmo sistema ou você pode conectar um cliente do Docker a um Docker remoto Dimons. O cliente e o daemon do Docker se comunicam usando uma API REST, via UNIX soquetes ou uma interface de rede. Outro cliente do Docker é o Docker Compose, que permite trabalhar com aplicativos que consistem em um conjunto de contêineres.

...

## Registro do Docker
Um registro do Docker é um sistema escalável de armazenamento e distribuição de software livre para imagens do Docker. Ele permite rastrear versões de imagens em repositórios com a identificação por meio de tags. Para isso, ele usa o git, uma ferramenta de controle de versão.

## Daemon do Docker
O Daemon do Docker é um serviço que cria e gerencia imagens do Docker, usando os comandos do cliente. Essencialmente, o daemon do Docker serve como o centro de controle da sua implementação do Docker. O servidor no qual o daemon do Docker é executado é chamado de host do Docker.

## O Cliente do Docker
O cliente possui duas alternativas, um aplicativo de linha de comando chamado docker ou um aplicativo de interface gráfica chamado Docker Desktop. Eles interagem com um servidor do Docker. Usam a API REST do Docker para enviar instruções para um servidor local ou remoto e funcionam como a interface primária que usamos para gerenciar os contêineres.

# Criando a primeira aplicação

## Configurar nosso ambiente
Não irei incluir neste artigo sobre como preparar o Windows para contêiners. Para saber mais sobre este tópico acesse este link: [Preparar contêineres do sistema operacional Windows | Microsoft Learn](https://learn.microsoft.com/pt-br/virtualization/windowscontainers/quick-start/set-up-environment?tabs=dockerce)

# Executando nosso primeiro contêiner

## Efetuar pull de uma imagem base do contêiner
A Microsoft ofecere várias imagens inicias, chamadas de imagens base, entre as quais escolher (para obter mais detalhes, confira Imagens base do contêiner). Esse procedimento efetua pull (baixar e instalar) da imagem base leve do Nano Server.

1. Abra uma janela de prompt de comando e, em seguida, execute o seguinte comando para baixar e instalar a imagem base:

```bash
docker pull mcr.microsoft.com/windows/nanoserver:ltsc2022
```

Se o Docker não iniciar depois da tentativa de efetuar pull da imagem, o daemon do Docker poderá ficar inacessível. Para resolver esse problema tente reiniciar o serviço do Docker.

2. Após a conclusão do download da imagem — leia o EULA enquanto espera — , verifique sua existência no sistema consultando o repositório local de imagens do Docker. Executar o comando docker images retorna uma lista de imagens instaladas.

```bash
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
microsoft/nanoserver   latest              105d76d0f40e        4
```

## Executar um contêiner do Windows
Para este exemplo simples, uma imagem de contêiner “Hello, World!” será criada e implantada. Para ter a melhor experiência, execute estes comandos em uma janela de prompt de comandos com privilégios elevados (mas não use o ISE do Windows PowerShell, pois ele não funciona para sessões interativas com contêineres, uma vez que os contêineres parecem travar).

1. Inicie um contêiner com uma sessão interativa na imagem nanoserver inserindo o seguinte comando na janela do prompt de comando:

```bash
docker run -it mcr.microsoft.com/windows/nanoserver:ltsc2022 cmd.exe
```

2. Após o contêiner ser iniciado, a janela de prompt de comando altera o contexto para o contêiner. Dentro do contêiner, criaremos um arquivo de texto “Olá, Mundo” simples e, em seguida, sairemos do contêiner inserindo os seguintes comandos:

```bash
echo "Hello World!" > Hello.txt
exit
```

3. Obtenha a ID do contêiner que você acabou de fechar executando o comando docker ps:

```bash
docker ps -a
```

4. Crie uma imagem ‘HelloWorld’ que inclui as alterações no primeiro contêiner que você executou. Para fazer isso, execute o comando docker commit, substituindo <containerid> pela ID do seu contêiner:

```bash
docker commit <containerid> helloworld
```

Quando tiver concluído, você terá uma imagem personalizada que contém o script hello world. Ele pode ser visto usando o comando docker images.

```bash
docker images
```

Veja um exemplo de saída:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
helloworld                             latest              a1064f2ec798        10 seconds ago      258MB
mcr.microsoft.com/windows/nanoserver   2022                2b9c381
```

5. Por fim, execute o novo contêiner usando o comando docker run com o parâmetro --rm, que remove automaticamente o contêiner quando a linha de comando (cmd.exe) para.

```bash
docker run --rm helloworld cmd.exe /s /c type Hello.txt
```

O resultado é que o Docker criou um contêiner com base na imagem ‘HelloWorld’, iniciou uma instância de cmd.exe no contêiner, e cmd.exe leu o nosso arquivo e gerou os conteúdos para o shell. Como última etapa, o Docker parou e removeu o contêiner.

# Referências

- [Introdução aos contêineres do Docker — Training | Microsoft Learn]([https://link1](https://learn.microsoft.com/pt-br/training/modules/intro-to-docker-containers/))
- [Docker Cheat Sheet: Guia de Referência de Códigos e Comandos (hostinger.com.br)]([https://link2](https://www.hostinger.com.br/tutoriais/docker-cheat-sheet)https://www.hostinger.com.br/tutoriais/docker-cheat-sheet)
- [O que é o Docker? | IBM]([https://link3](https://www.ibm.com/br-pt/topics/docker)https://www.ibm.com/br-pt/topics/docker)
- [Visão geral do Docker | Documentos do Docker](https://docs.docker.com/get-started/overview/)
- [Um guia para iniciantes em Docker — como criar sua primeira aplicação com o Docker (freecodecamp.org)](https://www.freecodecamp.org/portuguese/news/um-guia-para-iniciantes-em-docker-como-criar-sua-primeira-aplicacao-com-o-docker/)
- [Executar o primeiro contêiner do Windows | Microsoft Learn](https://learn.microsoft.com/pt-br/virtualization/windowscontainers/quick-start/run-your-first-container)
