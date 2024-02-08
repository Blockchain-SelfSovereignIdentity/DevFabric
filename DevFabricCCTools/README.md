# Desenvolvendo Dapps em plataforma - Hyperledger Fabric
#### CC-Tools Library
#### Hyperledger Labs

## Utilizando o readthedocs
https://goledger-cc-tools.readthedocs.io/en/latest/

## Veriﬁcando o docker
```shell
sudo systemctl enable docker
```

Limpando o docker

```shell
docker stop $(docker ps –a –q)
docker rm $(docker ps –a –q)
docker rmi –f $(docker images)
docker volume prune
docker system prune
```

## Repositórios oﬁciais
https://github.com/hyperledger-labs/cc-tools

https://github.com/hyperledger-labs/cc-tools-demo

https://github.com/hyperledger-labs/cc-tools-doc

## Desenvolvendo chaincodes
Mapeamento de dados

Gerenciamento de direito de escrita e leitura entre organizações (MSP)

Cadastramento de transações

## Biblioteca CC-Tools
A biblioteca CC-Tools é desenvolvida em GoLang e possui diversas
características que facilitam a jornada de aprendizado, desenvolvimento e
deployment em produção de um chaincode para Hyperledger Fabric.

## Biblioteca CC-Tools - características

Open source

Padronização de assets, keys e referências de assets (asset dentro de asset)

Tipos de dados padrão e customizáveis

Gerenciamento de organizações (MSP)

Cadastramento de Transações

Gerenciamento de permissões de escrita por propriedade de assets por MSP

Gerenciamento de private data.

## Biblioteca CC-Tools - características
Transações embedded:
* Create
* Read
* Update
* Delete
* Search (paginated)
* ReadHistory

Transações customizáveis

Integração das transações com a Rest API (GET, PUT, POST, DELETE)

Permissionamento de chamadas de transações por MSP

## Exemplo de Asset
```go
// Description of a book
var Book = assets.AssetType {
	Tag: "book",
	Label: "Book",
	Description: "Book",
	Props: []assets.AssetProp {
		{
			IsKey: true, // Primary Key
			Tag: "title",
			Label: "Book Title",
			DataType: "string",
			Writers: []string{`org2MSP`}, // This means only org2
			can create the asset (others can edit)
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
}
```

## Exemplo de Transação
```go
// Updates the tenant of a Book
// POST Method
var UpdateBookTenant = tx.Transaction {
	Tag: "updateBookTenant",
	Label: "Update Book Tenant",
	Description: "Change the tenant of a book",
	Method: "PUT",
	Callers: []string{`$org\dMSP`}, // Any orgs can call
	this transaction
	Args: []tx.Argument {
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
	Routine: func(stub *sw.StubWrapper, req
		map[string]interface{}) ([]byte, errors.ICCError) {
		...........................................................
		...........................................................
		...........................................................
		return bookJSON, nil
	},
}
```

## Ferramentas
#### Testes unitários
```go
func TestCreateNewLibrary(t *testing.T) {
    stub := mock.NewMockStub("org3MSP",
    new(cc.CCDemo))
    expectedResponse :=
    map[string]interface{} {
        "@key":
        "library:3cab201f-9e2b-579d-b7b2-72297ed1
        7f49",
        "@lastTouchBy": "org3MSP",
        "@lastTx": "createNewLibrary",
        "@assetType": "library",
        "name": "Maria's Library",
    }
}
```

#### Web service (Rest API) integrado
Operações básicas
* POST
    * /invoke/{txName}
    * Executa a transação e escreva o resultado na blockchain
* POST
    * /query/{txName}
    * Executa a transação txName e apenas retorna o resultado, sem escrevê-lo na blockchain
* GET
    * /query/getHeader
    * Busca informações do chaincode
* GET
    * /query/getTx
    * Solicita a lista de transações definidas
* POST
    * /query/getTx
    * Obtém a descrição de uma transação específica

#### Aplicação Web

## Repositório cc-tools-demo
Baixar o repositório

```shell
git clone https://github.com/hyperledger-labs/cc-tools-demo
```

Vendorar o chaincode no diretório chaincode

```shell
go mod vendor
```

## Iniciando o ambiente
Executar o script para HL Fabric 2.2 com 3 orgs
```shell
./startDev.sh
```

Executar o script para HL Fabric 1.4 com 1 org
```shell
./startDev.sh –n 1
```

Para subir a aplicação Web
```shell
./run-web-cc.sh
```

## cc-tools-demo – diretórios e scripts

```shell
-- chaincode # chaincode source code
    |-- assettypes # deﬁnições dos assets
    |-- datatypes # tipos de dados customizados
    |-- txdefs# transações customizadas
-- rest-server # web service (REST API)
    npmInstall.sh # vendorar web service
    startDev.sh # reiniciar apis (1.4)
    startDev2.sh # reiniciar apis (2.2)
-- fabric # artefatos HL Fabric 1.4
    startDev.sh # iniciar rede HL 1.4
-- fabric2 # artefatos HL Fabric 2.2
    startDev.sh # iniciar rede HL 2.2
renameProject.sh # renomear nome do chaincode
startDev.sh # iniciar rede HL Fabric 1.4
startDev2.sh # iniciar rede HL Fabric 2.2
```

## Deﬁnição de um asset
```go
// Deﬁnição dos assets dentro da pasta assettypes
var Person = assets.AssetType {
    Tag: "person", // Identiﬁcação do ativo (JSON)
    Label: "Person",
    // Texto identiﬁcador do ativo
    Description: "Personal data",
    // Texto idenﬁcador detalhado do ativo
    Props: []assets.AssetProp {
        {
            IsKey: true, // Propriedade faz parte da chave primária/composta
            Tag: "id",
            // Identiﬁcação da propriedade (JSON/interface{})
            Label: "CPF (Brazilian ID)",
            // Texto identiﬁcador da propriedade
            DataType: "cpf",
            // Tipo da propriedade (embedded ou custom)
            Writers: []string{`org1MSP`},
            // Organização que pode criar/alterar a propriedade
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
        },
    }
}
```

## Lista de assets do chaincode
Os assets devem estar explicitamente deﬁnidos no arquivo assetTypeList.go na pasta chaincode
```go
var assetTypeList = []assets.AssetType{
    assettypes.Person,
    assettypes.Book,
    assettypes.Library,
    assettypes.Secret,
}
```

## Datatypes embedded
CC-Tools possui os seguintes datatypes padrão

```shell
string # texto livre
number # numero ﬂutuante
datetime # data e hora
boolean # true / false
[]string # array de texto livre
[]number # array numero ﬂutuante
[]datetime # array data e hora
[]boolean # array de true / false
->[asset] # referencia a outro asset
[]->[asset] 
# array de referencias a assets
```

## Deﬁnição de um Datatype custom
Deﬁnição dos custom datatypes dentro da pasta datatypes

```go
var cpf = assets.DataType {
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
}
```

## Lista de custom datatypes do chaincode
Os custom datatypes devem estar explicitamente deﬁnidos no arquivo datatypes.go na pasta
chaincode/datatypes
```go
var CustomDataTypes = map[string]assets.DataType {
    "cpf": cpf,
    "bookType": bookType,
}
```

## Transações embedded
CC-Tools possui os seguintes transações embutidas automaticamente para uso.
```go
Tx.ReadAsset // Ler ativo no world state
tx.CreateAsset // Criar no ativo no channel
tx.UpdateAsset // Atualizar ativo no channel
tx.DeleteAsset // Deletar ativo no channel
tx.Search // Procurar ativos com rich query no world state
tx.ReadAssetHistory // Histórico de um ativo no ledger
```

## Deﬁnição de uma transação custom
```go
var CreateNewLibrary = tx.Transaction{
    Tag: "createNewLibrary", // Identiﬁcação da transação (API endpoint)
    Label: "Create New Library", // Texto identiﬁcador da transação
    Description: "Create a New Library", // Texto identiﬁcador detalhado da transação
    Method: "POST", // Método do web service
    Callers: []string{"$org3MSP", "$orgMSP"}, // Organizações (MSP) que podem chamar essa transação
    Args: []tx.Argument{
        {
            Tag:
            "name", // Idenﬁcação do argumento
            Label: "Name", // Texto identiﬁcador do argumento
            Description: "Name of the library", // Texto identiﬁcador detalhado do argumento
            DataType: "string", // Tipo de dados do argumento
            Required: true, // Argumento obrigatório (default=false)
        },
    },
    Routine: func(stub *sw.StubWrapper, req map[string]interface{}) ([]byte, errors.ICCError) {
        name, _ := req["name"].(string) // Argumento do chamada
        libraryMap := make(map[string]interface{})
        libraryMap["@assetType"] = "library"
        libraryMap["name"] = name
        libraryAsset, err := assets.NewAsset(libraryMap) // Preparação do ativo para gravação
        if err != nil {…}
        _, err = libraryAsset.PutNew(stub) // Criação do transaction proposal response
        if err != nil {…}
        libraryJSON, nerr := json.Marshal(libraryAsset)
        if nerr != nil {…}
        return libraryJSON, nil // Retorna resultado para a API
    },
}
```

## Lista de transações do chaincode
As transações, inclusive as embedded devem estar explicitamente deﬁnidas no arquivo txList.go na
pasta chaincode
```go
var txList = []tx.Transaction {
    tx.CreateAsset,
    tx.UpdateAsset,
    tx.DeleteAsset,
    txdefs.CreateNewLibrary,
    txdefs.GetNumberOfBooksFromLibrary,
    txdefs.UpdateBookTenant,
    txdefs.GetBooksByAuthor,
}
```

## Utilizando o Web Service padrão (API)
Web service padronizado para realizar chamadas na rede Blockchain.

Modelo padrão:

Invoke (realiza alteração no channel)
Métodos: POST, PUT, DELETE
api/invoke/:tx

Query (não altera o channel):
Métodos: GET, POST
api/query/:tx

## Endpoints de apoio
#### query/getHeader
* Método GET
* Retorna versão do CC-Tools e dados do header para visualização na interface

#### query/getSchema
* Métodos GET e POST
* Retorna informações dos assets ou de um asset especíﬁco

#### query/getTx
* Métodos GET e POST
* Retorna informações das transações ou de uma transação especíﬁca

## Exemplo de getSchema

## Endpoints para as tx embedded
#### invoke/createAsset
* Método POST
```go
{"asset": [ {JSON do ativo}, {JSON do ativo}, ... ] }
```

#### invoke/readAsset
* Método POST
```go
{“key": {“@assetType”: Tipo do ativo, Dados da chave } }
```

#### invoke/updateAsset
* Método PUT
```go
{“update": {“@assetType”: Tipo do ativo, Dados da chave, Propriedades a serem alteradas } }
```

#### invoke/deleteAsset
* Método DELETE
```go
{“key": {“@assetType”: Tipo do ativo, Dados da chave } }
```

## Identiﬁcação de chaves dentro do asset
#### Propriedade da chave
@key

#### Asset deve ter a identiﬁcação do tipo do asset
@assetType

Para algumas operações a chave é calculada automaticamente.
Exemplo:
```go
{ 
    "key": {
        "@assetType": "person",
        "id": "318.207.920-48"
    }
}
```
```go
{
    "key": {
        "@assetType": "person",
        "@key":"person:47061146-c642-51a1-844a-bf0b17cb5e19"
    }
}
```

## Utilizando a ferramenta Web App (GoInitus)

Dentro do diretório raiz
```go
./run-cc-web.sh
```

Acessar o browser http://localhost:8080

Conﬁgurar a interface para acessar a api correta (icone de ferramenta do header)


PG 28/73