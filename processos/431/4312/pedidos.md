
# Manutenção de Pedidos  
  
Gerenciar previsões de necessidades futuras de peças e materiais
  
## Rotas  
  
### List Pedidos  
  
> GET /4312/pedidos  
  
API para listagem paginada de pedidos  
  
#### Autenticação  
  
Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).  
  
#### Parâmetros da Requisição  
  
| Parâmetro | Tipo | Descrição | Obrigatório | Domínio |  
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------- | ----------- | ------- |  
| grupo_empresarial | string(60) | Identificador do grupo empresarial (Código ou Identificador GUID) para filtro dos registros. | Sim | |  
| tenant | bigint | Identificador do tenant para filtro dos registros. | Sim | |  
| criado_apos | datetime | Permite filtrar os registros criados após um determinada data e hora. | Não | |  
| criado_antes | datetime | Permite filtrar os registros criados antes de uma determinada data e hora. | Não | |  
| atualizado_apos | datetime | Permite filtrar os registros atualizados após um determinada data e hora. | Não | |  
| atualizado_antes | datetime | Permite filtrar os registros atualizados antes de uma determinada data e hora. | Não | |  
| apagado_apos | datetime | Permite filtrar os registros apagados após um determinada data e hora. | Não | |  
| apagado_antes | datetime | Permite filtrar os registros apagados antes de uma determinada data e hora. | Não | |    
| fornecedor| string(60) | Identificador do fornecedor(CPF/CNPJ ou Identificador GUID) para filtro dos registros. | Não | |  
| fields (*) | list(string) | Permite indicar uma lista de campos a recuperar na chamada (exemplo: `fields=numero,emissao`). | Não | |  
  
_(*) O parâmetro fields é o que permite a recuperação de qualquer propriedade do resgistro (além das propriedades de resumo, que sempre são trazidas, independentemente deste parâmetro)._  
  
_Obs.: Qualquer dos campos de propriedades simples do registro (desconsiderando listas ou objetos aninhados), pode ser utilizado como filtro na requisição (sendo que será considerado um filtro de condição de igualdade, por exemplo: `numero_externo=XPTO` irá filtrar os registros com campo `Número Externo` igual a `XPTO`)._  
  
#### Corpo da resposta  
  
*Pedido*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| id | uuid | Identificador do pedido | Não | Vazio | |  
| numero | string(30) | Númeração interna única de controle do pedido | Não | Vazio | |  
| numero_externo | string(30) | Número externo do pedido | Não | Vazio | |  
| emissao | date | Data de emissão do pedido | Não | Vazio | |  
| previsao_de_entrega | date | Data prevista para entrega | Não | Vazio | |  
| estabelecimento | string(30) | CNPJ/CPF do estabelecimento | Sim | Vazio | |  
| operacao_para_consumidor_final | boolean | Identifica se o pedido é destinado para um consumidor final | Sim | false | |  
| fornecedor| string(30) | CNPJ/CPF do fornecedor| Sim | Vazio | |   
| modalidade_frete | string(30) | Modalidade do frete | Sim | Vazio | ["por_conta_do_remetente", "por_conta_do_destinatario", "por_conta_de_terceiros", "transporte_proprio_remetente", "transporte_proprio_destinatario", "sem_frete"] |  
| trasnportadora | string(30) | CNPJ/CPF da transportadora | Não | Vazio | |  
| observacoes | string(1000) | Observações do pedido | Não | Vazio | |  
| total_itens | decimal(2) | Valor total dos itens | Não | Vazio | |  
| total_desconto | decimal(2) | Valor total de desconto | Não | Vazio | |  
| total_frete | decimal(2) | Valor total de frete | Não | Vazio | |  
| total | decimal(2) | Valor total do pedido | Não | Vazio | |  
| situacao | string(30) | Situação do pedido | Não | Vazio | ["pendente", "aprovado"] |  
| itens | item | Itens do pedido | Não | Vazio | |  
| cobrancas | cobranca | Formas de cobrança do pedido | Não | Vazio | |  
| criado_em | datetime | Indica data e hora de cadastro do registro. | Não | now() | |  
| criado_por | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro. | Sim | Vazio | |  
| atualizado_em | datetime | Indica data e hora da última atualização do registro. | Não | Vazio | |  
| atualizado_por | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim | now() | |  
| apagado_em | datetime | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer. | Não | Vazio | |  
| grupo_empresarial | string(60) | Identificador do grupo empresarial (Código ou Identificador GUID) ao qual o registro pertence. | Sim | Vazio | |  
| tenant | bigint | Identificador do tenant ao qual o registro pertence. | Sim | Vazio | |  
  
  
*Pedido >> Item*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| produto | produto | Produto vinculado ao pedido | Não | Vazio | |  
| quantidade | decimal(4) | Quantidade solicitada | Sim | Vazio | |  
| unidade | string(6) | Unidade de comercialização | Sim | Vazio | |  
| valor_unitario | decimal(10) | Valor unitário aplicado | Sim | Vazio | |  
| valor_frete | decimal(2) | Valor do frete | Sim | Vazio | |  
| valor_desconto | decimal(2) | Valor do desconto | Sim | Vazio | |  
| total | decimal(2) | Valor total | Sim | Vazio | |  
| considera_no_total | boolean | Item impacta o total do pedido | Não | True | |  
  
  
*Pedido >> Cobrança*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| forma_pagamento | string(30) | Identificador da forma de pagamento. | Não | Vazio | ["dinheiro ", "cheque", "cartao_de_credito", "cartao_de_debito", "credito_loja", "vale_alimentacao", "vale_refeicao", "vale_presente", "vale_combustivel", "boleto_bancario", "deposito_bancario", "pix", "transferencia", "cashback" , "sem_pagamento", "outros" ] |  
| forma_pagamento_outros | string(60) | Descrição da forma de pagamento quando outras. | Não | Vazio | |  
| numero | string(30) | Número da fatura. | Não | Vazio | |  
| parcela | int | Número da parcela. | Não | Vazio | |  
| valor | decimal(2) | Valor da parcela. | Não | Vazio | |  
| vencimento | date | Data de vencimento. | Não | Vazio | |  
  
##### Status possíveis  
..  
  
| HTTP Status | Descrição |  
| ----------- | ------------------------------------------------------------------------------------------------------- |  
| 200 | Sucesso. |  
| 400 | Requisição inválida. Causa provável: faltando parâmetros obrigatórios (tenant e grupo_empresarial). (*) |  
| 401 | Proibido acesso. Causa provável: falha na autenticação. (*) |  
| 403 | Proibida ação. Causa provável: falha na autorização. (*) |  
| 500 | Erro interno do servidor. Ver detalhes no corpo da resposta. |  
  
_(*): Ver disposições gerais sobre erros, para mais informações._  
  
##### Exemplo de chamada  
  
###### Requisição  
  
```http  
GET https://compras-api-inyrb33hja-uc.a.run.app/4312/pedidos?criado_apos=2022-08-09&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1  
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
"id": "14e27709-acae-4917-9ba9-b377b401e606",  
"numero": "1234",  
"numero_externo": "1234",  
"emissao": "2022-08-30",  
"previsao_de_entrega": "2022-09-10",  
"estabelecimento": "45.223.156/0001-70",  
"operacao_para_consumidor_final": "false",  
"fornecedor": "15.487.513/0001-46",  
"modalidade_frete": “por_conta_do_remetente”,  
"trasnportadora": "22.634.146/0001-21",  
"observacoes": " ... ",  
"total_itens": 80.00,  
"total_desconto": 0.00,  
"total_frete": 20.00,  
"total": 100.00,  
"situacao": "pendente",  
"itens": [  
{  
"produto": {  
"codigo": "123456",  
"descricao": "Camisa Branca",  
...  
},  
"quantidade": 4.00,  
"unidade": "UN",  
"valor_unitario": 20.00,  
"valor_frete": 20.00,  
"valor_desconto": 0.00,  
"total": 80.00,  
"considera_no_total": true  
},  
...  
],  
"cobrancas": [  
{  
"forma_pagamento": "dinheiro",  
"forma_pagamento_outros": "",  
"numero": "1234",  
"parcela": "1",  
"numero": "60",  
"valor": 100.00,  
"vencimento": "2022-08-30"  
},  
...  
],    
"criado_em": "2022-08-30T19:53:53",  
"criado_por": "teste@teste.com",  
"atualizado_em": "2022-08-30T19:53:53",  
"atualizado_por": "teste@teste.com",  
"grupo_empresarial": "1234",  
"tenant": 9999  
}  
]  
}  
```  
  
### GET Pedido  
  
> GET /4312/pedidos/{id}  
  
API para recuperação de um pedido, por meio do seu número de identificação.  
  
#### Autenticação  
  
Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).  
  
#### Parâmetros da Requisição  
  
| Parâmetro | Tipo | Descrição | Obrigatório | Domínio |  
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------- | ----------- | ------- |  
| grupo_empresarial | uuid | Identificador do grupo empresarial para filtro dos registros. | Sim | |  
| tenant | bigint | Identificador do tenant para filtro dos registros. | Sim | |  
| fields (*) | list(string) | Permite indicar uma lista de campos a recuperar na chamada (exemplo: `fields=numero,emissao`). | Não | |  
  
  
#### Corpo da resposta  
  
Segue o escopo já documentado anteriormente...  
  
##### Status possíveis  
| HTTP Status | Descrição |  
| ----------- | ------------------------------------------------------------------------------------------------------- |  
| 200 | Sucesso. |  
| 400 | Requisição inválida. Causa provável: faltando parâmetros obrigatórios (tenant e grupo_empresarial). (*) |  
| 401 | Proibido acesso. Causa provável: falha na autenticação. (*) |  
| 403 | Proibida ação. Causa provável: falha na autorização. (*) |  
| 404 | Não encontrado. Causa provável: identificador não existente no grupo_empresarial/tenant. (*) |  
| 500 | Erro interno do servidor. Ver detalhes no corpo da resposta. |  
  
_(*): Ver disposições gerais sobre erros, para mais informações._  
  
##### Exemplo de chamada  
  
###### Requisição  
  
```http  
GET https://compras-api-inyrb33hja-uc.a.run.app/4312/pedidos/1234?fields=numero,emissao&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1  
X-API-Key: **************  
Accept: application/json  
```  
  
###### Resposta  
  
```  
http  
HTTP/1.1 200 OK  
content-type: application/json; charset=utf-8  
Content-Encoding: gzip  
  
  
{  
"id": "14e27709-acae-4917-9ba9-b377b401e606",  
"numero": "1234",  
"numero_externo": "1234",  
"emissao": "2022-08-30",  
"previsao_de_entrega": "2022-09-10",  
"estabelecimento": "45.223.156/0001-70",  
"operacao_para_consumidor_final": "false",  
"fornecedor": "15.487.513/0001-46",  
"modalidade_frete": “por_conta_do_remetente”,  
"trasnportadora": "22.634.146/0001-21",  
"observacoes": " ... ",  
"total_itens": 80.00,  
"total_desconto": 0.00,  
"total_frete": 20.00,  
"total": 100.00,  
"situacao": "pendente",  
"itens": [  
{  
"produto": {  
"codigo": "123456",  
"descricao": "Camisa Branca",  
...  
},  
"quantidade": 4.00,  
"unidade": "UN",  
"valor_unitario": 20.00,  
"valor_frete": 20.00,  
"valor_desconto": 0.00,  
"total": 80.00,  
"considera_no_total": true  
},  
...  
],  
"cobrancas": [  
{  
"forma_pagamento": "dinheiro",  
"forma_pagamento_outros": "",  
"numero": "1234",  
"parcela": "1",  
"numero": "60",  
"valor": 100.00,  
"vencimento": "2022-08-30"  
},  
...  
],  
"criado_em": "2022-08-30T19:53:53",  
"criado_por": "teste@teste.com",  
"atualizado_em": "2022-08-30T19:53:53",  
"atualizado_por": "teste@teste.com",  
"grupo_empresarial": "1234",  
"tenant": 9999  
}  
}  
```  
### POST Pedido  
  
> POST /4312/pedido  
  
API para gravação de um novo pedido  
  
#### Autenticação  
  
Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).  
  
#### Corpo da requisição  
  
  
*Pedido*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| numero | string(30) | Númeração interna única de controle do pedido | Não | Vazio | |  
| numero_externo | string(30) | Número externo do pedido | Não | Vazio | |  
| emissao | date | Data de emissão do pedido | Não | Vazio | |  
| previsao_de_entrega | date | Data prevista para entrega | Não | Vazio | |  
| estabelecimento | string(30) | CNPJ/CPF do estabelecimento | Sim | Vazio | |  
| operacao_para_consumidor_final | boolean | Identifica se o pedido é destinado para um consumidor final | Sim | false | |  
| fornecedor| string(30) | CNPJ/CPF do fornecedor| Sim | Vazio | |  
| modalidade_frete | string(30) | Modalidade do frete | Sim | Vazio | ["por_conta_do_remetente", "por_conta_do_destinatario", "por_conta_de_terceiros", "transporte_proprio_remetente", "transporte_proprio_destinatario", "sem_frete"] |  
| trasnportadora | string(30) | CNPJ/CPF da transportadora | Não | Vazio | |  
| observacoes | string(1000) | Observações do pedido | Não | Vazio | |  
| total_itens | decimal(2) | Valor total dos itens | Não | Vazio | |  
| total_desconto | decimal(2) | Valor total de desconto | Não | Vazio | |  
| total_frete | decimal(2) | Valor total de frete | Não | Vazio | |  
| total | decimal(2) | Valor total do pedido | Não | Vazio | |  
| situacao | string(30) | Situação do pedido | Não | Vazio | ["pendente", "aprovado"] |  
| itens | item | Itens do pedido | Não | Vazio | |  
| cobrancas | cobranca | Formas de cobrança do pedido | Não | Vazio | |  
| criado_em | datetime | Indica data e hora de cadastro do registro. | Não | now() | |  
| criado_por | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro. | Sim | Vazio | |  
| atualizado_em | datetime | Indica data e hora da última atualização do registro. | Não | Vazio | |  
| atualizado_por | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim | now() | |  
| apagado_em | datetime | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer. | Não | Vazio | |  
| grupo_empresarial | string(60) | Identificador do grupo empresarial (Código ou Identificador GUID) ao qual o registro pertence. | Sim | Vazio | |  
| tenant | bigint | Identificador do tenant ao qual o registro pertence. | Sim | Vazio | |  
  
  
*Pedido >> Item*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| produto | string(30) | IDentificador do produto (Código de barras ou código) | Não | Vazio | |  
| quantidade | decimal(4) | Quantidade solicitada | Sim | Vazio | |  
| unidade | string(6) | Unidade de comercialização | Sim | Vazio | |  
| valor_unitario | decimal(10) | Valor unitário aplicado | Sim | Vazio | |  
| valor_frete | decimal(2) | Valor do frete | Sim | Vazio | |  
| valor_desconto | decimal(2) | Valor do desconto | Sim | Vazio | |  
| total | decimal(2) | Valor total | Sim | Vazio | |  
| considera_no_total | boolean | Item impacta o total do pedido | Não | True | |  
  
  
*Pedido >> Cobrança*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| forma_pagamento | string(30) | Identificador da forma de pagamento. | Não | Vazio | ["dinheiro ", "cheque", "cartao_de_credito", "cartao_de_debito", "credito_loja", "vale_alimentacao", "vale_refeicao", "vale_presente", "vale_combustivel", "boleto_bancario", "deposito_bancario", "pix", "transferencia", "cashback" , "sem_pagamento", "outros" ] |  
| forma_pagamento_outros | string(60) | Descrição da forma de pagamento quando outras. | Não | Vazio | |  
| numero | string(30) | Número da fatura. | Não | Vazio | |  
| parcela | int | Número da parcela. | Não | Vazio | |  
| valor | decimal(2) | Valor da parcela. | Não | Vazio | |  
| vencimento | date | Data de vencimento. | Não | Vazio | |  
  
##### Status possíveis  
| HTTP Status | Descrição |  
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |  
| 201 | Criado com sucesso. |  
| 400 | Requisição inválida. Causas prováveis: a) faltando propriedades obrigatórias. b) erro de formatação do json de entrada; c) formato incorreto em alguma das propriedades; (*) |  
| 401 | Proibido acesso. Causa provável: falha na autenticação. (*) |  
| 403 | Proibida ação. Causa provável: falha na autorização. (*) |  
| 500 | Erro interno do servidor. Ver detalhes no corpo da resposta. |  
  
_(*): Ver disposições gerais sobre erros, para mais informações._  
  
##### Exemplo de chamada  
  
###### Requisição  
  
```http  
POST https://compras-api-inyrb33hja-uc.a.run.app/4312/pedidos HTTP/1.1  
X-API-Key: **************  
Content-Type: application/json  
  
  
{  
"numero": "9999",  
"numero_externo": "9999",  
"emissao": "2022-08-30",  
"previsao_de_entrega": "2022-09-10",  
"estabelecimento": "45.223.156/0001-70",  
"operacao_para_consumidor_final": "false",  
"fornecedor": "15.487.513/0001-46",  
"modalidade_frete": “por_conta_do_remetente”,  
"trasnportadora": "22.634.146/0001-21",  
"observacoes": " ... ",  
"total_itens": 80.00,  
"total_desconto": 0.00,  
"total_frete": 20.00,  
"total": 100.00,  
"situacao": "pendente",  
"itens": [  
{  
"produto": "123456",  
"quantidade": 4.00,  
"unidade": "UN",  
"valor_unitario": 20.00,  
"valor_frete": 20.00,  
"valor_desconto": 0.00,  
"total": 80.00,  
"considera_no_total": true  
},  
...  
],  
"cobrancas": [  
{  
"forma_pagamento": "dinheiro",  
"forma_pagamento_outros": "",  
"numero": "1234",  
"parcela": "1",  
"numero": "60",  
"valor": 100.00,  
"vencimento": "2022-08-30"  
},  
...  
],  
"criado_em": "2022-08-30T19:53:53",  
"criado_por": "teste@teste.com",  
"atualizado_em": "2022-08-30T19:53:53",  
"atualizado_por": "teste@teste.com",  
"grupo_empresarial": "1234",  
"tenant": 9999  
}  
}  
```  
  
###### Resposta  
  
```http  
HTTP/1.1 201 Created  
```  
  
### PUT Pedido  
  
> PUT /2531/pessoas-fisicas/{id}  
  
API para atualziação de um pedido, por meio de seu documento de identificação.  
  
#### Autenticação  
  
Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).  
  
#### Corpo da requisição  
  
  
*Pedido*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| numero | string(30) | Númeração interna única de controle do pedido | Não | Vazio | |  
| numero_externo | string(30) | Número externo do pedido | Não | Vazio | |  
| emissao | date | Data de emissão do pedido | Não | Vazio | |  
| previsao_de_entrega | date | Data prevista para entrega | Não | Vazio | |  
| estabelecimento | string(30) | CNPJ/CPF do estabelecimento | Sim | Vazio | |  
| operacao_para_consumidor_final | boolean | Identifica se o pedido é destinado para um consumidor final | Sim | false | |  
| fornecedor| string(30) | CNPJ/CPF do fornecedor| Sim | Vazio | |  
| modalidade_frete | string(30) | Modalidade do frete | Sim | Vazio | ["por_conta_do_remetente", "por_conta_do_destinatario", "por_conta_de_terceiros", "transporte_proprio_remetente", "transporte_proprio_destinatario", "sem_frete"] |  
| trasnportadora | string(30) | CNPJ/CPF da transportadora | Não | Vazio | |  
| observacoes | string(1000) | Observações do pedido | Não | Vazio | |  
| total_itens | decimal(2) | Valor total dos itens | Não | Vazio | |  
| total_desconto | decimal(2) | Valor total de desconto | Não | Vazio | |  
| total_frete | decimal(2) | Valor total de frete | Não | Vazio | |  
| total | decimal(2) | Valor total do pedido | Não | Vazio | |  
| situacao | string(30) | Situação do pedido | Não | Vazio | ["pendente", "aprovado"] |  
| itens | item | Itens do pedido | Não | Vazio | |  
| cobrancas | cobranca | Formas de cobrança do pedido | Não | Vazio | |  
| criado_em | datetime | Indica data e hora de cadastro do registro. | Não | now() | |  
| criado_por | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro. | Sim | Vazio | |  
| atualizado_em | datetime | Indica data e hora da última atualização do registro. | Não | Vazio | |  
| atualizado_por | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim | now() | |  
| apagado_em | datetime | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer. | Não | Vazio | |  
| grupo_empresarial | string(60) | Identificador do grupo empresarial (Código ou Identificador GUID) ao qual o registro pertence. | Sim | Vazio | |  
| tenant | bigint | Identificador do tenant ao qual o registro pertence. | Sim | Vazio | |  
  
  
*Pedido >> Item*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| produto | string(30) | IDentificador do produto (Código de barras ou código) | Não | Vazio | |  
| quantidade | decimal(4) | Quantidade solicitada | Sim | Vazio | |  
| unidade | string(6) | Unidade de comercialização | Sim | Vazio | |  
| valor_unitario | decimal(10) | Valor unitário aplicado | Sim | Vazio | |  
| valor_frete | decimal(2) | Valor do frete | Sim | Vazio | |  
| valor_desconto | decimal(2) | Valor do desconto | Sim | Vazio | |  
| total | decimal(2) | Valor total | Sim | Vazio | |  
| considera_no_total | boolean | Item impacta o total do pedido | Não | True | |  
  
  
*Pedido >> Cobrança*  
  
| Propriedade | Tipo | Descrição | Obrigatório | Default | Domínio |  
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |  
| forma_pagamento | string(30) | Identificador da forma de pagamento. | Não | Vazio | ["dinheiro ", "cheque", "cartao_de_credito", "cartao_de_debito", "credito_loja", "vale_alimentacao", "vale_refeicao", "vale_presente", "vale_combustivel", "boleto_bancario", "deposito_bancario", "pix", "transferencia", "cashback" , "sem_pagamento", "outros" ] |  
| forma_pagamento_outros | string(60) | Descrição da forma de pagamento quando outras. | Não | Vazio | |  
| numero | string(30) | Número da fatura. | Não | Vazio | |  
| parcela | int | Número da parcela. | Não | Vazio | |  
| valor | decimal(2) | Valor da parcela. | Não | Vazio | |  
| vencimento | date | Data de vencimento. | Não | Vazio | |  
  

##### Status possíveis  
| HTTP Status | Descrição |  
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |  
| 204 | Sem conteúdo. Atualizado com sucesso com sucesso. |  
| 400 | Requisição inválida. Causas prováveis: a) faltando propriedades obrigatórias. b) erro de formatação do json de entrada; c) formato incorreto em alguma das propriedades; (*) |  
| 401 | Proibido acesso. Causa provável: falha na autenticação. (*) |  
| 403 | Proibida ação. Causa provável: falha na autorização. (*) |  
| 404 | Não encontrado. Causa provável: identificador não existente no grupo_empresarial/tenant. (*) |  
| 500 | Erro interno do servidor. Ver detalhes no corpo da resposta. |  
  
_(*): Ver disposições gerais sobre erros, para mais informações._  
  
##### Exemplo de chamada  
  
###### Requisição  
  
```http  
PUT https://compras-api-inyrb33hja-uc.a.run.app/4312/pedidos/1234 HTTP/1.1  
X-API-Key: **************  
Content-Type: application/json  
  
  
{  
"numero": "9999",  
"numero_externo": "9999",  
"emissao": "2022-08-30",  
"previsao_de_entrega": "2022-09-10",  
"estabelecimento": "45.223.156/0001-70",  
"operacao_para_consumidor_final": "false",  
"fornecedor": "15.487.513/0001-46",  
"modalidade_frete": “por_conta_do_remetente”,  
"trasnportadora": "22.634.146/0001-21",  
"observacoes": " ... ",  
"total_itens": 80.00,  
"total_desconto": 0.00,  
"total_frete": 20.00,  
"total": 100.00,  
"situacao": "pendente",  
"itens": [  
{  
"produto": "123456",  
"quantidade": 4.00,  
"unidade": "UN",  
"valor_unitario": 20.00,  
"valor_frete": 20.00,  
"valor_desconto": 0.00,  
"total": 80.00,  
"considera_no_total": true  
},  
...  
],  
"cobrancas": [  
{  
"forma_pagamento": "dinheiro",  
"forma_pagamento_outros": "",  
"numero": "1234",  
"parcela": "1",  
"numero": "60",  
"valor": 100.00,  
"vencimento": "2022-08-30"  
},  
...  
],  
"criado_em": "2022-08-30T19:53:53",  
"criado_por": "teste@teste.com",  
"atualizado_em": "2022-08-30T19:53:53",  
"atualizado_por": "teste@teste.com",  
"grupo_empresarial": "1234",  
"tenant": 9999  
}  
}  
```  
  
###### Resposta  
  
```http  
HTTP/1.1 204 No Content  
```