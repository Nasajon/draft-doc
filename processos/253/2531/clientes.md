# Associação de Clientes

Gerencia todas as atividades relacionadas com a manutenção de clientes.

É importante destacar que "cliente" é um entidade fraca, figurando como um relacionamento entre uma pessoa física ou jurídica, e um estabelecimento.

## Rotas

### List Clientes

> GET /2531/clientes

API para listagem paginada das associações do tipo cliente.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Parâmetros da Requisição

| Parâmetro         | Tipo         | Descrição                                                                                      | Obrigatório | Domínio |
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------- | ----------- | ------- |
| grupo_empresarial | uuid         | Identificador do grupo empresarial para filtro dos registros.                                  | Sim         |         |
| tenant            | bigint       | Identificador do tenant para filtro dos registros.                                             | Sim         |         |
| criado_apos       | datetime     | Permite filtrar os registros criados após um determinada data e hora.                          | Não         |         |
| criado_antes      | datetime     | Permite filtrar os registros criados antes de uma determinada data e hora.                     | Não         |         |
| atualizado_apos   | datetime     | Permite filtrar os registros atualizados após um determinada data e hora.                      | Não         |         |
| atualizado_antes  | datetime     | Permite filtrar os registros atualizados antes de uma determinada data e hora.                 | Não         |         |
| apagado_apos      | datetime     | Permite filtrar os registros apagados após um determinada data e hora.                         | Não         |         |
| apagado_antes     | datetime     | Permite filtrar os registros apagados antes de uma determinada data e hora.                    | Não         |         |
| fields (*)        | list(string) | Permite indicar uma lista de campos a recuperar na chamada (exemplo: `fields=site,enderecos`). | Não         |         |

_(*) O parâmetro fields é o que permite a recuperação de qualquer propriedade do resgistro (além das propriedades de resumo, que sempre são trazidas, independentemente deste parâmetro)._

_Obs.: Qualquer dos campos de propriedades simples do registro (desconsiderando listas ou objetos aninhados), pode ser utilizado como filtro na requisição (sendo que será considerado um filtro de condição de igualdade, por exemplo: `codigo=XPTO` irá filtrar os registros com campo `codigo` igual a `XPTO`)._

#### Corpo da resposta

| Propriedade       | Tipo        | Descrição                                                                                                              | Obrigatório | Default | Domínio |
| ----------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- | ----------- | ------- | ------- |
| id                | UUID        | Identificador único (UUID) do relacionamento do tipo cliente.                                                          | Sim         | Vazio   |         |
| estabelecimento   | string(60)  | Identificador (geralmente CNPJ) do estabelecimento com o qual a pessoa (física ou jurídica) se relaciona como cliente. | Sim         | Vazio   |         |
| cliente           | string(60)  | Identificador da pessoa física ou jurídica, que figura como cliente, neste relacionamento.                             | Sim         | Vazio   |         |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                                            | Não         | now()   |         |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.                                       | Sim         | Vazio   |         |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                                                  | Não         | Vazio   |         |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro.                            | Sim         | now()   |         |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.                               | Não         | Vazio   |         |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                                                        | Sim         | Vazio   |         |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                                                   | Sim         | Vazio   |         |

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
GET https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/clientes?criado_apos=2022-08-09&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1
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
      "id": "359ce9d9-ce5f-47bb-bbad-f162c2eaa3f3",
      "estabelecimento": "82325117000100",
      "cliente": "71688968000109",
      "criado_em": "2022-08-12T00:00:00",
      "criado_por": "teste@teste.com",
      "atualizado_em": "2022-08-12T00:00:00",
      "atualizado_por": "teste@teste.com",
      "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
      "tenant": 9999
    }
  ]
}
```

### GET Cliente

> GET /2531/clientes/{id}

API para recuperação de uma associação do tipo cliente, por meio do ID único da associação (UUID).

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Parâmetros da Requisição

| Parâmetro         | Tipo         | Descrição                                                                                      | Obrigatório | Domínio |
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------- | ----------- | ------- |
| grupo_empresarial | uuid         | Identificador do grupo empresarial para filtro dos registros.                                  | Sim         |         |
| tenant            | bigint       | Identificador do tenant para filtro dos registros.                                             | Sim         |         |
| fields (*)        | list(string) | Permite indicar uma lista de campos a recuperar na chamada (exemplo: `fields=site,enderecos`). | Não         |         |

_(*) O parâmetro fields é o que permite a recuperação de qualquer propriedade do resgistro (além das propriedades de resumo, que sempre são trazidas, independentemente deste parâmetro)._

_Obs.: Qualquer dos campos de propriedades simples do registro (desconsiderando listas ou objetos aninhados), pode ser utilizado como filtro na requisição (sendo que será considerado um filtro de condição de igualdade, por exemplo: `codigo=XPTO` irá filtrar os registros com campo `codigo` igual a `XPTO`)._

#### Corpo da resposta

| Propriedade       | Tipo        | Descrição                                                                                                              | Obrigatório | Default | Domínio |
| ----------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- | ----------- | ------- | ------- |
| id                | UUID        | Identificador único (UUID) do relacionamento do tipo cliente.                                                          | Sim         | Vazio   |         |
| estabelecimento   | string(60)  | Identificador (geralmente CNPJ) do estabelecimento com o qual a pessoa (física ou jurídica) se relaciona como cliente. | Sim         | Vazio   |         |
| cliente           | string(60)  | Identificador da pessoa física ou jurídica, que figura como cliente, neste relacionamento.                             | Sim         | Vazio   |         |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                                            | Não         | now()   |         |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.                                       | Sim         | Vazio   |         |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                                                  | Não         | Vazio   |         |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro.                            | Sim         | now()   |         |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.                               | Não         | Vazio   |         |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                                                        | Sim         | Vazio   |         |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                                                   | Sim         | Vazio   |         |

##### Status possíveis
| HTTP Status | Descrição                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------- |
| 200         | Sucesso.                                                                                                |
| 400         | Requisição inválida. Causa provável: faltando parâmetros obrigatórios (tenant e grupo_empresarial). (*) |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                             |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                |
| 404         | Não encontrado. Causa provável: identificador não existente no grupo_empresarial/tenant. (*)            |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                            |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplo de chamada

###### Requisição

```http
GET https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/clientes/359ce9d9-ce5f-47bb-bbad-f162c2eaa3f3?tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1
X-API-Key: **************
Accept: application/json
```

###### Resposta

```http
HTTP/1.1 200 OK
content-type: application/json; charset=utf-8
Content-Encoding: gzip

{
  "id": "359ce9d9-ce5f-47bb-bbad-f162c2eaa3f3",
  "estabelecimento": "82325117000100",
  "cliente": "71688968000109",
  "criado_em": "2022-08-12T00:00:00",
  "criado_por": "teste@teste.com",
  "atualizado_em": "2022-08-12T00:00:00",
  "atualizado_por": "teste@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999
}
```

### POST Cliente

> POST /2531/clientes

API para gravação de uma nova associação do tipo "cliente" (entre um estabelecimento e uma pessoa física ou jurídica).

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade       | Tipo        | Descrição                                                                                                              | Obrigatório | Default | Domínio |
| ----------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- | ----------- | ------- | ------- |
| id                | UUID        | Identificador único (UUID) do relacionamento do tipo cliente.                                                          | Sim         | Vazio   |         |
| estabelecimento   | string(60)  | Identificador (geralmente CNPJ) do estabelecimento com o qual a pessoa (física ou jurídica) se relaciona como cliente. | Sim         | Vazio   |         |
| cliente           | string(60)  | Identificador da pessoa física ou jurídica, que figura como cliente, neste relacionamento.                             | Sim         | Vazio   |         |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                                            | Não         | now()   |         |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.                                       | Sim         | Vazio   |         |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                                                  | Não         | Vazio   |         |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro.                            | Sim         | now()   |         |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.                               | Não         | Vazio   |         |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                                                        | Sim         | Vazio   |         |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                                                   | Sim         | Vazio   |         |

##### Status possíveis
| HTTP Status | Descrição                                                                                                                                                                    |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 201         | Criado com sucesso.                                                                                                                                                          |
| 400         | Requisição inválida. Causas prováveis: a) faltando propriedades obrigatórias. b) erro de formatação do json de entrada; c) formato incorreto em alguma das propriedades; (*) |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                                                                                                  |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                                                                                     |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                                                                                                 |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplo de chamada

###### Requisição

```http
POST https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/clientes HTTP/1.1
X-API-Key: **************
Content-Type: application/json

{
  "estabelecimento": "82325117000100",
  "cliente": "12646442300",
  "criado_em": "2022-08-12T00:00:00",
  "criado_por": "teste@teste.com",
  "atualizado_em": "2022-08-12T00:00:00",
  "atualizado_por": "teste@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999
}
```

###### Resposta

```http
HTTP/1.1 201 Created
```

### PUT Pessoa Jurídica

> PUT /2531/clientes/{id}

API para atualziação de uma associação do tipo cliente, por meio do ID único da associação (UUID).

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade       | Tipo        | Descrição                                                                                                              | Obrigatório | Default | Domínio |
| ----------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- | ----------- | ------- | ------- |
| id                | UUID        | Identificador único (UUID) do relacionamento do tipo cliente.                                                          | Sim         | Vazio   |         |
| estabelecimento   | string(60)  | Identificador (geralmente CNPJ) do estabelecimento com o qual a pessoa (física ou jurídica) se relaciona como cliente. | Sim         | Vazio   |         |
| cliente           | string(60)  | Identificador da pessoa física ou jurídica, que figura como cliente, neste relacionamento.                             | Sim         | Vazio   |         |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                                            | Não         | now()   |         |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.                                       | Sim         | Vazio   |         |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                                                  | Não         | Vazio   |         |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro.                            | Sim         | now()   |         |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.                               | Não         | Vazio   |         |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                                                        | Sim         | Vazio   |         |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                                                   | Sim         | Vazio   |         |

##### Status possíveis
| HTTP Status | Descrição                                                                                                                                                                    |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 204         | Sem conteúdo. Atualizado com sucesso com sucesso.                                                                                                                            |
| 400         | Requisição inválida. Causas prováveis: a) faltando propriedades obrigatórias. b) erro de formatação do json de entrada; c) formato incorreto em alguma das propriedades; (*) |
| 401         | Proibido acesso. Causa provável: falha na autenticação. (*)                                                                                                                  |
| 403         | Proibida ação. Causa provável: falha na autorização. (*)                                                                                                                     |
| 404         | Não encontrado. Causa provável: identificador não existente no grupo_empresarial/tenant. (*)                                                                                 |
| 500         | Erro interno do servidor. Ver detalhes no corpo da resposta.                                                                                                                 |

_(*): Ver disposições gerais sobre erros, para mais informações._

##### Exemplo de chamada

###### Requisição

```http
PUT https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/clientes/359ce9d9-ce5f-47bb-bbad-f162c2eaa3f3 HTTP/1.1
X-API-Key: **************
Content-Type: application/json

{
  "id": "359ce9d9-ce5f-47bb-bbad-f162c2eaa3f3",
  "estabelecimento": "82325117000100",
  "cliente": "71688968000109",
  "criado_em": "2022-08-12T00:00:00",
  "criado_por": "teste@teste.com",
  "atualizado_em": "2022-08-12T12:00:00",
  "atualizado_por": "teste@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999
}
```

###### Resposta

```http
HTTP/1.1 204 No Content
```



