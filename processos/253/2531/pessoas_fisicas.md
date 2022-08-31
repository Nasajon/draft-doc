# Manutenção de Pessoas Físicas

Gerencia todas as atividades relacionadas com a manutenção das pessoas físicas.

## Rotas

### List Pessoa Física

> GET /2531/pessoas-fisicas

API para listagem paginada de pessoas físicas.

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

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                       | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo                   | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao             | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome                     | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_social              | string(150) | Nome social do participante (ou qualquer tipo de nomenclatura alternativa).                 | Não         | Vazio    |            | observacao                                                                                                                                                  | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site                     | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo                  | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| nome_social              | string(150) | Nome social da Pessoa Física.                                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| caepf                    | string(18)  | Cadastro de Atividade Econômica da Pessoa Física.                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nit                      | string(11)  | Número de Inscrição do Trabalhador.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nascimento               | date        | Data de nascimento do participante (se pessoa física).                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nacionalidade            | string(5)   | Código do país de nacionalidade.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| naturalidade             | string(2)   | UF de nascimento do participante.                                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ibge_nascimento          | string(8)   | Código do município de nascimento do participante.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| obito                    | date        | Data de falescimento do participante (se pessoa física).                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| foto_url                 | string(250) | URL para fotografia do participante (normalmente pessoal física).                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| raca                     | string(10)  | Classificação de cor ou raça (conforme o IBGE).                                             | Não         | Vazio    | Sensível   | ["branca", "parda", "preta", "amarela", "indigena"] _(1)_                                                                                                   |
| grau_instrucao           | string(20)  | Classificação de grau de instrução (conforme o IBGE).                                       | Não         | Vazio    | Sensível   | ["sem_instrucao", "fundamental_incompleto", "fundamental_completo", "medio_incompleto", "medio_completo", "superior_incompleto", "superior_completo"] _(2)_ |
| sexo                     | string(10)  | Sexo do participante.                                                                       | Não         | Vazio    | Sensível   | ["homem", "mulher"] _(3)_                                                                                                                                   |
| estado_civil             | string(10)  | Estado civil do participante.                                                               | Não         | Vazio    | Sensível   | ["casado", "desquitado_separado", "divorciado", "solteiro", "viuvo"] _(4)_                                                                                  |
| nome_pai                 | string(250) | Nome do pai do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nome_mae                 | string(250) | Nome da mãe do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| definiciente_visual      | boolean     | Indica que o participante possui algum tipo de deficiência visual.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_auditivo    | boolean     | Indica que o participante possui algum tipo de deficiÊncia auditiva.                        | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_intelectual | boolean     | Indica que o participante possui algum tipo de deficiência intelectual.                     | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_mental      | boolean     | Indica que o participante possui algum tipo de deficiência mental.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_fisico      | boolean     | Indica que o participante possui algum tipo de deficiência física.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| reabilitado              | boolean     | Indica que o participante é reabilitado.                                                    | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| ctps                     | string(11)  | Número da Carteira de Trabalho e Previcência Social.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_ctps               | string(5)   | Série da Carteira de Trabalho e Previcência Social.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ctps                  | string(2)   | Unidade Federativa de expedição da Carteira de Trabalho e Previcência Social.               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ctps                | data        | Data de expedição da Carteira de Trabalho e Previcência Social.                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ric                      | string(30)  | Número do Registro de Identidade Civil.                                                     | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_ric                | string(20)  | Órgão emissor do Registro de Identidade Civil.                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ric                 | data        | Data de emissão do Registro de Identidade Civil.                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ric                   | string(2)   | UF do Registro de Identidade Civil.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_ric               | string(60)  | Cidade de emissão do Registro de Identidade Civil.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| rg                       | string(30)  | Número do Registro Geral (identidade).                                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_rg                 | string(20)  | Órgão emissor do Registro Geral (identidade).                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_rg                  | date        | Data de emissão do Registro Geral (identidade).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_rg                    | string(2)   | Unidade Federativa de emissão do Registro Geral (identidade).                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cnh                      | string(30)  | Número da Carteira Nacional de Habilitação.                                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_cnh                | string(20)  | Órgão emissor da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_cnh                   | string(2)   | UF de emissão da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cnh                 | date        | Data de emissão da Carteira Nacional de Habilitação.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_cnh             | date        | Data de Validade da Carteira Nacional de Habilitação.                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_primeira_cnh        | date        | Data de emissão da primeira Carteira Nacional de Habilitação.                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| categoria_cnh            | string(3)   | Categoria da Carteira Nacional de Habilitação.                                              | Não         | Vazio    | Pessoal    | ["a", "b", "c", "d", "e", "ab", "ac", "ad", "ae"]                                                                                                           |
| pais_passaporte          | string(8)   | Código do país de emissão do Passaporte.                                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_passaporte         | string(20)  | Órgão emissor do Passaporte.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_passaporte            | string(2)   | Unidade Federativa de emissão do Passaporte.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_passaporte          | date        | Data de emissão do Passaporte.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_passaporte      | date        | Data de validade do Passaporte.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nis                      | string(11)  | Número de Identificação Social.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| te                       | string(15)  | Número do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| zona_te                  | string(3)   | Zona do Título de Eleitor.                                                                  | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| secao_te                 | string(4)   | Sessão do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_te                    | string(2)   | Unidade Federativa de emissão do Título de Eleitor.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| atestado_obito           | string(32)  | Número do atestado de óbito.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_certidao            | string(10)  | Tipo da Certidão.                                                                           | Não         | Vazio    | Pessoal    | ["nascimento", "casamento"]                                                                                                                                 |
| numero_certidao          | string(30)  | Número da Certidão.                                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| livro_certidao           | string(15)  | Número do livro da Certidão.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| folha_certidao           | string(15)  | Número da folha do livro da Certidão.                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_certidao            | date        | Data de expedição da Certidão.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_certidao          | string(60)  | Cidade de expedição da Certidão.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_certidao              | string(2)   | Unidade Federativa de expedição da Certidão.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cartorio_certidao        | string(30)  | Identificação do cartório de expedição da Certidão.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cr                       | string(15)  | Número do Certificado de Registro (no exército).                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_cr                 | string(5)   | Série do Certificado de Registro (no exército).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cr                  | date        | Data de expedição do Certificado de Registro (no exército).                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_sanguineo           | string(2)   | Tipo sanguíneo do participante                                                              | Não         | Vazio    | Pessoal    | ["a", "b", "o", "ab"]                                                                                                                                       |
| criado_em                | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por               | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em            | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por           | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em               | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial        | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant                   | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |


_(1) As opções estão de acordo com as definições [oficiais do IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18319-cor-ou-raca.html#:~:text=De%20acordo%20com%20dados%20da,1%25%20como%20amarelos%20ou%20ind%C3%ADgenas.), inclusive há uma [reportagem da Gazeta](https://www.agazeta.com.br/es/gv/preto-ou-negro-ibge-explica-classificacao-de-cor-e-raca-em-pesquisas-1118), explicando o uso do termo "preta", e não "negro" (visto que "preto" é entendido como subconjunto de "negro", assim como "pardo")._

_(2) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://biblioteca.ibge.gov.br/visualizacao/livros/liv101736_informativo.pdf)._

_(3) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://brasilemsintese.ibge.gov.br/populacao/distribuicao-da-populacao-por-sexo.html)._

_(4) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://cidades.ibge.gov.br/brasil/pesquisa/23/22714)._

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
GET https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/pessoas-fisicas?fields=enderecos&criado_apos=2022-08-09&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1
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
      "id": "92751920306",
      "codigo": "FULANO_SILVA",
      "qualificacao": "fisica",
      "nome": "Fulano da Silva",
      "inativo": true,
      "criado_em": "2022-08-30T19:53:53",
      "criado_por": "teste@teste.com",
      "atualizado_em": "2022-08-30T19:53:53",
      "atualizado_por": "teste@teste.com",
      "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
      "tenant": 9999,
      "enderecos": [
        {
          "id": "c183da59-8098-426d-a16e-29c95d0dd765",
          "cep": "20050456",
          "tipo_logradouro": "rua",
          "logradouro": "Rua do Ouvidor",
          "numero": "60",
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

### GET Pessoa Física

> GET /2531/pessoas-fisicas/{id}

API para recuperação de uma pessoa física, por meio de seu documento de identificação.

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

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                       | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo                   | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao             | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome                     | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_social              | string(150) | Nome social do participante (ou qualquer tipo de nomenclatura alternativa).                 | Não         | Vazio    |            | observacao                                                                                                                                                  | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site                     | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo                  | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| nome_social              | string(150) | Nome social da Pessoa Física.                                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| caepf                    | string(18)  | Cadastro de Atividade Econômica da Pessoa Física.                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nit                      | string(11)  | Número de Inscrição do Trabalhador.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nascimento               | date        | Data de nascimento do participante (se pessoa física).                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nacionalidade            | string(5)   | Código do país de nacionalidade.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| naturalidade             | string(2)   | UF de nascimento do participante.                                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ibge_nascimento          | string(8)   | Código do município de nascimento do participante.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| obito                    | date        | Data de falescimento do participante (se pessoa física).                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| foto_url                 | string(250) | URL para fotografia do participante (normalmente pessoal física).                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| raca                     | string(10)  | Classificação de cor ou raça (conforme o IBGE).                                             | Não         | Vazio    | Sensível   | ["branca", "parda", "preta", "amarela", "indigena"] _(1)_                                                                                                   |
| grau_instrucao           | string(20)  | Classificação de grau de instrução (conforme o IBGE).                                       | Não         | Vazio    | Sensível   | ["sem_instrucao", "fundamental_incompleto", "fundamental_completo", "medio_incompleto", "medio_completo", "superior_incompleto", "superior_completo"] _(2)_ |
| sexo                     | string(10)  | Sexo do participante.                                                                       | Não         | Vazio    | Sensível   | ["homem", "mulher"] _(3)_                                                                                                                                   |
| estado_civil             | string(10)  | Estado civil do participante.                                                               | Não         | Vazio    | Sensível   | ["casado", "desquitado_separado", "divorciado", "solteiro", "viuvo"] _(4)_                                                                                  |
| nome_pai                 | string(250) | Nome do pai do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nome_mae                 | string(250) | Nome da mãe do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| definiciente_visual      | boolean     | Indica que o participante possui algum tipo de deficiência visual.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_auditivo    | boolean     | Indica que o participante possui algum tipo de deficiÊncia auditiva.                        | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_intelectual | boolean     | Indica que o participante possui algum tipo de deficiência intelectual.                     | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_mental      | boolean     | Indica que o participante possui algum tipo de deficiência mental.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_fisico      | boolean     | Indica que o participante possui algum tipo de deficiência física.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| reabilitado              | boolean     | Indica que o participante é reabilitado.                                                    | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| ctps                     | string(11)  | Número da Carteira de Trabalho e Previcência Social.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_ctps               | string(5)   | Série da Carteira de Trabalho e Previcência Social.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ctps                  | string(2)   | Unidade Federativa de expedição da Carteira de Trabalho e Previcência Social.               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ctps                | data        | Data de expedição da Carteira de Trabalho e Previcência Social.                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ric                      | string(30)  | Número do Registro de Identidade Civil.                                                     | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_ric                | string(20)  | Órgão emissor do Registro de Identidade Civil.                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ric                 | data        | Data de emissão do Registro de Identidade Civil.                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ric                   | string(2)   | UF do Registro de Identidade Civil.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_ric               | string(60)  | Cidade de emissão do Registro de Identidade Civil.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| rg                       | string(30)  | Número do Registro Geral (identidade).                                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_rg                 | string(20)  | Órgão emissor do Registro Geral (identidade).                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_rg                  | date        | Data de emissão do Registro Geral (identidade).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_rg                    | string(2)   | Unidade Federativa de emissão do Registro Geral (identidade).                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cnh                      | string(30)  | Número da Carteira Nacional de Habilitação.                                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_cnh                | string(20)  | Órgão emissor da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_cnh                   | string(2)   | UF de emissão da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cnh                 | date        | Data de emissão da Carteira Nacional de Habilitação.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_cnh             | date        | Data de Validade da Carteira Nacional de Habilitação.                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_primeira_cnh        | date        | Data de emissão da primeira Carteira Nacional de Habilitação.                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| categoria_cnh            | string(3)   | Categoria da Carteira Nacional de Habilitação.                                              | Não         | Vazio    | Pessoal    | ["a", "b", "c", "d", "e", "ab", "ac", "ad", "ae"]                                                                                                           |
| pais_passaporte          | string(8)   | Código do país de emissão do Passaporte.                                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_passaporte         | string(20)  | Órgão emissor do Passaporte.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_passaporte            | string(2)   | Unidade Federativa de emissão do Passaporte.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_passaporte          | date        | Data de emissão do Passaporte.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_passaporte      | date        | Data de validade do Passaporte.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nis                      | string(11)  | Número de Identificação Social.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| te                       | string(15)  | Número do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| zona_te                  | string(3)   | Zona do Título de Eleitor.                                                                  | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| secao_te                 | string(4)   | Sessão do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_te                    | string(2)   | Unidade Federativa de emissão do Título de Eleitor.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| atestado_obito           | string(32)  | Número do atestado de óbito.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_certidao            | string(10)  | Tipo da Certidão.                                                                           | Não         | Vazio    | Pessoal    | ["nascimento", "casamento"]                                                                                                                                 |
| numero_certidao          | string(30)  | Número da Certidão.                                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| livro_certidao           | string(15)  | Número do livro da Certidão.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| folha_certidao           | string(15)  | Número da folha do livro da Certidão.                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_certidao            | date        | Data de expedição da Certidão.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_certidao          | string(60)  | Cidade de expedição da Certidão.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_certidao              | string(2)   | Unidade Federativa de expedição da Certidão.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cartorio_certidao        | string(30)  | Identificação do cartório de expedição da Certidão.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cr                       | string(15)  | Número do Certificado de Registro (no exército).                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_cr                 | string(5)   | Série do Certificado de Registro (no exército).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cr                  | date        | Data de expedição do Certificado de Registro (no exército).                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_sanguineo           | string(2)   | Tipo sanguíneo do participante                                                              | Não         | Vazio    | Pessoal    | ["a", "b", "o", "ab"]                                                                                                                                       |
| criado_em                | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por               | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em            | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por           | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em               | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial        | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant                   | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |


_(1) As opções estão de acordo com as definições [oficiais do IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18319-cor-ou-raca.html#:~:text=De%20acordo%20com%20dados%20da,1%25%20como%20amarelos%20ou%20ind%C3%ADgenas.), inclusive há uma [reportagem da Gazeta](https://www.agazeta.com.br/es/gv/preto-ou-negro-ibge-explica-classificacao-de-cor-e-raca-em-pesquisas-1118), explicando o uso do termo "preta", e não "negro" (visto que "preto" é entendido como subconjunto de "negro", assim como "pardo")._

_(2) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://biblioteca.ibge.gov.br/visualizacao/livros/liv101736_informativo.pdf)._

_(3) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://brasilemsintese.ibge.gov.br/populacao/distribuicao-da-populacao-por-sexo.html)._

_(4) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://cidades.ibge.gov.br/brasil/pesquisa/23/22714)._

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
GET https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/pessoas-fisicas/92751920306?fields=enderecos,emails,telefones&tenant=9999&grupo_empresarial=728ddc4a-242f-45f0-a752-f3e46f9993ad HTTP/1.1
X-API-Key: **************
Accept: application/json
```

###### Resposta

```http
HTTP/1.1 200 OK
content-type: application/json; charset=utf-8
Content-Encoding: gzip

{
  "id": "92751920306",
  "codigo": "FULANO_SILVA",
  "qualificacao": "fisica",
  "nome": "Fulano da Silva",
  "inativo": true,
  "criado_em": "2022-08-30T19:53:53",
  "criado_por": "teste@teste.com",
  "atualizado_em": "2022-08-30T19:53:53",
  "atualizado_por": "teste@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999,
  "emails": [
    {
      "id": "1fa08ffd-c166-483c-b3ef-203823dd2df7",
      "email": "fulano@gmail.com",
      "descricao": null,
      "principal": false,
      "criado_em": "2022-08-29T00:00:00",
      "criado_por": "teste@teste.com",
      "atualizado_em": "2022-08-29T00:00:00",
      "atualizado_por": "teste@teste.com",
      "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
      "tenant": 9999
    }
  ],
  "enderecos": [
    {
      "id": "c183da59-8098-426d-a16e-29c95d0dd765",
      "cep": "20050456",
      "tipo_logradouro": "rua",
      "logradouro": "Rua do Ouvidor",
      "numero": "60",
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
  ],
  "telefones": [
    {
      "id": "b7c648af-5d47-4117-9f9a-d1dbc3a6b143",
      "descricao": "Cel 1",
      "principal": false,
      "tipo": "celular",
      "numero": "989785456",
      "criado_em": "2022-08-29T00:00:00",
      "criado_por": "teste@teste.com",
      "atualizado_em": "2022-08-29T00:00:00",
      "atualizado_por": "teste@teste.com",
      "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
      "tenant": 9999
    }
  ]
}
```

### POST Pessoa Física

> POST /2531/pessoas-fisicas

API para gravação de uma nova pessoa física.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                       | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo                   | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao             | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome                     | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_social              | string(150) | Nome social do participante (ou qualquer tipo de nomenclatura alternativa).                 | Não         | Vazio    |            | observacao                                                                                                                                                  | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site                     | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo                  | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| nome_social              | string(150) | Nome social da Pessoa Física.                                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| caepf                    | string(18)  | Cadastro de Atividade Econômica da Pessoa Física.                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nit                      | string(11)  | Número de Inscrição do Trabalhador.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nascimento               | date        | Data de nascimento do participante (se pessoa física).                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nacionalidade            | string(5)   | Código do país de nacionalidade.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| naturalidade             | string(2)   | UF de nascimento do participante.                                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ibge_nascimento          | string(8)   | Código do município de nascimento do participante.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| obito                    | date        | Data de falescimento do participante (se pessoa física).                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| foto_url                 | string(250) | URL para fotografia do participante (normalmente pessoal física).                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| raca                     | string(10)  | Classificação de cor ou raça (conforme o IBGE).                                             | Não         | Vazio    | Sensível   | ["branca", "parda", "preta", "amarela", "indigena"] _(1)_                                                                                                   |
| grau_instrucao           | string(20)  | Classificação de grau de instrução (conforme o IBGE).                                       | Não         | Vazio    | Sensível   | ["sem_instrucao", "fundamental_incompleto", "fundamental_completo", "medio_incompleto", "medio_completo", "superior_incompleto", "superior_completo"] _(2)_ |
| sexo                     | string(10)  | Sexo do participante.                                                                       | Não         | Vazio    | Sensível   | ["homem", "mulher"] _(3)_                                                                                                                                   |
| estado_civil             | string(10)  | Estado civil do participante.                                                               | Não         | Vazio    | Sensível   | ["casado", "desquitado_separado", "divorciado", "solteiro", "viuvo"] _(4)_                                                                                  |
| nome_pai                 | string(250) | Nome do pai do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nome_mae                 | string(250) | Nome da mãe do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| definiciente_visual      | boolean     | Indica que o participante possui algum tipo de deficiência visual.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_auditivo    | boolean     | Indica que o participante possui algum tipo de deficiÊncia auditiva.                        | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_intelectual | boolean     | Indica que o participante possui algum tipo de deficiência intelectual.                     | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_mental      | boolean     | Indica que o participante possui algum tipo de deficiência mental.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_fisico      | boolean     | Indica que o participante possui algum tipo de deficiência física.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| reabilitado              | boolean     | Indica que o participante é reabilitado.                                                    | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| ctps                     | string(11)  | Número da Carteira de Trabalho e Previcência Social.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_ctps               | string(5)   | Série da Carteira de Trabalho e Previcência Social.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ctps                  | string(2)   | Unidade Federativa de expedição da Carteira de Trabalho e Previcência Social.               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ctps                | data        | Data de expedição da Carteira de Trabalho e Previcência Social.                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ric                      | string(30)  | Número do Registro de Identidade Civil.                                                     | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_ric                | string(20)  | Órgão emissor do Registro de Identidade Civil.                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ric                 | data        | Data de emissão do Registro de Identidade Civil.                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ric                   | string(2)   | UF do Registro de Identidade Civil.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_ric               | string(60)  | Cidade de emissão do Registro de Identidade Civil.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| rg                       | string(30)  | Número do Registro Geral (identidade).                                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_rg                 | string(20)  | Órgão emissor do Registro Geral (identidade).                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_rg                  | date        | Data de emissão do Registro Geral (identidade).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_rg                    | string(2)   | Unidade Federativa de emissão do Registro Geral (identidade).                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cnh                      | string(30)  | Número da Carteira Nacional de Habilitação.                                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_cnh                | string(20)  | Órgão emissor da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_cnh                   | string(2)   | UF de emissão da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cnh                 | date        | Data de emissão da Carteira Nacional de Habilitação.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_cnh             | date        | Data de Validade da Carteira Nacional de Habilitação.                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_primeira_cnh        | date        | Data de emissão da primeira Carteira Nacional de Habilitação.                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| categoria_cnh            | string(3)   | Categoria da Carteira Nacional de Habilitação.                                              | Não         | Vazio    | Pessoal    | ["a", "b", "c", "d", "e", "ab", "ac", "ad", "ae"]                                                                                                           |
| pais_passaporte          | string(8)   | Código do país de emissão do Passaporte.                                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_passaporte         | string(20)  | Órgão emissor do Passaporte.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_passaporte            | string(2)   | Unidade Federativa de emissão do Passaporte.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_passaporte          | date        | Data de emissão do Passaporte.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_passaporte      | date        | Data de validade do Passaporte.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nis                      | string(11)  | Número de Identificação Social.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| te                       | string(15)  | Número do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| zona_te                  | string(3)   | Zona do Título de Eleitor.                                                                  | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| secao_te                 | string(4)   | Sessão do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_te                    | string(2)   | Unidade Federativa de emissão do Título de Eleitor.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| atestado_obito           | string(32)  | Número do atestado de óbito.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_certidao            | string(10)  | Tipo da Certidão.                                                                           | Não         | Vazio    | Pessoal    | ["nascimento", "casamento"]                                                                                                                                 |
| numero_certidao          | string(30)  | Número da Certidão.                                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| livro_certidao           | string(15)  | Número do livro da Certidão.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| folha_certidao           | string(15)  | Número da folha do livro da Certidão.                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_certidao            | date        | Data de expedição da Certidão.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_certidao          | string(60)  | Cidade de expedição da Certidão.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_certidao              | string(2)   | Unidade Federativa de expedição da Certidão.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cartorio_certidao        | string(30)  | Identificação do cartório de expedição da Certidão.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cr                       | string(15)  | Número do Certificado de Registro (no exército).                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_cr                 | string(5)   | Série do Certificado de Registro (no exército).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cr                  | date        | Data de expedição do Certificado de Registro (no exército).                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_sanguineo           | string(2)   | Tipo sanguíneo do participante                                                              | Não         | Vazio    | Pessoal    | ["a", "b", "o", "ab"]                                                                                                                                       |
| criado_em                | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por               | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em            | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por           | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em               | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial        | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant                   | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |


_(1) As opções estão de acordo com as definições [oficiais do IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18319-cor-ou-raca.html#:~:text=De%20acordo%20com%20dados%20da,1%25%20como%20amarelos%20ou%20ind%C3%ADgenas.), inclusive há uma [reportagem da Gazeta](https://www.agazeta.com.br/es/gv/preto-ou-negro-ibge-explica-classificacao-de-cor-e-raca-em-pesquisas-1118), explicando o uso do termo "preta", e não "negro" (visto que "preto" é entendido como subconjunto de "negro", assim como "pardo")._

_(2) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://biblioteca.ibge.gov.br/visualizacao/livros/liv101736_informativo.pdf)._

_(3) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://brasilemsintese.ibge.gov.br/populacao/distribuicao-da-populacao-por-sexo.html)._

_(4) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://cidades.ibge.gov.br/brasil/pesquisa/23/22714)._

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
POST https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/pessoas-fisicas HTTP/1.1
X-API-Key: **************
Content-Type: application/json

{
  "id": "12646442300",
  "codigo": "CICLANO",
  "nome": "Ciclano Silveira",
  "inativo": false,
  "criado_por": "email@teste.com",
  "atualizado_por": "email@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999,
  "enderecos": [
    {
      "cep": "23456765",
      "tipo_logradouro": "rua",
      "logradouro": "Avenoda Presidente Vargas",
      "numero": "1500",
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
HTTP/1.1 201 Created
```

### PUT Pessoa Física

> PUT /2531/pessoas-fisicas/{id}

API para atualziação de uma pessoa física, por meio de seu documento de identificação.

#### Autenticação

Deve-se enviar o header `X-API-Key` com uma API-Key válida (obtida na contratação do serviço).

#### Corpo da requisição

| Propriedade              | Tipo        | Descrição                                                                                   | Obrigatório | Default  | Domínio    |
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------- | ----------- | -------- | ---------- |
| id                       | string(60)  | CPF da pessoa física.                                                                       | Sim         | Vazio    |            |
| codigo                   | string(30)  | Identificador único, do ponto de vista do usuário final.                                    | Sim         | Vazio    |            |
| qualificacao             | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                          | Sim         | "fisica" | ["fisica"] |
| nome                     | string(150) | Nome do participante.                                                                       | Sim         | Vazio    |            |
| nome_social              | string(150) | Nome social do participante (ou qualquer tipo de nomenclatura alternativa).                 | Não         | Vazio    |            | observacao                                                                                                                                                  | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| site                     | string(250) | Endereço eletrônico do participante.                                                        | Não         | Vazio    |            |
| inativo                  | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).  | Sim         | False    |            |
| nome_social              | string(150) | Nome social da Pessoa Física.                                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| caepf                    | string(18)  | Cadastro de Atividade Econômica da Pessoa Física.                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nit                      | string(11)  | Número de Inscrição do Trabalhador.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nascimento               | date        | Data de nascimento do participante (se pessoa física).                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nacionalidade            | string(5)   | Código do país de nacionalidade.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| naturalidade             | string(2)   | UF de nascimento do participante.                                                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ibge_nascimento          | string(8)   | Código do município de nascimento do participante.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| obito                    | date        | Data de falescimento do participante (se pessoa física).                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| foto_url                 | string(250) | URL para fotografia do participante (normalmente pessoal física).                           | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| raca                     | string(10)  | Classificação de cor ou raça (conforme o IBGE).                                             | Não         | Vazio    | Sensível   | ["branca", "parda", "preta", "amarela", "indigena"] _(1)_                                                                                                   |
| grau_instrucao           | string(20)  | Classificação de grau de instrução (conforme o IBGE).                                       | Não         | Vazio    | Sensível   | ["sem_instrucao", "fundamental_incompleto", "fundamental_completo", "medio_incompleto", "medio_completo", "superior_incompleto", "superior_completo"] _(2)_ |
| sexo                     | string(10)  | Sexo do participante.                                                                       | Não         | Vazio    | Sensível   | ["homem", "mulher"] _(3)_                                                                                                                                   |
| estado_civil             | string(10)  | Estado civil do participante.                                                               | Não         | Vazio    | Sensível   | ["casado", "desquitado_separado", "divorciado", "solteiro", "viuvo"] _(4)_                                                                                  |
| nome_pai                 | string(250) | Nome do pai do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nome_mae                 | string(250) | Nome da mãe do participante.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| definiciente_visual      | boolean     | Indica que o participante possui algum tipo de deficiência visual.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_auditivo    | boolean     | Indica que o participante possui algum tipo de deficiÊncia auditiva.                        | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_intelectual | boolean     | Indica que o participante possui algum tipo de deficiência intelectual.                     | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_mental      | boolean     | Indica que o participante possui algum tipo de deficiência mental.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| definiciente_fisico      | boolean     | Indica que o participante possui algum tipo de deficiência física.                          | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| reabilitado              | boolean     | Indica que o participante é reabilitado.                                                    | Não         | Vazio    | Sensível   |                                                                                                                                                             |
| ctps                     | string(11)  | Número da Carteira de Trabalho e Previcência Social.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_ctps               | string(5)   | Série da Carteira de Trabalho e Previcência Social.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ctps                  | string(2)   | Unidade Federativa de expedição da Carteira de Trabalho e Previcência Social.               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ctps                | data        | Data de expedição da Carteira de Trabalho e Previcência Social.                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| ric                      | string(30)  | Número do Registro de Identidade Civil.                                                     | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_ric                | string(20)  | Órgão emissor do Registro de Identidade Civil.                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_ric                 | data        | Data de emissão do Registro de Identidade Civil.                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_ric                   | string(2)   | UF do Registro de Identidade Civil.                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_ric               | string(60)  | Cidade de emissão do Registro de Identidade Civil.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| rg                       | string(30)  | Número do Registro Geral (identidade).                                                      | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_rg                 | string(20)  | Órgão emissor do Registro Geral (identidade).                                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_rg                  | date        | Data de emissão do Registro Geral (identidade).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_rg                    | string(2)   | Unidade Federativa de emissão do Registro Geral (identidade).                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cnh                      | string(30)  | Número da Carteira Nacional de Habilitação.                                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_cnh                | string(20)  | Órgão emissor da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_cnh                   | string(2)   | UF de emissão da Carteira Nacional de Habilitação.                                          | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cnh                 | date        | Data de emissão da Carteira Nacional de Habilitação.                                        | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_cnh             | date        | Data de Validade da Carteira Nacional de Habilitação.                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_primeira_cnh        | date        | Data de emissão da primeira Carteira Nacional de Habilitação.                               | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| categoria_cnh            | string(3)   | Categoria da Carteira Nacional de Habilitação.                                              | Não         | Vazio    | Pessoal    | ["a", "b", "c", "d", "e", "ab", "ac", "ad", "ae"]                                                                                                           |
| pais_passaporte          | string(8)   | Código do país de emissão do Passaporte.                                                    | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| orgao_passaporte         | string(20)  | Órgão emissor do Passaporte.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_passaporte            | string(2)   | Unidade Federativa de emissão do Passaporte.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_passaporte          | date        | Data de emissão do Passaporte.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| validade_passaporte      | date        | Data de validade do Passaporte.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| nis                      | string(11)  | Número de Identificação Social.                                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| te                       | string(15)  | Número do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| zona_te                  | string(3)   | Zona do Título de Eleitor.                                                                  | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| secao_te                 | string(4)   | Sessão do Título de Eleitor.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_te                    | string(2)   | Unidade Federativa de emissão do Título de Eleitor.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| atestado_obito           | string(32)  | Número do atestado de óbito.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_certidao            | string(10)  | Tipo da Certidão.                                                                           | Não         | Vazio    | Pessoal    | ["nascimento", "casamento"]                                                                                                                                 |
| numero_certidao          | string(30)  | Número da Certidão.                                                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| livro_certidao           | string(15)  | Número do livro da Certidão.                                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| folha_certidao           | string(15)  | Número da folha do livro da Certidão.                                                       | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_certidao            | date        | Data de expedição da Certidão.                                                              | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cidade_certidao          | string(60)  | Cidade de expedição da Certidão.                                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| uf_certidao              | string(2)   | Unidade Federativa de expedição da Certidão.                                                | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cartorio_certidao        | string(30)  | Identificação do cartório de expedição da Certidão.                                         | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| cr                       | string(15)  | Número do Certificado de Registro (no exército).                                            | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| serie_cr                 | string(5)   | Série do Certificado de Registro (no exército).                                             | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| data_cr                  | date        | Data de expedição do Certificado de Registro (no exército).                                 | Não         | Vazio    | Pessoal    |                                                                                                                                                             |
| tipo_sanguineo           | string(2)   | Tipo sanguíneo do participante                                                              | Não         | Vazio    | Pessoal    | ["a", "b", "o", "ab"]                                                                                                                                       |
| criado_em                | datetime    | Indica data e hora de cadastro do registro.                                                 | Não         | now()    |            |
| criado_por               | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.            | Sim         | Vazio    |            |
| atualizado_em            | datetime    | Indica data e hora da última atualização do registro.                                       | Não         | Vazio    |            |
| atualizado_por           | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro. | Sim         | now()    |            |
| apagado_em               | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.    | Não         | Vazio    |            |
| grupo_empresarial        | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                             | Sim         | Vazio    |            |
| tenant                   | bigint      | Identificador do tenant ao qual o registro pertence.                                        | Sim         | Vazio    |            |


_(1) As opções estão de acordo com as definições [oficiais do IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18319-cor-ou-raca.html#:~:text=De%20acordo%20com%20dados%20da,1%25%20como%20amarelos%20ou%20ind%C3%ADgenas.), inclusive há uma [reportagem da Gazeta](https://www.agazeta.com.br/es/gv/preto-ou-negro-ibge-explica-classificacao-de-cor-e-raca-em-pesquisas-1118), explicando o uso do termo "preta", e não "negro" (visto que "preto" é entendido como subconjunto de "negro", assim como "pardo")._

_(2) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://biblioteca.ibge.gov.br/visualizacao/livros/liv101736_informativo.pdf)._

_(3) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://brasilemsintese.ibge.gov.br/populacao/distribuicao-da-populacao-por-sexo.html)._

_(4) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://cidades.ibge.gov.br/brasil/pesquisa/23/22714)._

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
PUT https://dadosmestre-api-inyrb33hja-uc.a.run.app/2531/pessoas-fisicas/12646442300 HTTP/1.1
X-API-Key: **************
Content-Type: application/json

{
  "id": "12646442300",
  "codigo": "CICLANO3",
  "qualificacao": "fisica",
  "nome": "Ciclano Silveira",
  "inativo": false,
  "criado_por": "email@teste.com",
  "atualizado_por": "email@teste.com",
  "grupo_empresarial": "728ddc4a-242f-45f0-a752-f3e46f9993ad",
  "tenant": 9999,
  "enderecos": [
    {
      "cep": "23456765",
      "tipo_logradouro": "rua",
      "logradouro": "Avenoda Presidente Vargas",
      "numero": "1500",
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



