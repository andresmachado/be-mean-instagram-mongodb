# Modulo MongoDB Aula 2


## Comandos vistos na aula

Foi visto nessa aula como inserir e consultar registros no banco e como criar collections vazias, bem como adicionar e salvar registros ao mesmo tempo.

> db.collection.insert()

> db.collection.save()

> db.collection.find() - "Retorna um cursor"

> db.collection.findOne() - "Retornar um objeto"

---

## Links úteis:

Link para o [slide da aula 2](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge7fc944ba_0_0)

Link para a [pokeAPI](http://pokeapi.co)

# Modulo MongoDB Aula 3

## Comandos vistos na aula

>db.find()

>db.findOne()

Ao inserir objetos no MongoDB, ele adiciona um `_id`, chamado de **UUID** - Underline Unique Identifier.

O `UUID` é um identificador único formado por:

> 4-bytes: valor que representa os segundos;

> 3-bytes: identificador da máquina;

> 2-bytes: ID do processo `pid`;

> 3-bytes: contador, com valor aleatório.


### Sintaxe de buscas no **MongoDB**

>db.colecao.find({clausulas}, {campos})

As buscas podem ser feitas através de variáveis.

>var query = {nome: "Pikachu"};

Então, pode passar a variável como parâmetro para o `find()`;

>db.colecao.find(query)

## Fields `{campos}`;

O JSON `query` é equivalente ao **WHERE** assim como o `fields` é equivalente ao **SELECT** onde selecionará os campos desejados;

No objeto de `fields` nós dizemos quais os campos que retornarão na nossa busca com **TRUE (1)** ou negar o campo com **FALSE (0)**;

*Exemplo*

Digamos que eu quero selecionar os campos `description` e `name` na minha busca;

>AndrePC(mongod-3.0.7) be-mean-pokemons> var query = {name: "Pikachu"};

>AndrePC(mongod-3.0.7) be-mean-pokemons> var fields = {name: 1, description: 1};

>AndrePC(mongod-3.0.7) be-mean-pokemons> db.pokemons.find(query, fields)

>{
  "_id": ObjectId("5643d3acc8e7c6976cee0169"),
  "name": "Pikachu",
  "description": "Ratinho eletrico fofinho"
}
Fetched 1 record(s) in 24ms

## Operadores aritméticos.

Os operadores no mongoDB têm seus equivalentes:

### Sintaxe Geral

>db.colecao.find({ "campo" : { $operador: value }});

`<` é `$lt` - less than;

>var query = {height: { $lt: 0.5 }};

Leitura do comando:

>{
  "height": {
    "$lt": 0.5
  }
}

Então, é só passar a variavél para a função `find()`:

>db.pokemons.find(query);

`<=` é `$lte` - Less than or equal;

`>` é `$gt` - greater than;

`>=` é `$gte` - greater than or equal;


## Operados lógicos

### Sintaxe geral

> var query = {$operador: [{cond: 1}, {cond: 2}]};
> db.collection.find(query);

`$or` - OU

O operador `$or` retorna o objeto verdadeiro de uma comparação entre duas ou mais premissas. O operador recebe um array de objetos. Você precisa testar no minimo duas premissas.

`$nor` - Not OR

O operador `$nor` **nega** a condição, sendo assim ele retorna os registros que não atendem as condições passadas.

`$and` - E

Retorna o objeto que satisfaça todas as condições **VERDADEIRAS**;

## Operadores `existênciais`

Retorna um objeto caso o campo **exista**;

### Sintaxe geral

>db.collection.find({ campo: {$exists: true }});

# Modulo MongoDB Aula 04

## Funções vistas na aula

> db.collection.update();

### FUNÇÃO UPDATE

O MongoDB altera os documentos de duas formas, `save` ou `update`. A diferença entre um e outro é que o **UPDATE** já atualiza o objeto direto, enquanto o **SAVE** precisa de uma query, passando os parâmetros.

A função `update` recebe três parâmetros:

1. query;
2. modificação;
3. options (não obrigatório).

## Sintaxe do comando

> db.colecao.update(query, mod, opt);

**ATENÇÃO**: Este comando altera o registro por completo, substituindo todos os valores existentes pelos valores passados na variável MOD. Para correta utilização do `update()` utilize-o em conjunto com os `operadores de modificação`;

Caso o campo não exista, ele será criado pela função.

>Dica:
>Para buscar dados de modo **unsensitive** basta colocar um **/i** após o campo que você deseja procurar.

*Exemplo*

> var query = {name:/testemon/i};

## Operadores de modificação

### `$set`

Modifica um valor ou cria, caso ele não exista.

**Sintaxe**

{$set: {campo, valor}};

### `$unset`

Remove os campos de um registro

**Sintaxe**

{$unset: {campo: true/false}};

### `$inc`

Incrementa um valor no registro com a quantidade desejada. Para **decrenentar** um valor, basta passar um inteiro *negativo*

**Sintaxe**

{$inc: {campo, +/-valor}};

>AndrePC(mongod-3.0.7) test> var mod = {$inc: {attack: 1}};
AndrePC(mongod-3.0.7) test> db.pokemons.update(query, mod);
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
Fetched 1 record(s) in 0ms

## Operadores de array

### `$push`

Este operador adiciona valor ao campo do array, caso ele seja existente. Caso não exista o campo será criado do tipo *Array* adicionando o valor passado no `$push`. Se o campo existir e não for do tipo *Array* um erro ocorrerá.

**Sintaxe**

{$push: {campo: valor}};

### `$pushAll`

Funciona como o `$push`, mas passa valores como *Array*;

**Sintaxe**

{$pushAll: {campo: [valores]}};

### `$pull`

Remove valores de um array caso ele exista e seja do tipo *Array*

**Sintaxe**

{$pull: {campo: valor}};

### `$pullAll`

Funciona como o `$pull`, mas passa valores como *Array*;

**Sintaxe**

{$pullAll: {campo: [valores]}};
