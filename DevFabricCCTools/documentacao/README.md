# CC-Tools

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