# Manutenção de Pessoas Jurídicas

Gerencia todas as atividades relacionadas com a manutenção das pessoas físicas.

## Rotas

### List Pessoa Jurídica

> GET /2531/pessoas-juridicas

API para listagem paginada de pessoas juríridicas.

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

| Propriedade       | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ----------------- | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo            | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao      | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome              | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_fantasia     | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).               | Não         | Vazio    |            | observacao | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site              | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo           | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| cno               | string(12)  | Cadastro Nacional de Obras.                                                                 | Não         | Vazio    |            |
| rntrc             | string(14)  | Registro Nacional de Transportadores Rodoviários de Cargas.                                 | Não         | Vazio    |            |
| cnae_principal    | string(7)   | Classificação Nacional de Atividade Econômica principal.                                    | Não         | False    |            |
| fpas              | string(6)   | Código de Fundo de Previdência e Assistência Social.                                        | Não         | False    |            |
| inicio_atividades | date        | Data de início das atividades (se pessoa jurídica ou similar).                              | Não         | Vazio    |            |
| final_atividades  | date        | Data de final das atividades (se pessoa jurídica ou similar).                               | Não         | Vazio    |            |
| logotipo_url      | string(250) | URL para imagem de logotipo do participante (normalmente pessoa jurídica).                  | Não         | Vazio    |            |
| ramo_atividade    | string(500) | Descrição do ramo de atividade principal do participante.                                   | Não         | Vazio    |            |
| cafir             | string(8)   | Cadastro de Imóveis Rurais.                                                                 | Não         | Vazio    |            |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |

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
GET https://dadosmestre-api-inyrb33hja-uc.a.run.app/dados-mestre/2531/pessoas-juridicas?fields=enderecos,nome_fantasia&criado_apos=2022-08-09&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad&id=71688968000109,71688968000109 HTTP/1.1
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
      "id": "71688968000109",
      "codigo": "PARTICIPANTE_TESTE_PESSOA_JUR",
      "qualificacao": "juridica",
      "nome": "Pessoa Jur\u00eddica de Teste",
      "nome_fantasia": "PJur",
      "inativo": true,
      "criado_em": "2022-08-30T19:53:45",
      "criado_por": "teste@teste.com",
      "atualizado_em": "2022-08-30T19:53:45",
      "atualizado_por": "teste@teste.com",
      "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
      "tenant": 9999,
      "enderecos": [
        {
          "id": "3f35089f-f6b7-48b1-b6a8-6c1e26908af4",
          "cep": "20090080",
          "tipo_logradouro": "rua",
          "logradouro": "Rua do Teste",
          "numero": "123",
          "complemento": null,
          "bairro": null,
          "uf": "RJ",
          "criado_em": "2022-08-12T00:00:00",
          "criado_por": "teste@teste.com",
          "atualizado_em": "2022-08-12T00:00:00",
          "atualizado_por": "teste@teste.com",
          "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
          "tenant": 9999
        }
      ]
    }
  ]
}
```

### GET Pessoa Jurídica

> GET /2531/pessoas-juridicas/{id}

API para recuperação de uma pessoa jurídica, por meio de seu documento de identificação.

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

| Propriedade       | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ----------------- | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo            | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao      | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome              | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_fantasia     | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).               | Não         | Vazio    |            | observacao | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site              | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo           | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| cno               | string(12)  | Cadastro Nacional de Obras.                                                                 | Não         | Vazio    |            |
| rntrc             | string(14)  | Registro Nacional de Transportadores Rodoviários de Cargas.                                 | Não         | Vazio    |            |
| cnae_principal    | string(7)   | Classificação Nacional de Atividade Econômica principal.                                    | Não         | False    |            |
| fpas              | string(6)   | Código de Fundo de Previdência e Assistência Social.                                        | Não         | False    |            |
| inicio_atividades | date        | Data de início das atividades (se pessoa jurídica ou similar).                              | Não         | Vazio    |            |
| final_atividades  | date        | Data de final das atividades (se pessoa jurídica ou similar).                               | Não         | Vazio    |            |
| logotipo_url      | string(250) | URL para imagem de logotipo do participante (normalmente pessoa jurídica).                  | Não         | Vazio    |            |
| ramo_atividade    | string(500) | Descrição do ramo de atividade principal do participante.                                   | Não         | Vazio    |            |
| cafir             | string(8)   | Cadastro de Imóveis Rurais.                                                                 | Não         | Vazio    |            |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |

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
GET https://dadosmestre-api-inyrb33hja-uc.a.run.app/dados-mestre/2531/pessoas-juridicas/71688968000109?fields=enderecos,nome_fantasia&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1
X-API-Key: **************
Accept: application/json
```

###### Resposta

```http
HTTP/1.1 200 OK
content-type: application/json; charset=utf-8
Content-Encoding: gzip

{
  "id": "71688968000109",
  "codigo": "PARTICIPANTE_TESTE_PESSOA_JUR",
  "qualificacao": "juridica",
  "nome": "Pessoa Jur\u00eddica de Teste",
  "nome_fantasia": "PJur",
  "inativo": true,
  "criado_em": "2022-08-30T19:53:45",
  "criado_por": "teste@teste.com",
  "atualizado_em": "2022-08-30T19:53:45",
  "atualizado_por": "teste@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999,
  "enderecos": [
    {
      "id": "3f35089f-f6b7-48b1-b6a8-6c1e26908af4",
      "cep": "20090080",
      "tipo_logradouro": "rua",
      "logradouro": "Rua do Teste",
      "numero": "123",
      "complemento": null,
      "bairro": null,
      "uf": "RJ",
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

### POST Pessoa Jurídica

> POST /2531/pessoas-juridicas

API para gravação de uma nova pessoa jurídica.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade       | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ----------------- | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo            | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao      | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome              | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_fantasia     | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).               | Não         | Vazio    |            | observacao | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site              | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo           | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| cno               | string(12)  | Cadastro Nacional de Obras.                                                                 | Não         | Vazio    |            |
| rntrc             | string(14)  | Registro Nacional de Transportadores Rodoviários de Cargas.                                 | Não         | Vazio    |            |
| cnae_principal    | string(7)   | Classificação Nacional de Atividade Econômica principal.                                    | Não         | False    |            |
| fpas              | string(6)   | Código de Fundo de Previdência e Assistência Social.                                        | Não         | False    |            |
| inicio_atividades | date        | Data de início das atividades (se pessoa jurídica ou similar).                              | Não         | Vazio    |            |
| final_atividades  | date        | Data de final das atividades (se pessoa jurídica ou similar).                               | Não         | Vazio    |            |
| logotipo_url      | string(250) | URL para imagem de logotipo do participante (normalmente pessoa jurídica).                  | Não         | Vazio    |            |
| ramo_atividade    | string(500) | Descrição do ramo de atividade principal do participante.                                   | Não         | Vazio    |            |
| cafir             | string(8)   | Cadastro de Imóveis Rurais.                                                                 | Não         | Vazio    |            |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |

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
POST https://dadosmestre-api-inyrb33hja-uc.a.run.app/dados-mestre/2531/pessoas-juridicas HTTP/1.1
X-API-Key: **************
Content-Type: application/json

{
  "id": "58773641000169",
  "codigo": "PARTICIPANTE2",
  "qualificacao": "juridica",
  "nome": "Nova pessoa jurídica",
  "nome_fantasia": "NOVAJur",
  "inativo": false,
  "criado_por": "email@teste.com",
  "atualizado_por": "email@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999,
  "enderecos": [
    {
      "cep": "20090075",
      "tipo_logradouro": "rua",
      "logradouro": "Rua do Novo",
      "numero": "456",
      "complemento": null,
      "bairro": null,
      "uf": "RJ",
      "criado_por": "email@teste.com",
      "atualizado_por": "email@teste.com"
    }
  ],
  "inscricoes_estaduais": [
    {
      "uf": "rj",
      "inscricao_estadual": "54657898524556",
      "criado_por": "email@teste.com",
      "atualizado_por": "email@teste.com"
    }
  ]
}
```

###### Resposta

```http
HTTP/1.1 201 Created
```

### PUT Pessoa Jurídica

> PUT /2531/pessoas-juridicas/{id}

API para atualziação de uma pessoa jurídica, por meio de seu documento de identificação.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade       | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ----------------- | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo            | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao      | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome              | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_fantasia     | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).               | Não         | Vazio    |            | observacao | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site              | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo           | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| cno               | string(12)  | Cadastro Nacional de Obras.                                                                 | Não         | Vazio    |            |
| rntrc             | string(14)  | Registro Nacional de Transportadores Rodoviários de Cargas.                                 | Não         | Vazio    |            |
| cnae_principal    | string(7)   | Classificação Nacional de Atividade Econômica principal.                                    | Não         | False    |            |
| fpas              | string(6)   | Código de Fundo de Previdência e Assistência Social.                                        | Não         | False    |            |
| inicio_atividades | date        | Data de início das atividades (se pessoa jurídica ou similar).                              | Não         | Vazio    |            |
| final_atividades  | date        | Data de final das atividades (se pessoa jurídica ou similar).                               | Não         | Vazio    |            |
| logotipo_url      | string(250) | URL para imagem de logotipo do participante (normalmente pessoa jurídica).                  | Não         | Vazio    |            |
| ramo_atividade    | string(500) | Descrição do ramo de atividade principal do participante.                                   | Não         | Vazio    |            |
| cafir             | string(8)   | Cadastro de Imóveis Rurais.                                                                 | Não         | Vazio    |            |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |

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
PUT https://dadosmestre-api-inyrb33hja-uc.a.run.app/dados-mestre/2531/pessoas-juridicas/58773641000169 HTTP/1.1
X-API-Key: **************
Content-Type: application/json

{
  "id": "58773641000169",
  "codigo": "PARTICIPANTE3",
  "qualificacao": "juridica",
  "nome": "Nova pessoa jurídica",
  "nome_fantasia": "NOVAJur5",
  "inativo": false,
  "criado_por": "email@teste.com",
  "atualizado_por": "email@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999,
  "enderecos": [
    {
      "cep": "20090075",
      "tipo_logradouro": "rua",
      "logradouro": "Rua do Novo",
      "numero": "456",
      "complemento": null,
      "bairro": null,
      "uf": "RJ",
      "criado_por": "email@teste.com",
      "atualizado_por": "email@teste.com"
    }
  ]
}
```

###### Resposta

```http
HTTP/1.1 204 No Content
```



