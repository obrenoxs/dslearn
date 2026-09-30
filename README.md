# DSLearn

Plataforma de ensino que gerencia cursos, turmas, alunos e um fórum de perguntas e respostas sobre os conteúdos. Projeto desenvolvido em Java com Spring Boot, como parte do curso **Java Spring Reference** da [DevSuperior](https://devsuperior.com.br).

> 🚧 **Status:** em desenvolvimento.

## Visão geral

O sistema mantém informações de cursos, suas turmas e alunos, além de um fórum de perguntas e respostas.

- Os atores são **alunos**, **professores** e **administradores**. Somente administradores cadastram cursos e turmas.
- Um **curso** é composto por vários **recursos** (trilhas de aprendizado, bônus, links externos e o fórum do curso).
- Cada recurso contém **seções**, e cada seção contém **aulas**, que podem ser conteúdos (vídeo e/ou texto) ou **tarefas** entregues pelos alunos.
- Uma tarefa tem peso, data de entrega, número de questões e quantidade mínima de acertos. Depois de entregue, aguarda o feedback do professor e pode ser aceita ou rejeitada.
- Cada turma de um curso é uma **oferta** (edição), com data de início e fim.
- Alunos e professores recebem **notificações**.

### Requisitos do fórum (planejados)

- Listar tópicos, com filtro por recurso/seção/aula, por texto (título e/ou corpo) e por perguntas do usuário logado
- Criar tópico (título e corpo) e responder tópico
- Marcar/desmarcar upvote em pergunta e em resposta (não pode ser o autor)
- Marcar/desmarcar melhor resposta (somente o autor do tópico e o instrutor)

## Modelo de domínio

![Modelo conceitual do DSLearn](docs/modelo-conceitual.png)

| Entidade | Descrição |
|---|---|
| `User` / `Role` | Usuários e seus perfis (`ROLE_STUDENT`, `ROLE_INSTRUCTOR`, `ROLE_ADMIN`) |
| `Course` | Curso |
| `Offer` | Oferta (edição/turma) de um curso |
| `Resource` | Grupo de conteúdos de uma oferta (trilha, fórum, etc.) |
| `Section` | Seção de um recurso, com auto-relacionamento de pré-requisito |
| `Lesson` | Aula (classe abstrata), com as subclasses `Content` e `Task` |
| `Enrollment` | Matrícula de um usuário em uma oferta (chave primária composta `EnrollmentPK`) |
| `Deliver` | Entrega de uma tarefa por um aluno matriculado |
| `Notification` | Notificação enviada a um usuário |
| `Topic` / `Reply` | Tópicos do fórum e suas respostas, ambos com curtidas |

Pontos de modelagem que valem destaque:

- **Herança `JOINED`** entre `Lesson`, `Content` e `Task`
- **Chave primária composta** em `Enrollment`, referenciada por `Deliver` e pela tabela `tb_lessons_done`
- **Auto-relacionamento** em `Section` (pré-requisito)
- **Relacionamentos `@ManyToMany`** para roles, aulas concluídas e curtidas em tópicos e respostas

## Tecnologias

- Java 17
- Spring Boot 3.1.1
- Spring Web
- Spring Data JPA / Hibernate
- H2 Database (perfil de teste)
- Maven

## Como executar

**Pré-requisitos:** JDK 17 e Maven (ou o wrapper do projeto, se houver).

```bash
# clonar o repositório
git clone https://github.com/obrenoxs/dslearn.git
cd dslearn

# executar a aplicação
mvn spring-boot:run
```

A aplicação sobe com o perfil `test`, usando o H2 em memória e carregando o `import.sql` automaticamente.

### Console do H2

Com a aplicação rodando, acesse `http://localhost:8080/h2-console` e use:

| Campo | Valor |
|---|---|
| JDBC URL | `jdbc:h2:mem:testdb` |
| User Name | `sa` |
| Password | *(vazio)* |

## Estrutura do projeto

```
src/main/java/com/devsuperior/dslearn
├── DslearnApplication.java
└── entities
    ├── enums        # ResourceType, DeliverStatus
    └── pk           # EnrollmentPK
src/main/resources
├── application.properties
├── application-test.properties
└── import.sql       # seed de dados
```
## Autor

**Breno Oliveira de Souza**

LinkedIn: [Breno Souza](www.linkedin.com/in/breno-oliveira-souza)