# CC-Tools
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
pip install --upgrade paramiko


docker-compose -v
docker-compose version 1.29.2, build 5becea4c
```

#### Node.js 12.18.2 e npm 6.14.5(Versão suportada)
* Instalação
```shell
sudo apt-get remove nodejs

sudo apt-get install npm
sudo npm install -g n
sudo rm /usr/bin/node
sudo n 12.18.2
sudo ln -s /usr/local/bin/node /usr/bin/node
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
```

Ver se é necessário:
```go
# Baixar o arquivo tarball do GoLang 1.14+
wget https://dl.google.com/go/go1.14.3.linux-amd64.tar.gz

# Extrair o arquivo tarball na pasta /usr/local
sudo tar -C /usr/local -xvf go1.14.3.linux-amd64.tar.gz

# Adicionar o caminho /usr/local/go/bin à variável de ambiente PATH
echo "export PATH=$PATH:/usr/local/go/bin" >> $HOME/.profile

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
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)


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
./renameProject.sh my-first-chaincode
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