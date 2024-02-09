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

* Docker 19+
```shell
matheuslazaro@matheuslazaro:~/cc-tools-demo$ docker -v
Docker version 19.03.12, build 48a66213fe
```
* GCC
```shell
matheuslazaro@matheuslazaro:~/cc-tools-demo$ gcc --version
gcc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
Copyright (C) 2021 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

* GoLang 1.14+
```shell
matheuslazaro@matheuslazaro:~/cc-tools-demo$ go version
go version go1.15.2 linux/amd64
```

* NodeJs 10+
```shell
matheuslazaro@matheuslazaro:~/cc-tools-demo$ node -v
v12.18.2
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














# Definição de Ativos

Os ativos de blockchain representam as informações que serão utilizadas pelo livro-razão da blockchain (Hyperledger Fabric Channel).

De maneira geral, um ativo pode ser comparado a uma tabela em um banco de dados relacional.

Assim como em bancos de dados, os ativos são formados por propriedades, algumas das quais podem fazer parte de um conjunto de chaves para esse ativo. Cada propriedade pode ter um tipo de dado específico, como string, número, booleano, etc.

Um ativo também pode ser um Dado Privado do Hyperledger Fabric, no qual o conteúdo da informação é registrado em um banco de dados transitório fora do livro-razão e apenas um subconjunto de organizações recebe esses dados, configurando a legibilidade.

Ativos também podem ser criados dinamicamente durante a execução do chaincode, usando a funcionalidade de Tipos de Ativos Dinâmicos do CC-Tool, na qual os tipos de ativos são definidos usando chamadas de invocação ao chaincode em execução.

## Exemplos de Ativos:

- Person
- Book
- Library
- Secret (dados privados)

## Definição de Ativos:

A definição de ativos é realizada na pasta chaincode/assettypes da biblioteca GoLedger CC-Tools. Abaixo está a lista de arquivos nesta pasta:

- person.go: definição de uma pessoa
- book.go: definição de um livro
- library.go: definição de uma biblioteca
- secret.go: definições de um segredo (dados privados)
- customAssets.go: lista de ativos inseridos via modo de Template do GoFabric
- dynamicAssetTypes.go: arquivo de configuração para Tipos de Ativos Dinâmicos

## Definição de Ativo:

- Tag: campo de string usado para definir o nome do ativo referenciado internamente pelo código e pelos pontos de extremidade da API Rest. Sem espaços ou caracteres especiais permitidos.
- Label: campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.
- Description: campo de string para descrição do ativo a ser usado por aplicativos externos. Texto livre.
- Props: propriedades do ativo. Possui seus próprios campos (descrição a seguir).
- Readers: usado para definir organizações de dados privados. Se não estiver vazio, apenas as organizações definidas na matriz terão a capacidade de ler ativos do tipo.
- Validate: função de validação de ativo. Permite um código personalizado para validar o ativo como um todo.
- Dynamic: campo booleano para indicar se o tipo de ativo pode ser modificado dinamicamente.

## Definição de Propriedade:

Uma propriedade tem os seguintes campos:

- Tag: campo de string usado para definir o nome da propriedade referenciado internamente pelo código e pelos pontos de extremidade da API Rest. Sem espaços ou caracteres especiais permitidos.
- Label: campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.
- Description: campo de string para descrição da propriedade a ser usado por aplicativos externos. Texto livre.
- IsKey: identifica se a propriedade faz parte das chaves do ativo.
- Required: identifica se a propriedade é obrigatória.
- ReadOnly: identifica se a propriedade não pode mais ser modificada após a criação.
- DefaultValue: valor padrão da propriedade.
- Writers: define as organizações que podem criar ou alterar essa propriedade.
- DataType: tipo de propriedade.
- Validate: função de validação de propriedade.

## Exemplos de Ativos:

O repositório cc-tools-demo possui exemplos de ativos, como person, book, library e secret, cada um com suas próprias definições.

### Definição do Ativo Person:

- Tag: "person"
- Label: "Person"
- Description: "Dados pessoais de alguém"
- Props: Lista de propriedades, incluindo ID (chave primária), nome, data de nascimento e altura.

### Definição do Ativo Book:

- Tag: "book"
- Label: "Book"
- Description: "Livro"
- Props: Lista de propriedades, incluindo título (chave composta), autor (chave composta), inquilino atual, gêneros, data de publicação e tipo de livro.

### Definição do Ativo Library:

- Tag: "library"
- Label: "Library"
- Description: "Biblioteca como coleção de livros"
- Props: Lista de propriedades, incluindo nome (chave primária), coleção de livros e código de entrada para a biblioteca.

### Definição do Ativo Secret:

- Tag: "secret"
- Label: "Secret"
- Description: "Segredo entre Org2 e Org3"
- Readers: Lista de organizações permitidas para leitura.
- Props: Lista de propriedades, incluindo nome secreto (chave primária) e segredo.

## Lista de Ativos:

A lista de ativos registrados no GoLedger CC-Tools está definida no arquivo chaincode/assetTypeList.go.

## Tipos de Ativos Dinâmicos:

CC-Tools permite a definição e criação dinâmica de ativos durante o tempo de execução, por meio de invocações ao chaincode.

### Configuração:

- Enabled: define se os tipos de ativos dinâmicos estão habilitados ou desabilitados durante o tempo de execução.
- Readers: usado para especificar quais organizações têm permissão para criar e gerenciar dinamicamente o tipo de ativo.

### Gerenciamento Dinâmico de Tipos de Ativos:

- createAssetType: criação de tipos de ativos dinamicamente.
- updateAssetType: atualização de tipos de ativos dinamicamente.
- deleteAssetType: exclusão de tipos de ativos dinamicamente.

### Exemplos de Criação de Tipos de Ativos Dinâmicos:

Payload JSON enviado em uma solicitação POST para criar um novo tipo.

### Exemplos de Atualização de Tipos de Ativos Dinâmicos:

Payload JSON enviado em uma solicitação POST para atualizar um tipo existente.

### Exemplos de Exclusão de Tipos de Ativos Dinâmicos:

Payload JSON enviado em uma solicitação POST para excluir um tipo existente.

Essas transações podem ser acessadas por meio da interface GoInitus ou por meio de solicitações POST para os pontos de extremidade da API Rest.










# Tipos de Dados

Os tipos de dados da GoLedger CC-Tools são uma maneira de categorizar e definir a natureza dos dados que podem ser armazenados em um ativo e manipulados em transações. É essencial validar se os dados inseridos correspondem ao tipo de dado esperado.

CC-Tools fornece alguns tipos de dados primitivos. Aqui estão eles:

- **string**: representa uma sequência de caracteres.
- **number**: representa um número de ponto flutuante de 64 bits.
- **integer**: representa números inteiros. O tamanho depende da arquitetura subjacente e pode variar entre 32 bits e 64 bits.
- **boolean**: representa dois valores possíveis: verdadeiro e falso.
- **datetime**: representa uma data no formato RFC3339, por exemplo, "2019-05-06T22:12:41Z".
- **@object**: representa uma string em formato JSON. É usado para definir a estrutura e propriedades de um objeto JSON. Consiste em pares chave-valor.

Também permite que os desenvolvedores injetem tipos de dados primitivos personalizados. Para usar a biblioteca GoLedger CC-Tools, a definição de tipos de dados é feita na pasta chaincode/datatypes.

## Exemplos de Tipos de Dados:

O repositório cc-tools-demo possui exemplos de tipos de dados, como `bookType` e `cpf`.

### Definição do Tipo de Dados `bookType`:

- **Tipo Enumerado**: representa um tipo enumerado.
- **Valores Aceitos**: 0, 1 e 2, que representam os tipos de livro Hardcover, Paperback e Ebook, respectivamente.
- **Função Parse**: recebe o valor de entrada, converte-o para float64 e verifica se o número é válido na função CheckType.

### Definição do Tipo de Dados `cpf`:

- **Formatos Aceitos**: "string".
- **Função Parse**: valida se os números do CPF fornecidos seguem a estrutura correta, incluindo os dígitos necessários e o algoritmo de verificação.

## Lista de Tipos de Dados:

O registro de tipos de dados personalizados GoLedger CC-Tools deve ser definido no arquivo chaincode/datatypes/datatypes.go.

```go
var CustomDataTypes = map[string]assets.DataType{
    "cpf":      cpf,
    "bookType": bookType,
}
```




# Eventos

O Hyperledger Fabric permite que aplicativos clientes (como a API rest-server) recebam eventos de blocos quando os blocos são confirmados no ledger do nó.

CC-Tools possui uma funcionalidade de evento integrada que permite um registro rápido do evento, que será automaticamente ouvido e tratado pelo padrão CCAPI.

Os eventos no CC-Tools podem ser de três tipos: log, registrando as informações enviadas na carga do evento nos logs do CCAPI; transação, que invoca transações de outro chaincode quando o evento é recebido; e personalizado, que executa uma função personalizada previamente definida no evento.

## Exemplo de Evento Pré-definido:

No repositório cc-tools-demo, há 1 evento pré-definido registrado:

- **Create Library Log**

### Definição do Evento:

A construção e definição de um evento são feitas criando um arquivo na pasta chaincode/eventtypes.

Um evento tem os seguintes campos:

- **Tag**: campo de string usado para definir o nome do evento referenciado internamente pelo código e pelos clientes rest-server. Sem espaços ou caracteres especiais permitidos.
- **Label**: campo de string para definir o rótulo a ser usado por aplicativos externos e pelos clientes rest-server. Texto livre.
- **Description**: campo de string de descrição do evento a ser usado por aplicativos externos. Texto livre.
- **BaseLog**: campo de string com uma mensagem a ser registrada no CCAPI em cada chamada de evento. Texto livre.
- **EventType**: define o tipo de evento. Opções possíveis sendo events.EventLog, events.EventTransaction e events.EventCustom.
- **Transaction**: campo de string com a tag da transação a ser chamada por eventos do tipo transação.
- **Channel**: campo de string com o nome do canal onde a transação a ser chamada por um evento do tipo transação está localizada. Se vazio, padrão para o mesmo canal do chamador.
- **Chaincode**: campo de string com o nome do chaincode onde a transação a ser chamada por um evento do tipo transação está localizada. Se vazio, padrão para o mesmo chaincode do chamador.
- **CustomFunction**: uma função personalizada a ser executada por eventos do tipo personalizado. Recebe um stub do tipo *sw.StubWrapper e uma carga do tipo []byte, enviada junto com o evento, e retorna um erro.
- **ReadOnly**: um booleano que define se as funções personalizadas para eventos do tipo personalizado podem alterar ou não o estado mundial. Se verdadeiro, nenhum ativo poderá ser registrado no ledger pela função personalizada.

### Exemplo de Evento:

O repositório cc-tools-demo possui o seguinte exemplo:

- **CreateLibraryLog**:
  - É do tipo log, registrando suas mensagens nos logs do CCAPI.
  - Cada evento desse tipo recebido terá a mensagem base "Nova biblioteca criada" registrada nos logs do CCAPI.
  - Apenas org2 e org serão registradas para esperar eventos desse tipo.

## Lista de Eventos:

O registro de eventos GoLedger CC-Tools deve ser definido no arquivo chaincode/eventTypeList.go.

```go
var eventTypeList = []events.Event{
    eventtypes.CreateLibraryLog,
}
```

## Chamando os Eventos:
Os eventos CC-Tools podem ser chamados das transações usando o objeto stub fornecido, e um []byte pode ser enviado junto a ele.

* Para eventos de log, a carga é uma string serializada:

```go
payload, ok := json.Marshal("a carga do evento")
```

* Para eventos de transação, a carga é um objeto serializado contendo os parâmetros para a transação chamada:

```go
payload, ok := json.Marshal(map[string]interface{}{
    "library": libraryAsset,
})
```

* Para eventos personalizados, a carga pode ser qualquer coisa no formato []byte, para ser manipulada conforme necessário pela função personalizada.

É possível chamar eventos usando o objeto de evento:

```go
eventObj.CallEvent(stub, payload)
```

Ou usando a tag do evento para ativá-lo:

```go
events.CallEvent(stub, "createLibraryLog", payload)
```










# Transações

- As transações do GoLedger CC-Tools representam os métodos GoLang que podem modificar os ativos dentro do ledger Blockchain (Canal Hyperledger Fabric).

- Ativos do Hyperledger Fabric só podem ser criados ou modificados executando transações de chaincode dentro de nós que endossam.

- A biblioteca GoLedger CC-Tools possui uma variedade de transações predefinidas:
  - **CreateAsset:** criação de um novo ativo
  - **UpdateAsset:** atualiza um ativo existente
  - **DeleteAsset:** remove um ativo do estado atual do ledger
  - **ReadAsset:** lê o ativo em seu último status no ledger
  - **ReadAssetHistory:** histórico do status do ativo no ledger
  - **Search:** listagem de ativos

- A biblioteca GoLedger CC-Tools permite a criação de transações personalizadas.

- O repositório cc-tools-demo fornece 3 exemplos de transações:
  - **CreateNewLibrary:** cria um novo ativo do tipo Library
  - **GetBooksByAuthor:** retorna ativos Book dado um nome de autor
  - **GetNumberOfBooksFromLibrary:** retorna o número de ativos Book dentro do ativo Library
  - **UpdateBookTenant:** atualiza o campo currentTenant dentro do ativo Book

**Definição de Transação:**

- A construção e definição de uma transação personalizada são feitas criando um arquivo dentro da pasta chaincode/txdefs.

- Uma transação tem os seguintes campos:
  - **Tag:** campo de string para definir o nome da transação referenciado internamente pelo código e pelos endpoints Rest API. Não pode ter espaços ou caracteres especiais. Se tiver o mesmo nome de uma transação predefinida (createAsset, updateAsset, deleteAsset, readAsset, readAssetHistory ou search), essa transação será substituída pela encontrada no diretório txdefs.
  - **Label:** campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.
  - **Description:** campo de string de descrição da transação a ser usado por aplicativos externos. Texto livre.
  - **Method:** tipo de método Rest API. Pode ser POST, GET, PUT ou DELETE.
  - **Callers:** usado para definir quais organizações podem chamar essa transação.
  - **Args:** argumentos. Possui seus próprios campos.
  - **Routine:** código-fonte da transação.
  - **ReadOnly:** campo booleano para indicar que a transação não altera o estado mundial.
  - **MetaTx:** campo booleano para indicar que a transação não codifica uma regra específica do negócio, mas um processo interno do chaincode, por exemplo, listar tipos de ativos disponíveis.

**Definição de Argumento de Transação:**

- Um argumento tem os seguintes campos:
  - **Tag:** campo de string para definir o nome do argumento referenciado internamente pelo código e pelos endpoints Rest API. Não pode ter espaços ou caracteres especiais.
  - **Label:** campo de string para definir o rótulo a ser usado por aplicativos externos. Texto livre.
  - **Description:** campo de string de descrição do argumento a ser usado por aplicativos externos. Texto livre.
  - **Required:** identifica se o argumento é obrigatório. Campo booleano.
  - **DataType:** tipo de propriedade. CC-Tools possui os seguintes tipos padrão: string, number, datetime, boolean e @object.
  - **Private:** campo booleano para indicar que o argumento será usado para dados privados.

**StubWrapper:**

- O StubWrapper tem como principal objetivo fornecer funcionalidades adicionais e simplificar o desenvolvimento de chaincodes.

- Mantém um WriteSet para garantir que as modificações feitas durante a execução de um chaincode sejam refletidas adequadamente ao consultar o estado do ledger.

- Mesmo que essas alterações ainda não tenham sido confirmadas no ledger, o StubWrapper registra as modificações pendentes no WriteSet. Isso permite que consultas subsequentes utilizem o WriteSet para retornar os dados atualizados, garantindo consistência e precisão das informações durante a execução do chaincode.

**Exemplos de Transação:**

- O repositório cc-tools-demo possui as seguintes configurações de transação personalizada:
  - **CreateNewLibrary:**
    - Somente org3 pode chamar esse método
    - Método POST para Rest API
    - O argumento "name" de tipo string é obrigatório
    - Usa a função NewAsset para preparar um novo ativo e a função PutNew para criá-lo no canal
    - Um evento de log para o tipo createLibraryLog é chamado com o nome da biblioteca criada

  - **GetBooksByAuthor:**
    - Somente org1 e org2 podem chamar esse método
    - Método GET para Rest API
    - O argumento "authorName" de tipo string é obrigatório
    - O argumento "limit" de tipo number é opcional
    - Usa a função Search para consultar ativos Book do ledger, filtrando por autor

  - **UpdateBookTenant:**
    - Qualquer organização que comece com "org" seguido por um número (org1, org2, org3, org4, etc) pode chamar esse método
    - Método PUT para Rest API
    - O argumento "book" de tipo Book é obrigatório
    - O argumento "person" de tipo Person é obrigatório
    - Usa as funções Get e Update para ler e atualizar informações dos ativos Book e Person no ledger

  - **GetNumberOfBooksFromLibrary:**
    - Somente org2 pode chamar esse método
    - Método GET para Rest API
    - O argumento "library" de tipo Library é obrigatório
    - Usa a função Get para ler as informações do ativo Library no ledger

**Lista de Transações:**

- O registro das transações que serão usadas pela biblioteca GoLedger CC-Tools também deve ser feito dentro do arquivo chaincode/txList.go.

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

## META-INF:

Índices permitem que um banco de dados seja consultado sem ter que examinar cada linha com cada consulta, tornando-os mais rápidos e eficientes.

Os arquivos de índice JSON devem estar localizados no caminho META-INF/statedb/couchdb/indexes, que está localizado dentro do diretório onde o chaincode reside.