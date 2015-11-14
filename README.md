# Be MEAN - MongoDB

Guia de estudos do módulo de MongoDB, do curso [Construa seu Instagram com MEAN](http://dagora.net/be-mean/) da [Webschool.io](https://github.com/Webschool-io/)

## Descrição das Aulas

### Aula 01 (Export e Import)

Slides:
 - [Aula 01](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.p)

Nessa aula foi falado sobre os bancos NoSQL, e cada tipo:

 - Chave e Valor;
 - Documento;
 - Grafos;
 - Colunas.

Além disso, foi ensinado sobre a construção e estrutura do MongoDB.

Finalizando a aula foram apresentados os comandos export & import do mongoDB e uma ferramenta chamada mongohacker que melhora a visualização de buscas e resultados no console.

#### Links

 - [Video da Aula](https://www.youtube.com/watch?v=leYxsEAL_yY)
 - [Exercicio da Aula](https://github.com/Webschool-io/be-mean-instagram/blob/master/apostila/mongodb/export_import.md)
 - [Exercicio Resolvido](https://github.com/Webschool-io/be-mean-instagram/blob/master/apostila/classes/mongodb/exercises/class-01-resolved-gilsondev-gilsonfilho.md)

### Aula 02

Slides:
 - [Aula 02](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge7fc94614_395_0)

O que foi falado na aula?

Iniciamos, com a instrução de envio dos exercícios para o repositório do `Webshool-io/be-mean-instagram`, por meio do Pull Request. Foi explicado o uso do comando `use <database_name>` que acessa o banco escolhido. Dessa forma fica assim:

 ```bash
test> use be-mean-instagram
switched to db be-mean-instagram

be-mean-instagram>
 ```

Foi mostrado o comando `show dbs` que lista os databases criados localmente. Exemplo:

```bash
be-mean-instagram> show dbs
be-mean  0.078GB
local    0.078GB
```

Como foi visto na aula, não foi exibido o banco `be-mean-instagram` porque ele só sera alocado, quando é inserido algo nele. Veja:

```bash
test> use be-mean-instagram
switched to db be-mean-instagram

be-mean-instagram> db.teste.insert({nome: "Suissa", idade: 30})
Inserted 1 record(s) in 1854ms
WriteResult({
	"nInserted": 1
})

be-mean-instagram> show dbs
be-mean            0.078GB
be-mean-instagram  0.078GB
local              0.078GB
```

Agora ele mostra o banco porque inserimos um documento nele.

Para listar as coleções do nosso banco, é usado o comando `show collections`:

```bash
be-mean-instagram> show collections
system.indexes  0.000MB / 0.008MB
test            0.000MB / 0.008MB
```

Caso deseja criar uma coleção, sem inserir nada, use o comando `db.createCollection()`. Segue uma explicação na documentação [aqui](https://docs.mongodb.org/manual/reference/method/db.createCollection/#db.createCollection). Nele pode colocar alguns parametros:

 - **name** (string): Nome da coleção que deseja criar
 - **options** (document): É opcional. São configurações para a criação da coleção, como pré alocação de espaço

```bash
be-mean-instagram> db.createCollection("other")
{
	"ok": 1
}
```

Inserindo um tamanho na criação. Abaixo estou alocando 5MB:
```bash
be-mean-instagram> db.createCollection("collection_resized", {size: 5000000})
```

Lembra que na primeira inserção, demorou? Se inserir novamente, não acontece esse grande delay:

```bash
var json = {escola: "Webshool", active: true}
be-mean-instagram> db.test.insert(json)

Inserted 1 record(s) 1ms
WriteResult({
	"nInserted": 1
})
```

Usamos acima direito o comando `db.collection.insert()`, e como o comando diz, ele inserir um documento na coleção selecionada.

```bash
var pokemon = {name: "Pikachu", "description": "Rato eletrico", type: "eletric", "attack": 55, "height": 0.4}
be-mean-instagram> db.pokemons.insert(pokemon)
Inserted 1 record(s) in 7ms
WriteResult({
  "nInserted": 1
})
```

Agora que acabamos de criar, vamos buscar o registro na coleção `pokemons`:

```bash
be-mean-instagram> db.pomekons.find()
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
Fetched 1 record(s) in 0ms
```

Tem outro comando que ele insere e salva, que é o `save()`:

```bash
be-mean-instagram> var pokemon = {'name':'Caterpie','description':'Larva lutadora','type': 'inseto', attack: 30, height: 0.3 }
be-mean-instagram> db.pokemons.save(pokemon)
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
```

Agora vamos buscar o nosso registro usando o `find()`:

```bash
be-mean-instagram> var query = {name: 'Caterpie'}
be-mean-instagram> var p = db.pokemons.find(query)
be-mean-instagram> p
{
  "_id": ObjectId("56422705613f89ac53a7b5d4"),
  "name": "Caterpie",
  "description": "Larva lutadora",
  "type": "inseto",
  "attack": 30,
  "height": 0.3
}
Fetched 1 record(s) in 1ms
be-mean-instagram> p.name
be-mean-instagram>
```

Porque não traz mais? Porque ele é retornado na forma de [Cursor](https://docs.mongodb.org/manual/core/cursors/). Assim quando acessarmos o valor de `p`, faz com que percorremos esse cursor, e sendo que tem somente um registro, significa que ela foi terminada. Tanto que para você interar pode fazer [dessa forma](https://docs.mongodb.org/manual/tutorial/iterate-a-cursor/).

Mas nesse caso, para manter o uso do documento, vamos usar o comando `findOne()`:

```bash
be-mean-instagram> var p = db.pokemons.findOne(query)
be-mean-instagram> p
{
  "_id": ObjectId("56422705613f89ac53a7b5d4"),
  "name": "Caterpie",
  "description": "Larva lutadora",
  "type": "inseto",
  "attack": 30,
  "height": 0.3
}
be-mean-instagram> p.name
Caterpie
```

Agora continuamos com o documento para usarmos quando quiser. Voltando para o `save()`, vamos alterar o ataque desse pokemon:

```bash
be-mean-instagram> p.defense = 35
35
be-mean-instagram> p
{
  "_id": ObjectId("56422705613f89ac53a7b5d4"),
  "name": "Caterpie",
  "description": "Larva lutadora",
  "type": "inseto",
  "attack": 30,
  "height": 0.3,
  "defense": 35
}

be-mean-instagram> db.pokemons.save(p)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

No caso do cursor, você pode iterar dessa forma:

```bash
var myCursor = db.pokemons.find();

while (myCursor.hasNext()) {
   print(tojson(myCursor.next()));
}
```


#### Links

 - [Video da Aula](https://youtu.be/PaNVk0V2UNI)
 - [Exercicio da Aula](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge7fc94614_395_213)
 - [Exercicio Resolvido](https://github.com/Webschool-io/be-mean-instagram/blob/master/apostila/classes/mongodb/exercises/class-02-resolved-gilsondev-gilsonfilho.md)


### Aula 03

Slides:
 - [Aula 03](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge7fc94614_395_231)

O que foi falado na aula?

Inicialmente foi feito a correção do exercício que foi passado na aula 02. Depois disso começamos a aprender sobre a busca das informações com o `find()` e `findOne()`.

O comando `find()` ele aceita dois parametros:

```bash
be-mean-pokemons> db.colecao.find({clausulas}, {campos})
```

Sendo que a *clausula* é como se fose o uso do *WHERE* do banco relacional, e o *campos* define quais campos deseja retornar da pesquisa. Por exemplo, vamos fazer a busca de um pokemon, usando o campo *name*:

```bash
be-mean-pokemons> var query = {name: "Pikachu"}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
Fetched 1 record(s) in 0ms
```

Para definirmos os campos que queremos que retorne, associamos o nome do campo como chave, e o valor usamos *1* para verdadeiro, ou seja, que ele é um campo desejado no retorno, e *0*, define como um campo indesejado, sendo assim falso.

```bash
be-mean-pokemons> var query = {name: "Pikachu"}
be-mean-pokemons> var fields = {name: 1, description: 1}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico"
}
Fetched 1 record(s) in 1ms
```

Mas acabou que veio o *_id*, mas como pode remover isso?

```bash
be-mean-pokemons> var query = {name: "Pikachu"}
be-mean-pokemons> var fields = {_id: 0, name: 1, description: 1}
be-mean-pokemons> db.pokemons.find(query)
{
  "name": "Pikachu",
  "description": "Rato eletrico"
}
Fetched 1 record(s) in 1ms
```

No MongoDB você tem os operadores aritméticos, para enriquecer sua pesquisa. Abaixo vamos ver alguns deles:

 - < é *$lt* (less than): Define se determinado valor é menor que *x*;
 - <= é *$lte* (less than or equal): Define se determinado valor é menor ou igual a *x*;
 - > é *$gt* (greater than): Define se determinado valor é maior que *x*;
 - >= é *$gte* (greater than or equal): Define se determinado valor é maior ou igual a *x*.

Exemplos:

```bash
be-mean-pokemons> var query = {height: {$lt: 20}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
Fetched 1 record(s) in 0ms
```

```bash
be-mean-pokemons> var query = {height: {$lte: 10}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
{
  "_id": ObjectId("564629c1b62557f6ed6281cb"),
  "name": "Wigglytuff",
  "description": "Pokemon cantora",
  "attack": 70,
  "defense": 45,
  "height": 10
}
Fetched 1 record(s) in 0ms
```

```bash
be-mean-pokemons> var query = {height: {$gt: 10}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("564629f4b62557f6ed6281cc"),
  "name": "Ekans",
  "description": "Cobrinha pokemon",
  "attack": 60,
  "defense": 44,
  "height": 69
}
Fetched 1 record(s) in 0ms
```

```bash
be-mean-pokemons> var query = {height: {$gte: 10}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("564629c1b62557f6ed6281cb"),
  "name": "Wigglytuff",
  "description": "Pokemon cantora",
  "attack": 70,
  "defense": 45,
  "height": 10
}
{
  "_id": ObjectId("564629f4b62557f6ed6281cc"),
  "name": "Ekans",
  "description": "Cobrinha pokemon",
  "attack": 60,
  "defense": 44,
  "height": 69
}
Fetched 1 record(s) in 0ms
```

Temos também os operadores lógicos que são *$or*, *$nor* e *$and*. Segue os exemplos:

 - Usando o operador *$or*:

```bash
be-mean-pokemons> var query = {$or: [{name: "Pikachu"}, {height: 10}]}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
{
  "_id": ObjectId("564629c1b62557f6ed6281cb"),
  "name": "Wigglytuff",
  "description": "Pokemon cantora",
  "attack": 70,
  "defense": 45,
  "height": 10
}
Fetched 2 record(s) in 1ms
```

Lembrando que o *$or* precisa receber um array de no mínimo, duas premissas.

 - Usando o operador *$nor*:

```bash
be-mean-pomekons> var query = {$nor: [{name: "Pikachu"}, {height: 10}]}
be-mean-pomekons> db.pokemons.find(query)
{
  "_id": ObjectId("564629f4b62557f6ed6281cc"),
  "name": "Ekans",
  "description": "Cobrinha pokemon",
  "attack": 60,
  "defense": 44,
  "height": 69
}
Fetched 1 record(s) in 1ms
```

 - Usando o operador *$and*:

```bash
be-mean-pokemons> var query = {$and: [{name: "Pikachu"}, {height: {$lt: 10}}]}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
Fetched 1 record(s) in 0ms
```

Lembrando que com o *$and*, ele irá retornar algo, se **todas suas premissas estiverem verdadeiras**.

E terminando, vamos aos operadores existenciais, com o operador *$exists*. Ele verifica se determinado campo existe:

```
be-mean-pokemons> db.pokemons.find({name: {$exists: true}})
{
  "_id": ObjectId("5643a6a374c9f8d5e993a639"),
  "name": "Pikachu",
  "description": "Rato eletrico",
  "type": "eletric",
  "attack": 55,
  "height": 0.4
}
{
  "_id": ObjectId("564629c1b62557f6ed6281cb"),
  "name": "Wigglytuff",
  "description": "Pokemon cantora",
  "attack": 70,
  "defense": 45,
  "height": 10
}
{
  "_id": ObjectId("564629f4b62557f6ed6281cc"),
  "name": "Ekans",
  "description": "Cobrinha pokemon",
  "attack": 60,
  "defense": 44,
  "height": 69
}
Fetched 3 record(s) in 1ms
```

Caso não existir:

```bash
be-mean-pokemons> db.pokemons.find({other: {$exists: true}})
Fetched 0 record(s) in 0ms
```

#### Links

 - [Video da Aula](https://www.youtube.com/watch?v=cIHjA1hyPPY)
 - [Exercicio da Aula](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge839fbc67_123_183)
 - [Exercicio Resolvido](https://github.com/Webschool-io/be-mean-instagram/blob/master/apostila/classes/mongodb/exercises/class-03-resolved-gilsondev-gilsonfilho.md)
