# Desafio Criativo: Planejando Automações com N8N

## Passo 1: Definição da automação

### Automação desejada

Quero criar uma automação no N8N para processar automaticamente uma lista de pedidos de um ERP e alterar os valores unitários de determinados produtos ou serviços de acordo com o percentual de venda definido para cada pedido.

### Público ou responsável

Equipe responsável pela operação e manutenção dos dados de pedidos no ERP.

### Resultado esperado

Ler automaticamente os pedidos, identificar os itens que devem ter seus valores alterados, calcular os novos valores com base no percentual de venda, evitar alterações em pedidos que já foram processados e gerar um arquivo com os pedidos atualizados para posterior importação no ERP.

---

## Passo 2: Contexto e regras

### Ferramentas envolvidas

* N8N
* ERP WK Radar
* Arquivo TXT utilizado para entrada e saída dos pedidos.

### Fluxo desejado

1. Ler automaticamente o arquivo TXT contendo a relação de pedidos e seus respectivos itens.
2. Interpretar e organizar os dados do arquivo, agrupando os registros por número do pedido.
3. Identificar os pedidos que possuem os itens específicos que participam da regra de negócio.
4. Verificar se os itens do pedido estão cancelados ou faturados e desconsiderar esses registros.
5. Identificar o percentual de venda informado no pedido.
6. Calcular o novo valor unitário dos itens de acordo com o percentual de venda.
7. Verificar se o pedido já foi alterado anteriormente pela automação para evitar alterações duplicadas.
8. Gerar um novo arquivo TXT contendo os pedidos e valores atualizados, em um formato compatível com a importação no ERP.
9. Registrar quais pedidos foram alterados e quais já haviam sido processados, permitindo a conferência da execução.

### Regras importantes

* Somente devem ser processados pedidos que possuam os dois tipos de itens necessários para a regra:

  * `VALOR BENEF. PRESTADO`
  * `VALOR MATERIA PRIMA APLICADA`
* Itens com situação `CANCELADO` ou `FATURADO` não devem participar do processamento.
* O percentual de venda deve ser obtido a partir da informação disponível no pedido.
* Se o percentual estiver informado como número inteiro, ele deve ser convertido para sua representação decimal antes do cálculo.
* Para o item `VALOR BENEF. PRESTADO`, o novo valor unitário deve ser calculado multiplicando o valor original pelo percentual de venda.
* Para o item `VALOR MATERIA PRIMA APLICADA`, o novo valor unitário deve ser calculado utilizando o percentual restante, ou seja, `1 - percentual de venda`.
* Os valores calculados devem ser arredondados para até quatro casas decimais.
* Pedidos que já tenham sido alterados pela automação não devem ser processados novamente.
* Pedidos sem percentual de venda válido não devem sofrer alteração.
* O processo deve preservar os demais dados dos pedidos e alterar somente os valores necessários.
* A automação deve gerar informações de auditoria contendo o pedido, valor original, percentual utilizado e novo valor calculado.

---

## Passo 3: Prompt final

# Automação de Alteração de Valores de Pedidos no ERP

Atue como um especialista em N8N e automação de processos empresariais.

Crie uma automação no N8N para processar automaticamente uma lista de pedidos provenientes de um ERP e alterar os valores unitários de determinados produtos ou serviços de acordo com o percentual de venda definido para cada pedido.

## Público

A automação será utilizada pela equipe responsável pela operação e manutenção dos dados de pedidos no ERP.

## Ferramentas envolvidas

* N8N, responsável pela execução e orquestração do workflow.
* ERP WK Radar, sistema de origem dos dados dos pedidos e destino das informações processadas.
* Arquivo TXT, utilizado para receber a relação de pedidos e gerar o arquivo final para importação no ERP.

## Fluxo desejado

1. Ler automaticamente um arquivo TXT contendo a relação de pedidos e seus respectivos itens.

2. Interpretar e organizar os dados do arquivo, identificando as colunas e agrupando os registros pelo número do pedido.

3. Identificar quais pedidos possuem os dois tipos de itens necessários para a regra de negócio:

   * `VALOR BENEF. PRESTADO`
   * `VALOR MATERIA PRIMA APLICADA`

4. Desconsiderar itens que estejam com situação `CANCELADO` ou `FATURADO`.

5. Identificar o percentual de venda informado no pedido.

6. Validar o percentual de venda antes de realizar qualquer alteração.

7. Verificar se o pedido já foi alterado anteriormente pela automação. Caso já tenha sido processado, não realizar uma nova alteração.

8. Calcular o novo valor unitário dos itens elegíveis de acordo com o percentual de venda.

9. Preservar os demais dados dos pedidos, alterando somente os valores unitários necessários.

10. Gerar informações de auditoria contendo, no mínimo:

    * Número do pedido;
    * Valor unitário original;
    * Percentual utilizado;
    * Novo valor unitário calculado.

11. Gerar um novo arquivo TXT contendo os pedidos processados em um formato compatível com a importação no ERP.

12. Separar ou identificar os pedidos que foram alterados e os pedidos que já haviam sido processados.

## Regras de negócio

* Somente devem ser processados pedidos que possuam os dois tipos de itens necessários para a regra:

  * `VALOR BENEF. PRESTADO`
  * `VALOR MATERIA PRIMA APLICADA`

* Itens com situação `CANCELADO` ou `FATURADO` não devem participar do processamento.

* O percentual de venda deve ser obtido a partir da informação disponível no pedido.

* Caso o percentual seja informado como número inteiro, ele deve ser convertido para sua representação decimal antes do cálculo. Por exemplo, `30` deve ser interpretado como `0,30`.

* Para o item `VALOR BENEF. PRESTADO`, o novo valor unitário deve ser calculado utilizando o percentual de venda:

  `Novo Valor = Valor Original × Percentual`

* Para o item `VALOR MATERIA PRIMA APLICADA`, o novo valor unitário deve utilizar o percentual restante:

  `Novo Valor = Valor Original × (1 - Percentual)`

* Os valores calculados devem ser arredondados para até quatro casas decimais.

* Pedidos que já tenham sido alterados pela automação não devem ser processados novamente.

* Pedidos sem percentual de venda válido não devem sofrer alteração.

* Os demais dados dos pedidos devem ser preservados.

* A automação deve permitir identificar quais registros foram alterados, quais foram ignorados e quais já haviam sido processados.

* O workflow deve possuir validações para evitar alterações indevidas ou duplicadas.

## Requisitos para a solução no N8N

Explique quais nós do N8N devem ser utilizados em cada etapa do workflow e qual será a função de cada um.

Apresente a sequência dos nós de forma organizada, explicando:

1. Como o arquivo TXT será lido.
2. Como os dados serão convertidos e organizados.
3. Como os pedidos serão agrupados e filtrados.
4. Como as regras de negócio serão aplicadas.
5. Como será feita a validação de pedidos já processados.
6. Como os novos valores serão calculados.
7. Como será realizada a auditoria das alterações.
8. Como o arquivo TXT final será gerado.
9. Como tratar erros, dados inválidos e pedidos que não atendam às regras.

A solução deve priorizar clareza, segurança dos dados, rastreabilidade e prevenção de alterações duplicadas.
