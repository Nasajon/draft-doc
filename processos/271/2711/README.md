# 2711 - Emitir e enviar as faturas/notas fiscais aos clientes

Esta atividade tem por objetivo receber informações sobre fatos fiscais a serem emitidos no formato de documentação eletrônica oficial, bem como disponibilizar tais documentos aos clientes (DANFE e XML).

## Entidades

### Natureza

TODO

### Cobrança (TODO - avaliar se ficará aqui, e validar com o José)

Descreve as informações de cobrança de um fato financeiro.

#### Propriedades

##### Identificação

| Propriedade     | Tipo            | Descrição                                                   |
| --------------- | --------------- | ----------------------------------------------------------- |
| forma_pagamento | FORMA_PAGAMENTO | Forma de pagamento (consultar tipos padrões TODO).          |
| valor           | decimal(2)      | Valor monetário associado à cobrança (e forma de pagamento) |
| vencimento      | date            | Data de vencimento da cobrança                              |


Obs.: Além destas, devem se considerar também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizado_por]. Ver a sessão de disposições gerais das APIs do ERP Nasajon.

##### Complementares

| Propriedade | Tipo | Descrição |
| ----------- | ---- | --------- |

### Item de Venda

Itens de uma venda de mercadoria (ainda não emitida), descrevendo os produtos, valores, e outras questões de cunho gerencial e/ou fiscal.

#### Propriedades

##### Identificação

| Propriedade    | Tipo              | Descrição                                                                                                                  |
| -------------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| produto        | [EAN/CODIGO/UUID] | Identificador do produto correspondente (EAN = Global Trade Item Number do produto, antigo código EAN ou código de barras) |
| valor_unitario | decimal(10)       | Valor unitário do item                                                                                                     |
| quantidade     | decimal(4)        | Quantidade comercial                                                                                                       |
| unidade        | UNIDADE           | Código da unidade comercial (consultar códigos padrões TODO)                                                               |
| cfop           | CFOP              | Código Fiscal da Operaçao (consultar códigos padrões TODO)                                                                 |
| local_estoque  | [CODIGO/UUID]     | Identificador do local de estoque donde a mercadoria será retirada (se houver)                                             |

Obs.: Além destas, devem se considerar também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizado_por]. Ver a sessão de disposições gerais das APIs do ERP Nasajon.

##### Complementares

| Propriedade                        | Tipo           | Descrição                                                                       |
| ---------------------------------- | -------------- | ------------------------------------------------------------------------------- |
| valor_frete                        | decimal(2)     | Valor do transporte da mercadoria (se houver)                                   |
| valor_seguro                       | decimal(2)     | Valor do seguro de transporte da mercadoria                                     |
| valor_desconto                     | decimal(2)     | Valor monetário do desconto                                                     |
| conta_contabil (TODO Alinhar José) | CONTA_CONTABIL | Conta referêncial para contabilização automática (consultar tipos padrões TODO) |
| projeto                            | [CODIGO/UUID]  | Identificador do projeto                                                        |
| centro_resultado                   | [CODIGO/UUID]  | Identificador do centro de resultado (custo e lucro)                            |

### Endereco (TODO - Mover para local de endereço)

Endereço de participante de uma nota fiscal ou pedido.

#### Propriedades

##### Identificação

| Propriedade     | Tipo            | Descrição                                                                                              |
| --------------- | --------------- | ------------------------------------------------------------------------------------------------------ |
| cep             | string(60)      |                                                                                                        |
| tipo_logradouro | TIPO_LOGRADOURO | Identifica o tipo de logradouro do endereço (R = Rua, AV = Avendida, etc; TODO consultar tipos padrões |
| logradouro      | string(60)      |                                                                                                        |
| numero          | string(60)      |                                                                                                        |
| complemento     | string(60)      |                                                                                                        |
| nome_municipio  | string(60)      |                                                                                                        |
| uf              | string(2)       |                                                                                                        |

Obs.: Além destas, devem se considerar também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizado_por]. Ver a sessão de disposições gerais das APIs do ERP Nasajon.

##### Complementares

| Propriedade      | Tipo       | Descrição                                                               |
| ---------------- | ---------- | ----------------------------------------------------------------------- |
| bairro           | string(60) |                                                                         |
| codigo_municipio | string(60) | Código do município, ou código IBGE (exemplo: Rio de Janeiro = 3304557) |
| codigo_pais      | string(4)  | Código do país (exemplo: Brasil = 1058)                                 |

### Venda sem Pedido

Representa uma venda de mercadoria ou serviço efetuada (sem pedido), porém ainda não emitida.

#### Propriedades

##### Identificação

| Propriedade    | Tipo                   | Descrição                                                                              |
| -------------- | ---------------------- | -------------------------------------------------------------------------------------- |
| emitente       | [CNPJ/UUID/CODIGO/CPF] | Identificador do participante emitente da nota                                         |
| emitente_ie    | string(20)             | Inscrição Estadual do participante emitente da nota (se houver)                        |
| destinatario   | [CNPJ/UUID/CODIGO/CPF] | Identificador do participante destinatário da nota                                     |
| emitente_ie    | string(20)             | Inscrição Estadual do participante destinatário da nota (se houver)                    |
| numero_externo | string(10)             | Número do pedido ou da ordem de compra (um tipo de chave candidata do próprio cliente) |
| valor          | decimal(2)             | Valor total da venda                                                                   |
| data_saida     | date                   | Data de saída da mercadoria                                                            |
| natureza       | CODIGO                 | Natureza da venda                                                                      |

Obs.: Além destas, devem se considerar também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizado_por]. Ver a sessão de disposições gerais das APIs do ERP Nasajon.

* CODIGO = string(30) - TODO Colocar isso nas disposições gerais

##### Complementares

| Propriedade       | Tipo            | Descrição                                                 |
| ----------------- | --------------- | --------------------------------------------------------- |
| desconto          | decimal(2)      | Deconto total aplicado na venda                           |
| endereco_retirada | Endereco        | Endereço de retirada da mercadoria                        |
| endereco_entrega  | Endereco        | Endereço de entrega da mercadoria                         |
| itens             | Item de Venda[] | Lista de itens da venda (mercadorias)                     |
| moeda             | MOEDA           | Código da moeda utilizada (consultar tipos padrões TODO)  |
| cobranca          | Cobranca[]      | Informações sobre o modo de cobrança do venda (se houver) |

## Rotas

### Registrar ordem de faturamento de venda sem pedido

> POST /faturamento-api/2711/ordens-vendas-sem-pedido

API assíncrona para recepção de vendas sem pedido, a serem emitidas pelo ERP.

#### Corpo da requisição

> **Entidade principal:** Venda de mercadoria

_Os campos da entidade principal serão herdados, considerando as regras a seguir:_

##### Propriedades adicionais

| Propriedade       | Tipo          | Obrigatório | Descrição                                    |
| ----------------- | ------------- | ----------- | -------------------------------------------- |
| tenant            | [int8/CODIGO] | Sim         | Tenant da instalação do ERP                  |
| grupo_empresarial | [UUID/CODIGO] | Sim         | Grupo empresarial para persistencia da venda |

##### Propriedades opcionais

* venda.valor
* venda.data_saida
* venda.desconto
* venda.moeda
* venda.cobranca
* TODO ... continuar

##### Propriedades suprimidas

TODO

##### Exemplo de requisição

```json
[
    {
        "tenant": "GEDPROD",
        "grupo_empresarial": "NASAJON",
        "emitente": "07644677000101",
        "destinatario": "16.939.188/0001-78",
        ...
    },
    ...
]
```

#### Formato de Retorno

_Se tratando de uma requisição assíncrona, o retorno aponta para a URL de acompanhamento do processamento._

##### Status possíveis
| HTTP Status | Descrição                                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------------------------- |
| 202         | Solicitação aceita.                                                                                                 |
| 400         | Requisição inválida. Causa provável: formato de entrada incorreto, ou faltando dados. (*)                           |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                                         |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                            |
| 404         | Não encontrado. Entidade relacionada não encontrada (tenant, grupo empresarial, estabelecimento, produto, etc). (*) |
| 409         | Conflito. Causa provável: entidade já existente                                                                     |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                                        |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplos de retorno

###### Sucesso na requisição
```http
HTTP/1.1 202 Accepted
Location: /faturamento-api/2711/ordens-vendas-sem-pedido/cfbb4ece-cfd5-4bf5-aaec-f415e83352aa
```

###### Exemplo de erro

TODO

##### Códigos internos de erro

| Codigo    | Descrição |
| --------- | --------- |
| 2711-E001 | TODO      |
