

## 1. Visão geral do projeto 🧩

O sistema é uma loja virtual baseada em  **OpenCart**  (e-commerce), que permite a clientes navegarem pelo catálogo de produtos, adicioná-los ao carrinho e concluir compras utilizando diferentes métodos de pagamento e envio.

O projeto tem como objetivo disponibilizar um  **fluxo completo de compra online**, desde a busca e seleção de produtos até a finalização do pedido, contemplando:

-   Catálogo com categorias e produtos.
-   Carrinho de compras.
-   Cadastro e autenticação de clientes.
-   Checkout com opções de entrega e pagamento.
-   Registro e acompanhamento de pedidos.

Nesta entrega específica, o foco é garantir que o  **checkout para clientes já cadastrados**  funcione corretamente, permitindo que um cliente existente conclua uma compra com sucesso.

----------

## 2. Contexto do negócio 🏪

A empresa é uma organização de varejo que deseja vender produtos online através de um canal digital unificado. O canal web (loja online):

-   Precisa suportar clientes que:
    -   Já possuem conta (clientes recorrentes).
    -   Acessam o site, escolhem produtos e finalizam pedidos.
-   Deve proporcionar uma experiência simples e confiável, reduzindo:
    -   Abandono de carrinho.
    -   Erros no fluxo de compra.
-   Deve garantir:
    -   Registro consistente de pedidos.
    -   Informações corretas de entrega e cobrança.
    -   Integração com meios de pagamento.

O fluxo de checkout é  **ponto crítico de geração de receita**  e exige alta confiabilidade, pois qualquer falha pode resultar em perda de vendas e reclamações de clientes.

Neste contexto, a funcionalidade alvo é o  **processo de checkout de um cliente já cadastrado e autenticado**, encerrando com a criação do pedido no sistema.

----------

## 3. Escopo da funcionalidade 🎯

### Escopo incluído

A funcionalidade coberta nesta documentação é:

> **Fluxo de checkout para cliente já cadastrado (customer existente) que realiza uma compra e conclui o pedido com sucesso.**

Inclui:

-   Seleção de produtos e adição ao carrinho.
-   Acesso ao checkout com cliente autenticado.
-   Preenchimento/seleção de endereço de entrega.
-   Seleção do método de envio (shipping).
-   Seleção do método de pagamento (payment).
-   Confirmação do pedido.
-   Criação do registro de pedido (order) no sistema.

### Fora de escopo (para este recorte)

-   Cadastro de novo cliente.
-   Checkout como convidado (guest checkout), se disponível.
-   Fluxos de cancelamento de pagamento, reembolso ou estorno.
-   Fluxos administrativos (gestão de pedidos no backoffice).
-   Promoções, cupons e programas de fidelidade.
-   Integrações externas reais com gateways de pagamento (considerar simuladas no demo).

----------

## 4. Histórias de Usuário 👤

### HU-001 – Finalizar compra como cliente cadastrado

Como  
**Cliente já cadastrado na loja (autenticado)**  
Quero  
**realizar o checkout dos produtos que selecionei no carrinho**  
Para  
**concluir minha compra e receber os produtos no endereço desejado.**

----------

## 5. Critérios de Aceitação ✅

> Observação: critério de aceitação aqui é usado no sentido de “condição que precisa ser verdadeira para que a história seja considerada entregue”, não como roteiro de teste.

### CA-001 – Acesso ao checkout com carrinho preenchido

-   Dado que o cliente esteja autenticado
-   E possua ao menos um produto no carrinho
-   Então o sistema deve permitir o acesso ao fluxo de checkout.

### CA-002 – Endereço de entrega válido

-   Dado que o cliente esteja no fluxo de checkout
-   Quando informar (ou selecionar) um endereço de entrega com todos os campos obrigatórios válidos
-   Então o sistema deve aceitar o endereço de entrega e permitir a continuidade do fluxo.

### CA-003 – Seleção de método de envio

-   Dado que um endereço de entrega válido tenha sido informado
-   Quando houver ao menos um método de envio disponível para aquele endereço
-   Então o sistema deve exibir as opções de envio disponíveis
-   E permitir que o cliente selecione um método de envio.

### CA-004 – Seleção de método de pagamento

-   Dado que um método de envio tenha sido selecionado
-   Quando houver ao menos um método de pagamento disponível
-   Então o sistema deve exibir as opções de pagamento disponíveis
-   E permitir que o cliente selecione um método de pagamento.

### CA-005 – Resumo do pedido antes da confirmação

-   Dado que o cliente tenha selecionado método de envio e pagamento
-   Então o sistema deve exibir um resumo do pedido contendo:
    -   Lista de produtos (nome, quantidade, preço unitário e subtotal).
    -   Custos de envio (quando aplicável).
    -   Total final do pedido.
    -   Endereço de entrega.
    -   Método de pagamento e envio selecionados.

### CA-006 – Confirmação e criação do pedido

-   Dado que o resumo do pedido esteja visível
-   Quando o cliente confirmar o pedido
-   Então o sistema deve:
    -   Registrar um novo pedido associado ao cliente.
    -   Exibir ao cliente uma confirmação contendo uma identificação única do pedido.
    -   Manter o histórico de pedidos disponível para consulta posterior.

### CA-007 – Persistência de dados coerente

-   Dado que um pedido tenha sido criado
-   Então as informações de produtos, preços, endereço, método de envio e pagamento associados ao pedido devem refletir o que foi apresentado no resumo de confirmação.

----------

## 6. Regras de Negócio 📏

### RN-001 – Cliente autenticado

-   Apenas clientes autenticados podem realizar o fluxo de checkout definido nesta história.
-   Se o cliente não estiver autenticado, o sistema deve direcionar para fluxo de login (ou impedir avanço no checkout) antes de prosseguir com a finalização da compra.

### RN-002 – Carrinho com itens obrigatórios

-   O checkout só pode ocorrer se o carrinho possuir pelo menos um item.
-   Se o carrinho não possuir itens, o checkout não deve ser concluído.

### RN-003 – Campos obrigatórios do endereço de entrega

-   O endereço de entrega deve conter, no mínimo:
    -   Nome/Sobrenome do destinatário (ou equivalente).
    -   Endereço (linha principal).
    -   Cidade.
    -   País.
    -   Estado/Região (quando aplicável).
    -   CEP (quando aplicável conforme configuração).
-   Campos obrigatórios não podem ser deixados em branco.

### RN-004 – Elegibilidade de métodos de envio

-   A oferta de métodos de envio depende:
    -   Do endereço de entrega informado.
    -   Das configurações de frete disponíveis (por exemplo: tarifa fixa, frete grátis, etc.).
-   Se nenhum método de envio estiver disponível para o endereço informado, o pedido não pode ser concluído.

### RN-005 – Elegibilidade de métodos de pagamento

-   A oferta de métodos de pagamento depende:
    -   Das configurações do sistema para a loja (ex.: pagamento na entrega, transferência, etc.).
-   O pedido só pode ser concluído caso ao menos um método de pagamento esteja disponível e selecionado.

### RN-006 – Cálculo de totais

-   O valor total do pedido deve ser calculado considerando:
    -   Soma dos subtotais de todos os itens do carrinho.
    -   Descontos (se aplicáveis).
    -   Impostos (se configurados).
    -   Custos de envio (se aplicáveis).
-   O total exibido antes da confirmação deve ser igual ao total registrado no pedido.

### RN-007 – Vinculação do pedido ao cliente

-   Todo pedido criado no checkout deve ser vinculado:
    -   A uma conta de cliente existente.
    -   Ao endereço de entrega selecionado ou informado no momento do checkout.

### RN-008 – Estado inicial do pedido

-   Um pedido recém-criado deve receber um estado inicial (status) padrão conforme configuração do sistema (por exemplo, “Pendente”).
-   Este estado inicial deve ser consistente com o fluxo de negócio de processamento de pedidos da loja.

----------

## 7. Requisitos Funcionais ⚙️

### RF-001 – Autenticação de cliente

O sistema deve permitir que clientes existentes façam login e tenham suas informações de conta carregadas para uso durante o checkout.

### RF-002 – Gestão de carrinho

O sistema deve manter um carrinho de compras por sessão/autenticação, permitindo que o cliente:

-   Adicione produtos.
-   Veja lista de itens no carrinho.
-   Visualize subtotais e total parcial.

### RF-003 – Acesso ao checkout

O sistema deve disponibilizar uma funcionalidade de “Checkout” ou “Finalizar compra” que leva o cliente autenticado ao fluxo de finalização do pedido a partir do carrinho.

### RF-004 – Captura/seleção de endereço de entrega

O sistema deve:

-   Permitir que o cliente selecione um endereço de entrega previamente cadastrado,  **ou**
-   Permitir que o cliente informe um novo endereço de entrega (conforme capacidades do OpenCart Demo).
-   Validar campos obrigatórios do endereço conforme RN-003.

### RF-005 – Seleção de método de envio

O sistema deve:

-   Exibir os métodos de envio disponíveis para o endereço informado.
-   Permitir que o cliente escolha um método de envio.
-   Armazenar o método de envio selecionado para o pedido.

### RF-006 – Seleção de método de pagamento

O sistema deve:

-   Exibir os métodos de pagamento disponíveis conforme configuração.
-   Permitir que o cliente escolha um método de pagamento.
-   Armazenar o método de pagamento selecionado para o pedido.

### RF-007 – Exibição do resumo do pedido

O sistema deve exibir uma página de resumo do pedido contendo:

-   Lista de produtos no carrinho (nome, quantidade, preço unitário, subtotal).
-   Custos de envio (quando aplicáveis).
-   Impostos e descontos (quando aplicáveis).
-   Total final.
-   Endereço de entrega selecionado.
-   Método de envio e de pagamento selecionados.

### RF-008 – Confirmação do pedido

O sistema deve:

-   Permitir que o cliente confirme o pedido a partir da página de resumo.
-   Criar um registro de pedido persistido no banco de dados.
-   Associar o pedido ao cliente autenticado.

### RF-009 – Exibição da confirmação do pedido

Após confirmar, o sistema deve:

-   Exibir uma mensagem de confirmação.
-   Mostrar um identificador único de pedido.
-   Apresentar um resumo básico do pedido recém-criado.

### RF-010 – Registro de pedido no histórico do cliente

O sistema deve registrar o pedido no histórico de pedidos do cliente, permitindo que ele seja visualizado posteriormente em uma área de “Meus pedidos” ou equivalente.

----------

## 8. Requisitos Não Funcionais 🛡️

### RNF-001 – Usabilidade

-   O fluxo de checkout deve ser apresentado em etapas claras e sequenciais, reduzindo ambiguidade para o usuário.
-   Mensagens de erro devem ser compreensíveis e indicar claramente quais campos precisam de correção (sem especificar como testar isso).

### RNF-002 – Desempenho

-   O tempo de resposta para avanço entre etapas do checkout deve ser aceitável para um usuário típico em ambiente web, considerando que o OpenCart Demo é um ambiente de demonstração e não de alta carga.

### RNF-003 – Segurança

-   Dados de cliente (inclusive endereço) devem ser associados à sessão do cliente autenticado, evitando exposição de dados de outros clientes.
-   Apenas usuários autenticados devem conseguir concluir o checkout desta história.

### RNF-004 – Confiabilidade

-   O processo de criação de pedido deve ser transacional: ou o pedido é criado com todos os dados consistentes, ou nenhuma alteração é efetivada (do ponto de vista do resultado final que o cliente percebe).

### RNF-005 – Compatibilidade

-   A funcionalidade deve operar em navegadores modernos com suporte padrão a HTML5 e JavaScript, conforme esperado para o OpenCart Demo.

----------

## 9. Premissas 🧱

-   O ambiente  **OpenCart Demo**  está configurado com:
    -   Pelo menos um produto disponível para compra.
    -   Pelo menos um método de envio habilitado e aplicável.
    -   Pelo menos um método de pagamento habilitado.
-   O cliente já possui conta cadastrada e credenciais válidas.
-   O sistema de login está operacional.
-   As taxas, impostos e regras de frete do demo estão previamente configuradas e funcionais.
-   Não há integrações externas reais que impeçam o fluxo (o demo utiliza configurações simuladas).

----------

## 10. Restrições 🚧

-   O escopo atual não contempla:
    -   Criação de novos clientes.
    -   Checkout como convidado.
    -   Cancelamento de pedido.
    -   Alteração de pedido após a confirmação.
-   O ambiente  **demo**  pode possuir dados compartilhados com outros usuários (por ser público), o que limita controle sobre dados disponíveis.
-   Configurações de métodos de pagamento/entrega podem ser alteradas por administradores do demo fora do controle do time (comportamento pode variar ao longo do tempo conforme o estado do demo).

----------

## 11. Dependências 🔗

-   Funcionamento do módulo de  **login/autenticação**.
-   Disponibilidade do módulo de  **carrinho de compras**.
-   Configuração dos módulos de:
    -   **Frete/Envio**  (shipping).
    -   **Pagamento**  (payment).
-   Banco de dados acessível para:
    -   Consulta de clientes, produtos, endereços.
    -   Criação de pedidos.
-   Configuração básica da loja (moeda, idioma, impostos, etc.) já realizada.

----------

## 12. Glossário 📚

-   **Cliente (Customer)**: Usuário final que realiza compras na loja e possui conta cadastrada.
-   **Carrinho de compras (Cart)**: Estrutura que armazena temporariamente os produtos selecionados pelo cliente até a finalização da compra.
-   **Checkout**: Conjunto de etapas que permitem ao cliente finalizar sua compra, informando/selecionando endereço, frete, pagamento e confirmando o pedido.
-   **Método de envio (Shipping method)**: Forma de entrega disponível (ex.: frete fixo, frete grátis, etc.), configurada no sistema.
-   **Método de pagamento (Payment method)**: Forma de pagamento disponibilizada pelo sistema para que o cliente efetive a compra.
-   **Pedido (Order)**: Registro persistido que representa a compra efetivada, incluindo produtos, valores, endereço e status.
-   **Histórico de pedidos**: Lista de pedidos anteriores associados a um cliente.
> Written with [StackEdit](https://stackedit.io/).
