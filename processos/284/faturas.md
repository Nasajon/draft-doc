
# Manutenção de Faturas

Gerencia todas as atividades relacionadas com Fatura.

## Rotas

### Post Faturas

> Post /284/faturas

API para gravação de uma nova fatura.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| empresa                  | string(30)  | CPF/CNPJ da empresa.                                    									   | Sim         | Vazio    |            |			 |
| prestador                | string(30)  | CPF/CNPJ do prestador.                                                                      | Sim         | Vazio    |            |			 |
| tomador             	   | string(30)  | CPF/CNPJ do tomador.                                                                        | Sim         | Vazio    |            | 			 |
| estabelecimento          | string(30)  | CPF/CNPJ do estabelecimento.                                                                | Sim         | Vazio    |            |			 |
| numero              	   | string(30)  |  Identificador único da fatura.												               | Sim         |          |            | O número da fatura deve ser enviado pelo cliente.           |
| emissao                  | date 		 | Data de emissão da fatura.                                                        		   | Sim         | Vazio    |            |			 |
| desconto                 | Decimal(15,2)| Desconto total na fatura.                                      							   | Não         | Vazio    |            |           |
| percentual_desconto      | Decimal(15,4)| Percetual de desconto na fatura.                                                    	   | Não         | 0.00     |     		 |			 |                                                                                                                                                             |
| sinal                    | Enum/string(30)| Sinal da fatura.                                                             		   	   | Sim         |          |["SinalFatura"]|["recebimento","pagamento""]|
| criado_em         	   | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |			 |
| criado_por        	   | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |			 |
| atualizado_em     	   | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |			 |	
| atualizado_por    	   | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Não         | now()    |            |			 |	
| apagado_em        	   | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |			 |
| grupo_empresarial        | string(36)  | ID do grupo empresarial.                                                                    | Sim         | Vazio    |            |			 |
| tenant            	   | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |		     |
| forma_pagamento_fatura   | [FormaPagamentoFautra]| Lista de objeto de forma de pagamentos para fatura.		                       | Sim         | Vazio    |    		 |           |
| item_fatura   		   | [ItemFautra]| Lista de objeto de item para fatura. Ex: Cte's .		                       						   | Sim         | Vazio    |    		 |           |

#### Corpo da FormaPagamentoFautra
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| valor        | Decimal(13,2)  | Valor total da parcela.                                                                    		   | Sim         | Vazio    |            |
| total_parcela| Int  			| Total de parcela se houver.                                                                    	   | Não         | 1    |            	 |
| conta  	   | string(30)	    | Código da conta que consta no ERP.                                                           			   | Sim      | Vazio |		 	 |    		 |
| tipo_forma_pagamento		    | string(30)| Forma de pagamento que será utilizada.                                                             | Sim         | Vazio    |["TipoFormaPagamento"]|["boleto_bancario", "cartao_credito", "cartao_debido", "cheque", "cheque_pre_datado", "carne", "dinheiro", "vale_troca", "debito_automatico", "pix_transferencia", "pix_qr_code"]|

#### Corpo do ItemFautra
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| valor        | Decimal(15,2)  | Valor total do item.                                                                    		   | Sim         | Vazio    |            |
| chave		   | string(44)| Chave única . Ex Cte: 31200201014373001318570010002042731470407154 / Ex Nfse: 123456_001(numeroNota_serie).                                                              | Sim      | Vazio |		 	 |    		 |
| temporario   | boolean   |Informa se o item será temporário para ser criado no ERP Desktop um documento temporário 					| Não     	  | false	|		 	 |    		 |

#### Corpo do Rateio Financeiro
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| valor        | Decimal(15,2)  | Valor do rateio financeiro                                                                    		   | Sim     | 	Vazio	|            |
| centro_custo		   | string(40)|Código do centro de custo                                                              				   | Sim     |  Vazio	|		 	 |    		 |
| centro_custo_descricao| string(255)|Descrição do centro de custo                                                            			   | Não     | centro_custo|		 	 |    		 |
| classificacao_financeira		   | string(40)|Código da classificação financeira                                                         | Sim     |  Vazio	|		 	 |    		 |
| classificacao_financeira_descricao| string(255)|Descrição da da classificação financeira                                                 | Não     | classificacao_financeira|		 	 |    		 |
| projeto		   | string(40)|Código do projeto                                                              				   			   | Sim     |  Vazio	|		 	 |    		 |
| projeto_descricao| string(255)|Descrição do projeto                                                             			   			   | Não     | projeto|		 	 |    		 |


##### Status possíveis
| HTTP Status | Descrição                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------- |
| 201         | Criado.                                                                                                 |
| 400         | Requisição inválida. Causa provável: faltando parâmetros obrigatórios (tenant e grupo_empresarial). (*) |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                             |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                |
| 422         | Proibida ação. Causa provável: Inserção já foi realizada anteriormente.                                               |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                            |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplo de chamada

###### Requisição

```http
POST https://tesouraria-api-inyrb33hja-uc.a.run.app/tesouraria/284/faturas
HTTP/1.1
X-API-Key: **************
Accept: application/json

{
    "empresa" : "99999999999",
    "prestador" :"99999999999",
    "tomador" : "99999999999",
    "emissao": "2022-10-26",
    "desconto" : 0.0,
    "percentual_desconto": 0.0,
    "tenant": 9999,
    "grupo_empresarial":"07393bcc-5b38-469a-8528-9ca9f70461d7",
    "estabelecimento":"99999999999",
    "numero":"1",
    "sinal": "recebimento",
    "forma_pagamento_fatura": [{
         "valor" : 175.00,
         "conta" : "BB",
         "tipo_forma_pagamento" : "cheque"
    },
    {
         "valor" : 25.00,
         "conta" : "BB",
         "tipo_forma_pagamento" : "dinheiro"
    }
    ],
    "item_fatura": [
        {
        "valor" : 150.00,
         "chave" : 1,
		 "temporario" : false
        },
        {
        "valor" : 50.00,
        "chave" : 2,
		"temporario" : false
        }

    ],
	"rateio_financeiro_fatura": [
		{
			"valor": "180.00",
			"centro_custo": "1.01",
			"centro_custo_descricao": "XPTO",
			"classificacao_financeira": "101002",
			"classificacao_financeira_descricao": "XPTO",
			"projeto": "0001",
			"projeto_descricao": "XPTO"
		},
		{
			"valor": "20.00",
			"centro_custo": "1.02",
			"centro_custo_descricao": "XPTO",
			"classificacao_financeira": "101003",
			"classificacao_financeira_descricao": "XPTO",
			"projeto": "0001",
			"projeto_descricao": "XPTO"
		}
	]

}
```



###### Resposta

```http
HTTP/1.1 201 OK
content-type: application/json; charset=utf-8
Content-Encoding: gzip

{
    "empresa": "99999999999",
    "prestador": "99999999999",
    "tomador": "99999999999",
    "emissao": "2022-10-26",
    "desconto": 0.0,
    "percentual_desconto": 0.0,
    "criado_em": "2022-11-03T18:52:47",
    "criado_por": "Fulano",
    "tenant": 9999,
    "grupo_empresarial": "07393bcc-5b38-469a-8528-9ca9f70461d7",
    "estabelecimento": "99999999999",
    "numero": "1",
    "sinal": "recebimento",
    "forma_pagamento_fatura": [
        {
            "tipo_forma_pagamento": "cheque",
            "valor": "150",
            "total_parcela": 1,
            "conta": "BB"
        },
        {
            "tipo_forma_pagamento": "dinheiro",
            "valor": "50",
            "total_parcela": 1,
            "conta": "BB"
        }
    ],
    "item_fatura": [
        {
            "valor": "150",
            "chave": "1",
			"temporario" : false
        },
        {
            "valor": "50",
            "chave": "2",
			"temporario" : false
        }
    ],
	"rateio_financeiro_fatura": [
		{
			"valor": "180.00",
			"centro_custo": "1.01",
			"centro_custo_descricao": "XPTO",
			"classificacao_financeira": "101002",
			"classificacao_financeira_descricao": "XPTO",
			"projeto": "0001",
			"projeto_descricao": "XPTO"
		},
		{
			"valor": "20.00",
			"centro_custo": "1.02",
			"centro_custo_descricao": "XPTO",
			"classificacao_financeira": "101003",
			"classificacao_financeira_descricao": "XPTO",
			"projeto": "0001",
			"projeto_descricao": "XPTO"
		}
	]
}
```

### List Faturas

> GET /284/faturas

API para listagem paginada de faturas.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Parâmetros da Requisição

| Parâmetro         | Tipo         | Descrição                                                                                      | Obrigatório | Domínio |
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------- | ----------- | ------- |
| grupo_empresarial | uuid         | Identificador do grupo empresarial para filtro dos registros.                                  | Sim         |         |
| tenant            | bigint       | Identificador do tenant para filtro dos registros.                                             | Sim         |         |
| situacao       	| Enum/string  | Permite filtrar todas as faturas por situação.                         						| Não         |["SituacaoFatura"]|["processado","erro_processamento""]         |


#### Corpo da resposta da Fatura

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| empresa                  | string(30)  | CPF/CNPJ da empresa.                                    									   | Sim         | Vazio    |            |			 |
| prestador                | string(30)  | CPF/CNPJ do prestador.                                                                      | Sim         | Vazio    |            |			 |
| tomador             	   | string(30)  | CPF/CNPJ do tomador.                                                                        | Sim         | Vazio    |            | 			 |
| numero              	   | string(30)  |  Identificador único da fatura.												               | Sim         |          |            | O número da fatura enviado pelo cliente.           |
| emissao                  | date 		 | Data de emissão da fatura.                                                        		   | Sim         | Vazio    |            |			 |
| desconto                 | Decimal(15,2)| Desconto total na fatura.                                      							   | Não         | Vazio    |            |           |
| percentual_desconto      | Decimal(15,4)| Percetual de desconto na fatura.                                                    	   | Não         | 0.00     |     		 |			 |                                                                                                                                                             |
| situacao       		   | Enum/string  | Permite filtrar todas as faturas por situação.                         					   | Não         |["SituacaoFatura"]|["processado","erro_processamento""]         |
| numero              	   | string(30)  |  Identificador único da fatura.												               | Sim         |          |            | O número da fatura enviado pelo cliente.           |
| sinal                    | Enum/string(30)| Sinal da fatura.                                                             		   	   | Sim         |          |["SinalFatura"]|["recebimento","pagamento""]|
| criado_em         	   | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |			 |
| criado_por        	   | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |			 |
| atualizado_em     	   | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |			 |	
| atualizado_por    	   | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Não         | now()    |            |			 |	
| apagado_em        	   | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |			 |
| grupo_empresarial        | string(36)  | ID do grupo empresarial.                                                                    | Sim         | Vazio    |            |			 |
| estabelecimento          | string(30)  | CPF/CNPJ do estabelecimento.                                                                | Sim         | Vazio    |            |			 |
| tenant            	   | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |		     |
| forma_pagamento_fatura   | [FormaPagamentoFautra]| Lista de objeto de forma de pagamentos para fatura.		                       | Sim         | Vazio    |    		 |           |
| item_fatura   		   | [ItemFautra]| Lista de objeto de item para fatura. Ex: Cte's .		                       				   | Sim         | Vazio    |    		 |           |


#### Corpo da resposta da FormaPagamentoFautra
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| valor        | Decimal(13,2)  | Valor total da parcela.                                                                    		   | Sim         | Vazio    |            |			 |
| total_parcela| Int  			| Total de parcela se houver.                                                                    	   | Não         | 1    	|            |			 |
| conta  	   | string(30)| Código da conta que consta no ERP.                                                           			   | Sim      	 | Vazio 		|		 	 |    		 |
| tipo_forma_pagamento| string(30)| Forma de pagamento que será utilizada.                                                             | Sim         | Vazio    |["TipoFormaPagamento"]|["boleto_bancario", "cartao_credito", "cartao_debido", "cheque", "cheque_pre_datado", "carne", "dinheiro", "vale_troca", "debito_automatico", "pix_transferencia", "pix_qr_code"]|


#### Corpo da resposta do ItemFautra
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| valor        | Decimal(15,2)  | Valor total do item.                                                                    		   	   | Sim         | Vazio    |            |
| chave		   | string(44)| Chave única . Ex Cte: 31200201014373001318570010002042731470407154 / Ex Nfse: 123456_001(numeroNota_serie).                                                              | Sim      | Vazio |		 	 |    		 |
| temporario   | boolean   |Retorna se o item será temporário para ser criado no ERP Desktop um documento temporário 					| Não     	  | false	|		 	 |    		 |

#### Corpo do Rateio Financeiro
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| valor        | Decimal(15,2)  | Valor do rateio financeiro                                                                    		   | Sim     | 	Vazio	|            |
| centro_custo		   | string(40)|Código do centro de custo                                                              				   | Sim     |  Vazio	|		 	 |    		 |
| centro_custo_descricao| string(255)|Descrição do centro de custo                                                            			   | Não     | centro_custo|		 	 |    		 |
| classificacao_financeira		   | string(40)|Código da classificação financeira                                                         | Sim     |  Vazio	|		 	 |    		 |
| classificacao_financeira_descricao| string(255)|Descrição da da classificação financeira                                                 | Não     | classificacao_financeira|		 	 |    		 |
| projeto		   | string(40)|Código do projeto                                                              				   			   | Sim     |  Vazio	|		 	 |    		 |
| projeto_descricao| string(255)|Descrição do projeto                                                             			   			   | Não     | projeto|		 	 |    		 |


##### Status possíveis
| HTTP Status | Descrição                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------- |
| 200         | Sucesso.                                                                                                 |
| 400         | Requisição inválida. Causa provável: faltando parâmetros obrigatórios (tenant e grupo_empresarial). (*) |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                             |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                |
| 422         | Proibida ação. Causa provável: Inserção já foi realizada anteriormente.                                               |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                            |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplo de chamada

###### Requisição

```http
GET https://tesouraria-api-inyrb33hja-uc.a.run.app/tesouraria/284/faturas?tenant=XXXX&grupo_empresarial=GUID HTTP/1.1
X-API-Key: **************
Accept: application/json
```

###### Resposta

```http
HTTP/1.1 200 OK
[
    {
        "empresa": "99999999999999",
        "prestador": "99999999999999",
        "tomador": "99999999999999",
        "emissao": "2022-10-26",
        "desconto": "0.00",
        "percentual_desconto": "0.0000",
        "situacao": "nao_processado",
        "criado_em": "2022-11-01T21:22:47",
        "criado_por": "Fulano",
        "atualizado_em": "2022-11-01T21:22:47",
        "atualizado_por": "Ciclano",
        "apagado_em": null,
        "apagado_por": null,
        "tenant": 14,
        "grupo_empresarial": "01324eab-bd7c-4db0-8d41-f1d9ef3643bc",
        "estabelecimento": "99999999999999",
        "numero": "1",
        "sinal": "recebimento",
        "motivo": "",
        "forma_pagamento_fatura": [
            {
                "valor": "100.00",
                "total_parcela": 1,
                "meio_eletronico_cartao": null,
                "operadora_cartao": null,
                "bandeira_cartao": null,
                "conta": "BB",
                "tipo_forma_pagamento": "dinheiro"
            }
        ],
        "item_fatura": [
            {
                "chave": "1",
                "valor": "50.00",
				"temporario" : false
            },
            {
                "chave": "2",
                "valor": "50.00",
				"temporario" : false
            }
        ],
		"rateio_financeiro_fatura": [
			{
				"valor": "180.00",
				"centro_custo": "1.01",
				"centro_custo_descricao": "XPTO",
				"classificacao_financeira": "101002",
				"classificacao_financeira_descricao": "XPTO",
				"projeto": "0001",
				"projeto_descricao": "XPTO"
			},
			{
				"valor": "20.00",
				"centro_custo": "1.02",
				"centro_custo_descricao": "XPTO",
				"classificacao_financeira": "101003",
				"classificacao_financeira_descricao": "XPTO",
				"projeto": "0001",
				"projeto_descricao": "XPTO"
			}
		]
    }
]
```
