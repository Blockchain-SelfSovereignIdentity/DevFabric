# CC-Tools

## Sumário

- [Getting Started](#getting-started)
- [Tutoriais](#tutoriais)
- [Assets](#assets)
- [Datatypes](#datatypes)
- [Events](#events)
- [Transactions](#transactions)
- [Ferramentas Externas](#ferramentas-externas)
- [Testes](#testes)
- [Godog Testes e Testes de Golang para o Chaincode do Hyperledger Fabric](#godog-testes-e-testes-de-golang-para-o-chaincode-do-hyperledger-fabric)

<hr/>

# Getting Started
# GoLedger CC-Tools


O GoLedger CC-Tools é uma biblioteca desenvolvida para ser utilizada no sistema operacional Linux.

Geralmente, utilizamos a distribuição Ubuntu 18+, no entanto, a biblioteca é compatível com outros ambientes, podendo ser necessários alguns ajustes.

#### Versão utilizada:
```shell
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.1 LTS
Release:        22.04
Codename:       jammy
```

## Download e Configuração

Para aprender como utilizar a biblioteca GoLedger CC-Tools, faça o download do repositório de código de demonstração disponível no GitHub.

```bash
cd $HOME
git clone https://github.com/hyperledger-labs/cc-tools-demo.git
cd cc-tools-demo
```
## Estrutura de Pastas

```go
. // pasta raiz do cc-tools-demo
|          
├── ccapi               // Código da API Rest (servidor Golang Gin) 
|
├── chaincode           // Código do Smart Contract (GoLang) 
|   └── assettypes      // Definições de Ativos
|   └── eventtypes      // Definições de Eventos
|   └── txdefs          // Transações Blockchain
|   └── datatypes       // Tipos de dados personalizados 
|
├── fabric              // Artefatos do Hyperledger Fabric 
```

Estes são todos os elementos necessários para utilizar as principais funções da biblioteca.

## Configuração do Ambiente
Os seguintes sistemas, plataformas e linguagens devem estar instalados:
#### 
```shell
sudo apt-get install git curl jq
```

#### docker
```shell
# Remover versão antiga do Docker:
sudo apt-get remove docker
sudo apt-get remove docker-ce
sudo apt-get remove docker-ce-cli
sudo apt-get purge docker.io
sudo apt-get autoremove --purge docker.io
sudo rm -rf /var/lib/docker
sudo groupdel docker
sudo rm -rf /etc/docker
sudo rm /usr/bin/docker
sudo rm /snap/bin/docker
sudo rm /usr/bin/dockerd
sudo rm /etc/apt/sources.list.d/docker.list



matheuslazaro@matheusLazaro:~$ sudo apt-get install docker
matheuslazaro@matheusLazaro:~$ sudo apt  install docker.io
matheuslazaro@matheusLazaro:~$ sudo usermod -aG docker $USER
matheuslazaro@matheusLazaro:~$ sudo apt-get install containerd
matheuslazaro@matheusLazaro:~$ echo $USER
matheuslazaro@matheusLazaro:~$ docker --version
Docker version 24.0.5, build 24.0.5-0ubuntu1~22.04.1

sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world


sudo chmod 666 /var/run/docker.sock
ls -l /var/run/docker.sock
sudo chown :docker /var/run/docker.sock
sudo service docker stop
sudo service docker start
grep docker /etc/group


matheuslazaro@matheusLazaro:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# Verificação (Se necessário, reiniciar a VM):
docker --version
Docker version 24.0.5, build 24.0.5-0ubuntu1~22.04.1
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

#### Docker compose
```shell
# Remover versão antiga:
sudo apt-get remove docker-compose
sudo rm /usr/local/bin/docker-compose

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
export PATH=$PATH:/usr/local/bin
export PATH=$PATH:/snap/bin

# Atualizar biblioteca paramiko
sudo apt-get install python-pip
pip install --upgrade paramiko


docker-compose -v
docker-compose version 1.29.2, build 5becea4c
```

#### Node.js 12.18.2 e npm 6.14.5(Versão suportada)
* Instalação
```shell
sudo rm -rf /usr/local/bin/node /usr/local/lib/node_modules

sudo apt-get remove nodejs

sudo apt-get install npm
sudo npm install -g n
sudo rm /usr/bin/node
sudo n 12.18.2
sudo ln -s /usr/local/bin/node /usr/bin/node

nvm install 12.18.2
nvm use 12.18.2
```

* Verificação:
    ```shell
   node -v
    v12.18.2

    npm --version
    6.14.5
    ```


* Hyperledger Fabric 2.5

Se estiver usando o Linux Ubuntu, execute o seguinte comando a partir do diretório raiz. Isso irá baixar e instalar os sistemas acima antes de iniciar o desenvolvimento.

```shell
sudo ./scripts/installPreReqUbuntu.sh

sudo systemctl start docker
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

Ver se é necessário:
```go
sudo su

# Baixar o arquivo tarball do GoLang 1.14+
wget https://dl.google.com/go/go1.14.3.linux-amd64.tar.gz

# Extrair o arquivo tarball na pasta /usr/local
sudo tar -C /usr/local -xvf go1.14.3.linux-amd64.tar.gz

# Adicionar o caminho /usr/local/go/bin à variável de ambiente PATH
sudo echo "export PATH=$PATH:/usr/local/go/bin" >> $HOME/.profile

# Recarregar o arquivo .profile para aplicar as mudanças
source $HOME/.profile

# Verificar se a instalação foi bem-sucedida
go version

sudo export PATH=$PATH:/usr/local/go/bin

sudo su
export PATH=$PATH:/usr/local/go/bin
```

Ao final, a seguinte mensagem de sucesso deve aparecer:

Eviroment configured

Agora você está pronto para Escrever Sua Primeira Aplicação.











# Tutoriais
## Escrevendo Sua Primeira Aplicação

Este tutorial mostrará como instalar e usar um smart contract (chaincode).

Antes de começar, é necessário ter configurado o ambiente. Detalhes podem ser encontrados em [Getting Started](#getting-started).

Vamos entender como a biblioteca GoLedger CC-Tools funciona, utilizando os artefatos fornecidos pela GoLedger.

### Integração CCAPI
### Atualização da API Hyperledger Fabric
### Desenvolvimento da Aplicação Web (cc-web)

O smart contract no repositório cc-tools-demo é ideal para iniciantes no desenvolvimento de tecnologia Hyperledger Fabric. É um excelente ponto de partida para compreender uma blockchain Hyperledger Fabric e facilitar o desenvolvimento de aplicações usando esse framework.

Você aprenderá como escrever uma aplicação e um smart contract (chaincode) para consultar e atualizar um ledger, e como se conectar à Blockchain através de uma API pronta para uso.

**NOTA:**

É necessário conhecer a linguagem de programação GoLang para completar este tutorial.

### Detalhes da Rede

A rede padrão a ser criada terá a seguinte configuração:

- 1 chaincode (cc-tools-demo)
- 3 organizações (org1, org2, org3)
- 3 clientes (servidores REST para org1, org2 e org3)
Há também a opção de criar uma rede menor com menos entidades, com a seguinte configuração:

- 1 chaincode (cc-tools-demo)
- 1 organização (org)
- 1 cliente (servidor REST para org)

### Detalhes do Chaincode

O chaincode fornecido em cc-tools-demo possui as seguintes características:

- 3 ativos
- 1 dado privado
- 4 transações

### Vendoring

Tanto o chaincode quanto o CCAPI, em Golang, precisam ser vendored (download de pacotes de dependência) para funcionar corretamente.

Para baixar as dependências do chaincode, execute o seguinte script:

```bash
sudo su

cd chaincode && \
go mod vendor && \
cd ..
```

Para baixar as dependências do CCAPI, execute o seguinte script:

```shell
cd ccapi && \
go mod vendor && \
cd ..
```

## Construindo Sua Rede GoLedger CC-Tools
Após instalar o ambiente e vendored os pacotes, a rede está pronta para ser instanciada.

Este processo ocorre com o seguinte script:

```shell
sudo docker stop $(docker ps -a -q)
sudo docker rm $(docker ps -a -q)

sudo docker image prune -a


sudo docker kill $(docker ps -q) # Limpar docker

#docker rm -vf $(docker ps -aq)
#docker rmi -f $(docker images -aq)
#docker system prune -a --volumes


sudo ./startDev.sh
```

## resultado:
```shell
root@matheuslazaro:/home/matheuslazaro/cc-tools-demo# docker ps
CONTAINER ID        IMAGE                                                                                                                                                                            COMMAND                  CREATED             STATUS              PORTS                                                                                  NAMES
fa07f5ee5064        ccapi_ccapi.org2.example.com                                                                                                                                                     "ccapi"                  31 seconds ago      Up 27 seconds       0.0.0.0:980->80/tcp, :::980->80/tcp                                                    ccapi.org2.example.com
85935b0709ec        ccapi_ccapi.org1.example.com                                                                                                                                                     "ccapi"                  31 seconds ago      Up 27 seconds       0.0.0.0:80->80/tcp, :::80->80/tcp                                                      ccapi.org1.example.com
56e39e2a7dd0        ccapi_ccapi.org3.example.com                                                                                                                                                     "ccapi"                  31 seconds ago      Up 27 seconds       0.0.0.0:1080->80/tcp, :::1080->80/tcp                                                  ccapi.org3.example.com
865d6fc4fa13        dev-peer0.org2.example.com-cc-tools-demo_0.1-ad35f3aeaebff7f1a85d54190bedc12305e5174dadfd5e7f37c2acd2899ba0f5-acaee92dca401d559ef5fffb9293da80dc2c1037bfa339fd1f960bfaa09b523e   "chaincode -peer.add…"   4 minutes ago       Up 4 minutes                                                                                               dev-peer0.org2.example.com-cc-tools-demo_0.1-ad35f3aeaebff7f1a85d54190bedc12305e5174dadfd5e7f37c2acd2899ba0f5
b01931643a3d        dev-peer0.org1.example.com-cc-tools-demo_0.1-ad35f3aeaebff7f1a85d54190bedc12305e5174dadfd5e7f37c2acd2899ba0f5-987f8a6269c23074f69cdc3a6ce4cf7f930df36a347df32233d6cddef6c2ea36   "chaincode -peer.add…"   4 minutes ago       Up 4 minutes                                                                                               dev-peer0.org1.example.com-cc-tools-demo_0.1-ad35f3aeaebff7f1a85d54190bedc12305e5174dadfd5e7f37c2acd2899ba0f5
524452dafb2d        dev-peer0.org3.example.com-cc-tools-demo_0.1-ad35f3aeaebff7f1a85d54190bedc12305e5174dadfd5e7f37c2acd2899ba0f5-075bcab8b9f6ada602b979af826081ace251483c4065b44029e2a72c6079181d   "chaincode -peer.add…"   4 minutes ago       Up 4 minutes                                                                                               dev-peer0.org3.example.com-cc-tools-demo_0.1-ad35f3aeaebff7f1a85d54190bedc12305e5174dadfd5e7f37c2acd2899ba0f5
7529ad2e7b04        hyperledger/fabric-tools:2.5.3                                                                                                                                                   "/bin/bash"              9 minutes ago       Up 9 minutes                                                                                               cli
4f3d8e41dc98        hyperledger/fabric-peer:2.5.3                                                                                                                                                    "peer node start"        9 minutes ago       Up 9 minutes        0.0.0.0:7051->7051/tcp, :::7051->7051/tcp, 0.0.0.0:7053->7053/tcp, :::7053->7053/tcp   peer0.org1.example.com
7dfd7f02aa44        hyperledger/fabric-peer:2.5.3                                                                                                                                                    "peer node start"        9 minutes ago       Up 9 minutes        0.0.0.0:8051->7051/tcp, :::8051->7051/tcp, 0.0.0.0:8053->7053/tcp, :::8053->7053/tcp   peer0.org2.example.com
b2b23cf8ae28        hyperledger/fabric-peer:2.5.3                                                                                                                                                    "peer node start"        9 minutes ago       Up 9 minutes        0.0.0.0:9051->7051/tcp, :::9051->7051/tcp, 0.0.0.0:9053->7053/tcp, :::9053->7053/tcp   peer0.org3.example.com
0f5bdaf29571        couchdb:3.1.1                                                                                                                                                                    "tini -- /docker-ent…"   9 minutes ago       Up 9 minutes        4369/tcp, 9100/tcp, 0.0.0.0:9984->5984/tcp, :::9984->5984/tcp                          couchdb2
8c53cce6b41d        couchdb:3.1.1                                                                                                                                                                    "tini -- /docker-ent…"   9 minutes ago       Up 9 minutes        4369/tcp, 9100/tcp, 0.0.0.0:5984->5984/tcp, :::5984->5984/tcp                          couchdb0
cc03cfcfa2a8        couchdb:3.1.1                                                                                                                                                                    "tini -- /docker-ent…"   9 minutes ago       Up 9 minutes        4369/tcp, 9100/tcp, 0.0.0.0:7984->5984/tcp, :::7984->5984/tcp                          couchdb1
7af003b413c0        hyperledger/fabric-orderer:2.5.3                                                                                                                                                 "orderer"                9 minutes ago       Up 9 minutes        0.0.0.0:7050->7050/tcp, :::7050->7050/tcp, 0.0.0.0:7052->7052/tcp, :::7052->7052/tcp   orderer.example.com
root@matheuslazaro:/home/matheuslazaro/cc-tools-demo# 

```

Este script realizará as seguintes tarefas:

* Limpar imagens e volumes Docker não utilizados
* Gerar os certificados MSP para cada organização
* Criar os containers do nó Blockchain (peers e orderers do Hyperledger Fabric) para 3 organizações (org1, org2, org3) com a configuração criptográfica correta para cada uma delas
* Criar o ledger para registro e permissão - Canal Hyperledger Fabric
* Entradas de organizações no canal (processo de adesão no Hyperledger Fabric)
* Instalação do chaincode nos endorsing peers
* Instalação da API de gerenciamento de rede Hyperledger Fabric
* Instantiação do chaincode no canal
* Implantação de containers de servidores REST com a configuração criptográfica correta, representando um Cliente Hyperledger Fabric para cada organização

Ao final do processo, as seguintes mensagens devem ser exibidas:

```txt
Chaincode definition committed on channel 'mainchannel'

...

Query chaincode definition successful on peer0.org1 on channel 'mainchannel'

...

Query chaincode definition successful on peer0.org2 on channel 'mainchannel'

...

Query chaincode definition successful on peer0.org3 on channel 'mainchannel'
Chaincode initialization is not required
[+] Running 3/3
 ⠿ Container ccapi.org2.example.com  Started                        0.3s
 ⠿ Container ccapi.org1.example.com  Started                        0.5s
 ⠿ Container ccapi.org3.example.com  Started                        0.5s

```

Os containers ccapi representam os servidores REST para cada organização. Eles podem levar alguns minutos para ficarem online.

Ao final, a seguinte mensagem deve aparecer:

```txt
docker logs ccapi.org1.example.com 

2023/07/27 14:00:53 Escutando na porta 80
2023/07/27/14:00:53 SDK criado a partir de './config/configsdk-org1.yaml'
2023/07/27/14:00:53 SDK criado a partir de './config/configsdk-org1.yaml'
Escutando na porta 80

```

Para instanciar a rede mínima usando apenas uma organização, use o seguinte comando:

```shell
./startDev.sh -n 1
```

## Atualizando Seu Chaincode
Atualizar um smart contract pode ser feito facilmente em chaincodes que usam a biblioteca GoLedger CC-Tools.

Primeiro, você deve verificar a sintaxe do código modificado.

```shell
cd chaincode \
go vet \
cd ..
```

Após a validação, a atualização do chaincode é feita com o script upgradeCC.sh, que recebe a versão do chaincode e a sequência como argumentos.

NOTA:

Um chaincode deve sempre ser atualizado com uma versão diferente de todas as versões anteriores e a sequência deve ser sequencial da anterior.

Exemplo:

```shell
./upgradeCC.sh 0.2 2
```

Ou se estiver usando a rede mínima:

```shell
./upgradeCC.sh 0.2 2 -n 1
```

## Recarregando o CCAPI
Se houver alterações no código do CCAPI, você deve recarregá-lo para que as alterações tenham efeito.

Primeiro, verifique a sintaxe do código modificado.

```shell
cd ccapi \
go vet \
cd ..
```

Após a validação, o script reloadCCAPI.sh pode ser usado para recarregar o servidor REST.

```shell
./reloadCCAPI.sh
```

Ou se estiver usando a rede mínima:

```shell
./reloadCCAPI.sh -n 1
```

## Renomeando Seu Projeto
Após testar cc-tools-demo e instanciar sua primeira rede Fabric, você está pronto para começar a desenvolver seu primeiro chaincode.

Para usar seu próprio nome de projeto, em vez de cc-tools-demo, o script renameProject.sh está disponível para renomear todas as menções a ele na pasta do projeto. O script aceita o novo nome como argumento.

Exemplo:
```shell
sudo ./renameProject.sh my-first-chaincode
```










# Assets

Os ativos blockchain representam as informações que serão utilizadas pelo Ledger blockchain (Hyperledger Fabric Channel).

Em termos gerais, um ativo pode ser comparado a uma tabela em um banco de dados relacional.

E assim como nos bancos de dados, os ativos são formados por propriedades, algumas das quais podem fazer parte de um conjunto de chaves para esse ativo. Cada propriedade pode ter um tipo de dado específico, como string, número, booleano, etc.

Um ativo também pode ser um dado privado do Hyperledger Fabric, no qual o conteúdo da informação é registrado em um banco de dados transitório fora do Ledger e apenas um subconjunto de organizações recebe esses dados, configurando a legibilidade.

Os ativos também podem ser criados dinamicamente durante a execução do chaincode, utilizando a funcionalidade de Tipos de Ativos Dinâmicos do CC-Tool, na qual os tipos de ativos são definidos usando chamadas de invocação ao chaincode em execução.

Para o repositório cc-tools-demo, existem 4 ativos (sendo um deles um dado privado):

- Person
- Book
- Library
- Secret (dado privado)

Para utilizar a biblioteca GoLedger CC-Tools, a definição dos ativos é feita na pasta chaincode/assettypes.

Segue a lista de arquivos:

```markdown
chaincode/
    assettypes/               # pasta de ativos
        person.go             # definição de uma pessoa
        book.go               # definição de um livro
        library.go            # definição de uma biblioteca
        secret.go             # definições de segredo (dado privado)
        customAssets.go       # lista de ativos inseridos via modo de Template do GoFabric
        dynamicAssetTypes.go  # arquivo de configuração para Tipos de Ativos Dinâmicos
assetTypeList.go              # lista de ativos instanciados
```

## Definição do Ativo
A construção e definição de um ativo são feitas criando um arquivo na pasta chaincode/assettypes.

Um ativo possui os seguintes campos:

**Tag**: campo de string usado para definir o nome do ativo referenciado internamente pelo código e pelos endpoints da API Rest. Sem espaços ou caracteres especiais permitidos.

**Label**: campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.

**Description**: campo de string de descrição do ativo a ser usado por aplicativos externos. Texto livre.

**Props**: propriedades do ativo. Possui seus próprios campos (descrição a seguir).

**Readers**: usado para definir organizações de dados privados. Se não estiver vazio, apenas as organizações definidas na matriz terão a capacidade de ler ativos do tipo.

**Validate**: função de validação do ativo. Permite um código personalizado para validar o ativo como um todo.

**Dynamic**: campo booleano para indicar que o tipo de ativo pode ser modificado dinamicamente. Para ativos codificados, é recomendado deixá-lo como falso e usar a funcionalidade de atualização de chaincode para atualizar a definição do ativo.

## Definição da Propriedade
Um ativo possui um conjunto de propriedades (Props) para estruturar o ativo.

Uma propriedade possui os seguintes campos:

**Tag**: campo de string usado para definir o nome da propriedade referenciado internamente pelo código e pelos endpoints da API Rest. Sem espaços ou caracteres especiais permitidos.

**Label**: campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.

**Description**: campo de string de descrição da propriedade a ser usado por aplicativos externos. Texto livre.

**IsKey**: identifica se a propriedade faz parte das chaves do ativo. Campo booleano.

**Required**: identifica se a propriedade é obrigatória. Campo booleano.

**ReadOnly**: identifica se a propriedade não pode mais ser modificada após a criação. Campo booleano.

**DefaultValue**: valor padrão da propriedade.

**Writers**: define as organizações que podem criar ou alterar esta propriedade. Se a propriedade for chave (campo IsKey: true), então o ativo inteiro só pode ser criado pela organização. Lista de strings.

**DataType**: tipo de propriedade. O CC-Tools possui os seguintes tipos padrão: string, number, datetime e boolean. Tipos personalizados podem ser definidos na pasta chaincode/datatypes. Arrays e referências a outros tipos de ativos também são possíveis.

**Validate**: função de validação da propriedade. Sugere-se apenas para validações simples; funções mais complexas devem usar tipos de dados personalizados.

## Exemplos de Ativos
O repositório cc-tools-demo possui os seguintes exemplos:

```markdown
chaincode/
    assettypes/               # pasta de ativos
        person.go             # definição de uma pessoa
        book.go               # definição de um livro
        library.go            # definição de uma biblioteca
        secret.go             # definições de segredo (dado privado)
        customAssets.go       # lista de ativos inseridos via modo de Template do GoFabric
        dynamicAssetTypes.go  # arquivo de configuração para Tipos de Ativos Dinâmicos
assetTypeList.go              # lista de ativos instanciados

```

Além dos arquivos de cada ativo, é necessário registrar os ativos que podem ser usados pela biblioteca GoLedger CC-Tools no arquivo chaincode/assetTypeList.go.

A definição do ativo Person é a seguinte:

```go
var Person = assets.AssetType{
    Tag:         "person",
    Label:       "Person",
    Description: "Dados pessoais de alguém",

    Props: []assets.AssetProp{
        {
            // Chave primária
            Required: true,
            IsKey:    true,
            Tag:      "id",
            Label:    "CPF (ID Brasileiro)",
            DataType: "cpf",               // Os tipos de dados são identificados na pasta datatypes
            Writers:  []string{"org1MSP", "orgMSP"}, // Isso significa que apenas a org1 e a org podem criar o ativo (outros podem editar)
        },
        {
            // Propriedade obrigatória
            Required: true,
            Tag:      "name",
            Label:    "Nome da pessoa",
            DataType: "string",
            // Função de validação
            Validate: func(name interface{}) error {
                nameStr := name.(string)
                if nameStr == "" {
                    return fmt.Errorf("o nome deve ser não-vazio")
                }
                return nil
            },
        },
        {
            // Propriedade opcional
            Tag:      "dateOfBirth",
            Label:    "Data de Nascimento",
            DataType: "datetime",
        },
        {
            // Propriedade com valor padrão
            Tag:          "height",
            Label:        "Altura da pessoa",
            DefaultValue: 0,
            DataType:     "number",
        },
    },
}

```

De acordo com a descrição acima, o ativo Person tem as seguintes características:

* Chave primária cpf (tipo de dado personalizado)
* Somente org1 e org podem criar ou alterar a propriedade cpf e o ativo Person (porque isso é uma chave)
* Propriedade name do tipo string é obrigatória e possui validação automática para evitar strings vazias ("")
* Propriedade dateOfBirth do tipo datetime é opcional.
* Propriedade height do tipo number, com valor padrão 0

A definição do ativo Book é a seguinte:

```go
var Book = assets.AssetType{
    Tag:         "book",
    Label:       "Book",
    Description: "Livro",

    Props: []assets.AssetProp{
        {
            // Chave Composta
            Required: true,
            IsKey:    true,
            Tag:      "title",
            Label:    "Título do Livro",
            DataType: "string",
            Writers:  []string{"org2MSP", "orgMSP"}, // Isso significa que apenas org2 e org podem criar o ativo (outros podem editar)
        },
        {
            // Chave Composta
            Required: true,
            IsKey:    true,
            Tag:      "author",
            Label:    "Autor do Livro",
            DataType: "string",
            Writers:  []string{"org2MSP", "orgMSP"}, // Isso significa que apenas org2 e org podem criar o ativo (outros podem editar)
        },
        {
            // Referência a outro ativo
            Tag:      "currentTenant",
            Label:    "Locatário Atual",
            DataType: "->person",
        },
        {
            // Lista de Strings
            Tag:      "genres",
            Label:    "Gêneros",
            DataType: "[]string",
        },
        {
            // Propriedade de Data
            Tag:      "published",
            Label:    "Data de Publicação",
            DataType: "datetime",
        },
        {
            // Tipo de dado personalizado
            Tag:      "bookType",
            Label:    "Tipo de Livro",
            DataType: "bookType",
        },
    },
}

```

De acordo com a descrição acima, o ativo Book tem as seguintes características:

* Chave composta title e author, ambos do tipo string
* Somente org2 e org podem criar ou alterar as propriedades title e author e o ativo Book (porque são chaves)
* Propriedade currentTenant do tipo ->person, que representa a referência a um ativo Person
* Propriedade genres do tipo []string, que representa uma array de strings
* Propriedade published do tipo datetime
* Propriedade bookType é do tipo de dado personalizado bookType, definido na pasta chaincode/datatypes

A definição do ativo Library é a seguinte:
```go
var Library = assets.AssetType{
    Tag:         "library",
    Label:       "Library",
    Description: "Biblioteca como coleção de livros",

    Props: []assets.AssetProp{
        {
            // Chave Primária
            Required: true,
            IsKey:    true,
            Tag:      "name",
            Label:    "Nome da Biblioteca",
            DataType: "string",
            Writers:  []string{"org3MSP", "orgMSP"}, // Isso significa que apenas org3 e org podem criar o ativo (outros podem editar)
        },
        {
            // Lista de Referências a Ativos
            Tag:      "books",
            Label:    "Coleção de Livros",
            DataType: "[]->book",
        },
        {
            // Lista de Referências a Ativos
            Tag:      "entranceCode",
            Label:    "Código de Entrada para a Biblioteca",
            DataType: "->secret",
        },
    },
}

```

De acordo com a descrição acima, o ativo Library tem as seguintes características:

* Chave primária name do tipo string
* Somente org3 e org podem criar ou alterar a propriedade name e o ativo Library (porque é uma chave)
* Propriedade books do tipo []->book, que representa uma array de referências ao ativo Book
* Propriedade entranceCode do tipo ->secret, que representa a referência a um tipo de dado privado

A definição do ativo Secret é a seguinte:

```go
var Secret = assets.AssetType{
    Tag:         "secret",
    Label:       "Secret",
    Description: "Segredo entre Org2 e Org3",

    Readers: []string{"org2MSP", "org3MSP", , "orgMSP"},
    Props: []assets.AssetProp{
        {
            // Chave Primária
            IsKey:    true,
            Tag:      "secretName",
            Label:    "Nome do Segredo",
            DataType: "string",
            Writers:  []string{"org2MSP", , "orgMSP"}, // Isso significa que apenas org2 e org podem criar o ativo (org3 pode editar)
        },
        {
            // Propriedade Obrigatória
            Required: true,
            Tag:      "secret",
            Label:    "Segredo",
            DataType: "string",
        },
    },
}

```

De acordo com a descrição acima, o ativo Secret tem recursos de privacidade (Hyperledger Fabric Private Data).

Não descreveremos os conceitos de dados privados do Hyperledger Fabric, sua operação, regras de política, etc. Mas, para simplificar, quando um ativo é definido como privado, apenas seu hash de conteúdo é registrado no canal, o conteúdo do ativo (suas propriedades) só é acessado por um conjunto limitado de organizações, o conteúdo registrado em bancos de dados transitórios em cada nó.

O conteúdo do ativo só pode ser lido por org, org2 e org3

Chave primária secretName do tipo string

Somente org2 e org podem criar ou alterar a propriedade secretName e o ativo Secret (porque é uma chave)

Propriedade secret do tipo string é obrigatória para o ativo

Ambos org, org2 e org3 podem criar ou alterar a propriedade secret

Definição da Lista de Ativos

O registro de ativos da GoLedger CC-Tools deve ser definido no arquivo chaincode/assetTypeList.go

```go
var assetTypeList = []assets.AssetType{
    assettypes.Person,
    assettypes.Book,
    assettypes.Library,
    assettypes.Secret,
}

```

## Tipos de Ativos Dinâmicos
As seções anteriores desta página descreveram ativos criados estaticamente, programados manualmente no código. Mas CC-Tools também permite a definição e criação dinâmica de ativos durante o tempo de execução, através de invocações para o chaincode.

### Configuração
A configuração para os tipos de ativos dinâmicos é armazenada no arquivo dynamicAssetTypes.go no cc-tools-demo. A configuração tem as seguintes opções:

**Enabled**: define se os tipos de ativos dinâmicos estão habilitados ou desabilitados durante a execução.

**Readers**: usado para especificar quais organizações têm permissão para criar e gerenciar dinamicamente o tipo de ativo.

### Gerenciando Tipos de Ativos Dinâmicos
CC-Tools oferece três transações incorporadas para gerenciar tipos de ativos dinamicamente: createAssetType, updateAssetType e deleteAssetType. Usando a API padrão cc-tools-demo para org1, essas transações podem ser acessadas através de uma solicitação ```POST``` para ```http://<HOST-IP>:80/api/invoke/<TRANSACTION-TAG>```, com as informações sobre os ativos modificados sendo enviadas no corpo JSON da solicitação. Essas transações também podem ser acessadas usando a interface GoInitus.

#### Criando Tipos de Ativos
O tipo de ativo a ser criado deve ser enviado na matriz assetTypes no corpo da solicitação. Cada objeto na matriz deve conter os campos que definem o tipo a ser criado, conforme visto na definição do ativo, com exceção do campo Validate, que não é suportado pelos tipos de ativos dinâmicos.

Junto com o campo de ativo, a matriz props deve ser usada para definir as propriedades do tipo de ativo. Cada objeto na matriz deve conter os campos de propriedade, conforme visto na definição da propriedade. Mais uma vez, a opção Validate não está disponível para as propriedades.

Um exemplo de carga útil para criar um novo tipo pode ser visto abaixo:

```json
{
    "assetTypes": [ 
        {
            "tag": "collection",
            "label": "Coleção",
            "description": "Uma coleção de livros",
            "props": [
                {
                    "tag": "name",
                    "label": "Nome",
                    "description": "Nome",
                    "dataType": "string",
                    "required": true,
                    "isKey": true
                },
                {
                    "tag": "rating",
                    "label": "Avaliação",
                    "description": "Avaliação",
                    "dataType": "number",
                    "defaultValue": 10
                },
                {
                    "tag": "books",
                    "label": "Livros",
                    "dataType": "[]->book",
                    "writers": ["org2MSP"]
                }
            ]
        }
    ]
}

```

#### Atualizando Tipos de Ativos
CC-Tools permite a atualização de tipos de ativos da mesma maneira usada no processo de criação. Note que apenas os tipos criados dinamicamente ou aqueles codificados com a opção Dynamic definida como true podem ser atualizados dinamicamente.

Ao atualizar um ativo codificado estaticamente, tenha cuidado ao atualizar a versão do chaincode, pois o tipo codificado será usado ao reconstruir a lista de tipos de ativos.

Para atualizar o ativo, apenas as propriedades e campos que serão modificados podem ser enviados. O único campo necessário para todos os tipos e propriedades a serem modificados é o tag, como meio de identificação. Também é impossível modificar o tag para um tipo ou propriedade dinamicamente.

Também é possível excluir uma propriedade existente de um tipo definindo o campo delete como true, desde que a propriedade em questão não seja uma chave.

Um exemplo de carga útil para atualizar um novo tipo pode ser visto abaixo:

```json
{
    "assetTypes": [ 
        {
            "tag": "collection",
            "description": "Uma série de coleção de livros",
            "props": [
                {
                    "tag": "rating",
                    "delete": true
                },
                {
                    "tag": "name",
                    "description": "O nome da coleção"
                }
            ]
        }
    ]
}

```

#### Excluindo Tipos de Ativos
CC-Tools também permite a exclusão de tipos de ativos. Assim como ao atualizar, apenas os tipos criados dinamicamente ou aqueles codificados com a opção Dynamic definida como true podem ser excluídos dinamicamente.

Os objetos da matriz assetTypes para a solicitação de exclusão só requerem o tag do tipo a ser excluído. Por padrão, se algum ativo do tipo existir no blockchain, a exclusão do ativo não será permitida. Para forçar a exclusão, a opção force pode ser enviada junto com o tag.

Também observe que, se qualquer outro tipo fizer referência ao tipo que está sendo excluído, o processo de exclusão falhará.

Um exemplo de carga útil para excluir um novo tipo pode ser visto abaixo:

```json
{
    "assetTypes": [ 
        {
            "tag": "collection", 
            "force": true
        }
    ]
}

```

# Datatypes
Os datatypes do GoLedger CC-Tools são uma forma de categorizar e definir a natureza dos dados que podem ser armazenados em um ativo e manipulados em transações. É essencial validar se os dados inseridos correspondem ao tipo de dados esperado.

O CC-Tools fornece alguns datatypes primitivos. Aqui estão eles:

**string**: representa uma sequência de caracteres.

**number**: representa um ponto flutuante de 64 bits.

**integer**: representa números inteiros. O tamanho depende da arquitetura subjacente e pode variar entre 32 bits e 64 bits.

**boolean**: representa dois valores possíveis: true e false.

**datetime**: representa uma data no formato RFC3339, por exemplo, "2019-05-06T22:12:41Z".

**@object**: representa uma string em formato JSON. Geralmente é usado para definir a estrutura e propriedades de um objeto JSON. É composto por pares chave-valor.

Também permite que os desenvolvedores injetem tipos de dados primitivos personalizados. Para usar a biblioteca GoLedger CC-Tools, a definição de datatypes é feita na pasta chaincode/datatypes.

Aqui está a lista de arquivos:

```shell
chaincode/
    datatypes/        # pastas de datatype personalizados
        bookType.go   # definição de um datatype de livro
        cpf.go        # definição de um datatype de CPF
        datatypes.go  # lista de datatypes instanciados
```

## Definição de Datatype
A construção e definição de um datatype personalizado é feita criando um arquivo na pasta chaincode/datatypes.

Um datatype possui os seguintes campos:

**AcceptedFormats**: lista de tipos "core" que podem ser aceitos (string, number, integer, boolean, datetime).

**Description**: texto que descreve o tipo de dados.

**DropDownValues**: conjunto de valores predeterminados a serem usados em um menu suspenso na renderização do frontend.

**Parse**: função chamada para verificar se o valor de entrada é válido, fazer as conversões necessárias e retornar uma representação em string do valor.

## Exemplos de Datatypes
O repositório cc-tools-demo possui os seguintes exemplos:

```shell
chaincode/
    datatypes/     # pastas de datatypes
        bookType.go   # definição de um datatype de livro
        cpf.go        # definição de um datatype de CPF
        datatypes.go  # lista de datatypes instanciados
```

Além dos arquivos de cada datatype, você deve registrar os datatypes que podem ser usados pela biblioteca GoLedger CC-Tools no arquivo datatypes.go.

A definição do datatype bookType é a seguinte:

```go
type BookType float64

const (
    BookTypeHardcover BookType = iota
    BookTypePaperback
    BookTypeEbook
)

// CheckType verifica se o valor fornecido é definido como constantes válidas de BookType
func (b BookType) CheckType() errors.ICCError {
    switch b {
    case BookTypeHardcover:
        return nil
    case BookTypePaperback:
        return nil
    case BookTypeEbook:
        return nil
    default:
        return errors.NewCCError("tipo inválido", 400)
    }
}

var bookType = assets.DataType{
    AcceptedFormats: []string{"number"},
    DropDownValues: map[string]interface{}{
        "Capa Dura": BookTypeHardcover,
        "Brochura":  BookTypePaperback,
        "Ebook":     BookTypeEbook,
    },
    Description: ``,

    Parse: func(data interface{}) (string, interface{}, errors.ICCError) {
        var dataVal float64
        switch v := data.(type) {
        case float64:
            dataVal = v
        case int:
            dataVal = float64(v)
        case BookType:
            dataVal = float64(v)
        case string:
            var err error
            dataVal, err = strconv.ParseFloat(v, 64)
            if err != nil {
                return "", nil, errors.WrapErrorWithStatus(err, "a propriedade do ativo deve ser um número inteiro, é %t", 400)
            }
        default:
            return "", nil, errors.NewCCError("a propriedade do ativo deve ser um número inteiro, é %t", 400)
        }

        retVal := BookType(dataVal)
        err := retVal.CheckType()
        return fmt.Sprint(retVal), retVal, err
    },
}

```

De acordo com a descrição acima, o datatype bookType tem as seguintes características:

* Representa um tipo enumerado.
* Os valores aceitos são os números 0, 1 e 2, que representam os tipos de livro Capa Dura, Brochura e Ebook, respectivamente.
* A função Parse recebe o valor de entrada, o analisa para float64 e verifica se o número é válido na função CheckType.

A definição do datatype cpf é a seguinte:

```go
var cpf = assets.DataType{
    AcceptedFormats: []string{"string"},
    Parse: func(data interface{}) (string, interface{}, errors.ICCError) {
        cpf, ok := data.(string)
        if !ok {
            return "", nil, errors.NewCCError("a propriedade deve ser uma string", 400)
        }

        cpf = strings.ReplaceAll(cpf, ".", "")
        cpf = strings.ReplaceAll(cpf, "-", "")

        if len(cpf) != 11 {
            return "", nil, errors.NewCCError("CPF deve ter 11 dígitos", 400)
        }

        var vd0 int
        for i, d := range cpf {
            if i >= 9 {
                break
            }
            dnum := int(d) - '0'
            vd0 += (10 - i) * dnum
        }
        vd0 = 11 - vd0%11
        if vd0 > 9 {
            vd0 = 0
        }
        if int(cpf[9])-'0' != vd0 {
            return "", nil, errors.NewCCError("CPF inválido", 400)
        }

        var vd1 int
        for i, d := range cpf {
            if i >= 10 {
                break
            }
            dnum := int(d) - '0'
            vd1 += (11 - i) * dnum
        }
        vd1 = 11 - vd1%11
        if vd1 > 9 {
            vd1 = 0
        }
        if int(cpf[10])-'0' != vd1 {
            return "", nil, errors.NewCCError("CPF inválido", 400)
        }

        return cpf, cpf, nil
    },
}

```

De acordo com a descrição acima, o datatype cpf tem as seguintes características:

* Aceita uma string como entrada no formato padrão de CPF, por exemplo, "861.232.710-59". A pontuação não é necessária.
* A função Parse valida se os números de CPF fornecidos seguem a estrutura correta, incluindo os dígitos necessários e o algoritmo de verificação.

## Lista de Datatypes
O registro de datatypes personalizados do GoLedger CC-Tools deve ser definido no arquivo chaincode/datatypes/datatypes.go.

```go
var CustomDataTypes = map[string]assets.DataType{
    "cpf":      cpf,
    "bookType": bookType,
}
```

# Events
O Hyperledger Fabric permite que aplicativos cliente (como a API rest-server) recebam eventos de bloco enquanto os blocos são confirmados no ledger do peer.

O CC-Tools possui uma funcionalidade de evento integrada que permite um registro rápido do evento, que será automaticamente ouvido e manipulado pela CCAPI padrão.

Os eventos no CC-Tools podem ser de três tipos:
* log, registrando as informações enviadas na carga útil do evento nos logs da CCAPI;
* transação, que invoca transações de outro chaincode quando o evento é recebido; e
* personalizado, que executa uma função personalizada previamente definida no evento.

Para o repositório cc-tools-demo, há 1 evento pré-definido registrado:

#### Create Library Log

A definição dos eventos é feita na pasta chaincode/eventtypes.

Aqui está a lista de arquivos:

```shell
chaincode/
    eventtypes/               # pasta de eventos
        createLibraryLog.go   # definição do evento de log de biblioteca
eventTypeList.go              # lista de eventos instanciados
```

## Definição de Evento
A construção e definição de um evento são feitas criando um arquivo na pasta chaincode/eventtypes.

Um evento possui os seguintes campos:

**Tag**: campo de string usado para definir o nome do evento referenciado internamente pelo código e pelos clientes rest-server.

**Label**: campo de string para definir o rótulo a ser usado por aplicativos externos e pelos clientes rest-server. Texto livre.

**Description**: campo de string de descrição do ativo a ser usado por aplicativos externos. Texto livre.

**BaseLog**: campo de string com uma mensagem a ser registrada na CCAPI a cada chamada de evento. Texto livre.

**EventType**: define o tipo de evento. As opções possíveis são events.EventLog, events.EventTransaction e events.EventCustom.

**Transaction**: campo de string com a tag da transação a ser chamada por eventos do tipo transação.

**Channel**: campo de string com o nome do canal onde a transação a ser chamada por um evento do tipo transação está localizada. Se vazio, o padrão é o mesmo canal do chamador.

**Chaincode**: campo de string com o nome do chaincode onde a transação a ser chamada por um evento do tipo transação está localizada. Se vazio, o padrão é o mesmo chaincode do chamador.

**CustomFunction**: uma função personalizada a ser executada por eventos do tipo personalizado. Recebe um stub do tipo *sw.StubWrapper e uma carga útil do tipo []byte, enviada junto com o evento, e retorna um erro.

**ReadOnly**: um booleano que define se as funções personalizadas para eventos do tipo personalizado podem alterar ou não o estado mundial. Se verdadeiro, nenhum ativo poderá ser confirmado no ledger pela função personalizada.

## Exemplo de Evento
O repositório cc-tools-demo possui o seguinte exemplo:
```shell
chaincode/
    eventtypes/               # pasta de eventos
        createLibraryLog.go   # definição do evento de log de biblioteca
eventTypeList.go              # lista de eventos instanciados
```

Além dos arquivos de cada evento, você deve registrar os eventos que podem ser usados pela biblioteca GoLedger CC-Tools no arquivo eventTypeList.go.

A definição do ativo CreateLibraryLog é a seguinte:

```go
var CreateLibraryLog = events.Event{
    Tag:         "createLibraryLog",
    Label:       "Create Library Log",
    Description: "Log de criação de biblioteca",
    Type:        events.EventLog,                 // Função do evento é fazer log na CCAPI
    BaseLog:     "Nova biblioteca criada",         // BaseLog é uma mensagem base a ser registrada
    Receivers:   []string{"$org2MSP", "$orgMSP"}, // Receivers são os MSPs que receberão o evento
}

```

De acordo com a descrição acima, o evento CreateLibraryLog tem as seguintes características:

* É do tipo de log, registrando suas mensagens nos logs da CCAPI.
* Cada evento desse tipo recebido terá a mensagem base "Nova biblioteca criada" registrada na CCAPI.
* Apenas org2 e org serão registrados para esperar eventos desse tipo.

## Lista de Eventos
O registro de eventos do GoLedger CC-Tools deve ser definido no arquivo chaincode/eventTypeList.go.

```go
var eventTypeList = []events.Event{
    eventtypes.CreateLibraryLog,
}
```

## Chamando os Eventos
Eventos CC-Tools podem ser chamados a partir das transações usando o objeto stub fornecido e um []byte pode ser enviado junto.

Para eventos de log, a carga útil é uma string serializada:

```go
payload, ok := json.Marshal("a carga útil do evento")
```

Para eventos de transação, a carga útil é um objeto serializado contendo os parâmetros para a transação chamada:

```go
payload, ok := json.Marshal(map[string]interface{}{
    "library": libraryAsset,
})
```

Para eventos personalizados, a carga útil pode ser qualquer coisa no formato []byte, para ser tratada adequadamente pela função personalizada.

É possível chamar eventos usando o objeto de evento:

```go
eventObj.CallEvent(stub, payload)
```

Ou usando a tag do evento para ativá-lo:

```go
events.CallEvent(stub, "createLibraryLog", payload)
```

# Transactions
As transações GoLedger CC-Tools representam os métodos GoLang que podem modificar os ativos dentro do ledger Blockchain (Canal Hyperledger Fabric).

Ativos Hyperledger Fabric só podem ser criados ou modificados executando transações de chaincode dentro de peers de endosso.

A biblioteca GoLedger CC-Tools possui uma variedade de transações predefinidas:

**CreateAsset**: criação de um novo ativo

**UpdateAsset**: atualiza um ativo existente

**DeleteAsset**: remove um ativo do estado atual do ledger

**ReadAsset**: lê o ativo em seu último estado no ledger

**ReadAssetHistory**: histórico do estado do ativo no ledger

**Search**: listagem de ativos

A biblioteca GoLedger CC-Tools permite a criação de transações personalizadas.

O repositório cc-tools-demo fornece 3 exemplos de transações:

**CreateNewLibrary**: cria um novo ativo do tipo Library

**GetBooksByAuthor**: retorna ativos Book dado um nome de autor

**GetNumberOfBooksFromLibrary**: retorna o número de ativos Book dentro do ativo Library

**UpdateBookTenant**: atualiza o campo currentTenant dentro do ativo Book
A definição de ativos é feita na pasta chaincode/txdefs para a biblioteca GoLedger CC-Tools.

Os arquivos da pasta txdefs são mostrados abaixo:

```shell
chaincode/
    txdefs/                             # pasta de transações
        createNewLibrary.go             # criação de biblioteca
        getBooksByAuthor.go             # retorna livros por um autor
        getNumberOfBooksFromLibrary.go  # retorna o número de livros no ativo biblioteca
        updateBookTenant.go             # altera o locatário do livro
txList.go                               # lista de transações personalizadas
```

## Definição de Transação
A construção e definição de uma transação personalizada são feitas criando um arquivo dentro da pasta chaincode/txdefs.

Uma transação possui os seguintes campos:

**Tag**: campo de string para definir o nome da transação referenciado internamente pelo código e pelos endpoints da API Rest. Não pode ter espaços ou caracteres especiais. Se tiver o mesmo nome que uma transação predefinida (createAsset, updateAsset, deleteAsset, readAsset, readAssetHistory ou search), esta transação será substituída pela presente no diretório txdefs.

**Label**: campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.

**Description**: campo de string de descrição do ativo para ser usado por aplicativos externos. Texto livre.

**Method**: tipo de método da API Rest. Pode ser POST, GET, PUT ou DELETE.

**Callers**: usado para definir quais organizações podem chamar esta transação.

**Args**: argumentos. Possui campos próprios.

**Routine**: código-fonte da transação.

**ReadOnly**: campo booleano para indicar que a transação não altera o estado mundial.

**MetaTx**: campo booleano para indicar que a transação não codifica uma regra específica do negócio, mas um processo interno do chaincode, por exemplo, listagem de tipos de ativos disponíveis.

## Definição de Argumento de Transação

Uma transação possui um conjunto de argumentos de entrada personalizáveis.

Um argumento possui os seguintes campos:

**Tag**: campo de string para definir o nome do argumento referenciado internamente pelo código e pelos endpoints da API Rest. Não pode ter espaços ou caracteres especiais.

**Label**: campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.

**Description**: campo de string de descrição do ativo para ser usado por aplicativos externos. Texto livre.

**Required**: identifica se o argumento é obrigatório. Campo booleano.

**DataType**: tipo de propriedade. CC-Tools possui os seguintes tipos 

**padrão**: string, number, datetime, boolean e @object.

**Private**: campo booleano para indicar que o argumento será usado para dados privados.

## StubWrapper
O objetivo principal do StubWrapper é fornecer funcionalidades adicionais e simplificar o desenvolvimento de chaincodes. O StubWrapper mantém um WriteSet para garantir que as modificações feitas durante a execução de um chaincode sejam refletidas corretamente ao consultar o estado do ledger. Mesmo que essas alterações ainda não tenham sido confirmadas no ledger, o StubWrapper registra as modificações pendentes no WriteSet. Isso permite que consultas subsequentes utilizem o WriteSet para retornar os dados atualizados, garantindo consistência e precisão das informações durante a execução do chaincode. O mesmo se aplica aos dados privados.

## Exemplos de Transação
O repositório cc-tools-demo possui a seguinte configuração de transação personalizada:

```shell
chaincode/
    txdefs/                             # pasta de transações
        createNewLibrary.go             # criação de biblioteca
        getBooksByAuthor.go             # retorna livros por um autor
        getNumberOfBooksFromLibrary.go  # retorna o número de livros no ativo biblioteca
        updateBookTenant.go             # altera o locatário do livro
txList.go                               # lista de transações personalizadas
```

Além dos arquivos para cada transação que pode ser usada pela biblioteca Goledger CC-Tools, eles também devem ser registrados no arquivo txList.go.

A definição da transação CreateNewLibrary é a seguinte:

```go
var CreateNewLibrary = tx.Transaction{
    Tag:         "createNewLibrary",
    Label:       "Create New Library",
    Description: "Create a New Library",
    Method:      "POST",
    Callers:     []string{"$org3MSP"}, // Apenas org3 pode chamar esta transação

    Args: []tx.Argument{
        {
            Tag:         "name",
            Label:       "Name",
            Description: "Name of the library",
            DataType:    "string",
            Required:    true,
        },
    },
    Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
        name, _ := req["name"].(string)

        libraryMap := make(map[string]interface{})
        libraryMap["@assetType"] = "library"
        libraryMap["name"] = name

        libraryAsset, err := assets.NewAsset(libraryMap)
        if err != nil {
            return nil, errors.WrapError(err, "Failed to create a new asset")
        }

        // Salva a nova biblioteca no canal
        _, err = libraryAsset.PutNew(stub)
        if err != nil {
            return nil, errors.WrapError(err, "Error saving asset on blockchain")
        }

        // Marshal asset de volta para o formato JSON
        libraryJSON, nerr := json.Marshal(libraryAsset)
        if nerr != nil {
            return nil, errors.WrapError(nil, "failed to encode asset to JSON format")
        }

        // Marshall da mensagem a ser registrada
        logMsg, ok := json.Marshal(fmt.Sprintf("New library name: %s", name))
        if ok != nil {
            return nil, errors.WrapError(nil, "failed to encode asset to JSON format")
        }

        // Chama o evento para registrar a mensagem
        events.CallEvent(stub, "createLibraryLog", logMsg)

        return libraryJSON, nil
    },
}

```

De acordo com a descrição acima, a transação CreateNewLibrary possui as seguintes características:

* Apenas org3 pode chamar este método
* Método POST para API Rest
* Argumento name do tipo string é obrigatório.
* A transação usa a função NewAsset para preparar um novo ativo (chaves, etc.) e a função PutNew para criar o ativo no canal.
* Um evento de log para o tipo createLibraryLog é chamado com o nome da biblioteca criada.

A definição da transação GetBooksByAuthor é a seguinte:

```go
var GetBooksByAuthor = tx.Transaction{
    Tag:         "getBooksByAuthor",
    Label:       "Get Books by the Author Name",
    Description: "Return all the books from an author",
    Method:      "GET",
    Callers:     []string{"$org1MSP", "$org2MSP", "$orgMSP"}, // Apenas org1 e org2 podem chamar esta transação

    Args: []tx.Argument{
        {
            Tag:         "authorName",
            Label:       "Author Name",
            Description: "Author Name",
            DataType:    "string",
            Required:    true,
        },
        {
            Tag:         "limit",
            Label:       "Limit",
            Description: "Limit",
            DataType:    "number",
        },
    },
    Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
        authorName, _ := req["authorName"].(string)
        limit, hasLimit := req["limit"].(float64)

        if hasLimit && limit <= 0 {
            return nil, errors.NewCCError("limit must be greater than 0", 400)
        }

        // Prepara a consulta do couchdb
        query := map[string]interface{}{
            "selector": map[string]interface{}{
                "@assetType": "book",
                "author":     authorName,
            },
        }

        if hasLimit {
            query["limit"] = limit
        }

        var err error
        response, err := assets.Search(stub, query, "", true)
        if err != nil {
            return nil, errors.WrapErrorWithStatus(err, "error searching for book's author", 500)
        }

        responseJSON, err := json.Marshal(response)
        if err != nil {
            return nil, errors.WrapErrorWithStatus(err, "error marshaling response", 500)
        }

        return responseJSON, nil
    },
}

```

Apenas org1 e org2 podem chamar este método.
Método GET para API Rest.
O argumento authorName do tipo string é obrigatório.
O argumento limit do tipo number é opcional.
A transação usa a função Search para consultar ativos Book no ledger, filtrando por autor.

A definição da transação UpdateBookTenant é a seguinte:

```go
var UpdateBookTenant = tx.Transaction{
    Tag:         "updateBookTenant",
    Label:       "Update Book Tenant",
    Description: "Change the tenant of a book",
    Method:      "PUT",
    Callers:     []string{`$org\dMSP`}, // Qualquer org pode chamar esta transação

    Args: []tx.Argument{
        {
            Tag:         "book",
            Label:       "Book",
            Description: "Book",
            DataType:    "->book",
            Required:    true,
        },
        {
            Tag:         "tenant",
            Label:       "tenant",
            Description: "New tenant of the book",
            DataType:    "->person",
        },
    },
    Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
        bookKey, ok := req["book"].(assets.Key)
        if !ok {
            return nil, errors.WrapError(nil, "Parameter book must be an asset")
        }
        tenantKey, ok := req["tenant"].(assets.Key)
        if !ok {
            return nil, errors.WrapError(nil, "Parameter tenant must be an asset")
        }

        // Retorna Book do canal
        bookAsset, err := bookKey.Get(stub)
        if err != nil {
            return nil, errors.WrapError(err, "failed to get asset from the ledger")
        }
        bookMap := (map[string]interface{})(*bookAsset)

        // Retorna person do canal
        tenantAsset, err := tenantKey.Get(stub)
        if err != nil {
            return nil, errors.WrapError(err, "failed to get asset from the ledger")
        }
        tenantMap := (map[string]interface{})(*tenantAsset)

        updatedTenantKey := make(map[string]interface{})
        updatedTenantKey["@assetType"] = "person"
        updatedTenantKey["@key"] = tenantMap["@key"]

        // Atualiza dados
        bookMap["currentTenant"] = updatedTenantKey

        bookMap, err = bookAsset.Update(stub, bookMap)
        if err != nil {
            return nil, errors.WrapError(err, "failed to update asset")
        }

        // Marshal asset de volta para o formato JSON
        bookJSON, nerr := json.Marshal(bookMap)
        if nerr != nil {
            return nil, errors.WrapError(err, "failed to marshal response")
        }

        return bookJSON, nil
    },
}

```

Qualquer organização que comece com "org" seguido de um número (org1, org2, org3, org4, etc.) pode chamar este método.
Método PUT para API Rest
O argumento book do tipo Book é obrigatório.
O argumento person do tipo Person é obrigatório.
A transação usa a função Get para ler as informações dos ativos Book e Person do ledger.
A transação usa a função Update para atualizar as informações do ativo Book no ledger.

A definição da transação GetNumberOfBooksFromLibrary é a seguinte:

```go
var GetNumberOfBooksFromLibrary = tx.Transaction{
    Tag:         "getNumberOfBooksFromLibrary",
    Label:       "Get Number Of Books From Library",
    Description: "Return the number of books of a library",
    Method:      "GET",
    Callers:     []string{"$org2MSP", "$orgMSP"}, // Apenas org2 pode chamar esta transação

    Args: []tx.Argument{
        {
            Tag:         "library",
            Label:       "Library",
            Description: "Library",
            DataType:    "->library",
            Required:    true,
        },
    },
    Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
        libraryKey, _ := req["library"].(assets.Key)

        // Retorna Library do canal
        libraryMap, err := libraryKey.GetMap(stub)
        if err != nil {
            return nil, errors.WrapError(err, "failed to get asset from the ledger")
        }

        numberOfBooks := 0
        books, ok := libraryMap["books"].([]interface{})
        if ok {
            numberOfBooks = len(books)
        }

        returnMap := make(map[string]interface{})
        returnMap["numberOfBooks"] = numberOfBooks

        // Marshal asset de volta para o formato JSON
        returnJSON, nerr := json.Marshal(returnMap)
        if nerr != nil {
            return nil, errors.WrapError(err, "failed to marshal response")
        }

        return returnJSON, nil
    },
}

```

Apenas org2 pode chamar este método.
Método GET para API Rest
O argumento library do tipo Library é obrigatório.
A transação usa a função Get para ler as informações do ativo Library do ledger.

## Lista de Transações
O registro das transações que serão usadas pela biblioteca GoLedger CC-Tools também deve ser feito dentro do arquivo chaincode/txList.go.

```go
var txList = []tx.Transaction{
    tx.CreateAsset,
    tx.UpdateAsset,
    tx.DeleteAsset,

    txdefs.CreateNewLibrary,
    txdefs.GetNumberOfBooksFromLibrary,
    txdefs.UpdateBookTenant,
    txdefs.GetBooksByAuthor,
}

```

## META-INF
Índices permitem que um banco de dados seja consultado sem ter que examinar cada linha com cada consulta, tornando-os mais rápidos e eficientes. É obrigatório criar índices para campos que serão usados em consultas ordenadas.

Os arquivos de índice JSON devem estar localizados no caminho META-INF/statedb/couchdb/indexes, que está localizado dentro do diretório onde o chaincode reside. Por exemplo:

```json
{
    "index":{
        "fields":[
            {"published": "asc"}
        ]
    },
    "ddoc":"indexListarBooksAscDoc",
    "name":"indexListarBooksAsc",
    "type":"json"
}
```

# Ferramentas Externas
A biblioteca GoLedger CC-Tools possui uma variedade de ferramentas externas que são extremamente úteis no desenvolvimento de uma Blockchain com permissões baseada no Hyperledger Fabric.

## cc-tools-demo
Repositório público com exemplos de ativos e transações utilizando a biblioteca GoLedger CC-Tools.

O repositório pode ser acessado da seguinte maneira:

```bash
git clone https://github.com/hyperledger-labs/cc-tools-demo.git
```

## cc-web-client
Imagem Docker que fornece uma aplicação web para testes.

O container pode ser instanciado da seguinte forma:

```shell
# Solução alternativa para erro de plataforma ARM64 (MAC) para AMD64 (Ubuntu)
sudo apt install -y qemu qemu-system-x86 binfmt-support qemu-user-static
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes

docker run -p 0.0.0.0:8080:80/tcp --name cc-webclient goledger/cc-webclient:latest
```

## GoFabric
GoFabric é uma plataforma de orquestração para o Hyperledger Fabric, totalmente compatível com a biblioteca GoLedger CC-Tools.

A plataforma pode ser acessada pelo seguinte link: https://gofabric.io e possui as seguintes características:

* Implantação de redes em ambientes em nuvem (AWS, IBM, Azure, etc) ou on-premise
* Instantiação de chaincodes (compatibilidade com CC-Tools)
* Atualização de chaincodes
* Adição ou remoção de peers
* Adição de orderers
* Adição de organizações
* Instantiação ou atualização de Servidores Rest
* Geração automática de código através dos Templates GoLedger


# Testes

O GoLedger CC-Tools oferece diferentes maneiras de testar o código-fonte no modo de desenvolvimento.

Para verificar a sintaxe do GoLang, execute o seguinte comando:

```bash
cd chaincode \
go vet
```

Após a instância ou atualização bem-sucedida do código, você pode verificar os logs diretamente dentro dos contêineres de execução do chaincode. Esses contêineres podem ser identificados iniciando com dev.

```shell
docker logs dev-peer0.org1.example.com-cc-tools-demo-0.1
```

## Usando o cc-webclient
Para realizar testes e integrações de maneira satisfatória, é sugerido o uso da ferramenta cc-webclient. Após concluir as etapas, execute os seguintes comandos para criar 3 aplicativos para se conectar às organizações 1, 2 e 3.

```shell
./run-cc-web.sh 8080 & \
./run-cc-web.sh 8090 & \
./run-cc-web.sh 8100 &
```

## Configurando rest-server e cc-webclient
Após a execução dos contêineres do cc-webclient, os aplicativos podem ser acessados diretamente através das portas definidas no script, por exemplo, 8080, 8090, 8100.

Ao acessar o cc-webclient pelo navegador, a configuração do endereço do servidor rest pode ser feita clicando no ícone da ferramenta.

## Configuração
Os acessos podem ser feitos com as seguintes configurações:

* org1: http://localhost:80
* org2: http://localhost:980
* org3: http://localhost:1080

## Barra lateral
O aplicativo cc-webclient possui uma barra lateral que mostra os ativos disponíveis, bem como as transações registradas no chaincode.

## Uso de endpoints
O uso do endpoint do rest-server é mostrado usando os botões CURL.

## CURL
Para cada tela, você pode verificar o uso do endpoint pressionando o botão curl.

Por exemplo, na tela de criação de ativos:

## CURL
## Listar, criar, editar, excluir ou verificar o histórico de um ativo

Para listar cada ativo, basta selecionar um ativo na barra lateral.

## Listar Ativo
Ao selecionar o botão CRIAR na janela de lista de ativos, aparecerá uma tela de criação de ativos.

Por exemplo, para o ativo Pessoa.

## Criar Ativo
A tela de edição é acessada selecionando o ícone de edição na janela de lista de ativos.

A remoção de um ativo é solicitada selecionando o ícone de exclusão na janela de lista de ativos.

O histórico de um ativo (todas as alterações registradas no livro-razão) pode ser visualizado selecionando o ícone de histórico na janela de um ativo.

## Executando uma transação
A execução de uma transação pode ser realizada selecionando a transação na barra lateral.

Por exemplo, para a transação UpdateBookTenant.

## Transação
## Usando Godog
Godog é o framework oficial de BDD Cucumber para Golang. Ele segue os princípios do BDD, que enfatiza a definição do comportamento desejado do software por meio de cenários focados no usuário escritos em linguagem natural. Esses cenários são escritos em um formato Given-When-Then, facilitando para os membros da equipe não técnicos entenderem e contribuírem para o processo de teste.

Para o repositório cc-tools-demo, existem testes godog para cada transação. Cada transação é representada como um recurso e cada recurso tem cenários a serem testados.

Os recursos são definidos na pasta chaincode/tests/features.

```shell
chaincode/
  tests/
    features/
      createNewLibrary.feature              # definição do recurso Criar Nova Biblioteca
      getBooksByAuthor.feature              # definição do recurso Obter Livros por Autor
      getNumberOfBooksFromLibrary.feature   # definição do recurso Obter Número de Livros da Biblioteca
      updateBookTenant.feature              # definição do recurso Atualizar Inquilino do Livro
    request_test.go                         # implementação de etapas para cenários
```

## Definição de Recurso
No arquivo de recurso, comece definindo o recurso usando a palavra-chave Feature seguida de uma breve descrição. Cada cenário dentro do recurso é definido usando a palavra-chave Scenario, seguida de um título ou descrição do cenário. Os cenários representam casos específicos ou situações que você deseja testar. Dentro de cada cenário, você define as etapas do teste usando as palavras-chave Given, When e Then. Essas etapas descrevem as precondições (Given), ações (When) e resultados esperados (Then) do cenário. Opcionalmente, você também pode usar And e But para esclarecer ou adicionar etapas adicionais.

Para mais informações sobre Godog/Cucumber, consulte Godog/Cucumber.

## Exemplos de Recursos
A definição do recurso Criar Nova Biblioteca é a seguinte:

```gherkin
Feature: Criar Nova Biblioteca
  Com o objetivo de criar uma nova biblioteca
  Como um cliente da API
  Eu quero fazer uma solicitação com o nome da biblioteca desejada

  Scenario: Criar uma nova biblioteca
      Given there is a running "" test network from scratch
      When I make a "POST" request to "/api/invoke/createNewLibrary" on port 880 with:
          """
          {
              "name": "Biblioteca da Elizabeth"
          }
          """
      Then the response code should be 200
      And the response should have:
          """
          {
              "@key":         "library:9cf6726a-a327-568a-baf1-5881393073bf",
              "@lastTouchBy": "orgMSP",
              "@lastTx":      "createNewLibrary",
              "@assetType":   "library",
              "name":         "Biblioteca da Elizabeth"
          }
          """
Cenário: Tentar criar uma nova biblioteca com um nome que já existe Dado que existe uma rede de teste “” em execução Dado que existe uma biblioteca com o nome “John’s Library” Quando eu faço uma requisição “POST” para “/api/invoke/createNewLibrary” na porta 880 com: “”" { “nome”: “John’s Library” } “”" Então o código de resposta deve ser 409


Esse recurso testa a capacidade de criar uma nova biblioteca usando a transação createNewLibrary através da API. Ele consiste em dois cenários: um caso de sucesso e outro testa ao tentar criar uma biblioteca com um nome que já existe.

Cenário 1: Este cenário representa a criação bem-sucedida de uma nova biblioteca. Ele verifica se a API pode lidar corretamente com a solicitação com um novo nome de biblioteca e retorna a resposta esperada.

Cenário 2: Este cenário testa o caso ao tentar criar uma biblioteca com um nome que já existe no sistema. Ele verifica se a API responde com o código de status apropriado (409) para indicar um conflito.

A definição do recurso Obter Livros por Autor é a seguinte:

```gherkin
Feature: Obter Livros por Autor
  Com o objetivo de obter todos os livros de um autor
  Como um cliente da API
  Eu quero fazer uma solicitação à transação getBooksByAuthor
  E receber os livros apropriados

  Scenario: Solicitar um autor com vários livros
      Given there is a running "" test network
      And there are 3 books with prefix "book" by author "Jack"
      When I make a "GET" request to "/api/query/getBooksByAuthor" on port 880 with:
          """
          {
              "authorName": "Jack"
          }
          """
      Then the response code should be 200
      And the "result" field should have size 3

  ...


```

Este recurso testa a capacidade de recuperar livros escritos por um autor específico usando a transação getBooksByAuthor através da API. Ele consiste em três cenários que abrangem diferentes casos com base no número de livros do autor solicitado.

Cenário 1: Este cenário verifica se a API pode lidar com sucesso com uma solicitação para um autor que possui vários livros no sistema. Ele verifica se a API retorna o código de resposta correto (200) e se o campo "result" na resposta contém todos os livros do autor com um tamanho de 3.

A definição do recurso Obter Número de Livros da Biblioteca é a seguinte:

```gherkin
Feature: Obter Número de Livros da Biblioteca
  Com o objetivo de obter o número de livros de uma biblioteca
  Como um cliente da API
  Eu quero fazer uma solicitação

  Scenario: Consultar Obter Número de Livros da Biblioteca que existe
      Given there is a running "" test network
      And I make a "POST" request to "/api/invoke/createAsset" on port 880 with:
          """
          {
              "asset": [
                  {
                      "@assetType": "book",
                      "title":      "Meu Nome é Maria",
                      "author":     "Maria Viana"
                  }
              ]
          }
          """
      And I make a "POST" request to "/api/invoke/createAsset" on port 880 with:
          """
          {
              "asset": [{
                  "@assetType": "library",
                  "name": "Biblioteca da Maria",
                  "books": [
                      {
                          "@assetType": "book",
                          "@key": "book:a36a2920-c405-51c3-b584-dcd758338cb5"
                      }
                  ]
              }]
        }
          """
      When I make a "GET" request to "/api/query/getNumberOfBooksFromLibrary" on port 880 with:
          """
          {
              "library": {
                  "@key": "library:3cab201f-9e2b-579d-b7b2-72297ed17f49",
                  "@assetType": "library"
          }
          }
          """
      Then the response code should be 200
      And the response should have:
          """
          {
              "numberOfBooks": 1.0
          }
          """

```

Este recurso testa a capacidade de recuperar o número de livros de uma biblioteca por meio da API. Ele consiste em dois cenários: um cenário lida com a consulta de uma biblioteca que existe e possui livros, enquanto o outro cenário testa a consulta de uma biblioteca que não existe.

Cenário 1: Este cenário verifica se a API pode recuperar com sucesso o número de livros de uma biblioteca que existe no sistema. Primeiro, cria um novo livro e uma biblioteca com o livro associado. Em seguida, consulta o número de livros da biblioteca e verifica se a API retorna o código de resposta correto (200) e se a resposta contém o número esperado de livros (1).

Cenário 2: Este cenário testa o caso ao tentar consultar o número de livros de uma biblioteca que não existe no sistema. Verifica se a API retorna o código de resposta apropriado (400) para indicar uma solicitação inválida devido à biblioteca inexistente.

A definição do recurso Atualizar Inquilino do Livro é a seguinte:

```gherkin
Feature: Atualizar Inquilino do Livro
  Com o objetivo de atualizar o inquilino de um livro
  Como um cliente da API
  Eu quero fazer uma solicitação

  Scenario: Atualizar livro com um inquilino existente 
      # As primeiras 3 declarações serão usadas por todos os cenários neste recurso
      Given there is a running "" test network
      And I make a "POST" request to "/api/invoke/createAsset" on port 880 with:
          """
          {
              "asset": [{
                      "@assetType": "book",
                      "title":      "Meu Nome é Maria",
                      "author":     "Maria Viana"
                  }]
          }
          """
      And I make a "POST" request to "/api/invoke/createAsset" on port 880 with:
          """
         {
              "asset": [{
                  "@assetType": "person",
                  "name": "Maria",
                  "id": "31820792048"
              }]
        }
          """
      When I make a "PUT" request to "/api/invoke/updateBookTenant" on port 880 with:
          """
          {
              "book": {
                  "@assetType": "book",
                  "@key": "book:a36a2920-c405-51c3-b584-dcd758338cb5"
          },
              "tenant": {
                  "@assetType": "person",
                  "@key": "person:47061146-c642-51a1-844a-bf0b17cb5e19"
              }
          }
          """
      Then the response code should be 200
      And the response should have:
          """
          {
              "@key": "book:a36a2920-c405-51c3-b584-dcd758338cb5",
              "@lastTouchBy": "orgMSP",
              "@lastTx": "updateBookTenant",
              "currentTenant": {
            "@assetType": "person",
            "@key": "person:47061146-c642-51a1-844a-bf0b17cb5e19"
          }
          }
          """

```

Este recurso testa a capacidade de atualizar o inquilino de um livro usando a API. Ele consiste em dois cenários: um cenário lida com a atualização de um livro com um inquilino existente, e o outro cenário testa a atualização de um livro com um inquilino que não existe.

Cenário 1: Este cenário verifica se a API pode atualizar com sucesso o inquilino de um livro com um inquilino existente. Cria um novo livro e uma nova pessoa (inquilino), associa a pessoa ao livro e, em seguida, faz uma solicitação PUT para atualizar o inquilino do livro. O cenário verifica se a API retorna o código de resposta correto (200) e se o inquilino do livro é atualizado corretamente na resposta.

Cenário 2: Este cenário testa o caso ao tentar atualizar o inquilino de um livro com um inquilino que não existe no sistema. Faz uma solicitação PUT para atualizar o inquilino do livro com o inquilino inexistente. O cenário verifica se a API retorna o código de resposta apropriado (404) para indicar que o inquilino não existe no sistema.

A implementação de etapas
Cada etapa é implementada no arquivo chaincode/tests/request_test.go.

Para Given there is a running "" test network from scratch, há a seguinte implementação:

```go
func thereIsARunningTestNetworkFromScratch(arg1 string) error {
    // Iniciar a rede de teste com apenas 1 organização
    cmd := exec.Command("../../startDev.sh", "-n", "1")

    _, err := cmd.Output()

    if err != nil {
        fmt.Println(err.Error())
        return err
    }

    // Aguardar o ccapi
    err = waitForNetwork("880")
    if err != nil {
        fmt.Println(err.Error())
        return err
    }

    return nil
}

```

Esta função inicia uma rede de teste a partir do zero com uma única organização. Executa um comando shell ../../startDev.sh -n 1 para iniciar a rede de teste. Após iniciar a rede, aguarda a disponibilidade do ccapi na porta 880 usando a função waitForNetwork.

Para When I make a "[method]" request to "[endpoint]" on port [port] with "[body]", há a seguinte implementação:

```go
func iMakeARequestToOnPortWith(ctx context.Context, method, endpoint string, port int, reqBody *godog.DocString) (context.Context, error) {
    var res *http.Response
    var req *http.Request
    var err error

    // Inicializar cliente http
    client := &http.Client{}

    // Criar solicitação
    if method == "GET" {
        b64str := b64.StdEncoding.EncodeToString([]byte(reqBody.Content))
        reqParam := "?@request=" + b64str
        req, err = http.NewRequest("GET", "http://localhost:"+strconv.Itoa(port)+endpoint+reqParam, nil)
        if err != nil {
            return ctx, err
        }
    } else {
        dataAsBytes := bytes.NewBuffer([]byte(reqBody.Content))

        req, err = http.NewRequest(method, "http://localhost:"+strconv.Itoa(port)+endpoint, dataAsBytes)
        if err != nil {
            return ctx, err
        }
    }

    // Definir cabeçalho e fazer a solicitação
    req.Header.Set("Content-Type", "application/json")
    res, err = client.Do(req)
    if err != nil {
        return ctx, err
    }

    // Obter código de status e corpo da resposta
    statusCode := res.StatusCode
    resBody, err := ioutil.ReadAll(res.Body)

    if err != nil {
        return ctx, err
    }
    res.Body.Close()

    // Adicionar código de status e corpo da resposta ao contexto
    bodyCtx := context.WithValue(ctx, bodyCtxKey{}, resBody)
    statusCtx := context.WithValue(bodyCtx, statusCtxKey{}, statusCode)
    return statusCtx, nil
}

```

Esta função faz uma solicitação HTTP a um endpoint específico em uma porta fornecida. A função suporta métodos GET e POST. Se o método for GET, o corpo da solicitação será codificado em base64 e adicionado como um parâmetro de consulta. Se o método for POST, o corpo da solicitação será incluído na solicitação como dados JSON.

Para Then the response code should be [code], há a seguinte implementação:

```go
func theResponseCodeShouldBe(ctx context.Context, expectedCode int) (context.Context, error) {
    // Obter código de status do contexto
    statusCode, ok := ctx.Value(statusCtxKey{}).(int)
    if !ok {
        return ctx, errors.New("contexto indisponível ao recuperar status")
    }

    if statusCode != expectedCode {
        // Obter corpo da resposta do contexto
        resBody, ok := ctx.Value(bodyCtxKey{}).([]byte)
        if !ok {
            return ctx, errors.New("contexto indisponível ao recuperar corpo")
        }

        // Teste falhou
        return ctx, fmt.Errorf("recebeu uma resposta de status incorreta. Recebido %d Esperado: %d\nCorpo da resposta: %s", statusCode, expectedCode, string(resBody))
    }

    return ctx, nil
}

```

Esta função verifica se o código de status da resposta HTTP recebida corresponde ao código esperado.

Para And the response should have, há a seguinte implementação:

```go
func theResponseShouldHave(ctx context.Context, body *godog.DocString) error {
    // Obter 'ResponseBody' do contexto
    respBody, ok := ctx.Value(bodyCtxKey{}).([]byte)
    if !ok {
        return errors.New("contexto indisponível")
    }

    var expected map[string]interface{}
    var received map[string]interface{}

    if err := json.Unmarshal([]byte(body.Content), &expected); err != nil {
        return err
    }
    if err := json.Unmarshal(respBody, &received); err != nil {
        return err
    }

    for key, value := range expected {
        if !reflect.DeepEqual(value, received[key]) {
            var expectedBytes []byte
            var receivedBytes []byte
            var err error
            if expectedBytes, err = json.MarshalIndent(value, "", "  "); err != nil {
                return err
            }
            if receivedBytes, err = json.MarshalIndent(received[key], "", "  "); err != nil {
                return err
            }

            return fmt.Errorf("Esperava que %s fosse igual a %s, mas recebeu %s", key, expectedBytes, receivedBytes)
        }
    }

    return nil
}

```

# Godog Testes e Testes de Golang para o Chaincode do Hyperledger Fabric

## Verificação de Resposta HTTP em Godog

Esta função verifica se o corpo da resposta HTTP recebida corresponde ao corpo de resposta esperado definido na etapa do cenário Godog. Primeiro, ela obtém o corpo de resposta real e o corpo de resposta esperado do contexto. Deserializa ambas as strings JSON em estruturas semelhantes a mapas para facilitar a comparação. A função realiza uma comparação profunda entre o valor de cada chave nos corpos de resposta esperados e recebidos usando a função `reflect.DeepEqual`.

### Para "O campo [field] deve ter tamanho [size]"

```go
func theFieldShouldHaveSize(ctx context.Context, field string, expectedSize int) (context.Context, error) {
    // Obter 'ResponseBody' do contexto
    respBody, ok := ctx.Value(bodyCtxKey{}).([]byte)
    if !ok {
        return ctx, errors.New("contexto indisponível")
    }

    var bodyMap map[string]interface{}

    if err := json.Unmarshal(respBody, &bodyMap); err != nil {
        return ctx, err
    }

    resultField, ok := bodyMap[field]
    if !ok {
        return ctx, errors.New("campo não disponível no corpo da resposta")
    }

    fieldLen := len(resultField.([]interface{}))
    if fieldLen != expectedSize {
        // Teste Falhou
        return ctx, fmt.Errorf("tamanho do campo recebido incorreto no corpo da resposta. Recebido %d, Esperado: %d\nCorpo da resposta: %s", fieldLen, expectedSize, string(respBody))
    }

    return ctx, nil
}
```

Esta função é usada para verificar se um campo específico no corpo da resposta JSON recebido tem o tamanho esperado (número de elementos). Geralmente, é usada em cenários Godog para verificar o tamanho de uma matriz ou campo semelhante a uma lista na resposta.

Para "Existem [número] livros com prefixo "[prefixo]" do autor "[nome]"

```go
func thereAreBooksWithPrefixByAuthor(ctx context.Context, nBooks int, prefix string, author string) (context.Context, error) {
    // Implementação...
}

```

Esta função é usada para garantir que exista um número específico de livros com um determinado prefixo e autor no sistema. Geralmente, é usada em cenários Godog para configurar os dados de teste necessários antes de executar testes que envolvem livros e autores.

Para "Dado que existe uma biblioteca com o nome "[nome]"

```go
func thereIsALibraryWithName(ctx context.Context, name string) (context.Context, error) {
    // Implementação...
}

```

Esta função é usada para garantir que uma biblioteca com um nome específico exista no sistema. Geralmente, é usada em cenários Godog para configurar os dados de teste necessários antes de executar testes que envolvem bibliotecas. Se a biblioteca não existir, ela cria um novo ativo de biblioteca.

É necessário inicializar o cenário com a implementação de cada etapa. A função abaixo descreve como fazer isso:

```go
func InitializeScenario(ctx *godog.ScenarioContext) {
    // Implementação das etapas Godog...
}

```

## Executando Testes Godog
Comece instalando o Godog usando o seguinte comando:

```shell
$ go install github.com/cucumber/godog/cmd/godog@latest
```

Depois de instalar com sucesso o Godog, utilize o script fornecido para executar os testes facilmente:

```shell
$ ./godog.sh
```

## Usando Testes Golang
Também é possível testar as transações desenvolvidas usando a biblioteca de testes Golang. Este tipo de teste permite simular uma chamada de transação e comparar seus resultados com um valor esperado.

É importante observar que esse tipo de teste não funciona em transações que leem o banco de dados de estado do chaincode (como consultas couchdb) nem transações que leem o histórico de um ativo.

Para o repositório cc-tools-demo, existem testes para as transações createNewLibrary, getNumberOfBooksFromLibrary e updateBookTenant.

Os testes estão definidos na pasta chaincode/:

* **main_test.go**: Configuração do ambiente de teste.
* **txdefs_createNewLibrary_test**.go: Testa a transação createNewLibrary.
* **txdefs_getNumberOfBooksFromLibrary_test.go**: Testa a transação getNumberOfBooksFromLibrary.
* **txdefs_updateBookTenant_test.go**: Testa a transação updateBookTenant.

## Exemplos de Teste
A definição do teste Create New Library é a seguinte:

```go
func TestCreateNewLibrary(t *testing.T) {
    // Implementação...
}

```

Os seguintes passos ocorrem neste teste:

* Um mock stub para org3 é criado.
* Os objetos com a resposta esperada para a transação createNewLibrary e a solicitação da transação são criados.
* A transação createNewLibrary é invocada usando a solicitação definida na etapa anterior.
* O carimbo de data/hora do stub é adicionado ao objeto expectedResponse.
* O status da invocação é verificado para o código 200.
* Uma comparação profunda é feita entre o objeto de resposta esperado e a resposta da invocação.
* O estado para a chave do objeto criado no stub também é comparado com a resposta esperada.

Os demais testes seguem uma lógica semelhante.

## Executando os Testes
Para executar os testes, execute o seguinte comando:

```sh
cd chaincode && \
go test && \
cd ..

```

Se os testes foram bem-sucedidos, as seguintes mensagens devem aparecer no terminal:

```go
main.go:131: 200 init 40.796µs 
main.go:131: 200 getNumberOfBooksFromLibrary 31.298µs 
main.go:131: 200 updateBookTenant 136.09µs 
main.go:131: 200 createNewLibrary 60.794µs 
PASS
ok      github.com/hyperledger-labs/cc-tools-demo/chaincode  0.006s

```

```sh
sudo npm install -g localtunnel

sudo lt --port 8080 --subdomain blockchain-stcs
sudo lt --port 80 --subdomain org1-stcs
sudo lt --port 980 --subdomain org2-stcs
sudo lt --port 1080 --subdomain org3-stcs
```
