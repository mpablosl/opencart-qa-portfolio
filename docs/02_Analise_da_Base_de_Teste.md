# Análise da Base de Teste

## Funcionalidade

Checkout de Cliente Cadastrado

## Objetivo da funcionalidade



Permitir que o cliente autenticado conclua a compra dos produtos

adicionados ao carrinho, após a informação do endereço, método de envio,

método de pagamento, gerando um pedido ao final do processo.



## Fluxo principal identificado



Cliente autenticado;

Adiciona um produto ao carrinho;

Preenche o endereço;

Seleciona o método de envio;

Seleciona a forma de pagamento;

Confirma as informações do pedido;

Realiza o checkout do pedido no sistema



## Regras de negócio identificadas



* RN001 - Cliente autenticado
* RN002 - Carrinho com itens obrigatórios
* RN003 - Campos obrigatórios do endereço de entrega
* RN004 - Elegibilidade de métodos de envio
* RN005 - Elegibilidade de métodos de pagamento
* RN006 - Cálculo de totais
* RN007 - Vinculação do pedido ao cliente
* RN008 - Estado inicial do pedido



## Premissas identificadas

* O ambiente OpenCart Demo está configurado com

  * Pelo menos um produto disponível pra compra;
  * Pelo menos um método de envio habilitado e aplicável;
  * Pelo menos um método de pagamento habilitado.
* O cliente já possui conta cadastrada e credenciais válidas
* O sistema de login está operacional
* As taxas, impostos e regras de frete do demo estão previamente configuradas e funcionais
* Não há integrações externas reais que impeçam o fluxo(o demo utiliza configurações simuladas)



## Restrições identificadas

* O escopo atual não contempla

  * Criação de novos cliente.
  * Checkout como convidado.
  * Cancelamento de pedido.
  * Alteração de pedido após a confirmação.
* O ambiente demo pode possuir dados compartilhados com outros usuários(por ser público), o que limita controle sobre dados disponíveis.
* Configurações de métodos de pagamento/entrega podem ser alteradas por administradores do demo fora do controle do time (comportamento pode variar ao longo do tempo conforme o estado do demo).

## Dúvidas para o Product Owner

* DQ001 - Caso não exista método de envio disponível para o CEP informado, qual deve ser o comportamento esperado?
* DQ002 - O cliente pode alterar a quantidade durante o checkout?
* Existe tempo máximo para finalizar o checkout antes da sessão expirar?


## Riscos identificados

* Mudanças na configuração do ambiente Demo podem alterar o comportamento esperado.
* Dados compartilhados podem gerar resultados inconsistentes.
* Mudanças na interface podem invalidar evidências e documentação.

