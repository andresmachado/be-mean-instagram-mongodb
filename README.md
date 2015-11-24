
## Links úteis:

Link para o [slide da aula 2](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge7fc944ba_0_0);

Link para a [pokeAPI](http://pokeapi.co);

Link para [documentação do MongoDB](https://docs.mongodb.org/manual/?_ga=1.76171165.942394937.1447847907)

---

# Modulo MongoDB Aula 2

## Comandos vistos na aula

Foi visto nessa aula como inserir e consultar registros no banco e como criar collections vazias, bem como adicionar e salvar registros ao mesmo tempo.

> db.collection.insert()

> db.collection.save()

> db.collection.find() - "Retorna um cursor"

> db.collection.findOne() - "Retorna um objeto"

---

# Módulo MongoDB Aula 3

## Funções vistas na aula

>db.find()

>db.findOne()

---

Ao inserir objetos no MongoDB, ele adiciona um `_id`, chamado de **UUID** - *Underline Unique Identifier*.

O `UUID` é um identificador único formado por:

> 4-bytes: valor que representa os segundos;

> 3-bytes: identificador da máquina;

> 2-bytes: ID do processo `pid`;

> 3-bytes: contador, com valor aleatório.

---

### FUNÇÃO `find()`

Retorna os registros de acordo com os parâmetros passados para a função

**Sintaxe**

>db.colecao.find({query}, {campos})

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

> { campo: {$exists: true }};

---

# Modulo MongoDB Aula 04

## Funções vistas na aula

> db.collection.update();

### FUNÇÃO UPDATE

O MongoDB altera os documentos de duas formas, `save` ou `update`. A diferença entre um e outro é que o **UPDATE** já atualiza o objeto direto, enquanto o **SAVE** precisa de uma query, passando os parâmetros.

A função `update` recebe três parâmetros:

1. query;
2. modificador;
3. options (não obrigatório).

## Sintaxe do comando

> db.colecao.update(query, modificador, options);

**ATENÇÃO**: Este comando altera o registro por completo, substituindo todos os valores existentes pelos valores passados na variável MOD. Para correta utilização do `update()` utilize-o em conjunto com os `operadores de modificação`;

Caso o campo não exista, ele será criado pela função.

>Dica:
>Para buscar dados de modo **unsensitive** basta colocar um **/i** após o campo que você deseja procurar.

*Exemplo*

> var query = {name:/testemon/i};

## Os parâmetros do `options` da função `update()`.

Este objeto servirá para configurarmos valores diferentes na função

**Sintaxe**

  > **upsert**: boolean (false por default),

  >**multi**: boolean (false por default),

  > **writeConcern**: document

### `upsert`

Esta opção serve para inserir um objeto caso ele não seja encontrado pela _**query**_. Por padrão, esta opção é **FALSE**.

O **upsert** so irá inserir no objeto as opções que forem passadas no _**modificador**_ da função.

**Operadores do `upsert`**

`$setOnInsert`

O operador `$setOnInsert` apenas inserirá os dados caso ocorra um `upsert`

>var query = {name: /NaoExisteMon/i}

>AndrePC(mongod-3.0.7) be-mean-pokemons> var mod = {$set: {active: true}, $setOnInsert{ name: "NaoExisteMon", attack: null, defense: null, height: null, description: "Sem maiores informações"}}

>AndrePC(mongod-3.0.7) be-mean-pokemons> var mod = {$set: {active: true}, $setOnInsert: {name: "NaoExisteMon", attack: null, defense: null, height: null, description: "Sem maiores informações"}}

> AndrePC(mongod-3.0.7) be-mean-pokemons> var options = {upsert: true};

>AndrePC(mongod-3.0.7) be-mean-pokemons> db.pokemons.update(query, mod, options);

Updated 1 new record(s) in 1ms

WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("564dcb6811ac130d1767dbaa")
})

### `multi`

Por padrão, o **MongoDB** só permite a edição de um registro por vez, caso você precise `$set`ar um valor em mais de um registros, você precisa passar **TRUE** na `option` da função.

> AndrePC(mongod-3.0.7) be-mean-pokemons> var query = {}

> AndrePC(mongod-3.0.7) be-mean-pokemons> var mod = {$set: {active: false}}}

> AndrePC(mongod-3.0.7) be-mean-pokemons> var opt = {multi: true};

AndrePC(mongod-3.0.7) be-mean-pokemons> db.pokemons.update(query, mod, opt)

Updated 8 existing record(s) in 1ms
WriteResult({
  "nMatched": 8,
  "nUpserted": 0,
  "nModified": 8
})


### `writeConcern`

---

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

> AndrePC(mongod-3.0.7) test> var mod = {$inc: {attack: 1}};

> AndrePC(mongod-3.0.7) test> db.pokemons.update(query, mod);

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

## Operadores de busca em *Array*

### `$in`

Retorna os documentos que possui(em) algum dos valores passados no **[Array_de_Valores]**. Os valores podem ser passados como _**expressões regulares (regex)**_;

**Sintaxe**

{campo: {$in: [Array_de_Valores]}};

> AndrePC(mongod-3.0.7) be-mean-pokemons> var query = {moves: {$in: [/choque do trovão/i]}};

> AndrePC(mongod-3.0.7) be-mean-pokemons> db.pokemons.find(query);

### `$nin`

Retorna qualquer documento que _**não possuam**_ dos valores passados no **[Array_de_Valores]**. Os valores podem ser passados como _**expressões regulares (regex)**_;

**Sintaxe**

{campo: {$nin: [Array_de_Valores]}};

### `$all`

Retorna os documentos que _**possuam todos**_ os valores passados no **[Array_de_Valores]**. Os valores podem ser passados como _**expressões regulares (regex)**_;

**Sintaxe**

{campo: {$all: [Array_de_Valores]}};

## Operadores de Negação

### `$ne` - not Equal

Retorna os valores que não são compatíveis com o **valor**. Este campo _**NÃO ACEITA EXPRESSÕES REGULARES**_; Os regitros precisam possuir o campo para ser avaliado.

**Sintaxe**

{campo: {$ne: valor}};

### `$not`

Retorna os valores que NÃO são iguais ao valor passado. Este campo _**ACEITA EXPRESSÕES REGULARES**_;

{campo: {$not: valor}};

---

## FUNÇÃO `DELETE`

> db.collection.remove();

Este comando remove um registro da coleção.

**ATENÇÃO**: Este valor é *multi: true*, caso não seja passado nenhum valor para ele, **TODOS** os registros serão apagados;

Para deletar uma coleção inteira, basta utilizar o _**db.collection.drop**_;

# Modulo MongoDB Aula 05

### Funções vistas na aula

> count();

> distinct();

> group();

> limit().skip();

> aggregate().

**Dica**: Para uma pesquisa de documentos mais performatica, dê prioridade sempre a função `count()` ao invés do `find()`, pois o find trás todos os registros para a memória, o que pode ocasionar engasgos no seu MongoDB. Todos os argumentos passados para o `find()` podem ser passados para o `count()`;

---

### FUNÇÃO `count()`

Conta a quantidade de registros de acordo com os parâmetros passados na função. Aceita _**Operadores aritméticos**_ como `$lte, $gte e etc`

**Sintaxe**

> db.collection.count({condicao1}, {condicao2}, ...)

### FUNÇÃO `distinct()`

Retorna quantos valores únicos de uma propriedade especifica.

_Exemplo_

> AndrePC(mongod-3.0.7) be-mean-instagram> db.restaurantes.distinct('borough');
> [
  "Bronx",
  "Brooklyn",
  "Manhattan",
  "Queens",
  "Staten Island",
  "Missing"
];

É possivel também cascatear funções no `distinct()`:

Para ordenar alfabeticamente:

> AndrePC(mongod-3.0.7) be-mean-instagram> db.restaurantes.distinct('borough');

> [
  "Bronx",
  "Brooklyn",
  "Manhattan",
  "Queens",
  "Staten Island",
  "Missing"
]

Para retornar a quantidade de propriedades:

> AndrePC(mongod-3.0.7) be-mean-instagram> db.restaurantes.distinct('borough').length;

>6

### FUNÇÃO `limit().skip()`

A função `limit()` limita o retorno de registros pela quantidade informada. A função `skip()` "pula" a quantidade de registros e começa a limitar a partir do valor informado. Como o retorno é paginado com indíce 0, você pode informar ao `skip()` qual pagina você quer limitar passando um **(valor * pagina)**

_Exemplo_

> db.collection.find({}, name: 1).limit(2).skip(2);

Para limitação por página:

> db.collection.find({}, name: 1).limit(10).skip(10 * 1);


### FUNÇÃO `group()`

Agrupa os resultados de forma mais perfomatica

**Sintaxe**

Para melhor visualizacao do uso do `group()` [veja a documentação](https://docs.mongodb.org/manual/reference/method/db.collection.group/);

### FUNÇÃO `aggregate()`

Retorna resultados agrupados de forma mais semântica que o `group()` e possui parâmetros próprios para cálculos de dados. Todos os parâmetros da função começam com o sinal de `$`

Para melhor visualização do uso do `aggregate()` [veja a documentação](https://docs.mongodb.org/v3.0/reference/operator/aggregation/group/)


# Modulo MongoDB Aula 06

## Funções vistas na aula

> db.invt.find(query)

> db.invt.insert({dados})

## Relacionamentos no MongoDB

1 - Não existem JOINS no MongoDB;
2 - Para criar um relacionamento, basta salvar o `_id` de uma **collection** em outra, com isso, criando um **inventário (`invt`)**;
3 - As buscas são feitas manualmente;
