# Desenvolvendo DApps em plataforma Hyperledger Fabric

## Configuração inicial

* Git
* Curl
* Docker
* Docker images do Fabric (Versão desejada)
* GoLang
* NodeJs

## Verificando o docker

```shell
sudo systemctl enable docker
```

#### Limpando o docker
```shell
docker stop $(docker ps –a –q)
docker rm $(docker ps –a –q)
docker rmi –f $(docker images)
docker volume prune
docker system prune
```

## Fabric-samples e Docker images v2.2

#### Instalando tudo
```shell
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.9 1.5.5
```

#### Repositório fabric-samples
https://github.com/hyperledger/fabric-samples.git

#### Imagens oficiais
https://hub.docker.com/r/hyperledger/fabric-peer
https://hub.docker.com/r/hyperledger/fabric-orderer
https://hub.docker.com/r/hyperledger/fabric-orderer
https://hub.docker.com/r/hyperledger/fabric-tools

## Test-network v2.2
#### Dentro do diretório fabric-samples/test-network

#### Utilizar o scritpt network network.sh

#### Se não for a 1ª vez limpar o ambiente

```shell
./network.sh down
```

#### Subir os containers com o comando

```shell
./network.sh up
```

## Ferramenta cryptogen
Ferramenta de geração de MSPs simulados para ambiente de desenvolvimento

Utiliza arquivos de configuração (yaml)

Ex:
```shell
PeerOrgs:
- Name: Org1
Domain: org1.example.com
EnableNodeOUs: true
Template:
Count: 1
SANS:
- localhost
Users:
Count: 1
```

## Utilizando Fabric-CA
Se não for a 1ª vez limpar o ambiente

```shell
./network.sh up –ca

tree organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/
organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/
└── msp
├── cacerts
│ └── localhost-7054-ca-org1.pem
├── config.yaml
├── IssuerPublicKey
├── IssuerRevocationPublicKey
├── keystore
│ └── 868e452a1fa47d12bfefbe306f7f2e9ebf383599c339914541b688b6ad5a3b6a_sk
├── signcerts
│ └── cert.pem
└── user
```

## Certificados gerados
Os certificados gerados para uso na rede Hyperledger Fabric são identificados:

```
Common Name (CN): nome e domínio do certificado
Organization (O): organização do certificado
Organization Unit (OU): peer, orderer, client, admin
```

```shell
openssl x509 -in localhost-7054-ca-org1.pem -text –noout

Certificate:ls

Data:
Version: 3 (0x2)
Serial Number:
5f:d1:6a:5f:a4:5f:0a:0f:a9:cd:1b:1c:d8:90:ee:38:9d:d3:b5:37
Signature Algorithm: ecdsa-with-SHA256
Issuer: C = US, ST = North Carolina, L = Durham, O = org1.example.com, CN = ca.org1.example.com
Validity
Not Before: Nov 7 17:15:00 2022 GMT
Not After : Nov 3 17:15:00 2037 GMT
Subject: C = US, ST = North Carolina, L = Durham, O = org1.example.com, CN = ca.org1.example.com
Subject Public Key Info:
Public Key Algorithm: id-ecPublicKey
Public-Key: (256 bit)
pub:
04:c9:3a:2f:1f:65:2b:4f:b8:cb:8d:d9:52:63:36:…
```

## Arquitetura proposta
Rede – blockchain business network - a ser criada

3 orgs
• org1
• org2
• orderer

1 channel – mychannel
1 chaincode - fabcar

## Ferramenta configtxgen
Gera artefatos de configuração da rede e inspeção da rede

Os artefatos de configuração são propostos ao orderer para poder realizar ações tais como:

* Criar genesis block
* Criar novo channel
* Adicionar peer
* Adicionar nova organização

Configura o arquivo em configtx.yaml

Deve estar em FABRIC_CFG_PATH ou utilizar o parâmetro –configPath

No diretorio test-network:

```shell
export FABRIC_CFG_PATH=$PWD/configtx
```

## Arquivo configtx.yaml
O arquivo configtx.yaml possui diversas sessões para realizar a
criação de artefatos de configuração.

* Organizations
* Capabilities (Channel, Orderer, Application)
* Application
* Orderer
* Channel
* Profiles

## Criando o bloco gênesis

O genesis block possui as regras iniciais de uma Blockchain business
network. No exemplo ele é criado no profile TwoOrgsOrdererGenesis:

```shell
TwoOrgsOrdererGenesis:
<<: *ChannelDefaults
Orderer:
<<: *OrdererDefaults
Organizations:
- *OrdererOrg
Capabilities:
<<: *OrdererCapabilities
Consortiums:
SampleConsortium:
Organizations:
- *Org1
- *Org2
```

Comando para gerar o artefato de configuração do genesis block

```shell
configtxgen -profile TwoOrgsOrdererGenesis -channelID system-channel -outputBlock ./system-genesis-
block/genesis.block
```

Inspecionando o bloco gênesis

```shell
configtxgen -inspectBlock ./system-genesis-block/genesis.block
```

## Iniciando a Business Network v2.2

Inspecionando o bloco gênesis

```shell
configtxgen -inspectBlock ./system-genesis-block/genesis.block
```

O orderer vai utilizar o genesis block para iniciar a rede.

Configuração do docker:

```shell
orderer.example.com:
environment:
- ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
volumes:
- ../system-genesis-block/genesis.block:/var/hyperledger/orderer/orderer.genesis.block
```

Configuração do docker:

```shell
2022-11-07 20:37:39.800 UTC [orderer.commmon.multichannel] Initialize -> INFO 013 Starting system
channel 'system-channel' with genesis block hash
ca7bb35a2fd70f7c4c86b8f0b778d45758e249f93ab8d47b8621dbe2a009e174 and orderer type etcdraft
```

## Criando um channel na rede
Utilizando o comando configtxgen para o profile TwoOrgsChannel para gerar a transação de criação de
channel.

TwoOrgsChannel:
Consortium: SampleConsortium
<<: *ChannelDefaults
Application:
<<: *ApplicationDefaults
Organizations:
- *Org1
- *Org2
Capabilities:
<<: *ApplicationCapabilities
Criação da transação mychannel.tx
configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/mychannel.tx -channelID mychannel
Criando o channel na rede
peer channel create -o localhost:7050 -c mychannel --ordererTLSHostnameOverride orderer.example.com -f ./channel-
artifacts/mychannel.tx --outputBlock ./channel-artifacts/mychannel.block --tls --cafile …
Bloco de configuração mychannel.block criado.










Entrada de um peer no channel
Entrada de um peer de uma org no channel através da aplicação do bloco de configuração.
setGlobals $ORG
peer channel join -b ./channel-artifacts/mychannel.block
O peer dentro do channel representa a org dentro do channnel
2022-11-07 22:52:52.152 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join
channelUtilizando o container cli
Container cli é para da imagem hyperledger/fabric-tools
Possibilita a operação entre peers sem a mudança de containers
Variáveis de ambiente utilizadas pelo comando peer
CORE_PEER_TLS_ENABLED
CORE_PEER_LOCALMSPID
CORE_PEER_TLS_ROOTCERT_FILE
CORE_PEER_MSPCONFIGPATH
CORE_PEER_ADDRESS
Exemplo
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051Atualizando blocos de configuração
Algumas operações são realizadas através da atualização dos blocos de configuração
peer channel fetch config | newest | oldest | blockNumber outputFile -o orderer.example.com:7050 –c mychannel …
O bloco de configuração é modificado e atualizado no channel
peer channel update -o orderer.example.com:7050 -c mychannel -f txFileConfiguração do Anchor Peer
Os anchor peers representam os peers que vão receber os novos blocos do orderer após um commit bem
sucedido.
A definição de anchor peer acontece com a atualização do bloco de configuração executando o script
setAnchorPeer.sh dentro do container cli.
Atualização de blocos de configuração com o uso da ferramenta configtxlator para converter de json para
protobuf.Chaincode lifecycle
Ciclo de vida dos chaincodes instanciados no channel
O channel possui uma política de endorso (endorsing policy) para a instanciação e atualização do chaincode no channel.
Cada chaincode possui uma endorsing policy para as suas transações.
Cada endorsing policy representa o conjunto mínimo de assinaturas para para completer a transação com sucesso. Ex:
Maioria simples
Todos
Qualquer um
Regra lógica, ex: org1 and [org2 or org3]
A instalação e instanciação do chaincode no channel requer as seguintes etapas:
Package: empacotamento do chaincode em um arquivo tar.
Install: instalação do chaincode nos endorsing peers.
Approve: aprovação do chaincode pelas orgs para validar o lifecycle endorsing policy
Commit: instanciar o chaincode no channel
Init (opcional): realizar uma transação inicial.Instalando um chaincode no channel (FabCar-go)
As seguintes etapas serão realizadas após o comando:
./network.sh deployCC -ccn fabcar -ccp ../chaincode/fabcar/go -ccl go
Preparando o chaincode
go mod vendor
Criando um pacote para o peer
peer lifecycle chaincode package fabcar.tar.gz --path ../fabcar/go --lang golang --label fabcar_1.0
Instalando o chaincode no peer
setGlobals $ORG
peer lifecycle chaincode install fabcar.tar.gz
Verificando os chaincodes instalados
peer lifecycle chaincode queryinstalled
Installed chaincodes on peer:
Package ID: fabcar_1.0:6c5c429e8a6734ff978f54d12a1d9e5e5296663e20dcacff6dd2276bb0e8b12b, Label: fabcar_1.0
Um chaincode instalado precisa ser instanciado para transformar o peer em endorsing peerInstanciando um chaincode
Aprovando o chaincode
setGloblals $ORG
peer lifecycle chaincode approveformyorg -o localhost:7050 … --channelID mychannel --name fabcar --version 1.0
--package-id fabcar_1.0:6c5c429e8a6734ff978f54d12a1d9e5e5296663e20dcacff6dd2276bb0e8b12b --
sequence 1
Aprovar em todas as orgs e verificar com a função:
peer lifecycle chaincode checkcommitreadiness --channelID mychannel --name fabcar --version 1.0 --sequence 1 --
output json
Comitar o chaincode
peer lifecycle chaincode commit -o localhost:7050 … --channelID mychannel --peerAddresses localhost:7051 --
peerAddresses localhost:9051 --name fabcar --version 1.0 --sequence 1Inicializando um chaincode
Configuração das variáveis de ambiente para inicializar o chaincode
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
Chamando a primeira transação
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile
"${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n fabcar --
peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --
peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c
'{"function":"InitLedger","Args":[]}’Testando o chaincode
Verificando o estado do peer
peer chaincode query -C mychannel -n fabcar -c '{"Args":["QueryAllCars",""]}’
[{"Key":"CAR0","Record":{"make":"Toyota","model":"Prius","colour":"blue","owner":"Tomoko"}},{"Key":"CAR1","Record":{"make":"Ford","model":"Mustang","c
olour":"red","owner":"Brad"}},{"Key":"CAR2","Record":{"make":"Hyundai","model":"Tucson","colour":"green","owner":"JinSoo"}},{"Key":"CAR3","Record":{"mak
e":"Volkswagen","model":"Passat","colour":"yellow","owner":"Max"}},{"Key":"CAR4","Record":{"make":"Tesla","model":"S","colour":"black","owner":"Adriana"}}
,{"Key":"CAR5","Record":{"make":"Peugeot","model":"205","colour":"purple","owner":"Michel"}},{"Key":"CAR6","Record":{"make":"Chery","model":"S22L","col
our":"white","owner":"Aarav"}},{"Key":"CAR7","Record":{"make":"Fiat","model":"Punto","colour":"violet","owner":"Pari"}},{"Key":"CAR8","Record":{"make":"Tat
a","model":"Nano","colour":"indigo","owner":"Valeria"}},{"Key":"CAR9","Record":{"make":"Holden","model":"Barina","colour":"brown","owner":"Shotaro"}}]
O operação query retorna o transaction proposal response.Chaincode FabCar
Contrato inteligente padrão fabric-samples
Gerenciamento de um conjunto de carros.
Ativo principal compartilhado na rede.
// Car describes basic details of what makes up a car
type Car struct {
Make string `json:"make"`
Model string `json:"model"`
Colour string `json:"colour"`
Owner string `json:"owner"`
}Funções principais
InitLedger – inicia o channel com um conjunto de 10 carros
CreateCar – Cria um novo carro
QueryCar – Leitura dos dados de um carro
QueryAllCars – Retorna as informações de todos os carros do
channel/chaincode.
ChangeCarOwner – Muda o proprietário de um carroAnalisando a função CreateCar
func (s *SmartContract) CreateCar(ctx contractapi.TransactionContextInterface, carNumber string, make
string, model string, colour string, owner string) error {
car := Car{
Make: make,
Model: model,
Colour: colour,
Owner: owner,
}
carAsBytes, _ := json.Marshal(car)
}
return ctx.GetStub().PutState(carNumber, carAsBytes)A função PutState
A função PutState realiza uma alteração no transaction proposal response.
func (s *ChaincodeStub) PutState(key string, value []byte) error
key: chave única referência no channel. Chave pode ser composta (usando a
função CreateCompositeKey)
value: dados a serem gravados no channelORGANIZAÇÃO A
ORGANIZAÇÃO B
Endorsing
Peer
Endorsing
Peer
Client
SDK
Anchor
Peer
ORGANIZAÇÃO C (ORDERER)
Anchor
PeerConfiguração de variáveis
Configuração das variáveis de ambiente para inicializar o chaincode
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export
CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051Chamando a função CreateCar
peer chaincode invoke \
-o localhost:7050 \
--ordererTLSHostnameOverride orderer.example.com \
--tls --cafile
"${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.
com-cert.pem" \
-C mychannel -n fabcar \
--peerAddresses localhost:7051 \
--tlsRootCertFiles
"${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" \
--peerAddresses localhost:9051 --tlsRootCertFiles
"${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" \
-c '{"function":"CreateCar","Args":["CAR10", "Gurgel", "Mini", "branco", "Marcos"]}'
2022-11-09 18:40:39.119 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 001 Chaincode invoke successful.
result: status:200
peer chaincode query -C mychannel -n fabcar -c '{"Args":["QueryCar","CAR10"]}’
{"make":"Gurgel","model":"Mini","colour":"branco","owner":"Marcos"}Chamando a função QueryCar
// QueryCar returns the car stored in the world state with given id
func (s *SmartContract) QueryCar(ctx contractapi.TransactionContextInterface, carNumber string) (*Car, error) {
carAsBytes, err := ctx.GetStub().GetState(carNumber)
if err != nil {
return nil, fmt.Errorf("Failed to read from world state. %s", err.Error())
}
if carAsBytes == nil {
return nil, fmt.Errorf("%s does not exist", carNumber)
}
car := new(Car)
_ = json.Unmarshal(carAsBytes, car)
}
}
return car, nilA função GetState
A função GetState retorna o world state do ativo presente no peer.
func (s *ChaincodeStub) GetState(key string) ([]byte, error)
key: chave única referência no channel.Lendo a informação de um carro
Verificando o estado do peer
peer chaincode query -C mychannel -n fabcar -c '{"Args":["QueryCar", "CAR2"]}'
O operação query retorna o transaction proposal response.Atualizando um chaincode no channel
Para atualizar um chaincode deve-se executar os passos similares a instanciação do chaincode.
./network.sh deployCC -ccn fabcar -ccp ../chaincode/fabcar/go/ -ccl go -ccv 2.0 -ccs 2
Verificar a sintaxe GoLang do chaincode usando o comando
go vet
Os chaincodes atualizados devem:
•
Possuir versão inédita
•
Sequenciado em relação ao chaincode anterior (1, 2, 3...)
Fluxo igual ao do instanciar.
Package: empacotamento do chaincode em um arquivo tar.
Install: instalação do chaincode nos endorsing peers.
Approve: aprovação do chaincode pelas orgs para validar o lifecycle endorsing policy
Commit: instanciar o chaincode no channel
Init (opcional): realizar uma transação inicial.Biblioteca CC-ToolsDesenvolvendo chaincodes
Mapeamento de dados
Gerenciamento de direito de escrita e leitura entre organizações (MSP)
Cadastramento de transaçõesBiblioteca CC-Tools
A biblioteca CC-Tools é desenvolvida em GoLang e possui diversas
características que facilitam a jornada de aprendizado, desenvolvimento e
deployment em produção de um chaincode para Hyperledger Fabric.Biblioteca CC-Tools - características
Open source
Padronização de assets, keys e referências de assets (asset dentro de asset)
Tipos de dados padrão e customizáveis
Gerenciamento de organizações (MSP)
Cadastramento de Transações
Gerenciamento de permissões de escrita por propriedade de assets por MSP
Gerenciamento de private data.Biblioteca CC-Tools - características
Transações embedded:
•
•
•
•
•
•
Create
Read
Update
Delete
Search (paginated)
ReadHistory
Transações customizáveis
Integração das transações com a Rest API (GET, PUT, POST, DELETE)
Permissionamento de chamadas de transações por MSPExemplo de Asset
// Description of a book
var Book = assets.AssetType{
Tag: "book",
Label: "Book",
Description: "Book",
Props: []assets.AssetProp{
{
IsKey: true, // Primary Key
Tag: "title",
Label: "Book Title",
DataType: "string",
Writers: []string{`org2MSP`}, // This means only org2 can create the asset (others can edit)
},
{
Tag: "currentTenant",
Label: "Current Tenant",
DataType: "->person", /// Reference to another asset
},
{
Tag: "genres",
Label: "Genres",
DataType: "[]string", // String list
},
{
Tag: "published",
Label: "Publishment Date",
DataType: "datetime", // Date property
},
},
}Exemplo de Transação
// Updates the tenant of a Book
// POST Method
var UpdateBookTenant = tx.Transaction{
Tag: "updateBookTenant",
Label: "Update Book Tenant",
Description: "Change the tenant of a book",
Method: "PUT",
Callers: []string{`$org\dMSP`}, // Any orgs can call this transaction
}
Args: []tx.Argument{
{
Tag: "book",
Label: "Book",
Description: "Book",
DataType: "->book",
Required: true,
},
{
Tag: "tenant",
Label: "tenant",
Description: "New tenant of the book",
DataType: "->person",
},
},
Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
...........................................................
...........................................................
...........................................................
return bookJSON, nil
},Ferramentas
Testes unitários
Web service (Rest API )
integrado
Aplicação Web
func TestCreateNewLibrary(t *testing.T) {
stub := mock.NewMockStub("org3MSP", new(cc.CCDemo))
expectedResponse := map[string]interface{}{
"@key": "library:3cab201f-9e2b-579d-b7b2-72297ed17f49",
"@lastTouchBy": "org3MSP",
"@lastTx": "createNewLibrary",
"@assetType": "library",
"name": "Maria's Library",
}Repositório cc-tools-demo
Baixar o repositório
git clone https://github.com/goledgerdev/cc-tools-demo.git
Vendorar o chaincode no diretório chaincode
go mod vendor
Vendorar o web service no diretório rest-server
./npmInstall.shIniciando o ambiente
Executar o script para HL Fabric 1.4 com 3 orgs
./startDev.sh
Executar o script para HL Fabric 2.2 com 3 orgs
./startDev2.sh
Executar o script para HL Fabric 1.4 com 1 org
./startDev.sh –n 1
Para subir a aplicação Web
./run-web-cc.shcc-tools-demo – diretórios e scripts
-- chaincode
|-- assettypes
|-- datatypes
|-- txdefs
-- rest-server
npmInstall.sh
startDev.sh
startDev2.sh
-- fabric
startDev.sh
-- fabric2
startDev.sh
renameProject.sh
startDev.sh
startDev2.sh
# chaincode source code
# definições dos assets
# tipos de dados customizados
# transações customizadas
# web service (REST API)
# vendorar web service
# reiniciar apis (1.4)
# reiniciar apis (2.2)
# artefatos HL Fabric 1.4
# iniciar rede HL 1.4
# artefatos HL Fabric 2.2
# iniciar rede HL 2.2
# renomear nome do chaincode
# iniciar rede HL Fabric 1.4
# iniciar rede HL Fabric 2.2Definição de um asset
Definição dos assets dentro da pasta assettypes
var Person = assets.AssetType{
Tag:
"person",
// Identificação do ativo (JSON)
Label:
"Person",
// Texto identificador do ativo
Description: "Personal data",
// Texto idenficador detalhado do ativo
Props: []assets.AssetProp{
{
IsKey: true,
// Propriedade faz parte da chave primária/composta
Tag: "id",
// Identificação da propriedade (JSON/interface{})
Label: "CPF (Brazilian ID)", // Texto identificador da propriedade
DataType: “cpf",
// Tipo da propriedade (embedded ou custom)
Writers: []string{`org1MSP`}, // Organização que pode criar/alterar a propriedade
},
{
// Mandatory property
Required: true,
Tag: "name",
Label: "Name of the person",
DataType: "string",
Validate: func(name interface{}) error {
// Função de validação (criação ou alteração da propriedade)
nameStr := name.(string)
if nameStr == "" {
return fmt.Errorf("name must be non-empty")
}
return nil
},
},Lista de assets do chaincode
Os assets devem estar explicitamente definidos no arquivo assetTypeList.go na pasta chaincode
var assetTypeList = []assets.AssetType{
assettypes.Person,
assettypes.Book,
assettypes.Library,
assettypes.Secret,
}Datatypes embedded
CC-Tools possui os seguintes datatypes padrão
string
number
datetime
boolean# texto livre
# numero flutuante
# data e hora
# true / false
[]string
[]number
[]datetime
[]boolean# array de texto livre
# array numero flutuante
# array data e hora
# array de true / false
->[asset]
[]->[asset]# referencia a outro asset
# array de referencias a assetsDefinição de um Datatype custom
Definição dos custom datatypes dentro da pasta datatypes
var cpf = assets.DataType{
Parse: func(data interface{}) (string, interface{}, errors.ICCError) {
cpf, ok := data.(string)
if !ok {
return "", nil, errors.NewCCError("property must be a string", 400)
}
cpf = strings.ReplaceAll(cpf, ".", "")
cpf = strings.ReplaceAll(cpf, "-", "")
if len(cpf) != 11 {
return "", nil, errors.NewCCError("CPF must have 11 digits", 400)
}
…
return cpf, cpf, nil
},
}Lista de custom datatypes do chaincode
Os custom datatypes devem estar explicitamente definidos no arquivo datatypes.go na pasta
chaincode/datatypes
var CustomDataTypes = map[string]assets.DataType{
"cpf": cpf,
"bookType": bookType,
}Transações embedded
CC-Tools possui os seguintes transações embutidas automaticamente para uso.
Tx.ReadAsset
tx.CreateAsset
tx.UpdateAsset
tx.DeleteAsset
tx.Search
tx.ReadAssetHistory
// Ler ativo no world state
// Criar no ativo no channel
// Atualizar ativo no channel
// Deletar ativo no channel
// Procurar ativos com rich query no world state
// Histórico de um ativo no ledgerDefinição de uma transação custom
var CreateNewLibrary = tx.Transaction{
Tag:
"createNewLibrary",
// Identificação da transação (API endpoint)
Label:
"Create New Library",
// Texto identificador da transação
Description: "Create a New Library",
// Texto identificador detalhado da transação
Method: "POST",
// Método do web service
Callers: []string{"$org3MSP", "$orgMSP"},
// Organizações (MSP) que podem chamar essa transação
Args: []tx.Argument{
{
Tag:
"name",
// Idenficação do argumento
Label:
"Name",
// Texto identificador do argumento
Description: "Name of the library",
// Texto identificador detalhado do argumento
DataType: "string",
// Tipo de dados do argumento
Required: true,
// Argumento obrigatório (default=false)
},
},
Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
name, _ := req["name"].(string)
// Argumento do chamada
libraryMap := make(map[string]interface{})
libraryMap["@assetType"] = "library"
libraryMap["name"] = name
},
libraryAsset, err := assets.NewAsset(libraryMap) // Preparação do ativo para gravação
if err != nil {…}
_, err = libraryAsset.PutNew(stub)
// Criação do transaction proposal response
if err != nil {…}
libraryJSON, nerr := json.Marshal(libraryAsset)
if nerr != nil {…}
return libraryJSON, nil
// Retorna resultado para a APILista de transações do chaincode
As transações, inclusive as embedded devem estar explicitamente definidas no arquivo txList.go na
pasta chaincode
var txList = []tx.Transaction{
tx.CreateAsset,
tx.UpdateAsset,
tx.DeleteAsset,
txdefs.CreateNewLibrary,
txdefs.GetNumberOfBooksFromLibrary,
txdefs.UpdateBookTenant,
txdefs.GetBooksByAuthor,
}Utilizando o Web Service padrão (API)
Web service padronizado para realizar chamadas na rede Blockchain.
Modelo padrão:
Invoke (realiza alteração no channel)
Métodos: POST, PUT, DELETE
api/invoke/:tx
Query (não altera o channel):
Métodos: GET, POST
api/query/:txEndpoints de apoio
query/getHeader
Método GET
Retorna versão do CC-Tools e dados do header para visualização na interface
query/getSchema
Métodos GET e POST
Retorna informações dos assets ou de um asset específico
query/getTx
Métodos GET e POST
Retorna informações das transações ou de uma transação específicaExemplo de getSchemaEndpoints para as tx embedded
invoke/createAsset
Método POST
{"asset": [ {JSON do ativo}, {JSON do ativo}, ... ] }
invoke/readAsset
Método POST
{“key": {“@assetType”: Tipo do ativo, Dados da chave } }
invoke/updateAsset
Método PUT
{“update": {“@assetType”: Tipo do ativo, Dados da chave, Propriedades a serem alteradas } }
invoke/deleteAsset
Método DELETE
{“key": {“@assetType”: Tipo do ativo, Dados da chave } }Identificação de chaves dentro do asset
Propriedade da chave
@key
Asset deve ter a identificação do tipo do asset
@assetType
Para algumas operações a chave é calculada automaticamente.
Exemplo:
{ "key": { "@assetType": "person", "id": "318.207.920-48" } }
{ "key": { "@assetType": "person", "@key":"person:47061146-c642-51a1-844a-bf0b17cb5e19“ } }Utilizando a ferramenta Web App (GoInitus)
Dentro do diretório raiz
./run-cc-web.sh
Acessar o browser http://localhost:8080
Configurar a interface para acessar a api correta (icone de ferramenta do header)Usando o GoInitus
Organização (MSP) do web service
Ativos que podem ser criados pela organização
Ativos em private data (PVC)
Ativos que não podem ser criados (talvez possam ser atualizados ou
deletados)
Transações que podem ser chamadas pela organização
Transações que não podem ser chamadas pela organizaçãoTrabalhando com ativos
Chamada para operações de
CreateAsset
ReadAssetHistory
ReadAsset
UpdateAsset
DeleteAssetDocumentação CC-Tools
https://goledger-cc-tools.readthedocs.io/en/latest/Documentação Biblioteca GoLang
https://pkg.go.dev/github.com/goledgerdev/cc-tools@v0.7.5Pacotes
O pacote asset é responsável pelas funções relativas ao gerenciamento de
ativos no channel.
O pacote transactions é responsável pelas funções relativas as transações.Pacotes
O pacote asset é responsável pelas funções relativas ao gerenciamento de ativos no channel.
Todas as funções do pacote asset validam as definições dos ativos criados no diretório assettypes.
Tipos (type) do pacote asset:
Asset : objeto map[string]interface{} de um ativo completo com as suas propriedades.
Key : objeto map[string]interface{} das informações da chave do asset.
O pacote transactions é responsável pelas funções relativas as transações.
O pacote transaction possui as transações embedded.Criando/alterando ativos no channel
As principais funções para criar/alterar um asset (variação do PutState) são:
func NewAsset(m map[string]interface{}) (a Asset, err errors.ICCError)
Prepara o objeto tipo Asset para ser utilizando em operações futuras. Esperar
func (a *Asset) PutNew(stub *sw.StubWrapper) (map[string]interface{}, errors.ICCError)
Cria novo asset. Se asset já existir, retorna erro.
func (a *Asset) Put(stub *sw.StubWrapper) (map[string]interface{}, errors.ICCError)
Cria asset. Se asset já existir, atualiza
func (a *Asset) Update(stub *sw.StubWrapper, update map[string]interface{}) (map[string]interface{}, errors.ICCError)
Atualiza asset. Se asset não existir, retorna erro
Todas as transações de alteração de ativos validam as regras registradas na definição dos assets (assettypes)Lendo ativos
As principais funções para ler um asset são (variações do GetState):
func NewAsset(m map[string]interface{}) (a Asset, err errors.ICCError)
Prepara o objeto tipo Asset para ser utilizando em operações futuras.
func (k *Key) ExistsInLedger(stub *sw.StubWrapper) (bool, errors.ICCError)
Verifica a existência de um asset.
func (a *Asset) Get(stub *sw.StubWrapper) (*Asset, errors.ICCError)
Busca um asset, caso exista, retorna em um objeto Asset
func (k *Key) GetMap(stub *sw.StubWrapper) (map[string]interface{}, errors.ICCError)
Busca um asset, caso exista, retorna em um objeto map[string]interface{}
func (k *Key) GetBytes(stub *sw.StubWrapper) ([]byte, errors.ICCError)
Busca um asset, caso exista, retorna em um objeto []byte
func (a *Asset) GetRecursive(stub *sw.StubWrapper) (map[string]interface{}, errors.ICCError)
Busca um asset, caso exista, retorna em um objeto map[string]interface{} com todos as referências resolvidas.Transação CC-Tools
Uma transação definida no CC-Tools possui as seguintes características.
Identificação para chamadas Rest API
Tag:
“nome da transação“
Método da Rest API
Method: “POST” | “GET” | “PUT” | “DELETE”
Argumentos da transação
Args: []tx.Argument{
{
Tag:
“nome do argumento",
Label:
“Descrição do argumento",
Description: “Descrição detalhada do argumento",
DataType: “tipo do argumento",
Required: true,
},…
Função para executar a transação
Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {…Lendo os argumentos recebidos na transação
Os argumentos enviados pela API podem ser lidos da seguinte forma:
variavel, _ := req[“nome do argumento"].(tipo do argumento)
Exemplo:
name, _ := req["name"].(string) // argumento name de tipo string
name, ok := req["name"].(string) // argumento name verificando se está é string
limit, _ := req["limit"].(float64) // argumento do tipo number
birthDate, _ := req[“birthdate"].(time.Time) // argumento do tipo datetime
check, _ := req[“check"].(bool) // argumento do tipo boolean
libraryKey, _ := req["library"].(assets.Key) // argumento do tipo referencia a um asset (->asset)
names, _ := req["names"].([]string) // argumento name de tipo []string
limits, _ := req["limits"].([]float64) // argumento do tipo []number
birthDate, _ := req[“birthdate"].([]time.Time) // argumento do tipo []datetime
librarys, _ := req["librarys"].([]interface{}) // argumento do tipo array de referencias a um asset ([]->asset)Retornando um resultado da transação
Caso a transação falhe, deve-se retornar um erro usando o pacote errors
Exemplos:
if !ok { errors.NewCCError("type parameter is missing or in wrong format", 400) }
if err { return nil, errors.WrapError(err, " parameter missing") }
Caso a funcão não tenha apresentado falhas, deve-se retornar um []byte.
Exemplo:
ret, _ := json.Marshal(asset) // Retornando um json
return ret, nil
Uma transação bem sucedida NÃO garante a atualização do channel.Datatypes customizados
Datatypes podem ter tipos específicos para os chaincodes. São definidos na pasta datatypes
Um datatype custom possui as seguintes características:
AcceptedFormats: []string{“tipos de datatypes“} // opcional
DropDownValues: map[string]interface{}{lista do dropdown} //opcional
Description: // descrição do datatype custom
Parse: função de validação do datatypeValidando um valor com datatype custom
Dentro da função Parse, caso o valor tenha alguma inconsistência deve-se retornar erro utilizando o
pacote errors
Exemplo:
return "", nil, errors.NewCCError("asset property must be an integer", 400)
Em caso de sucesso, deve retornar o valor em formato string e o valor real
Exemplo:
return fmt.Sprint(retVal), retVal, errAtualizando o chaincode
Para atualizar o chaincode em ambiente de validação deve usar o script:
upgradeCC.sh <version># para HL Fabric 1.4
upgradeCC2.sh <version> <sequence># para HL Fabric 2.2Queries no channel
As buscas são feitas dentro do peer através de operações de pesquisa.
As pesquisas são realizadas nas bases de state escolhida (LevelDb ou CouchDb)
LevelDb é a base default e roda dentro do container peer
Pesquisas em LevelDb são realizadas para as chaves (primárias ou compostas)
CouchDb é um modelo mais poderoso e roda em um container separado do peer.
Rich queries e indexação são permitidas para CouchDbTrabalhando com CouchDb
O uso do CouchDb pode ser realizado com a configuração dos endorsing peer
Váriáveis de ambiente:
CORE_LEDGER_STATE_STATEDATABASE=CouchDB
CORE_LEDGER_STATE_COUCHDBCONFIG_COUCHDBADDRESS=couch.peer0.org.example.com:5984
CORE_LEDGER_STATE_COUCHDBCONFIG_USERNAME=admin
CORE_LEDGER_STATE_COUCHDBCONFIG_PASSWORD=adminpw
Exemplo de acesso ao couchdb via interface Web (Fauxion)
http://localhost:5984/_utils/#loginUsando a interface do CouchDB
O base do state é identificada pelo nome do channel junto com o nome
do chaincode.
Queries podem ser executadas da interface através do item Run Query
with MangoTabela de Query CouchDb
Queries podem ser realizadas através de uma sintaxe. Maiores detalhes na documentação oficial do CouchDb
https://docs.couchdb.org/en/3.2.2-docs/api/database/find.html
Exemplo:
Busca documentos com status “draft”
"selector": {
"status": { "$eq": "draft" }
}
Busca asset com year maior ou igual a 2020
“selector”: {
"year": { "$gte": 1900 }
}Função Search
Função Search utlizando pesquisas realizadas no CouchDB.
Exemplo:
// Prepare couchdb query
query := map[string]interface{}{
"selector": map[string]interface{}{
"@assetType": "book",
"author": authorName,
},
}
response, err := assets.Search(stub, query, "", true)
…Utilizando funções de pesquisa
O Hyperleger Fabric possui diversas funções para realizar queries dentro do chaincode. Essas funções trabalham
com iterators dentro de loops.
Exemplos:
GetQueryResult
GetQueryResultWithPagination
GetStateByPartialCompositeKey
GetHistoryForKey
Exemplo:
resultsIterator, _ = stub.GetQueryResult(queryString)
for resultsIterator.HasNext() {
queryResponse, err := resultsIterator.Next()
…TarefaTarefa Criando um Token
•Objetivo
•Começar com 1 org (./startDev.sh –n 1)
•Criar um token para transferencia entre organizações
•Definitions do token
•Método de criação de token
•Método de transferência particionada do token.
•Função de contabilidade do token.
•Refazer a rede com 3 orgs
•Limitar método de criação para org1 e métodos e trasferência para org2
(contabilidade sem restrições)Definição do token
•Ativo Token
•Deve possuir os campos:
•id (string) -> chave
•proprietário (string) -> obrigatório
•quantidade (number) -> default 0
Método criar token
•Método POST
•Argumentos -> iguais a definição do token
•Validação:
•Quantidade deve ser maior ou igual a 0Método transferir token
•
•
•
•
•
Método PUT
Argumento destinatario (string)
NÃO VERIFICAR quantidade
Criar um novo token para o destinatário.
Reduzir a quantidade do token origem
Método de contabilidade
•Método GET
•Argumento proprietatio
•Usar a função SearchPrivate Data Collections
A biblioteca CC-Tools possui mecanismos para trabalhar com PDCs.
Assets que terão as informações gravadas em PDCs devem ter o operador
Readers.Private Data Collection
•Um channel pode conter informações acessíveis apenas a certas orgs.
•Essas informações ficam gravadas externamente ao peer dentro de bases do CouchDb
em um banco de dados transiente.
•Dentro do ledger ficam registradas apenas os hashs das informações.
•Esse conceito é chamado de Private Data Collection.
•As organizações que podem acessar a informação privada ou alterada são definidas em
uma Private Data Collection Policy.
•Uma Private Data Collection pode ter tempo de vida.Private Data Collections
•Caso o transação de um chaincode faça uso de PDC, o fluxo da transação possui
etapa extra para geração de um TRANSACTION PROPOSAL RESPONSE.
•Transação possui leitura ou escrita em um PDC então:
•Gravação da informação privada gravada em base de state separada dentro do
CouchDB.
•Replicação das informações privadas entre peers das organizações.
•PDC possui um Policy para definir as orgs que podem ter acesso a informação
privada.
•Possibilidade de utilização de Transient Data para que os argumentos da transação
não sejam gravados no bloco.
•Transaction flow de uma transação que usa PDC é mais complexo.Exemplo de um channel com PDCs
•PDC1: Distributor, Farmer and Shipper
•PDC2: Distributor and Wholesaler
•PDC3: Wholesaler, Retailer and ShipperPVC Policy (A or B; mínimo 2 peers)
ORGANIZAÇÃO A
ORGANIZAÇÃO B
ORGANIZAÇÃO C
Endorsing Peer
Método Chaincode f(x)
...
PutPrivateData(asset)
TransientData
World State
[asset]
[asset]
ORGANIZAÇÃO ORDERER
Peer
TransientData
World State
[asset]
[asset]
Peer
World State
[hash(asset)]Exemplo de um asset em PDC
var Secret = assets.AssetType{
Tag:"secret",
Label:"Secret",
Description: "Secret between Org2 and Org3",
Readers: []string{"org2MSP", "org3MSP"},
Props: []assets.AssetProp{
{
IsKey: true,
Tag:
"secretName",
Label: "Secret Name",
DataType: "string",
Writers: []string{`org2MSP"}, // This means only org2 can create the asset (org3 can edit)
},
{
Tag:
"secret",
Label: "Secret",
DataType: "string",
},
},
}Arquivo collections2.json
Cada asset PDC (Readers) deve ter um elemento correspondente dentro do arquivo collections2.json
[
{
"name": "secret",
"requiredPeerCount": 0,
"maxPeerCount": 3,
"blockToLive": 1000000,
"memberOnlyRead": true,
"policy": "OR('org2MSP.member', 'org3MSP.member')"
}
]
DOrquestradores BlockchainOrquestradores Hyperledger Fabric
Estão disponíveis diversos orquestrados para facilitar a operação em redes Hyperledger Fabric
•Orquestradores open source
•Orquestradores em nuvem
•Orquestradores licenciadosOrquestradores open-source
Disponibilizados pela Hyperledger Foundation
Hyperledger Cello
https://www.hyperledger.org/use/cello
Hyperledger Bevel
https://www.hyperledger.org/use/bevel
Fabric Operator (Hyperledger Labs)
https://labs.hyperledger.org/labs/hlf-operator.htmlOrquestradores em nuvem e licenciados
Disponíveis na nuvens públicas
Amazon Managed Blockchain
https://aws.amazon.com/pt/managed-blockchain/
IBM Blockchain Platform
https://www.ibm.com/products/blockchain-platform-hyperledger-fabric
Oracle Blockchain Platform Service
https://www.oracle.com/br/blockchain/cloud-platform/
Binario Cloud Blockchain (GoFabric engine)
https://binario.cloud/produtos/blockchainGoFabric
Orquestrador disponibilizado pela empresa GoLedger
https://gofabric.io/
•Lista de redes Hyperledger Fabric
•Dashboard de rede
•Gestão multichannel
•Operações de escalabilidade da rede (addPeer, AddOrg, etc)
•Integração automática com chaincodes desenvolvidos com a biblioteca CC-Tools
•Atualização de chaincodes
•Atualização de APIs
•Mapamento de dados dos chaincodes utilizando Templates de dadosGoFabric
Orquestrador disponibilizado pela empresa GoLedger
https://gofabric.io/
•Lista de redes Hyperledger Fabric
•Dashboard de rede
•Gestão multichannel
•Operações de escalabilidade da rede (addPeer, AddOrg, etc)
•Integração automática com chaincodes desenvolvidos com a biblioteca CC-Tools
•Atualização de chaincodes
•Atualização de APIs
•Mapamento de dados dos chaincodes utilizando Templates de dadosGoFabric – DashboardGoFabric – Data Mapping TemplatesGoFabric – deployment na nuvemGoFabric – multichannelGoFabric – Chaincode marketplaceGoFabric – Private ChaincodeGoFabric – Atualizar Chaincode