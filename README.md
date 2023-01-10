# Comandos Knex.js
- Os comandos utilizados são visando uma tabela de games que possui dois campos, nome e preco. 
## Insert
- Inserir dados dentro de uma tabela.
```
const game = await database.insert({nome: 'Sea of thieves', preco: 50.67}).into("games")
```
### Resposta da promise
- Retorna o id que foi inserido dentro da tabela. isso também significa que foi inserido com sucesso.

## Select
- Buscar todas os dados dentro da tabela, ou então buscar dados sobre um campo especifico
```
const game = await database.select().table("games")
console.log(game)
```
### Resposta da promise
- Retorna todos os dados da tabela
```
[
  { id: 1, nome: 'Sea of thieves', preco: 50.67 },
  { id: 3, nome: 'GTA v', preco: 80.79 },
  { id: 4, nome: 'Call of duty 2', preco: 249.99 }
]
```
### Observações 
```
database.select(["id", "preco"]) - Pode utilizar dessa forma para pesquisar somente id e preco da tabela, esse é somente um exemplo.
```
## Where
- Procurar uma linha na tabela por um valor especifico dentro de um determinado campo. No exemplo abaixo estou procurando o campo ID de numero 6, referente a tabela games.
```
const game = await database.select().where("id", 6).table("games")
```
### Resposta da promise
```
[ { id: 6, nome: 'Rainbow six - Siege', preco: 120.99 } ]
```
## Delete
- Antes de excluir uma linha é necessario lozalizar a mesma, por esse motivo antes da função delete utilizamos um where, para localizar a linha que desejamos deletar.
```
await database.where({id: 2}).delete().table("games")
```
### Resposta da promise
- O retorno pode ser 1 ou 0. 1 significa que a linha foi excluida, 0 significa que o comando não foi executado com sucesso.
## Update
- Utilizado para fazer a alteração de um determinado campo, por esse motivo antes da função dele utilizamos um where, para localizar o campo que desejamos alterar.
```
const game = await database.where("id", 1).update({preco: 60}).table("games")
```
### Resposta da promise
- O retorno pode ser 1 ou 0. 1 significa que a linha foi alterada, 0 significa que o comando não foi executado com sucesso.
## OrderBy
- Utilizado para ordenar os dados que estamos requisitando. OrderBy exige dois parametros, a coluna que desejamos ordernar e o tipo, desc = 'Decrescente' ou asc = 'crescente', conforme mostra o exemplo abaixo:
```
const games = await database.table("games").orderBy('nome', "asc")
```
### Resposta da promise
- Como podemos ver o array retornado está em ordem alfabetica pelo nome.
```
[
  { id: 4, nome: 'Call of duty 2', preco: 249.99 },
  { id: 7, nome: 'Gta IV', preco: 320 },
  { id: 8, nome: 'Gta IV', preco: 320 },
  { id: 6, nome: 'Rainbow six - Siege', preco: 120.99 },
  { id: 1, nome: 'Sea of thieves', preco: 60 },
  { id: 5, nome: 'WOW', preco: 60 }
]
```
## Join
- Antes de entrarmos para mostrar o codigo vou explicar algumas coisas, o join ele só é utilizado para tabelas que possui relacionamento, ou seja, precisar existe uma foreign key que liga uma tabela a outra. Nesse caso eu criei uma tabela de estudios como exemplo, ela tem uma foreign key que é game_id que é relacionado com o campo id da tabela games.
```
const games = await database.select(["games.nome", "estudios.nome as estudio_nome"]).table("games").innerJoin("estudios", "estudios.game_id", "games.id")
```
### Resposta da promise
```
[
  { nome: 'Rainbow six - Siege', estudio_nome: 'Ubisoft' },
  { nome: 'Rainbow six - Siege', estudio_nome: 'Activision' },
  { nome: 'Rainbow six - Siege', estudio_nome: 'Blizzard' }
]

```
### Observações
- as é utilizado para renomear o nome do campo no selecet, ou seja, o campo nome da tabela estudios.nome irá ficar estudio_nome. Isso é feito somente pois os campos nome é igual na tabela games e estudios, então irá dar erro no retorno da query.
