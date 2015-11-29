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


### Aula 04 (Parte 01)

Slides:
 - [Aula 04 (Parte 01)](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.ge839fbc67_123_188)

O que foi falado na aula?

Começamos a estudar a diferença entre o `update()` e o `save()metr)`, no ato de atualizar algum documento. E quais são as diferenças?

 - No `save` você precisa buscar o documento antes de modificar;
 - No `update` não precisa, mas para realmente atualizar o documento, precisa passar pelo menos o seu `ObjectID`.

O `update` recebe três parametros: **query**, **modification**, **options**. Vamos ver cada um deles.

 - **query**: É onde você vai definir a sua query, para fazer sua busca;
 - **modification**: Define os campos a serem modificados;
 - **options**: 

Ele vai ser da seguinte forma:
```bash
db.colecao.update(query, mod, options);
```

Agora vamos criar um novo pokemon, para depois atualizar:

```bash
be-mean-pokemons> var pokemon = {name: "Poketest", attack: 40, defense: 20, height: 4, description: "Pokemon Teste"}

be-mean-pokemons> db.pokemons.save(pokemon)

Inserted 1 record(s) in 30ms
WriteResult({
  "nInserted": 1
})
```

Criamos o documento. Então, vamos buscar ele, para pegarmos o `ObjectID` para assim, mostrarmos a atualização:

```bash
be-mean-pokemons> var query = {name: /poketest/i}
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "name": "Poketest",
  "attack": 40,
  "defense": 20,
  "height": 4,
  "description": "Pokemon Teste"
}
Fetched 1 record(s) in 1ms
```

Agora vamos mudar a descrição:

```bash
be-mean-pokemons> var id = {"_id": ObjectId("56489706ak219as219asasl129")}
be-mean-pokemons> var modification = {description: "Alterado"}
be-mean-pokemons> db.pokemons.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

Quando for buscar novamente retorna isso:

```bash
be-mean-pokemons> db.pokemons.find(query)

{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "description": "Alterado"
}

Fetched 1 record(s) in 1ms
```

Mas aonde está os outros campos que inserimos? O MongoDB removeu, porque as modificações que armazenamos na variável `modifications` definiu somente a descrição. Para evitar esse tipo de situação, temos que usar o operador [$set](https://docs.mongodb.org/manual/reference/operator/update/set/). Ele auxilia na definição dos campos que desejamos alterar, sem afetar nas que não inserimos no parametro de modificação. Exemplo:

```bash
{$set: {campo: valor}}
```

Assim podemos definir quais campos o mongo deve atualizar. No nosso caso vai ser:

```bash
be-mean-pokemons> var query = {"_id": ObjectId("56489706ak219as219asasl129")}
be-mean-pokemons> var modification = {$set: {description: "Alterado"}}
be-mean-pokemons> db.pokemons.update(query, modification)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

```bash
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "description": "Alterado",
  "name": "Poketest",
  "attack": 40,
  "defense": 20,
  "height": 4
}
Fetched 1 record(s) in 1ms
```

Dessa forma, deixamos claro para o mongo, o que deve modificar.

Um outro operador que faz o contrário acima, é o [$unset](https://docs.mongodb.org/manual/reference/operator/update/unset/), que ele remove um campo em particular.

```bash
{$unset: {campo: valor}}
```

Se quiséssemos remover o campo de peso, é só fazer da seguinte forma:

```bash

be-mean-pokemons> var query = {"_id": ObjectId("56489706ak219as219asasl129")}
be-mean-pokemons> var modification = {$unset: {height: 1}}
be-mean-pokemons> db.pokemons.update(query, modification)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

```bash
be-mean-pokemons> var query = {"_id": ObjectId("56489706ak219as219asasl129")}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "description": "Alterado",
  "name": "Poketest",
  "attack": 40,
  "defense": 20
}
Fetched 1 record(s) in 1ms
```

Temos também o operador [$inc](https://docs.mongodb.org/manual/reference/operator/update/inc/) que serve para incrementar um campo que tenha um valor do tipo numérico. Se esse campo não existe, então seta o valor passado e o salva no documento. Para incrementar, coloca um **valor positivo**, caso contrário um **valor negativo**.

```bash
{$inc: {field: value}}
```

Segue um exemplo:

```bash
be-mean-pokemon> var mod = {$inc: { attack: 1 }}
be-mean-pokemon> db.pokemons.update(query, mod)

Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

O comando [$push](https://docs.mongodb.org/manual/reference/operator/update/push/) adiciona um valor dentro de um array. Caso esse campo não exista, ele cria o mesmo com o tipo array, e mesmo assim o campo não for um array, retorna um erro.

```bash
{$push: {campo: valor}}
```

Vamos adicionar um campo:
```bash
be-mean-pokemons> var modification = {$push: {moves: 'Poder de testar as coisas'}}
be-mean-pokemons> db.pokemons.update(query, modification)

Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

Quando vermos o nosso documento:

```bash
be-mean-pokemons> var query = {"_id": ObjectId("56489706ak219as219asasl129")}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "description": "Alterado",
  "name": "Poketest",
  "attack": 40,
  "defense": 20,
  "moves": ["Poder de testar as coisas"]
}
Fetched 1 record(s) in 1ms
```

Com o comando [$pushAll](https://docs.mongodb.org/manual/reference/operator/update/pushAll/) não seria diferente, a não ser que ele recebe um array inteiro.

```bash
{$pushAll: {campo: ["itemN"]}}
```

```bash
be-mean-pokemons> var moves = ["Testando 1", "Testando 2"]
be-mean-pokemons> var modification = {$pushAll: {moves: moves}}
be-mean-pokemons> db.pokemons.update(query, modification)

Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

Temos o comando [$pull](https://docs.mongodb.org/manual/reference/operator/update/pull/) que remove o valor do campo definido, caso ele seja um array existente. Se não existir, faz nada, e se o campo não for um array, retorna um erro.

```bash
{$pull: {campo: valor}}
```

```bash
be-mean-pokemons> var modification = {$pull: {moves: 'Testando 1'}}
be-mean-pokemons> db.pokemons.update(query, modification)

Updated 1 existing record(s) in 5ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

Com o [$pullAll](https://docs.mongodb.org/manual/reference/operator/update/pushAll/) você passa um array de valores que deseja ser removido.

```bash
{$pushAll: {campo: ["itemN"]}}
```

```bash
be-mean-pokemons> var moves = ["Testando 1", "Testando 2"]
be-mean-pokemons> var modification = {$pullAll: {moves: moves}}
be-mean-pokemons> db.pokemons.update(query, modification)

Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```


### Aula 04 (Parte 02)

Slides:
 - [Aula 04 (Parte 02)](https://docs.google.com/presentation/d/1KXxmcwd47x4v2SymyiBPK7ucn80PruSvcw4mZ5S3nWc/edit#slide=id.gd8825a620_121_0)

Nessa aula é a continuação dos operadores usados no parametro de *options* da função `update()`.

Vamos começar com o operador `upsert`, `multi` e `writeConcern`.

#### upsert

Com o upsert, que por padrão é false, se definido como verdadeiro, ele insere os dados a ser modificados, caso não retornar nenhum documento usando com critério a query. Segue um exemplo, em que vamos atualizar um documento em que o mongo vai encontrar, e atualizar um valor usando o operador `$set`.

```bash
be-mean-pokemons> var query = {name: /ekans/i}
be-mean-pokemons> var mod = {$set: {active: true}}
be-mean-pokemons> var options = {upsert: true}
be-mean-pokemons> db.pokemons.update(query, mod, options)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 0
})
```

Como buscarmos novamente, vamos ver que ele foi atualizado:

```bash
be-mean-pokemons> var query = {name: /ekans/i}
be-mean-pokemons> db.pokemons.update(query)
{
  "_id": ObjectId("564629f4b62557f6ed6281cc"),
  "name": "Ekans",
  "description": "Cobrinha pokemon",
  "attack": 60,
  "defense": 44,
  "height": 69,
  "active": true
}
Fetched 1 record(s) in 0ms
```

Agora o que acontece se o registro que for buscar não existe?

```bash
be-mean-pokemons> var query = {name: /notexists/i}
be-mean-pokemons> var mod = {$set: {active: true}}
be-mean-pokemons> var options = {upsert: true}
be-mean-pokemons> db.pokemons.update(query, mod, options)
Updated 1 new record(s) in 1ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("aslk1290aslk2190aslk2190")
})
```

```bash
be-mean-pokemons> var query = {"_id": ObjectId("aslk1290aslk2190aslk2190")}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("aslk1290aslk2190aslk2190"),
  "active": true
}
```

Assim, quando não encontra o registro, ele cria um novo documento com os valores passados pelo operador `$set`. Mas temos um operador que pode ser usado no caso do `upsert` em que ele envia os valores a serem inseridos, caso o upsert for invocado, que é o [$setOnInsert]().

```bash
be-mean-pokemons> var query = {/notexists/i}
be-mean-pokemons> var mod = {
  $set: {active: true},
  $setOnInsert: {
    name: "Not Exist Pokemon",
    attack: null,
    defense: null,
	height: null,
	description: "Not exists"
  }
}
be-mean-pokemons> var options = {upsert: true}
be-mean-pokemons> db.pokemons.update(query, mod, options)
Updated 1 new record(s) in 1ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("cjals19aslk1291298asjlk12jkas8912")
})
```

```bash
be-mean-pokemons> var query = {"_id": ObjectId("cjals19aslk1291298asjlk12jkas8912")}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("cjals19aslk1291298asjlk12jkas8912"),
  "active": true,
  "name": "Not Exist Pokemon",
  "attack": null,
  "defense": null,
  "height": null,
  "description": "Not exists"
}
```

#### multi

Esse operador é onde definimos se queremos fazer uma atualização em massa ou não, seguindo o que foi definido na query. Segue um exemplo:

```bash
be-mean-pokemons> var query = {}
be-mean-pokemons> var mod = {$set: {active: true}}
be-mean-pokemons> var options = {multi: true}
be-mean-pokemons> db.pokemons.update(query, mod, options)

Updated 5 existing record(s) in 2ms
WriteResult({
	"nMatched": 5,
	"nUpserted": 0,
	"nModified": 5
})
```

Desse modo, como não especificamos nada na query, ele vai efetuar a atualização para todos os documentos da coleção. Como vemos ele fez a atualização de todos os cinco documentos armazenados.


#### writeConcern

 Foi citado do operador [writeConcern](https://docs.mongodb.org/manual/reference/write-concern/) que define um nivel de comprometimento com a escrita, sendo forte para não se preocupar com tempo de resposta menor, mas tendo mais segurança na persistência, ou mais fraca que define o contrário disso. Fica para um estudo mais aprofundado desse e de outros operadores.

### Operadores no find

Agora que abordamos boa parte dos operadores a serem usados no `update()`, vamos falar de alguns para o `find()` nessa aula.

#### Operador $in

Com o operador [$in](https://docs.mongodb.org/manual/reference/operator/query/in/), verificamos se determinado(s) valor(es) passados na query estão em um campo do tipo array. Dessa forma, ele vai trazer no resultado, documentos que batem com a consulta. O formato do operador é:

```bash
{field: {$in: [<value1>, <valueN>]}}
```

Seu comportamento é parecido com o operador `$or`. Por exemplo, vamos procurar alguns dos ataques nos pokemons armazenados:

```bash
be-mean-pokemons> var query = {moves: {$in: [/choque do trovão/i]}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "name": "Pikachu",
  "description": "Eletrico",
  "attack": 40,
  "defense": 20,
  "moves": ["Choque do Trovão"]
}
Fetched 1 record(s) in 0ms
```

#### Operador $nin

O operador [$nin](https://docs.mongodb.org/manual/reference/operator/query/nin/) faz o contrário que o `$in`, porque ele vai trazer documentos que não tenham os valores passados na query. Exemplo:


```bash
be-mean-pokemons> var query = {moves: {$nin: [/choque do trovão/i]}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "name": "Squirtle",
  "description": "Pokemon aquatico",
  "attack": 35,
  "defense": 15,
  "moves": ["Hidrobomba", "Investida"]
}
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "name": "Pokeaqua",
  "description": "Pokemon aquatico",
  "attack": 15,
  "defense": 10,
  "moves": ["Hidrobomba", "Jato de Agua"]
}
Fetched 2 record(s) in 0ms
```

#### Operador $all

O operador [$all](https://docs.mongodb.org/manual/reference/operator/query/all/) vai trazer os documentos que tiverem **todos** os itens em um campo do tipo array, conforme definido na query. Seu comportamento é parecido com o operador `$and`. Exemplo:

```bash
be-mean-pokemons> var query = {moves: {$all: [/choque do trovão/i, /investida/i]}}
be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("56489706ak219as219asasl129"),
  "name": "Squirtle",
  "description": "Pokemon aquatico",
  "attack": 35,
  "defense": 15,
  "moves": ["Hidrobomba", "Investida"]
}
Fetched 1 record(s) in 0ms
```

### Operadores de negação

Vamos explicar os operadores de negação usados nas pesquisas.

#### $ne - Not Equal

Esse operador [$ne](https://docs.mongodb.org/manual/reference/operator/query/ne/) vai trazer documentos em que determinado campo tenha um valor diferente do que foi passado na query. Segue o formato do operador:

```bash
{campo: {$ne: valor}}
```

Segue um exemplo:

```bash
be-mean-pokemons> var query = {speed: {$ne: 80}}
{
  "_id": ObjectId("564b1daf25337263280d048d"),
  "attack": 50,
  "created": "2013-11-03T15:05:41.388462",
  "defense": 40,
  "height": "6",
  "hp": 40,
  "name": "Poliwag",
  "speed": 90,
  "types": [
    "water"
  ]
}
Fetched 1 record(s) in 0ms
```

**Observação**: Esse operador não aceita regex. Ele mostra o seguinte erro:

```bash
Error: error: {
  "$err": "Can't canonicalize query: BadValue Can't have regex as arg to $ne.",
  "code": 17287
}
```

#### Operador $not

Como o próprio operador mostra, o [$not](https://docs.mongodb.org/manual/reference/operator/query/not/) vai trazer documentos em que determinado valor de um campo seja diferente do que foi passado na query.

### Usando o remove

A função `remove` serve para remover um documento de uma coleção. Para isso é só definir uma query, para que ele encontre os documentos que encaixe na busca, e depois as remove.

Exemplo:

```bash
be-mean-pokemons> var query = {name: /squirtle/i}
be-mean-pokemons> db.pokemons.remove(query)
WriteResult({
	"nRemoved": 1
})
```

**Cuidado**: A função remove vem com o operador `multi` como *true*, ao contrário do `update`.
