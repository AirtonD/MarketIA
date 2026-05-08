# Backlog

**Projeto:** MarketIA  
**Versao:** 1.0  
**Data:** 2026-05-07

## Checkout

### HU01 - Revisar carrinho

Como usuario, quero revisar os detalhes do carrinho para confirmar os itens, quantidades e valores antes de continuar.

#### Criterios de aceitacao - BDD

```gherkin
Funcionalidade: Revisar carrinho

  Cenario: Exibir os detalhes do carrinho antes da confirmacao
    Dado que o usuario adicionou um produto ao carrinho
    Quando ele acessa a tela de revisao do checkout
    Entao o sistema deve exibir nome do produto, quantidade, preco unitario e subtotal
    E o sistema deve mostrar o valor total do pedido

  Cenario: Impedir continuidade com carrinho vazio
    Dado que o carrinho esta vazio
    Quando o usuario tenta acessar o checkout
    Entao o sistema deve impedir a continuidade
    E deve informar que nao ha itens para revisar
```

### HU02 - Informar dados basicos

Como usuario, quero informar meus dados basicos no checkout para que o pedido possa ser registrado corretamente.

### HU03 - Escolher forma de pagamento

Como usuario, quero escolher PIX como forma de pagamento para concluir a compra de maneira simples.

#### Criterios de aceitacao - BDD

```gherkin
Funcionalidade: Escolher forma de pagamento PIX

  Cenario: Selecionar PIX como forma de pagamento
    Dado que o usuario esta na etapa de pagamento
    Quando ele seleciona a opcao PIX
    Entao o sistema deve registrar PIX como forma de pagamento
    E deve exibir as instrucoes para pagamento

  Cenario: Manter pedido pendente ate confirmacao
    Dado que o usuario concluiu o pedido com PIX
    Quando o pagamento ainda nao foi confirmado
    Entao o pedido deve permanecer com status pendente
    E o sistema nao deve marcar o pedido como pago
```

### HU04 - Confirmar o pedido

Como usuario, quero confirmar o pedido antes de finalizar para evitar compras com dados ou valores incorretos.

### HU05 - Acompanhar o status do pagamento

Como usuario, quero ver o status do pagamento do pedido para saber se ele esta pendente, pago ou expirado.
