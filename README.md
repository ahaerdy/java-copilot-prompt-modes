# ☕ Modos do Copiloto para Devs Java (Ask, Edit, Plan, Agent e Study)

![dio/me](https://img.shields.io/badge/dio-me-ff2d55)
![Java](https://img.shields.io/badge/Java-21%20%7C%2025%20LTS-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x%20%7C%204.x-6DB33F)
![Frontend](https://img.shields.io/badge/Frontend-React%20%7C%20Angular-blue)
![Prompt](https://img.shields.io/badge/Prompt-engineering-yellow)

Este repositório reúne um **conjunto de prompts (instruções) para usar IA como copiloto** no desenvolvimento de novas features em projetos **Java**, com foco em **backend Spring Boot** e, quando o projeto pedir, **frontend em React ou Angular**.

O Copiloto oferece diferentes **modos de interação**: desde **tirar dúvidas sem mexer no código**, até **editar trechos específicos**, **planejar mudanças maiores** ou **delegar tarefas mais complexas** a um modo mais autônomo. A ideia é simples: você escolhe o modo que combina com o objetivo do momento e ganha velocidade **sem perder o controle**.

> 💡 Este material é uma adaptação do repositório de referência do curso **"Usando IA Como Copiloto para Criar Novas Features no Seu Projeto"** (DIO), reescrito para o ecossistema Java.

---

# 🎯 Para quem é

Para quem está se especializando em **Java no backend** e quer usar a IA como um **colega técnico que conhece o ecossistema** (Spring, JPA, testes, segurança) e que, em projetos mais profissionais, também se integra bem com um frontend **React** ou **Angular**.

---

# 🧱 Stack de referência

Os prompts assumem esta stack por padrão. Tudo é **editável** no bloco `STACK` de cada arquivo.

| Camada | Padrão assumido | Alternativas comuns |
|---|---|---|
| Linguagem | **Java 21** (LTS) | Java 25 (LTS), Java 17 |
| Framework | **Spring Boot** (Web MVC) | 3.x ou 4.x conforme o projeto |
| Build | **Maven** | Gradle |
| Persistência | **Spring Data JPA + Hibernate** | JDBC / jOOQ |
| Banco | **PostgreSQL** + **Flyway** | MySQL, Liquibase |
| Segurança | **Spring Security** (JWT / OAuth2 Resource Server) | Keycloak, sessão |
| Testes | **JUnit 5 + Mockito + AssertJ**, Spring Boot Test, **Testcontainers** | — |
| Docs da API | **OpenAPI (springdoc)** | — |
| Qualidade | Spotless/Checkstyle, JaCoCo, SonarLint | — |
| Infra | **Docker / docker-compose** | Kubernetes, cloud |
| Frontend (opcional) | **React + TypeScript** (Vite, TanStack Query) | **Angular** (standalone, signals, RxJS) |

> Spring Boot 4 mantém Java 17 como mínimo e é compatível com Java 25. Se o seu projeto ainda usa Spring Boot 3.x, **diga isso ao copiloto** — as diferenças (starters modulares, Jackson 3, etc.) mudam o código gerado.

---

# 🗺️ Visão geral dos modos

| Modo | Pergunta que responde | Altera código? | Prompt |
|---|---|---|---|
| ❓ **Ask** | "O que está acontecendo aqui?" | Não | [prompt-ask.md](prompts/prompt-ask.md) |
| ✏️ **Edit** | "Transforme este trecho assim." | Sim (cirúrgico) | [prompt-edit.md](prompts/prompt-edit.md) |
| 🧭 **Plan** | "Qual o melhor caminho antes de codar?" | Não (só plano) | [prompt-plan.md](prompts/prompt-plan.md) |
| 🤖 **Agent** | "Implemente essa feature de ponta a ponta." | Sim (múltiplos arquivos) | [prompt-agent.md](prompts/prompt-agent.md) |
| 📚 **Study** | "Me ensine isso de verdade." | Não (didático) | [prompt-study.md](prompts/prompt-study.md) |

---

# ❓ Ask

O modo **Ask** serve para **perguntar e entender, sem alterar seu código**. Você pode perguntar sobre uma classe, um bean que não sobe, uma *stack trace*, uma query lenta ou um conceito geral.

O Copiloto lê o contexto que você fornecer (trechos, logs, `pom.xml`, `application.yml`) e responde como um **mentor técnico**: diagnóstico curto, causa provável, como confirmar e opções. **Ele não modifica nada.**

**Exemplos em Java:**
- "Por que estou recebendo `LazyInitializationException` neste endpoint?"
- "Qual a diferença entre `@Transactional` no controller e no service?"
- "Esse `@OneToMany` pode estar causando N+1? Como confirmo?"

📄 **Prompt:** [prompts/prompt-ask.md](prompts/prompt-ask.md)

---

# ✏️ Edit

O modo **Edit** serve para **alterar código existente**. Você seleciona um trecho (ou arquivo), descreve a mudança e o Copiloto aplica de forma **mínima e controlada**, sem reescrever o que não foi pedido.

Ideal para:
- refactors (ex.: POJO/Lombok → `record`, field injection → injeção por construtor)
- modernizar código (streams, `Optional`, `switch` com pattern matching)
- tratamento de erros e validação (`@Valid`, `@RestControllerAdvice`)
- logs com SLF4J e melhorias de performance (ex.: resolver N+1)
- adicionar testes ou migrar `javax.*` → `jakarta.*`
- ajustes em componentes React/Angular, quando o arquivo for de frontend

Aqui o foco é: **"pegue isso que já existe e transforme"**.

📄 **Prompt:** [prompts/prompt-edit.md](prompts/prompt-edit.md)

---

# 🧭 Plan

Quando a tarefa é maior, o Copiloto entra em **planejamento**: ele **pensa e descreve os passos antes de sair codando**.

Ele:
- divide o problema em etapas pequenas e incrementais
- lista arquivos/camadas afetados (controller, service, repository, migration, testes)
- aponta riscos (segurança, transações, performance, compatibilidade)
- define como validar — e só depois, com a sua aprovação, parte para a implementação

Muito útil para **novas features**, **mudanças no modelo de dados** e para **combinar o contrato da API** antes de o frontend começar.

📄 **Prompt:** [prompts/prompt-plan.md](prompts/prompt-plan.md)

---

# 🤖 Agent

O **Agent** é o modo mais autônomo. Ele **cria e modifica vários arquivos**, mantém o contexto entre os passos e entrega a feature **organizada por camadas, com migration, testes e instruções de execução** — como um dev júnior trabalhando com você.

Você dá um objetivo (ex.: *"implemente CRUD de tarefas com paginação e autenticação JWT"*) e ele decide o que precisa ser feito: entidade, DTOs, repository, service, controller, tratamento de erros, testes e, se você pedir, a tela em React/Angular.

📄 **Prompt:** [prompts/prompt-agent.md](prompts/prompt-agent.md)

---

# 📚 Study

O modo **Study** é focado em **aprendizado ativo**, não só em chegar ao código final.

Em vez de só explicar ou executar, ele:
- ensina e guia o raciocínio (JVM, Collections, Streams, concorrência, Spring IoC, JPA…)
- destaca conceitos, armadilhas e *trade-offs*
- faz perguntas reflexivas e propõe mini-exercícios
- avança em progressão gradual de dificuldade

Funciona quase como um **tutor particular de Java**.

📄 **Prompt:** [prompts/prompt-study.md](prompts/prompt-study.md)

---

# 🔄 Fluxo recomendado para uma nova feature

Os modos funcionam melhor **combinados**. Um fluxo que costuma dar certo:

1. 📚 **Study** — se a feature envolve um conceito novo para você (ex.: Spring Security, transações).
2. ❓ **Ask** — entender o código existente onde a feature vai se encaixar.
3. 🧭 **Plan** — gerar o plano, **revisar e aprovar** (inclusive o contrato da API).
4. 🤖 **Agent** — implementar a partir do plano aprovado.
5. ✏️ **Edit** — ajustar pontos específicos depois da revisão.
6. ❓ **Ask** — pedir uma segunda opinião sobre o resultado (segurança, N+1, edge cases).

**Exemplo — feature "Tarefas com filtro e paginação":**

```text
[Plan]  Quero adicionar CRUD de tarefas (título, status, prazo) com filtro por status
        e paginação. Spring Boot + JPA + PostgreSQL. Gere o plano.

[Agent] Plano aprovado. Implemente o passo 1 e 2 (migration + entidade + repository),
        com testes de repository usando Testcontainers.

[Edit]  Neste controller, troque o retorno da entidade por um DTO (record) e adicione
        @Valid no corpo da requisição.

[Ask]   Revise esta classe e diga se há risco de N+1 ou de expor dados indevidos.
```

---

# 🔌 Backend Java + Frontend React ou Angular

Em projetos com frontend, os prompts seguem uma regra: **backend primeiro, contrato no meio, frontend depois**.

- **Contrato da API como fonte da verdade (API-first):** endpoints, DTOs, códigos de status e formato de erro são definidos no **Plan** e documentados em **OpenAPI**.
- **Erros padronizados:** o backend responde com `ProblemDetail` (RFC 9457), e o frontend trata esse formato em um único lugar.
- **Autenticação:** JWT/OAuth2 no backend; no frontend, interceptor HTTP (Angular) ou camada de cliente/`axios`/`fetch` (React).
- **CORS:** configurado explicitamente no backend, por ambiente — nunca `*` em produção.
- **Cliente tipado:** quando fizer sentido, gerar tipos/cliente TypeScript a partir do OpenAPI (ex.: `openapi-typescript` ou `openapi-generator`) em vez de digitar interfaces à mão.
- **Testes nos dois lados:** JUnit/MockMvc/Testcontainers no backend; Vitest + Testing Library (React) ou o runner padrão do Angular no frontend.

Se você não disser qual framework de frontend usa, o copiloto **declara a suposição** ou pergunta — e continua priorizando o backend.

---

# 🛠️ Como usar os prompts

1. **Abra o arquivo do modo** que você quer (`prompts/prompt-*.md`).
2. **Ajuste o bloco `STACK`** para refletir o seu projeto (versão do Java, Spring Boot, Maven/Gradle, banco, frontend).
3. **Cole o conteúdo como instruções** do seu assistente. Por exemplo, no VS Code com GitHub Copilot, como *custom instructions*, *prompt file* ou *chat mode/agent* (confira a documentação da sua ferramenta, pois os nomes e caminhos mudam com frequência).
4. **Dê contexto real** nas perguntas: versão do Java, versão do Spring Boot, *stack trace* completa (incluindo `Caused by`), trechos de código e `application.yml` (**sem segredos**).
5. **Revise tudo antes de aceitar.** Rode os testes, o build e leia o diff.

---

# ✏️ Como personalizar

Em cada prompt existem blocos marcados como **(EDITÁVEL)**:

- **STACK** → troque Maven por Gradle, Java 21 por 25, PostgreSQL por MySQL, ou defina `React`/`Angular`.
- **PERSONALIDADE** → mantenha o estilo "Cortana-like" do curso ou troque por outro tom (mais formal, mais didático, mais seco…).
- **FRAMEWORK** → se usar **Quarkus** ou **Micronaut** em vez de Spring, troque o bloco de stack e as convenções (anotações, testes, config).

---

# ✅ Checklist ao usar IA como copiloto

- [ ] Entendi o que foi gerado? (se não, use o **Study**)
- [ ] Rodei `./mvnw verify` (ou `./gradlew build`) e os testes passaram?
- [ ] Nenhuma entidade JPA está exposta diretamente na API?
- [ ] Inputs validados e erros tratados de forma padronizada?
- [ ] Nada de segredo, senha ou token no código ou no prompt?
- [ ] Autorização conferida (não só autenticação)?
- [ ] Migration versionada e reversível/segura?
- [ ] Nenhuma dependência nova foi adicionada sem eu perceber?

---

# 🧠 Resumo mental rápido

- **Ask** → entender  
- **Plan** → planejar antes de agir  
- **Edit** → mudar um trecho com precisão  
- **Agent** → executar tarefas grandes de ponta a ponta  
- **Study** → aprender de verdade  

---

# 📁 Estrutura do repositório

```text
.
├── readme.md
└── prompts/
    ├── prompt-ask.md
    ├── prompt-edit.md
    ├── prompt-plan.md
    ├── prompt-agent.md
    └── prompt-study.md
```
