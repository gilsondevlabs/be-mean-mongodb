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
 - [Aula 02]()

Nessa aula foi falado sobre os comandos de acessar database, listar os bancos disponíveiso

O que foi falado na aula?

 - Iniciamos, com a instrução de envio dos exercícios para o repositório do `Webshool-io/be-mean-instagram`, por meio do Pull Request.
 - Foi explicado o uso do comando `use <database_name>` que acessa o banco escolhido. Dessa forma fica assim:

 ```bash
use be-mean-instagram
switched to db be-mean-instagram
 ```

 - Foi mostrado o comando `show dbs` que lista os databases criados localmente. Exemplo:

```bash
show dbs
be-mean  0.078GB
local    0.078GB
```

 - Como foi visto na aula, não foi exibido o banco `be-mean-instagram` porque ele só sera alocado, quando é inserido algo nele. Veja:

```bash
use be-mean-instagram
switched to db be-mean-instagram

db.teste.insert({nome: "Suissa", idade: 30})
Inserted 1 record(s) in 1854ms
WriteResult({
	"nInserted": 1
})

show dbs
be-mean            0.078GB
be-mean-instagram  0.078GB
local              0.078GB
```

Agora ele mostra o banco porque inserimos um documento nele.



#### Links

 - [Video da Aula](https://youtu.be/PaNVk0V2UNI)
 - [Exercicio da Aula]()
 - [Exercicio Resolvido]()
