INTEGRANDO
===========

Checkout Transparente HTML
---------------------------

####Pré-requisitos:

-- Possuir Aplicação configurada como "**Integração com desenvolvimento próprio**"

####Aplicabilidade:

 - Vendas de maior variedade de itens.
 - Maior controle sobre as vendas.
 - Suporte automático a novos meios de pagamento.

####Fluxo:

<pre>
<div class="mermaid">sequenceDiagram
    participant Cliente
    participant Loja
    participant MD
    Cliente->>+Loja: 1 - Comprar
    Loja->>MD: 2 - Consultar Meios de Pagamento
    MD-->>Loja: 3 - Retorno Meios de Pagamento
    Loja-->>-Cliente: 4 - Html com meios de pagamento
    Cliente->>+Loja: 5 - Dados de Pagamento
    Loja->>MD: 6 - Iniciar Pagamento
    MD-->>Loja: 7 - Retorno do Pedido
    Loja->>-Cliente: 8 - Compra Finalizada</div> 
</pre>

####Como utilizar:

1. **Consultar Meios de Pagamento**:

	A consulta meios de pagamento serve para ver os Meios de pagamento que estão habilitados pela aplicação. Para fazer essa consulta deve-se enviar uma requisição HTTP através dos métodos **GET** ou **POST** para a URL: https://moeda.digital/gateway.asmx/ConsultaMeiosDePagamentoHTMLv2

	Na consulta devem ser enviados como parâmetros:

 - Token da Loja: **Loja**
 - Nome da aplicação: **Aplicacao**
 - Meios de pagamento: **Meios** \*
 - Valor total da compra: **Valor**
 - Idioma do cliente: **Idioma** \*

	\* Com esse parâmetro você escolhe quais os meios entre os habilitados na aplicação que apareceram como opção de pagamento, os valores aceitos são ( "Todos" , "Credito", "Debito", "Boleto" , etc..) 

	\* O idioma pode ser ( "PT-BR", "EN_US", "ES_ES")

	Você receberá como resposta o XML .... que contém um código HTML para ser exibido ao cliente dentro de um formulário.

	**Exemplos**:
	<div class="code-sample-options">[Code](code-example/ConsultarMeiosDePagamentoHTMLv2.md)<div>

1. **Iniciar Pagamento**:

	Nesta etapa a sua aplicação deverá enviar um XML contendo as informações do pedido para ser registrado na Moeda Digital, esta retornará um XML contendo, entre outras informações, um código  HTML a ser exibido ao cliente para proceder o pagamendo (ex: o link com a imagem do boleto).Nos casos dos meios de pagamento de crédito ou debito se enviados com os dados do cartão não haverá um link para ser mostrado, apenas o status da transação.

	O XML contendo o pedido deve ser enviado como parâmetro através dos métodos **GET** ou **POST** para a URL: https://moeda.digital/gateway.asmx/IniciarPagamentoXML

	Nome do parâmetro: ***PedidoXML***

	O valor PedidoXML está definido no item [Parâmetros / **Pedido**](#parametros-pedido).

	**Exemplos**:
	<div class="code-sample-options">[Code](code-example/IniciarPagamentoXML.md)<div>

1. **Exibir o código ao cliente**:
	
	>Nota: Caso o meio de pagamento seja Crédito ou Débito juntamente com o envio dos dados do cartão, não há o que ser exibido ao cliente.

	O passo 2 retorna um XML com informações e o status do pedido, em caso de ter sido bem sucedido, ele possui um código HTML para ser exibido ao cliente para proceder o pagamento.

	O Retorno do Pedido está definido no item Parâmetros XML.

1. **Acompanhar o Status do pedido**:

	Após o pedido ser registrado, ele pode ser acompanhado através de métodos disponibilizados para consulta pela Moeda Digital e por meio de WebHook, descritos neste manual nas áreas : Consultar Status do Pedido e WebHook - URL de retorno


Checkout Transparente
-----------------------

####Pré-requisitos:

-- Possuir Aplicação configurada como "**Integração com desenvolvimento próprio**"

####Aplicabilidade:

 - Vendas de maior variedade de itens.
 - Maior controle sobre as vendas.
 - Maior capacidade de costumização do processo.

####Fluxo:

<pre>
<div class="mermaid">sequenceDiagram
    participant Cliente
    participant Loja
    participant MD
    Cliente->>+Loja: Comprar
    Loja->>+MD: (opcional) Consultar Meios de Pagamento
    MD-->>-Loja: Retorno Meios de Pagamento
    Loja->>+MD: (opcional) - Consultar Parcelas
    MD-->>-Loja: Cálculo dos valores parcelados
    Loja-->>-Cliente: Página 100% própria de pagamento
    Cliente->>+Loja: Dados de Pagamento
    Loja->>MD: Iniciar Pagamento
    MD-->>Loja: Retorno do Pedido
    Loja->>-Cliente: Resultado do pedido</div> 
</pre>

####Como utilizar:

1. **(Opcional) Consultar Meios de Pagamento**:

	A consulta meios de pagamento serve para ver os Meios de pagamento que estão habilitados pela aplicação. Para fazer essa consulta deve-se enviar uma requisição HTTP através dos métodos **GET** ou **POST** para a URL: https://moeda.digital/gateway.asmx/ConsultaMeiosDePagamento

	Na consulta devem ser enviados como parâmetros:

 - Token da Loja: **Loja**
 - Nome da aplicação: **Aplicacao**
 - Meios de pagamento: **Meios** \*

	\* Com esse parâmetro você pode verificar a disponibilidade de um meio específico ou verificar todos os disponíveis, os valores aceitos são ( "Todos" , "Credito", "Debito", "Boleto" , etc..) 

	Você receberá como resposta o XML Array de Retorno Meios Pagamento XML descrito mais a baixo.

	**Exemplos**:
	<div class="code-sample-options">[Code](code-example/ConsultarMeiosDePagamento.md)<div>

1. **(Opcional) Consultar Parcelas:**

	Nesta etapa pode-se consultar o valores das parcelas já acrescidas de juros que estão habilitados na aplicação para um devido valor.  Para fazer essa consulta deve-se enviar uma requisição HTTP através dos métodos GET ou POST para a URL: https://moeda.digital/gateway.asmx/ConsultaParcelasArray

	Na consulta devem ser enviados como parâmetros:

 - Token da Loja: **Loja**
 - Nome da aplicação: **Aplicacao**
 - Valor da compra: **Valor** ( sem pontuação )

	Você receberá como resposta o XML Retorno Parcelas Array descrito mais a baixo.
	
	**Exemplos**:
	<div class="code-sample-options">[Code](code-example/ConsultaParcelasArray.md)<div>

1. **Iniciar Pagamento**:

	Nesta etapa a sua aplicação deverá enviar um XML contendo as informações do pedido para ser registrado na Moeda Digital, esta retornará um XML contendo, entre outras informações, um código  HTML a ser exibido ao cliente para proceder o pagamendo (ex: o link com a imagem do boleto).Nos casos dos meios de pagamento de crédito ou debito se enviados com os dados do cartão não haverá um link para ser mostrado, apenas o status da transação.

	O XML contendo o pedido deve ser enviado como parâmetro através dos métodos **GET** ou **POST** para a URL: https://moeda.digital/gateway.asmx/IniciarPagamentoXML

	Nome do parâmetro: ***PedidoXML***

	O valor PedidoXML está definido no item [Parâmetros / **Pedido**](#parametros-pedido).

	**Exemplos**:
	<div class="code-sample-options">[Code](code-example/IniciarPagamentoXML.md)<div>


1. **Exibir o código ao cliente**:
	
	>Nota: Caso o meio de pagamento seja Crédito ou Débito juntamente com os dados do cartão, não há o que ser exibido ao cliente, pular para o passo 5.

	O passo 2 retorna um XML com informações e o status do pedido, em caso de ter sido bem sucedido, ele possui um código HTML para ser exibido ao cliente para proceder o pagamento.

	O Retorno do Pedido está definido no item Parâmetros XML.

1. **Acompanhar o Status do pedido**:

	Após o pedido ser registrado, ele pode ser acompanhado através de métodos disponibilizados para consulta pela Moeda Digital e por meio de WebHook, descritos neste manual nas áreas : Consultar Status do Pedido e WebHook - URL de retorno


ACOMPANHAMENTO DOS PEDIDOS
==========================

Consultar Status
---------------

####Como utilizar:

 O status de cada pedido pode ser consultado através de requisições HTTP utilizando os metodos POST ou GET para a URL: https://moeda.digital/gateway.asmx/ConsultaStatusPagamento 
 
 Na consulta devem ser enviados como parâmetros:

 - Token da Loja: Loja
 - Nome da aplicação: Aplicação
 - Identificador do pedido: Pedido 

Você receberá como resposta o XML Retorno Status Pagamento descrito mais a baixo.


WebHook - URL de retorno
-------------------------------------

####Como utilizar:

Para cada aplicação é possível fornecer uma **URL** de retorno que será chamada pela Moeda Digital sempre que um pedido tiver seu Status alterado. A requisição pode ser recebida através dos métodos **GET** ou **POST**.

1. Acesse sua Conta MD na plataforma da Moeda Digital.
 
2. Acesse no menu superior ***Administração*** → ***Aplicações*** e selecione a aplicação que deseja acompanhar.

3. Preencha o campo "**URL de Retorno**" com a URL da sua aplicação que receberá a notificação.

4. Programe seu site para receber o seguinte paramêtro contendo o número identificador do pedido que teve seu status alterado.

	**Parâmetros**:

 - Identificador do Pedido na Loja: **Pedido** 
 
5. Por motivos de segurança após receber o chamado de alteração do status do pedido, a loja deve consultar o status do pedido através do procedimento descrito em Consultar Status do Pedido utilizando-se do identificador recebido.

**Exemplos de Requisição**:
	<div class="code-sample-options">[Code](code-example/WebHook.md)</div>
