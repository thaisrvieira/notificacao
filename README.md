# Microsserviço de Notificação

Microsserviço responsável pelo envio de notificações por e-mail para os usuários do sistema de agendamento de tarefas, com conteúdo gerado dinamicamente a partir de templates Thymeleaf.

## 📌 Sobre o projeto

Este serviço faz parte de uma aplicação distribuída em arquitetura de microsserviços, composta por:

- [**BFF**](https://github.com/thaisrvieira/bff-agendador-tarefas) — orquestra as chamadas para os demais serviços
- [**Usuário**](https://github.com/thaisrvieira/usuario) — gerenciamento de usuários, autenticação e endereços
- [**Agendador de Tarefas**](https://github.com/thaisrvieira/agendador-tarefas) — CRUD de tarefas agendadas
- **Notificação** (este repositório) — envio de e-mails de notificação

## 🚀 Tecnologias utilizadas

- **Java 17**
- **Spring Boot 4** / Spring Framework 7
- **Spring Mail** — envio de e-mails via SMTP (Jakarta Mail)
- **Thymeleaf** — templates dinâmicos de e-mail
- **Gradle** — gerenciamento de dependências e build
- **Docker** (build multi-stage com Gradle + Eclipse Temurin 17)
- **Lombok**

## 🏗️ Arquitetura

Estrutura em camadas:

```
controller/                  → Endpoint REST de disparo de e-mail
business/
  EmailService                → Regra de negócio de montagem e envio do e-mail
  dto/TarefasDTO               → Dados da tarefa recebidos para notificação
  enums/StatusNotificacaoEnum   → Status da notificação (PENDENTE, NOTIFICADO, CANCELADO)
infrastructure/
  exceptions/EmailException     → Exceção customizada para falhas no envio
```

## 📋 Funcionalidades

- Recebimento dos dados de uma tarefa (`nomeTarefa`, `descricao`, `dataEvento`, `emailUsuario`, entre outros) via `POST /email`
- Geração do corpo do e-mail a partir do template Thymeleaf `notificacao`, com nome da tarefa, data do evento e descrição
- Envio do e-mail via SMTP, com remetente e nome do remetente configuráveis por propriedades (`envio.email.remetente`, `envio.email.nomeRemetente`)
- Tratamento de erros de envio através de exceção customizada (`EmailException`)

## 🔌 Endpoint

| Método | Rota   | Descrição                                  |
|--------|--------|---------------------------------------------|
| POST   | /email | Recebe os dados de uma tarefa e dispara o e-mail de notificação correspondente |

## ⚙️ Como executar

### Pré-requisitos
- Java 17
- Gradle
- Uma conta SMTP configurada (ex: Gmail com senha de aplicativo) nas propriedades `spring.mail.*`, `envio.email.remetente` e `envio.email.nomeRemetente`

### Rodando localmente

```bash
./gradlew clean build
./gradlew bootRun
```

### Rodando com Docker

O projeto já possui um `Dockerfile` com build multi-stage (Gradle + Eclipse Temurin 17), expondo a porta **8082**:

```bash
docker build -t notificacao .
docker run -p 8082:8082 notificacao
```

Ou, via Docker Compose (junto com os demais microsserviços):

```bash
docker compose up --build
```

## 👩‍💻 Autora

**Thaís Rodrigues Vieira**
[LinkedIn](https://www.linkedin.com/in/thais-vieira-8471523a2/) | [GitHub](https://github.com/thaisrvieira)
