# Manutenção de Cobrança

Gerencia todas as atividades relacionadas com a Cobrança bancária.

## Rotas

### List Cobranças

> GET /284/cobrancas

API para listagem paginada de pessoas físicas.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Parâmetros da Requisição

| Parâmetro         | Tipo         | Descrição                                                                                      | Obrigatório | Domínio |
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------- | ----------- | ------- |
| grupo_empresarial | uuid         | Identificador do grupo empresarial para filtro dos registros.                                  | Sim         |         |
| tenant            | bigint       | Identificador do tenant para filtro dos registros.                                             | Sim         |         |
| cliente       | varchar(30)     | Permite filtrar todas as cobranças do cliente pelo cpf/cnpj.                         | Não         |         |
| vencimento      | datetime     | Permite filtrar os registros por uma determinada data de vencimento(20220831).                     | Não         |         |
| vencimento_inicial   | datetime     | Permite filtrar os registros por um intervalo de data, tem que ser usado juntamente com o vencimento_final         | Não         |         |
| vencimento_final  | datetime     | Permite filtrar os registros por um intervalo de data, tem que ser usado juntamente com o vencimento_inicial                    | Não         |         |


#### Corpo da resposta da cobrança

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| grupo_empresarial        | string(36)  | ID do grupo empresarial.                                                                    | Sim         | Vazio    |            |
| empresa                  | string(30)  | CPF/CNPJ da empresa.                                    									   | Sim         | Vazio    |            |
| cliente             	   | string(30)  | CPF/CNPJ do cliente.                                                                        | Sim         | Vazio    |            |
| estabelecimento          | string(30)  | CPF/CNPJ do estabelecimento.                                                                | Sim         | Vazio    |            |
| numero              	   | string(30)  |  Identificador único da cobrança.												           | Sim         | Auto Increment    |            |  | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| emissao                  | date 		 | Data de emissão da cobrança.                                                        		   | Sim         | Vazio    |            |
| vencimento               | date     	 | Vencimento da cobrança.  																   | Sim         | Vazio    |            |
| situacao                 | Enum 		 | Situação da cobrança                                                               		   | Sim         | ["aberto"]| ["SituacaoCobranca"]           |["Aberto", "Quitado","Em Débito", "Cancelado","Protestado", "Estornado","Agendado","Suspenso","Aberto Descontado","Cobrança Duvidosa"]
| parcela                  | Integer     | Número da parcela da cobrança.                                                              | Não         | 1    |     |                                                                                                                                                             |
| total_parcelas           | Integer     | Quantidade total de parcelas que tem a cobrança.                                            | Não         | 1    |     |                                                                                                                                                             |
| valor               	   | Decimal(8,2)| Valor total da cobrança.                                      							   | Sim         | Vazio    |     |                                                                                                                                                             |
| outros_acrescimos        | Decimal(8,2)| Outros acréscimos referente a cobrança.                                                     | Não         | 0.00    |     |                                                                                                                                                             |
| percentual_outros_acrescimos | Decimal(8,2)| Percentual outros acréscimos.                                                            | Não         | 0.00    |     |                                                                                                                                                             |
| outras_retencoes         | Decimal(8,2)| Outras retenções.                                         								   | Não         | 0.00    |     |                                                                                                                                                             |
| descricao_outras_retencoes| string(255)| Descrição das outras retenções.                                                             | Não         | Vazio    |     |                                                                                                                                                             |
| abatimento               | Decimal(8,2)|Valor de abatimento referente a cobrança.                                                    | Não         | 0.00    |     |                                                                                                                                                             |
| created_at               | datetime  	 | Data de criação da cobrança.                                                                | Não         | now()    | Sensível   |                                        |
| created_by               | string(150) | Usuário que criou a cobrança.                                                               | Não         | Vazio    |    | |
| updated_at               | datetime  	 | Data de atualização da cobrança.                                                            | Não         | now()    |    |                                                                                                                                    |
| updated_by               | string(150) | Usuário que atualizou a cobrança.                                                           | Não         | Vazio    |    |                                                                                  |
| deleted_at               | datetime 	 | Data de deleção da cobrança.                                                                | Não         | now()    |     |                                                                                                                                                             |
| deleted_by               | string(150) | Usuário que deletou a cobrança.                                                             | Não         | Vazio    |     |                                                                                                                                                             |
| tenant     			   | Integer     | Identificador do tenant para filtro dos registros.                           			   | Não         | Vazio    |    ||
| numero_erp               | string(150) | Numeração do título gerada no ERP.                                                             | Não         | Vazio    | 
| moeda    				   | string(30)  | Indica que o participante possui algum tipo de deficiÊncia auditiva.                        | Não         | Vazio    |    |                                                                                                                                                             |
| id_integracao			   | string(36)  | Identificador do ERP caso haja.                      | Não         | Vazio    |    |                                                                                                                                                             |
| boletos			   | []  | Lista de objeto de boleto                     | Não         | Vazio    |    |      |

#### Corpo da resposta do boleto
| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    | Observação|
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |---------- |
| grupo_empresarial        | string(36)  | ID do grupo empresarial.                                                                    | Sim         | Vazio    |            |
| empresa                  | string(30)  | CPF/CNPJ da empresa.                                    									   | Sim         | Vazio    |            |
| cliente             	   | string(30)  | CPF/CNPJ do cliente.                                                                        | Sim         | Vazio    |            |
| emitente                 | string(30)  | CPF/CNPJ do emitente.                                                                | Sim         | Vazio    |            |
| conta              	   | string(10)  | Conta do emitente do boleto.												           | Sim         |Vazio   |            |  | string | Texto livre de observações sobre o participante. | Sim | Vazio |  |
| conta_dv                 | string(1)   | Dígito da conta do emitente do boleto.                                                       		   | Sim         | Vazio    |            |
| agencia                  | string(10)   | Agência do emitente do boleto.  																   | Sim         | Vazio    |            |
| agencia_dv                 | string(1)  		 | Dígito da agência do emitente do boleto.                                                               		   | Sim         |Vazio |            |
| banco                  | string(10)      | Código de identificação do banco.                                                             | Sim         | Vazio    |     |                                                                                                                                                             |
| nosso_numero           | string(30)      | Identificador enviado para o banco.                                            | Sim         | Vazio    |     |                                                                                                                                                             |
| codigo_barras               	   | string(44) | Código de barras.                                      							   | Sim         | Vazio    |     |                                                                                                                                                             |
| situacao                 | Enum 		 | Situação do boleto.                                                             		   | Sim         | ["enviado"]| ["SituacaoBoleto"]           |["enviado(Aberto)","liquidado(Pago)","retirado_cobranca(Cancelado)","entrada_confirmada(Aberto)",   "rejeitado(Cancelado)","retirado_cobranca_liquidado(Pago)","alteracao_dados_confirmada(Aberto)", "alteracao_dados_rejeitada(Aberto)","cancelamento_cobranca_rejeitado(Cancelado)",  "alegacoes_pagador(Aberto)","alteracao_carteira(Aberto)","instrucao_rejeitada(Aberto)"]
| data_baixa | date| Data de retorno do boleto.                                                            | Não         | Vazio    |     |                                                                                                                                                             |
| vencimento         | date| Data de vencimento do boleto.                                         								   | Sim         | Vazio    |     |                                                                                                                                                             |
| emissao| date| Data de emissão do boleto.                                                             | Sim         | Vazio    |     |                                                                                                                                                             |
| url_Impressao               | string(255)|Link de disponibilização do boleto.                                                    | Não         | Vazio    |     |                                                                                                                                                             |
| valor               | Decimal(8,2)|Valor total do boleto.                                                   | Sim         |Vazio    |     |                                                                                                                                                             |
| desconto               | Decimal(8,2)|Valor monetário do desconto do boleto.                                                  | Não         | 0.00    |     |                                                                                                                                                             |
| percentual_desconto               | Decimal(8,2)| Percentual de desconto do boleto.                                                    | Não         | 0.00    |     |                                                                                                                                                             |
| data_limite_desconto               | date| Data referente até o dia que é permitido o desconto.                                                    | Não         | Vazio    |     |                                                                                                                                                             |
| juros               | Decimal(8,2)|Valor monetário do juros.                                                    | Não         | 0.00    |     |                                                                                                                                                             |
| multa               | Decimal(8,2)|Valor monetário da multa.                                               | Não         | 0.00    |     |                                                                                                                                                             |
| percentual_multa               | Decimal(8,2)|Percentual da multa do boleto.                                                      | Não         | 0.00    |     |                                                                                                                                                             |
| data_inicio_multa               | date|Data que se inicializa a cobrança da multa no boleto.                                                    | Não         | Vazio    |     |                                                                                                                                                             |
| percentual_juros_diario               | Decimal(8,2)|Percentual de juros diários que será cobrado no boleto.                                                    | Não         | 0.00    |     |                                                                                                                                                             |
| numero_cobranca              | strint(30)|Identificador de referência da cobrança.                                                  | Sim         | Vazio    |     |                                                                                                                                                             |
| estabelecimento               | string(30)|CPF/CNPJ do estabelecimento.                                                    | Sim         | Vazio    |     |                                                                                                                                                             |
| linha_digitavel               | string(54)|Linha digitável do boleto.                                                    | Sim         | Vazio    |     |                                                                                                                                                             |
| created_at               | datetime  	 | Data de criação da cobrança.                                                                | Não         | now()    |    |                                        |
| created_by               | string(150) | Usuário que criou a cobrança.                                                               | Não         | Vazio    |    | |
| updated_at               | datetime  	 | Data de atualização da cobrança.                                                            | Não         | now()    |    |                                                                                                                                    |
| updated_by               | string(150) | Usuário que atualizou a cobrança.                                                           | Não         | Vazio    |    |                                                                                 |
| deleted_at               | datetime 	 | Data de deleção da cobrança.                                                                | Não         | now()    |     |                                                                                                                                                             |
| deleted_by               | string(150) | Usuário que deletou a cobrança.                                                             | Não         | Vazio    |     |                                                                                                                                                             |
| tenant     			   | Integer     | Identificador do tenant para filtro dos registros.                           			   | Não         | Vazio    |    |                                                                                                                                                             |

##### Status possíveis
| HTTP Status | Descrição                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------- |
| 200         | Sucesso.                                                                                                |
| 400         | Requisição inválida. Causa provável: faltando parâmetros obrigatórios (tenant e grupo_empresarial). (*) |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                             |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                            |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplo de chamada

###### Requisição

```http
GET https://apihomolog.nasajon.app/284/tesouraria/cobrancas?tenant=47&grupo_empresarial=2cddf8bf-b68f-4de8-a9ee-1eccdbc634d3 HTTP/1.1
X-API-Key: **************
Accept: application/json
```

###### Resposta

```http
HTTP/1.1 200 OK
content-type: application/json; charset=utf-8
Content-Encoding: gzip

{
    "next": null,
    "prev": null,
    "result": [
        {
            "grupo_empresarial": "2cddf8bf-b68f-4de8-a9ee-1eccdbc634d3",
            "empresa": "61393735000117",
            "cliente": "54276569000103",
            "emissao": "2022-08-23",
            "vencimento": "2022-09-23",
            "situacao": "aberto",
            "parcela": 1,
            "total_parcelas": 10,
            "valor": "553.25",
            "outros_acrescimos": null,
            "percentual_outros_acrescimos": "0.00",
            "outras_retencoes": "0.00",
            "descricao_outras_retencoes": null,
            "abatimento": "0.00",
            "moeda": "reais",
            "id_integracao": null,
            "created_at": "2022-08-23T00:00:00",
            "created_by": "sergiosilva@nasajon.com.br",
            "updated_at": "2022-08-23T00:00:00",
            "updated_by": "sergiosilva@nasajon.com.br",
            "deleted_at": null,
            "deleted_by": null,
            "estabelecimento": "61393735000117",
            "numero": "1234",
            "tenant": 47,
            "numero_erp" : "123456",
            "boletos": [
                {
                    "grupo_empresarial": "2cddf8bf-b68f-4de8-a9ee-1eccdbc634d3",
                    "empresa": "61393735000117",
                    "cliente": "54276569000103",
                    "emitente": "61393735000117",
                    "conta": "3655",
                    "conta_dv": "8",
                    "agencia": "44505",
                    "agencia_dv": "9",
                    "banco": "001",
                    "nosso_numero": "87915648745616876513",
                    "codigo_barras": "87915648745616876513000000005648970056481000",
                    "situacao": "",
                    "data_baixa": null,
                    "vencimento": "2022-09-23",
                    "emissao": "2022-08-23",
                    "url_Impressao": "https://th.bing.com/th/id/R.06c19d41ded0e8da14cc4cee8c0ddc54?rik=%2ffH0Tk1bBS%2ffLg&pid=ImgRaw&r=0",
                    "valor": 553.25,
                    "desconto": 0.0,
                    "percentual_desconto": 0.0,
                    "data_limite_desconto": null,
                    "juros": 1.0,
                    "multa": 55.32,
                    "percentual_multa": 10.0,
                    "data_inicio_multa": "2022-09-23T00:00:00",
                    "percentual_juros_diario": 0.6,
                    "created_at": "2022-08-23T00:00:00",
                    "created_by": "sergiosilva@nasajon.com.br",
                    "updated_at": "2022-08-23T00:00:00",
                    "updated_by": "sergiosilva@nasajon.com.br",
                    "deleted_at": null,
                    "deleted_by": null,
                    "numero_titulo": "1234",
                    "estabelecimento": "61393735000117",
                    "linha_digitavel": "87915648745616876513000000000000005648970056481000",
                    "tenant": 47
                }
            ],
            "contrato" = []
        }
    ]
}
```
