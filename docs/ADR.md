# ADR 001 - Arquitetura e Tecnologias para E-commerce Minimalista

**Status:** Aceito  
**Data:** 2026-05-05  
**Projeto:** MarketIA

## Contexto

O produto será um e-commerce minimalista com as seguintes características:

- Apenas uma tela inicial.
- Listagem fixa ou pouco volátil de 12 produtos.
- Busca simples na própria listagem.
- Implementação assistida por CODEX.
- Escopo enxuto, sem necessidade inicial de carrinho, checkout, autenticação ou integrações complexas.

Nesse cenário, a prioridade é reduzir complexidade técnica, acelerar entrega e manter a base de código fácil de evoluir.

## Decisão

Adotar uma **arquitetura monolítica modular** com **Node.js** como plataforma principal.

### Stack recomendada

- **Runtime:** Node.js LTS
- **Linguagem:** TypeScript
- **Framework web:** Fastify
- **Renderização:** Server-Side Rendering com templates simples ou HTML estático com dados injetados no backend
- **Banco de dados:** SQLite
- **ORM:** Prisma
- **Validação:** Zod
- **Testes:** Vitest
- **Lint/Format:** ESLint + Prettier
- **Deploy:** container único com Docker

## Justificativa

### Por que monólito

- O domínio é pequeno e não justifica microserviços.
- Reduz custo operacional e esforço de infraestrutura.
- Facilita o trabalho do CODEX, com menos pontos de integração e menos ambiguidade arquitetural.
- Simplifica observabilidade, deploy e manutenção.

### Por que Node.js

- Ecossistema maduro para aplicações web leves.
- Boa produtividade para uma equipe pequena ou fluxo automatizado por IA.
- Compartilhamento de linguagem entre backend e frontend, se houver scripts ou interações leves no cliente.
- Ótimo ajuste para uma aplicação com baixa complexidade de domínio e baixa concorrência inicial.

### Por que TypeScript

- Reduz erros em tempo de execução.
- Melhora a legibilidade do código gerado por CODEX.
- Facilita refatorações futuras caso o escopo cresça.

### Por que Fastify

- Leve, rápido e com boa organização para APIs e rotas web.
- Menor sobrecarga que frameworks mais opinativos.
- Adequado para um monólito pequeno com necessidade de simplicidade.

### Por que SQLite

- Suficiente para 12 produtos e busca simples.
- Baixo custo operacional.
- Elimina a necessidade de gerenciar um banco servidor separado no início.
- Pode ser substituído depois por PostgreSQL sem reescrever a aplicação inteira, desde que o acesso a dados esteja isolado.

### Por que Prisma

- Modelagem clara dos dados.
- Migrações simples.
- Boa produtividade com TypeScript.
- Facilita a evolução do banco se o projeto crescer.

## Diretrizes de Implementação

1. Organizar o monólito por camadas ou módulos de negócio, não por tipo de arquivo apenas.
2. Separar claramente:
   - `routes`
   - `controllers`
   - `services`
   - `repositories`
   - `schemas`
3. Manter a busca no backend como query simples por nome, categoria ou descrição.
4. Evitar overengineering:
   - sem microserviços
   - sem mensageria
   - sem cache distribuído
   - sem SSR complexo se não houver necessidade
5. Preferir código legível e previsível em vez de abstrações genéricas demais.
6. Criar testes básicos para:
   - listagem de produtos
   - busca
   - validação de entrada
7. Registrar a configuração via variáveis de ambiente, mesmo para um projeto pequeno.

## Estrutura Sugerida

```text
src/
  app.ts
  server.ts
  modules/
    products/
      products.routes.ts
      products.controller.ts
      products.service.ts
      products.repository.ts
      products.schema.ts
      products.test.ts
  shared/
    db/
    errors/
    utils/
```

## Consequências

### Positivas

- Menor tempo de desenvolvimento.
- Menor custo de operação.
- Menor complexidade para o CODEX gerar e manter o código.
- Facilidade de evoluir para novas telas e funcionalidades futuras.

### Negativas

- Escalabilidade horizontal menos flexível do que em uma arquitetura distribuída.
- SQLite pode se tornar limitador caso o volume ou a concorrência aumentem muito.
- SSR simples pode exigir ajuste caso o front evolua bastante.

## Alternativas Consideradas

### Microserviços

Rejeitado por excesso de complexidade para o escopo atual.

### Frontend SPA com backend separado

Rejeitado porque adiciona mais decisões e mais superfície de integração sem ganho real para uma única tela.

### Banco em memória ou JSON puro

Possível para protótipo, mas menos adequado se houver necessidade de persistência confiável e evolução incremental.

## Recomendação Final

Para este projeto, a melhor escolha é:

- **Arquitetura:** monólito modular
- **Backend:** Node.js + TypeScript + Fastify
- **Persistência:** SQLite + Prisma
- **Validação:** Zod
- **Testes:** Vitest
- **Entrega:** aplicação única em Docker

Essa combinação entrega o menor custo de implementação e manutenção, com boa base para crescimento futuro sem complicar o MVP.
