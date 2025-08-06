# Manual Básico de Docker

## Sumário

- [Introdução ao Docker](#introdução-ao-docker)
  - [Por que usar Docker?](#por-que-usar-docker)
- [Imagens e Contêineres: Os Blocos Construtores do Docker](#imagens-e-contêineres-os-blocos-construtores-do-docker)
  - [Imagens Docker](#imagens-docker)
  - [Contêineres Docker](#contêineres-docker)
- [Comandos Essenciais do Docker](#comandos-essenciais-do-docker)
  - [`docker pull` - Baixando Imagens](#docker-pull---baixando-imagens)
  - [`docker run` - Executando Contêineres](#docker-run---executando-contêineres)
  - [`docker stop`, `docker start`, `docker pause`, `docker unpause` - Gerenciando o Ciclo de Vida do Contêiner](#docker-stop-docker-start-docker-pause-docker-unpause---gerenciando-o-ciclo-de-vida-do-contêiner)
  - [`docker exec` - Acessando o Terminal de um Contêiner em Execução](#docker-exec---acessando-o-terminal-de-um-contêiner-em-execução)
  - [`docker ps` - Listando Contêineres](#docker-ps---listando-contêineres)
  - [`docker rm` - Removendo Contêineres](#docker-rm---removendo-contêineres)
  - [`docker image ls` - Listando Imagens](#docker-image-ls---listando-imagens)
  - [`docker rmi` - Removendo Imagens](#docker-rmi---removendo-imagens)
- [Dockerfile - Construindo Suas Próprias Imagens](#dockerfile---construindo-suas-próprias-imagens)
  - [Estrutura Básica de um Dockerfile](#estrutura-básica-de-um-dockerfile)
  - [Principais Instruções do Dockerfile](#principais-instruções-do-dockerfile)
  - [Boas Práticas para Dockerfiles](#boas-práticas-para-dockerfiles)
- [`docker build` - Construindo Imagens a Partir de um Dockerfile](#docker-build---construindo-imagens-a-partir-de-um-dockerfile)
- [Docker Hub - Compartilhando Suas Imagens](#docker-hub---compartilhando-suas-imagens)
  - [`docker login` - Autenticando-se no Docker Hub](#docker-login---autenticando-se-no-docker-hub)
  - [`docker push` - Enviando Imagens para o Docker Hub](#docker-push---enviando-imagens-para-o-docker-hub)
- [Comandos Úteis Adicionais](#comandos-úteis-adicionais)
  - [`docker logs` - Visualizando Logs de Contêineres](#docker-logs---visualizando-logs-de-contêineres)
  - [`docker inspect` - Obtendo Informações Detalhadas de Objetos Docker](#docker-inspect---obtendo-informações-detalhadas-de-objetos-docker)
  - [`docker network` - Gerenciando Redes Docker](#docker-network---gerenciando-redes-docker)
- [Docker Volumes - Persistência de Dados](#docker-volumes---persistência-de-dados)
  - [Tipos de Volumes](#tipos-de-volumes)
  - [Gerenciando Volumes Nomeados](#gerenciando-volumes-nomeados)
  - [Usando Volumes com Contêineres](#usando-volumes-com-contêineres)
- [Dicas e Boas Práticas para o Uso do Docker](#dicas-e-boas-práticas-para-o-uso-do-docker)
- [Recursos Adicionais e Links Úteis](#recursos-adicionais-e-links-úteis)
  - [Documentação Oficial](#documentação-oficial)
  - [Tutoriais e Guias](#tutoriais-e-guias)
  - [Ferramentas Úteis](#ferramentas-úteis)
  - [Comandos de Referência Rápida](#comandos-de-referência-rápida)

## Introdução ao Docker

O Docker é uma plataforma de código aberto que automatiza a implantação, escala e gerenciamento de aplicações usando contêineres. Contêineres são unidades padronizadas de software que empacotam o código da sua aplicação e todas as suas dependências, permitindo que a aplicação seja executada de forma rápida e confiável em qualquer ambiente de computação. Isso resolve o problema comum de "funciona na minha máquina", garantindo consistência entre diferentes ambientes de desenvolvimento, teste e produção.

### Por que usar Docker?

- **Consistência:** Garante que sua aplicação funcione da mesma forma em qualquer lugar, eliminando problemas de compatibilidade de ambiente.
- **Isolamento:** Cada contêiner é isolado do sistema hospedeiro e de outros contêineres, o que aumenta a segurança e evita conflitos de dependências.
- **Portabilidade:** Contêineres podem ser facilmente movidos entre diferentes máquinas e sistemas operacionais.
- **Eficiência:** Contêineres são leves e iniciam rapidamente, utilizando menos recursos do sistema do que máquinas virtuais tradicionais.
- **Escalabilidade:** Facilita a replicação e o gerenciamento de múltiplas instâncias de uma aplicação para lidar com o aumento da demanda.

Em resumo, o Docker simplifica o ciclo de vida de desenvolvimento de software, desde a codificação até a implantação, tornando-o uma ferramenta essencial para desenvolvedores e equipes de DevOps.



## Imagens e Contêineres: Os Blocos Construtores do Docker

Para entender o Docker, é fundamental compreender dois conceitos principais: **Imagens** e **Contêineres**.

### Imagens Docker

Uma **Imagem Docker** é um template leve, autônomo e executável que inclui tudo o que é necessário para executar uma aplicação: o código, um runtime, bibliotecas, variáveis de ambiente e arquivos de configuração. Pense em uma imagem como um "snapshot" de um sistema de arquivos que contém uma aplicação pronta para ser executada. As imagens são construídas a partir de um `Dockerfile`, que é um arquivo de texto com instruções sobre como construir a imagem.

- **Camadas (Layers):** Imagens são construídas em camadas. Cada instrução em um `Dockerfile` cria uma nova camada. Isso torna as imagens muito eficientes, pois as camadas podem ser compartilhadas entre diferentes imagens e apenas as camadas alteradas precisam ser reconstruídas ou baixadas.
- **Repositórios:** Imagens são armazenadas em repositórios, como o Docker Hub, que funciona como um registro público de imagens Docker. Você pode baixar imagens existentes (como `ubuntu`, `nginx`, `node`) ou fazer upload das suas próprias imagens.

### Contêineres Docker

Um **Contêiner Docker** é uma instância executável de uma imagem. Quando você "roda" uma imagem, você está criando um contêiner. Um contêiner é um processo isolado que compartilha o kernel do sistema operacional hospedeiro, mas possui seu próprio sistema de arquivos, rede e processos. É o ambiente onde sua aplicação realmente roda.

- **Isolamento:** Cada contêiner é isolado dos outros contêineres e do sistema hospedeiro. Isso significa que as dependências de uma aplicação em um contêiner não interferem nas dependências de outra aplicação em outro contêiner ou no próprio sistema hospedeiro.
- **Efemeridade:** Contêineres são projetados para serem efêmeros. Você pode iniciá-los, pará-los, removê-los e recriá-los facilmente. Quaisquer alterações feitas dentro de um contêiner que não sejam persistidas em um volume serão perdidas quando o contêiner for removido.

Em resumo, uma **imagem** é a receita ou o molde, e um **contêiner** é a instância viva e em execução dessa receita. Você pode ter várias instâncias (contêineres) da mesma imagem rodando simultaneamente.



## Comandos Essenciais do Docker

Esta seção detalha os comandos mais utilizados no dia a dia com Docker, explicando suas funcionalidades e fornecendo exemplos práticos.

### `docker pull` - Baixando Imagens

O comando `docker pull` é utilizado para baixar imagens de um registro, como o Docker Hub, para o seu ambiente local. É o primeiro passo para obter as bases necessárias para construir e executar seus contêineres.

**Sintaxe:**
```bash
docker pull [OPÇÕES] NOME_DA_IMAGEM[:TAG]
```

- `NOME_DA_IMAGEM`: O nome da imagem que você deseja baixar (ex: `ubuntu`, `nginx`, `node`).
- `TAG`: (Opcional) A versão específica da imagem. Se não for especificada, o Docker assume a tag `latest` (mais recente) por padrão.

**Exemplos:**

- **Baixar a imagem mais recente do Ubuntu:**
```bash
docker pull ubuntu
```

- **Baixar uma versão específica do Node.js (por exemplo, a versão 18):**
```bash
docker pull node:18
```

- **Baixar a imagem oficial do Nginx:**
```bash
docker pull nginx
```

Ao executar este comando, o Docker verifica se a imagem já existe localmente. Se não existir ou se houver uma versão mais recente disponível no registro, ele fará o download de todas as camadas necessárias para a imagem. Você verá o progresso do download no terminal.



### `docker run` - Executando Contêineres

O comando `docker run` é um dos mais importantes, pois ele cria e inicia um novo contêiner a partir de uma imagem. Ele combina as funcionalidades de `docker create` e `docker start`.

**Sintaxe:**
```bash
docker run [OPÇÕES] IMAGEM [COMANDO] [ARGUMENTOS]
```

- `IMAGEM`: O nome da imagem a partir da qual o contêiner será criado e executado.
- `COMANDO`: (Opcional) O comando a ser executado dentro do contêiner. Se não for especificado, o Docker usará o comando padrão definido na imagem (via `CMD` ou `ENTRYPOINT` no Dockerfile).
- `ARGUMENTOS`: (Opcional) Argumentos para o comando.

**Opções Comuns:**

- `-d` ou `--detach`: Executa o contêiner em segundo plano (modo *detached*), liberando o terminal. Se você não usar essa flag, o contêiner será executado em primeiro plano e o terminal ficará anexado à saída do contêiner.
- `--name NOME_DO_CONTÊINER`: Atribui um nome específico ao contêiner. Se não for fornecido, o Docker gerará um nome aleatório. Nomes facilitam a identificação e o gerenciamento de contêineres.
- `-p HOST_PORTA:CONTÊINER_PORTA` ou `--publish HOST_PORTA:CONTÊINER_PORTA`: Mapeia uma porta do seu sistema hospedeiro para uma porta dentro do contêiner. Essencial para acessar serviços rodando dentro do contêiner a partir do seu navegador ou de outras aplicações externas.
- `-it` ou `--interactive --tty`: Combinação de flags para alocar um pseudo-TTY e manter o STDIN aberto, permitindo a interação com o contêiner (por exemplo, para acessar o terminal de um sistema operacional).
- `--rm`: Remove automaticamente o contêiner quando ele é encerrado. Útil para contêineres temporários ou de teste.
- `-v HOST_CAMINHO:CONTÊINER_CAMINHO` ou `--volume HOST_CAMINHO:CONTÊINER_CAMINHO`: Monta um volume (diretório ou arquivo) do sistema hospedeiro dentro do contêiner, permitindo a persistência de dados e o compartilhamento de arquivos.

**Exemplos:**

- **Rodar um contêiner Ubuntu em segundo plano com um nome específico e mantê-lo ativo por um dia:**
```bash
docker run -d --name meu_ubuntu ubuntu sleep 1d
```
  *Explicação:* O comando `sleep 1d` faz com que o contêiner Ubuntu permaneça em execução por um dia. Isso é útil para contêineres que não possuem uma aplicação principal que os mantenha ativos (como um servidor web).

- **Rodar um servidor Nginx e mapear a porta 80 do contêiner para a porta 8080 do hospedeiro:**
```bash
docker run -d -p 8080:80 --name meu_nginx nginx
```
  *Explicação:* Após executar este comando, você poderá acessar o servidor Nginx no seu navegador através de `http://localhost:8080`.

- **Iniciar um terminal interativo dentro de um contêiner Ubuntu:**
```bash
docker run -it ubuntu bash
```
  *Explicação:* Isso permite que você execute comandos diretamente dentro do ambiente do contêiner Ubuntu. Para sair do terminal do contêiner, digite `exit` ou pressione `Ctrl+D`.

- **Rodar uma aplicação Node.js e mapear a porta:**
```bash
docker run -d -p 8080:3000 minhaApi:1.0
```
  *Explicação:* Sua aplicação `minhaApi` (versão 1.0) que escuta na porta 3000 dentro do contêiner estará acessível na porta 8080 do seu sistema hospedeiro. Você pode acessá-la via `http://localhost:8080` no seu navegador. Se estiver usando uma VM ou WSL, pode ser necessário usar o IP da sua máquina virtual (obtido com `hostname -I`) em vez de `localhost`.



### `docker stop`, `docker start`, `docker pause`, `docker unpause` - Gerenciando o Ciclo de Vida do Contêiner

Esses comandos permitem controlar o estado de execução dos seus contêineres.

**Sintaxe:**
```bash
docker [stop|start|pause|unpause] NOME_OU_ID_DO_CONTÊINER [...NOME_OU_ID_DO_CONTÊINER]
```

- `NOME_OU_ID_DO_CONTÊINER`: O nome ou o ID do contêiner que você deseja gerenciar. Você pode especificar múltiplos contêineres.

**Comandos:**

- **`docker stop`**: Interrompe um ou mais contêineres em execução. O Docker envia um sinal `SIGTERM` (solicitação de encerramento) e, após um tempo limite (padrão de 10 segundos), um `SIGKILL` (encerramento forçado) se o contêiner não parar graciosamente.
  **Exemplo:**
  ```bash
  docker stop meu_nginx
  ```

- **`docker start`**: Inicia um ou mais contêineres que foram previamente parados.
  **Exemplo:**
  ```bash
  docker start meu_ubuntu
  ```

- **`docker pause`**: Pausa todos os processos dentro de um ou mais contêineres em execução. Isso "congela" o contêiner, suspendendo todos os seus processos.
  **Exemplo:**
  ```bash
  docker pause meu_aplicacao
  ```

- **`docker unpause`**: Despausa um ou mais contêineres que foram previamente pausados, retomando seus processos.
  **Exemplo:**
  ```bash
  docker unpause meu_aplicacao
  ```

É importante notar que `stop` e `start` são usados para iniciar e parar contêineres, enquanto `pause` e `unpause` são usados para suspender e retomar processos dentro de contêineres já em execução, sem realmente pará-los.



### `docker exec` - Acessando o Terminal de um Contêiner em Execução

O comando `docker exec` permite que você execute um comando dentro de um contêiner que já está em execução. É frequentemente usado para acessar o terminal de um contêiner e interagir com ele, como se estivesse fazendo login em uma máquina virtual.

**Sintaxe:**
```bash
docker exec [OPÇÕES] NOME_OU_ID_DO_CONTÊINER COMANDO [ARGUMENTOS]
```

- `NOME_OU_ID_DO_CONTÊINER`: O nome ou o ID do contêiner onde o comando será executado.
- `COMANDO`: O comando a ser executado dentro do contêiner (ex: `bash`, `sh`, `ls`, `ps`).
- `ARGUMENTOS`: (Opcional) Argumentos para o comando.

**Opções Comuns:**

- `-i` ou `--interactive`: Mantém o STDIN aberto, mesmo que não esteja anexado. Essencial para interagir com o terminal.
- `-t` ou `--tty`: Aloca um pseudo-TTY. Cria uma interface de terminal para o contêiner, permitindo que você digite comandos e veja a saída formatada.

**Exemplo:**

- **Acessar o terminal Bash de um contêiner chamado `meu_ubuntu`:**
```bash
docker exec -it meu_ubuntu bash
```
  *Explicação:* Este comando abrirá um terminal interativo dentro do contêiner `meu_ubuntu`. Você poderá navegar pelo sistema de arquivos do contêiner, instalar pacotes, executar scripts, etc. Para sair do terminal do contêiner, digite `exit` ou pressione `Ctrl+D`. É importante lembrar que você está usando o ambiente do contêiner, não o seu sistema hospedeiro diretamente.



### `docker ps` - Listando Contêineres

O comando `docker ps` é usado para listar os contêineres Docker. Ele fornece informações sobre os contêineres em execução e, com opções adicionais, também sobre os contêineres parados.

**Sintaxe:**
```bash
docker ps [OPÇÕES]
```

**Opções Comuns:**

- `-a` ou `--all`: Mostra todos os contêineres, incluindo aqueles que estão parados (não em execução). Sem esta flag, apenas os contêineres em execução são exibidos.
- `-s` ou `--size`: Exibe o tamanho total do arquivo de cada contêiner.
- `-q` ou `--quiet`: Exibe apenas os IDs numéricos dos contêineres, útil para automação ou para encadear comandos.
- `-f` ou `--filter FILTRO`: Filtra a saída com base em condições específicas (ex: `status=exited`, `name=meu_container`).

**Exemplos:**

- **Listar todos os contêineres (em execução e parados):**
```bash
docker ps -a
```
  *Explicação:* Esta é a forma mais comum de verificar o estado de todos os seus contêineres, incluindo aqueles que foram encerrados ou que falharam ao iniciar.

- **Listar apenas os contêineres em execução:**
```bash
docker ps
```

- **Listar apenas os IDs dos contêineres em execução:**
```bash
docker ps -q
```

- **Listar contêineres parados com o status 'Exited':**
```bash
docker ps -a -f status=exited
```

A saída do `docker ps` geralmente inclui colunas como `CONTAINER ID`, `IMAGE`, `COMMAND`, `CREATED`, `STATUS`, `PORTS` e `NAMES`, fornecendo um resumo rápido do estado dos seus contêineres.



### `docker rm` - Removendo Contêineres

O comando `docker rm` é usado para remover um ou mais contêineres. Contêineres que estão em execução não podem ser removidos diretamente; eles precisam ser parados primeiro, a menos que a flag `--force` seja utilizada.

**Sintaxe:**
```bash
docker rm [OPÇÕES] NOME_OU_ID_DO_CONTÊINER [...NOME_OU_ID_DO_CONTÊINER]
```

- `NOME_OU_ID_DO_CONTÊINER`: O nome ou o ID do contêiner que você deseja remover. Você pode especificar múltiplos contêineres.

**Opções Comuns:**

- `-f` ou `--force`: Força a remoção de um contêiner em execução. Isso envia um sinal `SIGKILL` para o contêiner, encerrando-o abruptamente antes de removê-lo. Use com cautela, pois pode resultar em perda de dados não salvos.
- `-v` ou `--volumes`: Remove os volumes anônimos associados ao contêiner. Volumes nomeados não são removidos por padrão.

**Exemplos:**

- **Remover um contêiner parado:**
```bash
docker rm meu_container_parado
```

- **Remover um contêiner em execução, forçando a parada:**
```bash
docker rm -f meu_container_em_execucao
```

- **Remover todos os contêineres parados:**
  Primeiro, liste os IDs de todos os contêineres parados usando `docker ps -a -q` e, em seguida, passe esses IDs para `docker rm`.
```bash
docker rm $(docker ps -a -q)
```
  *Explicação:* O comando `$(docker ps -a -q)` executa `docker ps -a -q` e substitui sua saída (uma lista de IDs de contêineres) como argumentos para `docker rm`. Isso é uma forma eficiente de limpar seu ambiente Docker.

É uma boa prática remover contêineres que não são mais necessários para liberar recursos do sistema e manter seu ambiente Docker organizado.



### `docker image ls` - Listando Imagens

O comando `docker image ls` (ou seu atalho `docker images`) é usado para listar as imagens Docker que estão armazenadas localmente em sua máquina.

**Sintaxe:**
```bash
docker image ls [OPÇÕES]
```

**Opções Comuns:**

- `-a` ou `--all`: Mostra todas as imagens, incluindo as camadas intermediárias.
- `-q` ou `--quiet`: Exibe apenas os IDs numéricos das imagens.
- `--filter FILTRO`: Filtra a saída com base em condições específicas (ex: `dangling=true` para imagens sem tag).

**Exemplos:**

- **Listar todas as imagens locais:**
```bash
docker image ls
```

- **Listar apenas os IDs das imagens:**
```bash
docker image ls -q
```

A saída do `docker image ls` geralmente inclui colunas como `REPOSITORY`, `TAG`, `IMAGE ID`, `CREATED` e `SIZE`, fornecendo um resumo das imagens disponíveis localmente.



### `docker rmi` - Removendo Imagens

O comando `docker rmi` é usado para remover uma ou mais imagens do seu armazenamento local. Você não pode remover uma imagem se houver contêineres baseados nela em execução ou parados.

**Sintaxe:**
```bash
docker rmi [OPÇÕES] NOME_OU_ID_DA_IMAGEM [...NOME_OU_ID_DA_IMAGEM]
```

- `NOME_OU_ID_DA_IMAGEM`: O nome ou o ID da imagem que você deseja remover. Você pode especificar múltiplas imagens.

**Opções Comuns:**

- `-f` ou `--force`: Força a remoção da imagem, mesmo que ela esteja sendo usada por um contêiner. Use com extrema cautela, pois isso pode levar a contêineres órfãos ou problemas de dependência.

**Exemplos:**

- **Remover uma imagem específica:**
```bash
docker rmi ubuntu:latest
```

- **Remover uma imagem pelo ID:**
```bash
docker rmi a24bb4013296
```

- **Remover todas as imagens não utilizadas (dangling images):**
  Imagens *dangling* são aquelas que não estão associadas a nenhum repositório ou tag, mas ainda ocupam espaço em disco. Elas geralmente são o resultado de uma nova construção de imagem que substitui uma versão anterior.
```bash
docker image prune
```
  Ou para remover todas as imagens não utilizadas (não apenas as *dangling*), incluindo aquelas que não são referenciadas por nenhum contêiner:
```bash
docker system prune -a
```
  *Cuidado:* O comando `docker system prune -a` removerá todos os contêineres parados, todas as redes não utilizadas, todas as imagens não utilizadas (incluindo as não *dangling*) e todo o cache de build. Use-o com responsabilidade.



## Dockerfile - Construindo Suas Próprias Imagens

Um `Dockerfile` é um arquivo de texto que contém todas as instruções para construir uma imagem Docker. Ele define o ambiente, as dependências, o código da aplicação e como a aplicação deve ser executada dentro do contêiner. O Docker lê essas instruções sequencialmente para criar uma imagem.

### Estrutura Básica de um Dockerfile

Um Dockerfile geralmente começa com uma imagem base e, em seguida, adiciona camadas para configurar o ambiente e copiar o código da aplicação. Cada instrução no Dockerfile cria uma nova camada na imagem.

**Exemplo de Dockerfile para uma aplicação Node.js:**
```dockerfile
# Usa a imagem oficial do Node.js como base
FROM node:latest

# Define o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copia o arquivo package.json e package-lock.json para o diretório de trabalho
# Isso permite que o Docker utilize o cache de camadas para npm install
COPY package*.json ./

# Instala as dependências do Node.js
RUN npm install

# Copia o restante do código da aplicação para o diretório de trabalho
COPY . .

# Expõe a porta em que a aplicação Node.js estará escutando
EXPOSE 3000

# Define o comando que será executado quando o contêiner for iniciado
ENTRYPOINT ["npm", "start"]
```

### Principais Instruções do Dockerfile:

- `FROM IMAGEM[:TAG]`: Define a imagem base para a sua imagem. Deve ser a primeira instrução não-comentário no Dockerfile.
  - Ex: `FROM ubuntu:latest`, `FROM node:18-alpine`.

- `WORKDIR /caminho/no/container`: Define o diretório de trabalho para quaisquer instruções `RUN`, `CMD`, `ENTRYPOINT`, `COPY` e `ADD` que o seguem no Dockerfile. Se o diretório não existir, ele será criado.

- `COPY <origem> <destino>`: Copia arquivos ou diretórios do seu sistema de arquivos local (onde o Dockerfile está sendo construído) para o sistema de arquivos do contêiner. O `<origem>` é relativo ao contexto de build.
  - Ex: `COPY . .` (copia todo o conteúdo do diretório atual para o diretório de trabalho do contêiner).
  - Ex: `COPY package.json ./` (copia apenas o arquivo `package.json` para o diretório de trabalho).

- `RUN COMANDO`: Executa qualquer comando em uma nova camada sobre a imagem atual e *commita* o resultado. É usado para instalar pacotes, configurar o ambiente, etc.
  - Ex: `RUN apt-get update && apt-get install -y curl`.
  - Ex: `RUN npm install`.

- `EXPOSE PORTA [PORTA...]`: Informa ao Docker que o contêiner escuta na porta especificada em tempo de execução. Não publica a porta; apenas documenta qual porta a aplicação usa. Para publicar a porta, use a flag `-p` com `docker run`.
  - Ex: `EXPOSE 80`, `EXPOSE 3000`.

- `CMD [


CMD ["executável", "param1", "param2"]` ou `CMD comando param1 param2`: Define o comando padrão a ser executado quando o contêiner é iniciado. Se um `COMANDO` for especificado com `docker run`, ele sobrescreverá o `CMD`.
  - Ex: `CMD ["nginx", "-g", "daemon off;"]`.

- `ENTRYPOINT ["executável", "param1", "param2"]`: Define o comando principal que será executado quando o contêiner for iniciado. Diferente do `CMD`, o `ENTRYPOINT` não é sobrescrito por argumentos passados para `docker run`; em vez disso, os argumentos são passados como parâmetros para o `ENTRYPOINT`.
  - Ex: `ENTRYPOINT ["npm", "start"]`.

### Boas Práticas para Dockerfiles:

- **Use imagens base pequenas:** Prefira imagens base menores (ex: `alpine` em vez de `latest`) para reduzir o tamanho final da imagem e o tempo de download.
- **Aproveite o cache de camadas:** Organize as instruções no Dockerfile de forma que as camadas que mudam com menos frequência (como instalação de dependências) venham antes das que mudam mais (como o código da aplicação). Isso otimiza o processo de build.
- **Combine instruções `RUN`:** Use `&&` para encadear múltiplos comandos `RUN` em uma única instrução, reduzindo o número de camadas e o tamanho da imagem.
- **Não instale ferramentas desnecessárias:** Evite instalar ferramentas de desenvolvimento ou depuração que não serão usadas em produção.
- **Use `.dockerignore`:** Crie um arquivo `.dockerignore` para excluir arquivos e diretórios desnecessários (ex: `.git`, `node_modules` locais, arquivos temporários) do contexto de build, acelerando o processo e reduzindo o tamanho da imagem.



## `docker build` - Construindo Imagens a Partir de um Dockerfile

O comando `docker build` é usado para construir uma imagem Docker a partir de um `Dockerfile` e um "contexto" (o conjunto de arquivos no diretório especificado). O processo de build lê as instruções no Dockerfile e cria as camadas da imagem.

**Sintaxe:**
```bash
docker build [OPÇÕES] CAMINHO_DO_CONTEXTO
```

- `CAMINHO_DO_CONTEXTO`: O caminho para o diretório que contém o `Dockerfile` e os arquivos que serão adicionados à imagem. Geralmente, usa-se `.` para indicar o diretório atual.

**Opções Comuns:**

- `-t NOME_DA_IMAGEM[:TAG]` ou `--tag NOME_DA_IMAGEM[:TAG]`: Atribui um nome e uma tag à imagem construída. É altamente recomendável usar tags para versionar suas imagens.
  - `NOME_DA_IMAGEM`: Geralmente no formato `seu_usuario/nome_da_aplicacao` para imagens que você pretende enviar para o Docker Hub.
  - `TAG`: A versão da sua imagem (ex: `1.0`, `latest`, `dev`).
- `--no-cache`: Não usa o cache durante o processo de build. Útil para garantir que todas as camadas sejam reconstruídas.

**Exemplos:**

- **Construir uma imagem a partir do Dockerfile no diretório atual, nomeando-a `minhaapi` com a tag `1.0`:**
```bash
docker build -t minhaapi:1.0 .
```
  *Explicação:* O `.` no final indica que o contexto de build é o diretório atual, onde o `Dockerfile` deve estar localizado.

- **Construir uma imagem com um namespace de usuário (para Docker Hub):**
```bash
docker build -t seu_usuario/minhaapi:1.0 .
```
  *Importante:* O nome da imagem deve seguir o formato `seu_usuario/nome_do_repositorio` se você planeja enviá-la para o Docker Hub sob sua conta. Caso contrário, você receberá um erro de permissão ao tentar fazer o `docker push`.

Durante o processo de build, você verá cada etapa do Dockerfile sendo executada e as camadas sendo criadas. Se uma etapa já foi construída anteriormente e não houve alterações nas instruções ou nos arquivos relevantes, o Docker utilizará o cache, acelerando o processo.



## Docker Hub - Compartilhando Suas Imagens

O Docker Hub é um serviço de registro de imagens Docker baseado em nuvem. Ele permite que você encontre, armazene e compartilhe imagens Docker com sua equipe ou com a comunidade. Para compartilhar suas próprias imagens, você precisará de uma conta no Docker Hub.

### `docker login` - Autenticando-se no Docker Hub

Antes de poder enviar suas imagens para o Docker Hub, você precisa se autenticar.

**Sintaxe:**
```bash
docker login [OPÇÕES] [SERVIDOR]
```

- `SERVIDOR`: (Opcional) O endereço do servidor de registro. Para o Docker Hub, não é necessário especificar, pois é o padrão.

**Exemplo:**

- **Fazer login no Docker Hub:**
```bash
docker login -u seu_nome_de_usuario
```
  *Explicação:* Após executar este comando, o Docker solicitará sua senha. Digite-a (a senha não será exibida no terminal por segurança) e pressione Enter. Se o login for bem-sucedido, você verá uma mensagem de confirmação.

### `docker push` - Enviando Imagens para o Docker Hub

Depois de construir sua imagem e fazer login no Docker Hub, você pode enviá-la para o seu repositório.

**Sintaxe:**
```bash
docker push NOME_DA_IMAGEM[:TAG]
```

- `NOME_DA_IMAGEM`: O nome da imagem que você deseja enviar. **É crucial que o nome da imagem esteja no formato `seu_nome_de_usuario/nome_do_repositorio[:TAG]` para que o Docker Hub saiba para qual conta e repositório enviar a imagem.**

**Exemplo:**

- **Enviar a imagem `minhaapi:1.0` para o seu repositório no Docker Hub:**
```bash
docker push seu_nome_de_usuario/minhaapi:1.0
```
  *Explicação:* O Docker fará o upload das camadas da sua imagem para o Docker Hub. Se a imagem já existir, apenas as camadas alteradas serão enviadas, tornando o processo eficiente. Uma vez que a imagem esteja no Docker Hub, outros usuários (ou você mesmo em outras máquinas) poderão baixá-la usando `docker pull seu_nome_de_usuario/minhaapi:1.0`.

Certifique-se de que o `seu_nome_de_usuario` no nome da imagem corresponda exatamente ao seu nome de usuário do Docker Hub, caso contrário, o push falhará devido a permissões.



## Comandos Úteis Adicionais

Além dos comandos essenciais, o Docker oferece uma série de outras ferramentas que podem ser muito úteis para depuração, inspeção e gerenciamento de rede.

### `docker logs` - Visualizando Logs de Contêineres

O comando `docker logs` permite que você visualize a saída de log de um contêiner em execução ou que já foi encerrado. Isso é fundamental para depurar aplicações e entender o que está acontecendo dentro do seu contêiner.

**Sintaxe:**
```bash
docker logs [OPÇÕES] NOME_OU_ID_DO_CONTÊINER
```

**Opções Comuns:**

- `-f` ou `--follow`: Segue a saída de log em tempo real, como o comando `tail -f`.
- `--tail NÚMERO`: Exibe apenas as últimas `NÚMERO` linhas do log.
- `-t` ou `--timestamps`: Exibe timestamps (carimbos de data/hora) para cada linha de log.

**Exemplos:**

- **Visualizar os logs de um contêiner:**
```bash
docker logs meu_nginx
```

- **Seguir os logs em tempo real:**
```bash
docker logs -f minhaapi
```

- **Visualizar as últimas 100 linhas de log com timestamps:**
```bash
docker logs --tail 100 -t meu_servidor
```

### `docker inspect` - Obtendo Informações Detalhadas de Objetos Docker

O comando `docker inspect` retorna informações detalhadas de baixo nível sobre objetos Docker, como contêineres, imagens, volumes ou redes. A saída é em formato JSON, o que a torna ideal para automação e scripts.

**Sintaxe:**
```bash
docker inspect [OPÇÕES] NOME_OU_ID_DO_OBJETO [...NOME_OU_ID_DO_OBJETO]
```

**Exemplos:**

- **Inspecionar um contêiner:**
```bash
docker inspect meu_nginx
```

- **Inspecionar uma imagem:**
```bash
docker inspect ubuntu:latest
```

- **Obter o endereço IP de um contêiner (usando `jq` para parsear JSON):**
```bash
docker inspect -f '{{.NetworkSettings.IPAddress}}' meu_nginx
```
  *Explicação:* A flag `-f` permite formatar a saída usando templates Go, o que é muito útil para extrair informações específicas sem precisar parsear o JSON completo manualmente.

### `docker network` - Gerenciando Redes Docker

O Docker cria redes virtuais para que os contêineres possam se comunicar entre si e com o mundo exterior. O comando `docker network` permite gerenciar essas redes.

**Sintaxe:**
```bash
docker network [SUBCOMANDO] [OPÇÕES]
```

**Subcomandos Comuns:**

- `ls`: Lista todas as redes Docker.
- `create`: Cria uma nova rede.
- `connect`: Conecta um contêiner a uma rede.
- `disconnect`: Desconecta um contêiner de uma rede.
- `rm`: Remove uma ou mais redes.
- `inspect`: Exibe informações detalhadas sobre uma rede.

**Exemplos:**

- **Listar todas as redes:**
```bash
docker network ls
```

- **Criar uma nova rede bridge:**
```bash
docker network create --driver bridge minha_rede_app
```

- **Conectar um contêiner a uma rede existente:**
```bash
docker network connect minha_rede_app meu_container_web
```

- **Inspecionar uma rede:**
```bash
docker network inspect bridge
```

O gerenciamento de redes é crucial para construir aplicações multi-contêineres que precisam se comunicar de forma segura e eficiente.



## Docker Volumes - Persistência de Dados

Por padrão, os dados dentro de um contêiner são efêmeros, o que significa que eles são perdidos quando o contêiner é removido. Para persistir dados gerados e usados por contêineres Docker, você deve usar **Volumes**.

Volumes são o mecanismo preferido para persistir dados gerados por e usados por contêineres Docker. Eles são gerenciados pelo Docker e são mais eficientes e seguros do que montar diretórios do host diretamente.

### Tipos de Volumes:

1.  **Volumes Nomeados (Named Volumes):** São a forma mais comum e recomendada de persistir dados. O Docker gerencia a criação, localização e conteúdo desses volumes. Você os referencia por um nome.
2.  **Bind Mounts:** Permitem que você monte um arquivo ou diretório do sistema de arquivos do host diretamente dentro de um contêiner. Isso dá controle total sobre a estrutura do arquivo no host, mas pode ter problemas de portabilidade e segurança.
3.  **`tmpfs` Mounts:** Monta um sistema de arquivos temporário na memória do host. Os dados não são persistidos no disco e são perdidos quando o contêiner é parado.

### Gerenciando Volumes Nomeados

#### Criando um Volume:
```bash
docker volume create meu_volume_dados
```

#### Listando Volumes:
```bash
docker volume ls
```

#### Inspecionando um Volume:
```bash
docker volume inspect meu_volume_dados
```

#### Removendo um Volume:
```bash
docker volume rm meu_volume_dados
```

#### Removendo todos os volumes não utilizados:
```bash
docker volume prune
```

### Usando Volumes com Contêineres

Você pode anexar um volume a um contêiner usando a flag `-v` ou `--mount` com o comando `docker run`.

**Sintaxe para Volumes Nomeados (`-v`):**
```bash
docker run -d -v NOME_DO_VOLUME:/caminho/no/container IMAGEM
```

**Exemplo:**

- **Rodar um contêiner Nginx e persistir seus logs em um volume nomeado:**
```bash
docker run -d -p 80:80 --name meu_nginx_com_volume -v nginx_logs:/var/log/nginx nginx
```
  *Explicação:* O volume `nginx_logs` será criado (se não existir) e montado no diretório `/var/log/nginx` dentro do contêiner. Os logs do Nginx serão escritos neste volume e persistirão mesmo se o contêiner for removido.

**Sintaxe para Bind Mounts (`-v`):**
```bash
docker run -d -v /caminho/no/host:/caminho/no/container IMAGEM
```

**Exemplo:**

- **Montar um diretório local para servir arquivos estáticos com Nginx:**
```bash
docker run -d -p 80:80 --name meu_nginx_bind_mount -v /home/usuario/meus_sites:/usr/share/nginx/html nginx
```
  *Explicação:* O conteúdo do diretório `/home/usuario/meus_sites` no seu sistema hospedeiro estará disponível dentro do contêiner Nginx em `/usr/share/nginx/html`, permitindo que o Nginx sirva esses arquivos. Qualquer alteração nos arquivos no host será refletida instantaneamente no contêiner.

Volumes são essenciais para aplicações que precisam de persistência de dados, como bancos de dados, ou para compartilhar arquivos entre o host e o contêiner de forma controlada.



## Dicas e Boas Práticas para o Uso do Docker

Para tirar o máximo proveito do Docker e evitar problemas comuns, considere as seguintes dicas e boas práticas:

### 1. Mantenha suas Imagens Leves

- **Use imagens base pequenas:** Prefira imagens base minimalistas como `alpine` (ex: `node:18-alpine`, `python:3.9-alpine`) em vez de imagens completas. Isso reduz o tamanho da imagem final, o tempo de download e a superfície de ataque.
- **Minimize o número de camadas:** Cada instrução `RUN`, `COPY`, `ADD` no Dockerfile cria uma nova camada. Combine comandos `RUN` usando `&&` para reduzir o número de camadas.
- **Limpe o cache e arquivos temporários:** Após instalar pacotes, remova arquivos de cache e dependências de build que não são mais necessários na imagem final. Por exemplo, em distribuições baseadas em Debian/Ubuntu, use `apt-get clean` e remova `/var/lib/apt/lists/*`.

### 2. Otimize o Cache de Build do Docker

- **Organize seu Dockerfile:** Coloque as instruções que mudam com menos frequência (como `FROM`, `RUN` para instalação de dependências) no início do Dockerfile. As instruções que mudam com mais frequência (como `COPY` do código da aplicação) devem vir depois. Isso permite que o Docker reutilize o cache de camadas de builds anteriores, acelerando o processo de construção.
- **Use `.dockerignore`:** Crie um arquivo `.dockerignore` na raiz do seu projeto para excluir arquivos e diretórios desnecessários (ex: `.git`, `node_modules` locais, `*.log`, `tmp/`) do contexto de build. Isso reduz o tamanho do contexto enviado para o daemon Docker e acelera o build.

### 3. Gerenciamento de Dados com Volumes

- **Use volumes para persistência de dados:** Nunca armazene dados importantes diretamente no sistema de arquivos gravável do contêiner, pois eles serão perdidos se o contêiner for removido. Use volumes nomeados ou bind mounts para persistir dados de bancos de dados, logs, uploads de usuários, etc.
- **Volumes nomeados vs. Bind Mounts:** Prefira volumes nomeados para dados de aplicação e bancos de dados, pois são gerenciados pelo Docker e mais portáteis. Use bind mounts para desenvolvimento, quando você precisa que as alterações no código local sejam refletidas instantaneamente no contêiner.

### 4. Segurança

- **Não execute contêineres como root:** Por padrão, os processos dentro de um contêiner são executados como `root`. Use a instrução `USER` no Dockerfile para criar um usuário não-root e executar sua aplicação com privilégios mínimos.
- **Mantenha suas imagens atualizadas:** Use tags específicas (ex: `node:18.17.0` em vez de `node:latest`) e atualize-as regularmente para garantir que você esteja usando versões com as últimas correções de segurança.
- **Escaneie suas imagens:** Utilize ferramentas de segurança para escanear suas imagens Docker em busca de vulnerabilidades conhecidas.

### 5. Monitoramento e Logs

- **Colete logs:** Configure suas aplicações para enviar logs para `STDOUT` e `STDERR`. O Docker pode facilmente coletar esses logs e você pode visualizá-los com `docker logs` ou enviá-los para um sistema de gerenciamento de logs centralizado.
- **Monitore recursos:** Use `docker stats` para monitorar o uso de CPU, memória, rede e I/O dos seus contêineres em tempo real.

### 6. Limpeza Regular

- **Remova contêineres e imagens não utilizados:** Contêineres parados e imagens antigas podem ocupar muito espaço em disco. Use `docker system prune` (com cautela) ou comandos específicos como `docker container prune`, `docker image prune` e `docker volume prune` para limpar seu ambiente regularmente.

Seguindo essas práticas, você pode construir, executar e gerenciar suas aplicações Docker de forma mais eficiente, segura e confiável.


## Recursos Adicionais e Links Úteis

Para aprofundar seus conhecimentos em Docker e acompanhar as melhores práticas, consulte os seguintes recursos:

### Documentação Oficial

- **[Documentação Oficial do Docker](https://docs.docker.com/)** - A fonte mais completa e atualizada de informações sobre Docker.
- **[Docker Hub](https://hub.docker.com/)** - Registro oficial de imagens Docker, onde você pode encontrar milhares de imagens prontas para uso.
- **[Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)** - Referência completa de todas as instruções disponíveis em um Dockerfile.

### Tutoriais e Guias

- **[Docker Get Started](https://docs.docker.com/get-started/)** - Tutorial oficial para iniciantes.
- **[Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)** - Melhores práticas recomendadas pela equipe do Docker.

### Ferramentas Úteis

- **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** - Interface gráfica para gerenciar Docker no Windows e macOS.
- **[Portainer](https://www.portainer.io/)** - Interface web para gerenciamento de contêineres Docker.

### Comandos de Referência Rápida

Para sua conveniência, aqui está um resumo dos comandos mais utilizados:

```bash
# Gerenciamento de Imagens
docker pull <imagem>                    # Baixar imagem
docker image ls                         # Listar imagens
docker rmi <imagem>                     # Remover imagem
docker build -t <nome>:<tag> .          # Construir imagem

# Gerenciamento de Contêineres
docker run -d --name <nome> <imagem>    # Executar contêiner
docker ps -a                            # Listar contêineres
docker stop <contêiner>                 # Parar contêiner
docker start <contêiner>                # Iniciar contêiner
docker rm <contêiner>                   # Remover contêiner
docker exec -it <contêiner> bash        # Acessar terminal

# Logs e Monitoramento
docker logs <contêiner>                 # Ver logs
docker stats                            # Monitorar recursos
docker inspect <objeto>                 # Inspecionar objeto

# Limpeza
docker system prune                     # Limpar recursos não utilizados
docker container prune                  # Remover contêineres parados
docker image prune                      # Remover imagens não utilizadas
```

---

**Nota:** Este manual foi criado como um guia de referência pessoal para aprendizado de Docker. Para informações mais detalhadas e atualizadas, sempre consulte a documentação oficial do Docker.

