# Prova de Banco de Dados: MongoDB + Docker

Este repositório contém a resolução da prova prática de banco de dados NoSQL, utilizando Docker para orquestração do ambiente e MongoDB para persistência e manipulação de dados.

---

## 🚀 1. Configuração do Ambiente (Docker)

Comandos utilizados para preparar o ambiente e acessar o shell do banco de dados:

```bash
# Baixando a imagem oficial do MongoDB v7
docker pull mongo:7.0

# Criando e iniciando o contêiner com porta mapeada (27018) e volume para persistência
docker run -d -p 27018:27017 --name jones_container -v mongo_dados:/data/db mongo:7

# Acessando o terminal interativo do MongoDB (mongosh)
docker exec -it jones_container mongosh


(MONGOSH)

// Selecionando o banco de dados
use escola

// Inserindo os alunos para a prova
db.alunos.insertMany([
  {
    nome: "Jones",
    idade: 20,
    curso: "ENG.SOFT.",
    notas:[7, 8, 9],
    endereco: { cidade: "Maricá", estado: "RJ" }
  },
  {
    nome: "João Silva",
    idade: 20,
    curso: "ADS",
    notas:[6, 7, 5],
    endereco: { cidade: "Maricá", estado: "RJ" }
  },
  {
    nome: "Maria",
    idade: 18,
    curso: "Medicina",
    notas:[10, 8, 9],
    endereco: { cidade: "Itaboraí", estado: "RJ" }
  },
  {
    nome: "Lúcia",
    idade: 19,
    curso: "Enfermagem",
    notas:[9, 9, 6],
    endereco: { cidade: "Maricá", estado: "RJ" }
  },
  {
    nome: "Diego",
    idade: 30,
    curso: "Medicina",
    notas:[5, 10, 5],
    endereco: { cidade: "Maricá", estado: "RJ" }
  }
])

// Buscar alunos
db.alunos.find()

// Buscar alunos por curso
db.alunos.find({ curso: "ADS" })

// Buscar alunos com idade maior que 21
db.alunos.find({ idade: { $gt: 21 } })

// Atualizar a idade de um aluno
db.alunos.updateOne({ nome: "Jones" }, { $set: { idade: 19 } })

// Adicionar uma nova nota a um aluno
db.alunos.updateOne({ nome: "Diego" }, { $push: { notas: 10 } })

// Remover um aluno
db.alunos.deleteOne({ nome: "Lúcia" })

// Média de notas por aluno
db.alunos.aggregate([
  {
    $project: {
      nome: 1,
      media: { $avg: "$notas" }
    }
  }
])

// Quantidade de alunos por curso
db.alunos.aggregate([
  {
    $group: {
      _id: "$curso",
      total_alunos: { $sum: 1 }
    }
  }
])
