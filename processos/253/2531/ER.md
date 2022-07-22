# 2531 - Entidades e Relacionamentos - Garantir a retenção e fidelidade de clientes

Descrição das entidades de banco de dados, e seus relacionamentos, que compõem o processo 2711 da MOPE.

**Para correta compreensão das entidades aqui presentes, deve-se ler a [sessão de disposicoes gerais do banco do ERP4](../../../er-disposicoes-gerais.md).**

## Entidades Base

### Participante Único

> Tabela: participante_unico

Tabela que representa qualquer tipo de participante do ERP, independentemente do tipo de relacionamento, ou papel assumido pelo mesmo. Assim, essa mesma tabela concentrará registros do tipo:

* Pessoas físicas
* Pessoas jurídicas

Além disso, os papeís assumidos por estes participantes não serão atributos nos registros em si, mas sim atributos de relacionamentos entre as entidades. Seguem os papeis possíveis:

* Lead
* Cliente 
* Fornecedor
* Técnico
* Vendedor
* Transportadora
* Representante Comercial
* Representante Técnico

#### Propriedades comuns a todos os tipos de participante

| Propriedade               | Tipo        | Descrição                                                                                  | Not Null | Default | Domínio                                                                                                                                                                                                                                                                                                                                          |
| ------------------------- | ----------- | ------------------------------------------------------------------------------------------ | -------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| codigo                    | string(30)  | Identificador único, do ponto de vista do usuário final.                                   | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| qualificacao              | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                         | Sim      | Vazio   | ["fisica", "juridica", "publica_federal", "publica_estadual", "publica_municipal", "cooperativa_credito", "cooperativa_agro", "cooperativa", "financeira", "seguradora_capitalizacao", "corretora_autonoma_seguro", "previdencia_complementar_aberta", "previdencia_complementar_fechada", "economia_mista", "outras_publica_federal", "outros"] |
| documento                 | string(14)  | CNPJ ou CPF do participante (só contendo números).                                         | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nome                      | string(150) | Nome do participante.                                                                      | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nome_fantasia             | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).              | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  | observacao | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| propriedades_customizadas | json        | JSON de propriedades customizadas pelo cliente.                                            | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| site                      | string(250) | Endereço eletrônico do participante.                                                       | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| inativo                   | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário). | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |

#### Propriedades dos participante que representem pessoas jurídicas

| Propriedade       | Tipo        | Descrição                                                                  | Not Null | Default | Domínio |
| ----------------- | ----------- | -------------------------------------------------------------------------- | -------- | ------- | ------- |
| cno               | string(12)  | Cadastro Nacional de Obras.                                                | Não      | Vazio   |         |
| rntrc             | string(14)  | Registro Nacional de Transportadores Rodoviários de Cargas.                | Não      | Vazio   |         |
| cnae_principal    | string(7)   | Classificação Nacional de Atividade Econômica principal.                   | Não      | False   |         |
| fpas              | string(6)   | Código de Fundo de Previdência e Assistência Social.                       | Não      | False   |         |
| inicio_atividades | date        | Data de início das atividades (se pessoa jurídica ou similar).             | Não      | Vazio   |         |
| final_atividades  | date        | Data de final das atividades (se pessoa jurídica ou similar).              | Não      | Vazio   |         |
| logotipo_url      | string(250) | URL para imagem de logotipo do participante (normalmente pessoa jurídica). | Não      | Vazio   |         |
| ramo_atividade    | string(500) | Descrição do ramo de atividade principal do participante.                  | Não      | Vazio   |         |
| cafir             | string(8)   | Cadastro de Imóveis Rurais.                                                | Não      | Vazio   |         |

#### Propriedades dos participante que representem pessoas físicas

| Propriedade              | Tipo        | Descrição                                                                     | Not Null | Default | LGPD     | Domínio                                                                                                                                                     |
| ------------------------ | ----------- | ----------------------------------------------------------------------------- | -------- | ------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nome_social              | string(150) | Nome social da Pessoa Física.                                                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| caepf                    | string(18)  | Cadastro de Atividade Econômica da Pessoa Física.                             | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nit                      | string(11)  | Número de Inscrição do Trabalhador.                                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nascimento               | date        | Data de nascimento do participante (se pessoa física).                        | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nacionalidade            | string(5)   | Código do país de nacionalidade.                                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| naturalidade             | string(2)   | UF de nascimento do participante.                                             | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| ibge_nascimento          | string(8)   | Código do município de nascimento do participante.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| obito                    | date        | Data de falescimento do participante (se pessoa física).                      | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| foto_url                 | string(250) | URL para fotografia do participante (normalmente pessoal física).             | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| raca                     | string(10)  | Classificação de cor ou raça (conforme o IBGE).                               | Não      | Vazio   | Sensível | ["branca", "parda", "preta", "amarela", "indigena"] _(1)_                                                                                                   |
| grau_instrucao           | string(20)  | Classificação de grau de instrução (conforme o IBGE).                         | Não      | Vazio   | Sensível | ["sem_instrucao", "fundamental_incompleto", "fundamental_completo", "medio_incompleto", "medio_completo", "superior_incompleto", "superior_completo"] _(2)_ |
| sexo                     | string(10)  | Sexo do participante.                                                         | Não      | Vazio   | Sensível | ["homem", "mulher"] _(3)_                                                                                                                                   |
| estado_civil             | string(10)  | Estado civil do participante.                                                 | Não      | Vazio   | Sensível | ["casado", "desquitado_separado", "divorciado", "solteiro", "viuvo"] _(4)_                                                                                  |
| nome_pai                 | string(250) | Nome do pai do participante.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nome_mae                 | string(250) | Nome da mãe do participante.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| definiciente_visual      | boolean     | Indica que o participante possui algum tipo de deficiência visual.            | Não      | Vazio   | Sensível |                                                                                                                                                             |
| definiciente_auditivo    | boolean     | Indica que o participante possui algum tipo de deficiÊncia auditiva.          | Não      | Vazio   | Sensível |                                                                                                                                                             |
| definiciente_intelectual | boolean     | Indica que o participante possui algum tipo de deficiência intelectual.       | Não      | Vazio   | Sensível |                                                                                                                                                             |
| definiciente_mental      | boolean     | Indica que o participante possui algum tipo de deficiência mental.            | Não      | Vazio   | Sensível |                                                                                                                                                             |
| definiciente_fisico      | boolean     | Indica que o participante possui algum tipo de deficiência física.            | Não      | Vazio   | Sensível |                                                                                                                                                             |
| reabilitado              | boolean     | Indica que o participante é reabilitado.                                      | Não      | Vazio   | Sensível |                                                                                                                                                             |
| ctps                     | string(11)  | Número da Carteira de Trabalho e Previcência Social.                          | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| serie_ctps               | string(5)   | Série da Carteira de Trabalho e Previcência Social.                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_ctps                  | string(2)   | Unidade Federativa de expedição da Carteira de Trabalho e Previcência Social. | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_ctps                | data        | Data de expedição da Carteira de Trabalho e Previcência Social.               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| ric                      | string(30)  | Número do Registro de Identidade Civil.                                       | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_ric                | string(20)  | Órgão emissor do Registro de Identidade Civil.                                | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_ric                 | data        | Data de emissão do Registro de Identidade Civil.                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_ric                   | string(2)   | UF do Registro de Identidade Civil.                                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cidade_ric               | string(60)  | Cidade de emissão do Registro de Identidade Civil.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| rg                       | string(30)  | Número do Registro Geral (identidade).                                        | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_rg                 | string(20)  | Órgão emissor do Registro Geral (identidade).                                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_rg                  | date        | Data de emissão do Registro Geral (identidade).                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_rg                    | string(2)   | Unidade Federativa de emissão do Registro Geral (identidade).                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cnh                      | string(30)  | Número da Carteira Nacional de Habilitação.                                   | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_cnh                | string(20)  | Órgão emissor da Carteira Nacional de Habilitação.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_cnh                   | string(2)   | UF de emissão da Carteira Nacional de Habilitação.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_cnh                 | date        | Data de emissão da Carteira Nacional de Habilitação.                          | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| validade_cnh             | date        | Data de Validade da Carteira Nacional de Habilitação.                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_primeira_cnh        | date        | Data de emissão da primeira Carteira Nacional de Habilitação.                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| categoria_cnh            | string(3)   | Categoria da Carteira Nacional de Habilitação.                                | Não      | Vazio   | Pessoal  | ["a", "b", "c", "d", "e", "ab", "ac", "ad", "ae"]                                                                                                           |
| pais_passaporte          | string(8)   | Código do país de emissão do Passaporte.                                      | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_passaporte         | string(20)  | Órgão emissor do Passaporte.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_passaporte            | string(2)   | Unidade Federativa de emissão do Passaporte.                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_passaporte          | date        | Data de emissão do Passaporte.                                                | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| validade_passaporte      | date        | Data de validade do Passaporte.                                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| passaporte               | string(30)  | Número do Passaporte.                                                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nis                      | string(11)  | Número de Identificação Social.                                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| te                       | string(15)  | Número do Título de Eleitor.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| zona_te                  | string(3)   | Zona do Título de Eleitor.                                                    | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| secao_te                 | string(4)   | Sessão do Título de Eleitor.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_te                    | string(2)   | Unidade Federativa de emissão do Título de Eleitor.                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| atestado_obito           | string(32)  | Número do atestado de óbito.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| tipo_certidao            | string(10)  | Tipo da Certidão.                                                             | Não      | Vazio   | Pessoal  | ["nascimento", "casamento"]                                                                                                                                 |
| numero_certidao          | string(30)  | Número da Certidão.                                                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| livro_certidao           | string(15)  | Número do livro da Certidão.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| folha_certidao           | string(15)  | Número da folha do livro da Certidão.                                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_certidao            | date        | Data de expedição da Certidão.                                                | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cidade_certidao          | string(60)  | Cidade de expedição da Certidão.                                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_certidao              | string(2)   | Unidade Federativa de expedição da Certidão.                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cartorio_certidao        | string(30)  | Identificação do cartório de expedição da Certidão.                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cr                       | string(15)  | Número do Certificado de Registro (no exército).                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| serie_cr                 | string(5)   | Série do Certificado de Registro (no exército).                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_cr                  | date        | Data de expedição do Certificado de Registro (no exército).                   | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| tipo_sanguineo           | string(2)   | Tipo sanguíneo do participante                                                | Não      | Vazio   | Pessoal  | ["a", "b", "o", "ab"]                                                                                                                                       |
 
_(1) As opções estão de acordo com as definições [oficiais do IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18319-cor-ou-raca.html#:~:text=De%20acordo%20com%20dados%20da,1%25%20como%20amarelos%20ou%20ind%C3%ADgenas.), inclusive há uma [reportagem da Gazeta](https://www.agazeta.com.br/es/gv/preto-ou-negro-ibge-explica-classificacao-de-cor-e-raca-em-pesquisas-1118), explicando o uso do termo "preta", e não "negro" (visto que "preto" é entendido como subconjunto de "negro", assim como "pardo")._

_(2) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://biblioteca.ibge.gov.br/visualizacao/livros/liv101736_informativo.pdf)._

_(3) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://brasilemsintese.ibge.gov.br/populacao/distribuicao-da-populacao-por-sexo.html)._

_(4) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://cidades.ibge.gov.br/brasil/pesquisa/23/22714)._

#### Restrições

* unique: (tenant, grupo_empresarial, documento)
* unique: (tenant, grupo_empresarial, codigo)
* FK: (participante_matriz) aponta para `participante` coluna (id)

### Entidades Empresariais

#### Empresas

> Tabela: empresa

Representa a personificação de um participante único enquanto empresa matriz.

| Propriedade       | Tipo       | Descrição                                                      | Not Null | Default | Domínio |
| ----------------- | ---------- | -------------------------------------------------------------- | -------- | ------- | ------- |
| participante      | string(14) | ID do participante que representa a empresa matriz.            | Sim      | Vazio   |         |
| grupo_empresarial | uuid       | Identificador do grupo empresarial ao qual a empresa pertence. | Sim      | Vazio   |         |

##### Restrições

* PK: (tenant, participante)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (grupo_empresarial) aponta para `grupo_empresarial` coluna (id)

#### Estabelecimentos

> Tabela: estabelecimento

Representa a personificação de um participante único enquanto estabelecimento de uma empresa.

| Propriedade       | Tipo       | Descrição                                                              | Not Null | Default | Domínio |
| ----------------- | ---------- | ---------------------------------------------------------------------- | -------- | ------- | ------- |
| participante      | string(14) | ID do participante que representa o estabelecimento.                   | Sim      | Vazio   |         |
| empresa           | uuid       | Identificador da empresa à qual o estabelecimento pertence.            | Sim      | Vazio   |         |
| grupo_empresarial | uuid       | Identificador do grupo empresarial ao qual o estabelecimento pertence. | Sim      | Vazio   |         |

##### Restrições

* PK: (tenant, participante)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (grupo_empresarial) aponta para `grupo_empresarial` coluna (id)
* FK: (empresa) aponta para `empresa` coluna (participante)

### Relacionamento Participantes

#### Leads

> Tabela: rel_lead

Representa um relacionamento de Lead entre participantes.

| Propriedade  | Tipo | Descrição                                                                 | Not Null | Default | Domínio |
| ------------ | ---- | ------------------------------------------------------------------------- | -------- | ------- | ------- |
| lead         | uuid | ID do participante que figura como Lead no relacionamento.                | Sim      | Vazio   |         |
| participante | uuid | ID do participante que figura como possível fornecedor no relacionamento. | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, lead)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (lead) aponta para `participante` coluna (id)

#### Clientes

> Tabela: rel_cliente

Representa um relacionamento de Cliente entre participantes.

| Propriedade    | Tipo          | Descrição                                                        | Not Null | Default | Domínio |
| -------------- | ------------- | ---------------------------------------------------------------- | -------- | ------- | ------- |
| cliente        | uuid          | ID do participante que figura como Cliente no relacionamento.    | Sim      | Vazio   |         |
| participante   | uuid          | ID do participante que figura como fornecedor no relacionamento. | Sim      | Vazio   |         |
| limite_credito | numeric(20,2) | Limite de crédito do cliente.                                    | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, cliente)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (cliente) aponta para `participante` coluna (id)

#### Fornecedores

> Tabela: rel_fornecedor

Representa um relacionamento de Fornecedor entre participantes.

| Propriedade  | Tipo | Descrição                                                        | Not Null | Default | Domínio |
| ------------ | ---- | ---------------------------------------------------------------- | -------- | ------- | ------- |
| fornecedor   | uuid | ID do participante que figura como Fornecedor no relacionamento. | Sim      | Vazio   |         |
| participante | uuid | ID do participante que figura como cliente no relacionamento.    | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, fornecedor)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (fornecedor) aponta para `participante` coluna (id)

#### Técnicos

> Tabela: rel_tecnico

Representa um relacionamento de Técnico entre participantes.

| Propriedade  | Tipo | Descrição                                                         | Not Null | Default | Domínio |
| ------------ | ---- | ----------------------------------------------------------------- | -------- | ------- | ------- |
| tecnico      | uuid | ID do participante que figura como Técnico no relacionamento.     | Sim      | Vazio   |         |
| participante | uuid | ID do participante que figura como contratante no relacionamento. | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, tecnico)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (tecnico) aponta para `participante` coluna (id)

#### Vendedores

> Tabela: rel_vendedor

Representa um relacionamento de Vendedor entre participantes.

| Propriedade  | Tipo | Descrição                                                         | Not Null | Default | Domínio |
| ------------ | ---- | ----------------------------------------------------------------- | -------- | ------- | ------- |
| vendedor     | uuid | ID do participante que figura como Vendedor no relacionamento.    | Sim      | Vazio   |         |
| participante | uuid | ID do participante que figura como contratante no relacionamento. | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, vendedor)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (vendedor) aponta para `participante` coluna (id)

#### Transportadoras

> Tabela: rel_transportadora

Representa um relacionamento de Transportadora entre participantes.

| Propriedade    | Tipo | Descrição                                                            | Not Null | Default | Domínio |
| -------------- | ---- | -------------------------------------------------------------------- | -------- | ------- | ------- |
| transportadora | uuid | ID do participante que figura como Transportadora no relacionamento. | Sim      | Vazio   |         |
| participante   | uuid | ID do participante que figura como contratante no relacionamento.    | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, transportadora)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (transportadora) aponta para `participante` coluna (id)

#### Representantes Comerciais

> Tabela: rel_representante_comercial

Representa um relacionamento de Representante Comercial entre participantes.

| Propriedade             | Tipo | Descrição                                                                     | Not Null | Default | Domínio |
| ----------------------- | ---- | ----------------------------------------------------------------------------- | -------- | ------- | ------- |
| representante_comercial | uuid | ID do participante que figura como Representante Comercial no relacionamento. | Sim      | Vazio   |         |
| participante            | uuid | ID do participante que figura como contratante no relacionamento.             | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, representante_comercial)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (representante_comercial) aponta para `participante` coluna (id)

#### Representantes Técnicos

> Tabela: rel_representante_tecnico

Representa um relacionamento de Representante Técnico entre participantes.

| Propriedade           | Tipo | Descrição                                                                   | Not Null | Default | Domínio |
| --------------------- | ---- | --------------------------------------------------------------------------- | -------- | ------- | ------- |
| representante_tecnico | uuid | ID do participante que figura como Representante Técnico no relacionamento. | Sim      | Vazio   |         |
| participante          | uuid | ID do participante que figura como contratante no relacionamento.           | Sim      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, representante_tecnico)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (representante_tecnico) aponta para `participante` coluna (id)

#### Contato

> Tabela: rel_contato

Representa um relacionamento de Contato entre participantes.

| Propriedade  | Tipo    | Descrição                                                       | Not Null | Default | Domínio |
| ------------ | ------- | --------------------------------------------------------------- | -------- | ------- | ------- |
| contato      | uuid    | ID do participante que figura como Contato no relacionamento.   | Sim      | Vazio   |         |
| participante | uuid    | ID do participante que figura como aquele que contém o contato. | Sim      | Vazio   |         |
| financeiro   | boolean | Indica se é um contato de natureza financeira.                  | Não      | Vazio   |         |
| fiscal       | boolean | Indica se é um contato de natureza fiscal.                      | Não      | Vazio   |         |
| comercial    | boolean | Indica se é um contato de natureza comercial.                   | Não      | Vazio   |         |
| pessoal      | boolean | Indica se é um contato de natureza pessoal.                     | Não      | Vazio   |         |
| tecnico      | boolean | Indica se é um contato de natureza técnica.                     | Não      | Vazio   |         |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, contato)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (contato) aponta para `participante` coluna (id)

#### Trabalhadores

> Tabela: rel_trabalhador

Representa um relacionamento de Trabalho entre participantes.

| Propriedade  | Tipo       | Descrição                                                         | Not Null | Default | Domínio                                                                                                      |
| ------------ | ---------- | ----------------------------------------------------------------- | -------- | ------- | ------------------------------------------------------------------------------------------------------------ |
| trabalhador  | uuid       | ID do participante que figura como Trabalhador no relacionamento. | Sim      | Vazio   |                                                                                                              |
| participante | uuid       | ID do participante que figura como empregador no relacionamento.  | Sim      | Vazio   |                                                                                                              |
| tipo         | string(20) | Tipo de relacionamento de trabalho.                               | Sim      | Vazio   | ["estagiario", "jovem_aprendiz", "clt", "socio", "diretor", "temporario"] TODO Rever com a equipe do produto |
| matricula    | string(14) | Matrícula do trabalhador na empresa contratante.                  | Sim      | Vazio   |                                                                                                              |

TODO Rever campos com a equipe do produto.

##### Restrições

* unique: (tenant, grupo_empresarial, participante, trabalhador)
* unique: (tenant, grupo_empresarial, participante, matricula)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (trabalhador) aponta para `participante` coluna (id)

#### Dependente

> Tabela: rel_dependente

Representa um relacionamento de Dependência entre participantes.

| Propriedade     | Tipo       | Descrição                                                         | Not Null | Default | Domínio                                                                                                                           |
| --------------- | ---------- | ----------------------------------------------------------------- | -------- | ------- | --------------------------------------------------------------------------------------------------------------------------------- |
| dependente      | uuid       | ID do participante que figura como Dependente no relacionamento.  | Sim      | Vazio   |                                                                                                                                   |
| participante    | uuid       | ID do participante que figura como responsável no relacionamento. | Sim      | Vazio   |                                                                                                                                   |
| inclusao        | date       | Data de inclusão do dependente.                                   | Sim      | Now()   |                                                                                                                                   |
| tipo_parentesco | string(30) | Tipo de parentensco para com o responsável.                       | Sim      | Vazio   | ["conjuge", "uniao_estavel", "filho_enteado", "guarda_judicial", "pais_avos_bisavos", "incapaz", "ex_conjuge", "agregado_outros"] |

##### Restrições

* unique: (tenant, grupo_empresarial, participante, dependente)
* FK: (participante) aponta para `participante` coluna (id)
* FK: (dependente) aponta para `participante` coluna (id)

## Entidades Auxiliares

### Inscrição Estadual

> Tabela: inscricao_estadual

Representa uma inscrição estadual do participante (se houver). Assim, um participante pode ter múltiplas inscrições estaduais (no limite, uma por estado da federação).

| Propriedade        | Tipo       | Descrição                                              | Not Null | Default | Domínio |
| ------------------ | ---------- | ------------------------------------------------------ | -------- | ------- | ------- |
| participante       | uuid       | Identificador do participante relacionado.             | Sim      | Vazio   |         |
| uf                 | string(2)  | Sigla da unidade federativa.                           | Sim      | Vazio   |         |
| inscricao_estadual | string(20) | Inscrição estadual do participante (na UF em questão). | Vazio    | Vazio   |         |

#### Restrições

* unique: (tenant, grupo_empresarial, participante, uf)
* FK: (participante) aponta para `participante` coluna (id)

### Inscrição Municipal

> Tabela: inscricao_municipal

Representa uma inscrição municipal do participante (se houver). Assim, um participante pode ter múltiplas inscrições municipais.

| Propriedade         | Tipo       | Descrição                                                      | Not Null | Default | Domínio |
| ------------------- | ---------- | -------------------------------------------------------------- | -------- | ------- | ------- |
| participante        | uuid       | Identificador do participante relacionado.                     | Sim      | Vazio   |         |
| ibge                | string(8)  | Código IBGE do município em questão.                           | Sim      | Vazio   |         |
| inscricao_municipal | string(20) | Inscrição municipal do participante (no município em questão). | Vazio    | Vazio   |         |

#### Restrições

* unique: (tenant, grupo_empresarial, participante, ibge)
* FK: (participante) aponta para `participante` coluna (id)

### E-Mail

> Tabela: email

Representa um e-mail de um participante.

| Propriedade  | Tipo        | Descrição                                                                                                                                         | Not Null | Default | Domínio |
| ------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- | ------- |
| participante | uuid        | Identificador do participante relacionado.                                                                                                        | Sim      | Vazio   |         |
| descricao    | string(150) | Descrição do e-mail (de acordo com o ponto de vista do cliente).                                                                                  | Não      | Vazio   |         |
| endereco     | string(150) | Endereço de e-mail.                                                                                                                               | Sim      | Vazio   |         |
| principal    | boolean     | Indica se este é o e-mail principal do participante.                                                                                              | Não      | Vazio   |         |
| financeiro   | boolean     | Indica se é um e-mail para contato de natureza financeira (normalmente útil para e-mails de contato não ligados a uma pessoa física).             | Não      | Vazio   |
| faturamento  | boolean     | Indica se é um e-mail para contato de natureza fiscal, de faturamento (normalmente útil para e-mails de contato não ligados a uma pessoa física). | Não      | Vazio   |
| comercial    | boolean     | Indica se é um e-mail para contato de natureza comercial (normalmente útil para e-mails de contato não ligados a uma pessoa física).              | Não      | Vazio   |
| ordem        | int         | Indica a ordem de importância deste e-mail (de acordo com a ordenação do usuário).                                                                | Sim      | Vazio   |         |

#### Restrições

* unique: (tenant, grupo_empresarial, participante, email)
* unique: (tenant, grupo_empresarial, participante, principal)
* FK: (participante) aponta para `participante` coluna (id)

### Telefone

> Tabela: telefone

Representa um telefone de um participante.

| Propriedade         | Tipo        | Descrição                                                                                                                                           | Not Null | Default | Domínio                            |
| ------------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- | ---------------------------------- |
| participante        | uuid        | Identificador do participante relacionado.                                                                                                          | Sim      | Vazio   |                                    |
| descricao           | string(150) | Descrição do telefone (de acordo com o ponto de vista do cliente).                                                                                  | Não      | Vazio   |                                    |
| tipo                | string(20)  | Tipo do número de telefone.                                                                                                                         | Sim      | Vazio   | ["fixo", "celular", "fax", "voip"] |
| ddi                 | string(2)   | DDI do telefone.                                                                                                                                    | Sim      | Vazio   |                                    |
| ddd                 | string(2)   | DDD do telefone.                                                                                                                                    | Sim      | Vazio   |                                    |
| numero              | string(9)   | Número do telefone.                                                                                                                                 | Sim      | Vazio   |                                    |
| principal           | boolean     | Indica se este é o e-mail principal do participante.                                                                                                | Não      | Vazio   |                                    |
| ramal               | string(12)  | Ramal do telefone.                                                                                                                                  | Sim      | Vazio   |                                    |
| residencial_pessoal | boolean     | Indica se é um telefone residencial, ou de carater pessoal (normalmente útil para telefones de contato de  persoa física).                          | Não      | Vazio   |
| financeiro          | boolean     | Indica se é um telefone para contato de natureza financeira (normalmente útil para telefones de contato de pessoa jurídica ou similar).             | Não      | Vazio   |
| faturamento         | boolean     | Indica se é um telefone para contato de natureza fiscal, de faturamento (normalmente útil para telefones de contato de pessoa jurídica ou similar). | Não      | Vazio   |
| comercial           | boolean     | Indica se é um telefone para contato de natureza comercial (normalmente útil para telefones de contato de pessoa jurídica ou similar).              | Não      | Vazio   |
| ordem               | int         | Indica a ordem de importância deste telefone (de acordo com a ordenação do usuário).                                                                | Sim      | Vazio   |                                    |

#### Restrições

* unique: (tenant, grupo_empresarial, participante, ddi, ddd, telefone)
* unique: (tenant, grupo_empresarial, participante, principal)
* FK: (participante) aponta para `participante` coluna (id)

### Endereços

> Tabela: endereco

Representa um endereco de um participante.

| Propriedade     | Tipo        | Descrição                                                                                      | Not Null | Default | Domínio                                                  |
| --------------- | ----------- | ---------------------------------------------------------------------------------------------- | -------- | ------- | -------------------------------------------------------- |
| participante    | uuid        | Identificador do participante relacionado.                                                     | Sim      | Vazio   |                                                          |
| descricao       | string(150) | Descrição do endereço (de acordo com o ponto de vista do cliente).                             | Não      | Vazio   |                                                          |
| cep             | string(15)  | Código de Endereçamento Postal.                                                                | Sim      | Vazio   |                                                          |
| tipo_logradouro | string(10)  | Tipo do logradouro (rua, aveida, etc).                                                         | Sim      | Vazio   | [Ver lista de tipos de logradouro.](tipos_logradouro.md) |
| logradouro      | string(250) | Nome da rua, avenida, etc.                                                                     | Sim      | Vazio   |                                                          |
| numero          | string(10)  | Númeração dentro do logradouro.                                                                | Não      | Vazio   |                                                          |
| complemento     | string(250) | Complemento do endereço.                                                                       | Não      | Vazio   |                                                          |
| bairro          | string(60)  | Bairro do endereço.                                                                            | Não      | Vazio   |                                                          |
| uf              | string(2)   | Unidade Federativa do endereço.                                                                | Não      | Vazio   |                                                          |
| pais            | string(5)   | Código do país (exemplo: Brasil = 1058).                                                       | Sim      | Vazio   |                                                          |
| ibge            | string(250) | Código IBGE do município do endereço.                                                          | Não      | Vazio   |                                                          |
| referencia      | string(250) | Referência para melhor localização do endereço.                                                | Não      | Vazio   |                                                          |
| local           | boolean     | Indica se este é o endereço de localização do participante (residencial para pessoas físicas). | Não      | Vazio   |                                                          |
| entrega         | boolean     | Indica se é um endereço para entrega de mercadoria.                                            | Não      | Vazio   |
| cobranca        | boolean     | Indica se é um endereço para cabrança financeira.                                              | Não      | Vazio   |
| comercial       | boolean     | Indica se é um endereço de referÊncia comercial do participante.                               | Não      | Vazio   |
| ordem           | int         | Indica a ordem de importância deste endereço (de acordo com a ordenação do usuário).           | Sim      | Vazio   |                                                          |

#### Restrições

* unique: (tenant, grupo_empresarial, participante, local)
* FK: (participante) aponta para `participante` coluna (id)
