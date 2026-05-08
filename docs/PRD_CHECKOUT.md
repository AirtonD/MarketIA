# PRD - Checkout

**Projeto:** MarketIA  
**Versao:** 1.0  
**Data:** 2026-05-07

## 1. Objetivo

Definir o processo de checkout do MarketIA para transformar um item selecionado em um pedido confirmado, com foco em simplicidade, baixa friccao e execucao rapida.

O checkout deve:

- Registrar os dados basicos do comprador.
- Confirmar quantidade, preco unitario e subtotal.
- Permitir uma forma de pagamento simples, com prioridade para PIX.
- Gerar um pedido com status inicial apropriado para acompanhamento.

Este PRD segue o ADR do projeto, que prioriza um e-commerce minimalista, monolito modular, baixa complexidade operacional e implementacao enxuta.

## 2. Contexto Funcional

O fluxo base do produto parte da listagem de produtos, passa pela selecao de um item e evolui para o carrinho e checkout.

O checkout neste MVP nao deve incluir:

- Marketplace multi-vendedor.
- Cupons de desconto.
- Frete calculado por transportadora.
- Cadastro obrigatorio de conta.
- Integração complexa com adquirentes.
- Recuperacao de carrinho abandonado.

## 3. Escopo do Checkout

### Incluido

- Coleta de dados do usuario.
- Revisao do pedido.
- Escolha de forma de pagamento.
- Geracao do pedido.
- Confirmacao visual do status da operacao.

### Nao incluido

- Login e autenticação.
- Endereco de entrega detalhado, caso o negocio nao exija entrega fisica no MVP.
- Parcelamento.
- Pagamento com cartao de credito.
- Assincronia complexa de conciliacao financeira.

## 4. Personas

### 4.1 Cliente final

Pessoa que deseja comprar rapidamente um produto disponivel.

Necessidades:

- Concluir a compra sem passos desnecessarios.
- Pagar de forma simples e conhecida.
- Entender claramente valor total e status do pedido.

### 4.2 Administrador da operacao

Pessoa responsavel por acompanhar pedidos e validar a operacao.

Necessidades:

- Visualizar pedidos criados.
- Identificar pedidos pagos, pendentes ou cancelados.
- Ter dados suficientes para atendimento e conferência.

### 4.3 Suporte interno

Pessoa que auxilia o cliente em caso de erro, duvida ou falha de pagamento.

Necessidades:

- Consultar o pedido pelo identificador.
- Identificar forma de pagamento.
- Ver historico minimo de status.

## 5. Regras Basicas de Pagamento

### 5.1 PIX como prioridade

- O checkout deve suportar PIX como forma principal de pagamento.
- O sistema deve exibir a chave PIX ou o meio de copia apropriado, se aplicavel.
- O pagamento via PIX deve ser tratado como pendente ate confirmacao.

### 5.2 Status de pagamento

O pedido deve iniciar com status coerente com a etapa de pagamento, por exemplo:

- `PENDENTE_PAGAMENTO`
- `PAGO`
- `CANCELADO`
- `EXPIRADO`

### 5.3 Validade do pagamento

- O pagamento PIX deve possuir prazo de validade definido.
- Expirado o prazo, o pedido pode ser marcado como `EXPIRADO` ou retornar a estado equivalente de nao confirmado.

### 5.4 Confirmacao

- A confirmacao de pagamento deve atualizar o pedido para status pago.
- A interface deve refletir claramente quando o pagamento ainda nao foi confirmado.

### 5.5 Valores

- O valor final deve ser calculado com base em:
  - preco unitario
  - quantidade
  - subtotal
- Nao devem existir alteracoes de valor sem registro previo.

### 5.6 Integridade

- Nao permitir pagamento de produto indisponivel.
- Nao permitir fechamento de pedido com quantidade invalida.
- O pedido deve registrar o preco do momento da compra.

## 6. Regras de Negocio

1. Um pedido deve estar associado a um usuario e a um produto.
2. A quantidade minima permitida e 1.
3. O produto deve estar disponivel para compra no momento do checkout.
4. O subtotal deve ser calculado como `quantidade x precoUnitario`.
5. O checkout deve registrar data de criacao do pedido.
6. O sistema deve manter rastreabilidade minima do estado do pedido.

## 7. Dados Minimos do Checkout

### Usuario

- nome
- email
- telefone
- cidade

### Pedido

- usuarioId
- produtoId
- quantidade
- precoUnitario
- subtotal
- status
- criadoEm

## 8. Fluxo Esperado

1. O usuario seleciona um produto.
2. O produto entra no carrinho.
3. O usuario revisa quantidade e valor.
4. O usuario informa seus dados basicos.
5. O usuario escolhe PIX como forma de pagamento.
6. O pedido e criado com status pendente.
7. A confirmacao de pagamento atualiza o status.

## 9. Critérios de Aceitação

- O usuario consegue concluir o checkout em um fluxo curto.
- O sistema registra pedido com status inicial correto.
- O valor total e exibido antes da confirmacao final.
- O checkout suporta PIX como forma de pagamento basica.
- O pedido nao pode ser criado com dados obrigatorios ausentes.
- O pedido nao pode ser criado para produto indisponivel.

## 10. Observacoes de Implementacao

Em linha com o ADR do projeto:

- manter o fluxo simples e previsivel;
- evitar integracoes financeiras complexas no MVP;
- usar validacao de entrada para proteger os dados do checkout;
- persistir pedidos de forma consistente para consulta posterior.
