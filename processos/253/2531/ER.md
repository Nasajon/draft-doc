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

**Esta mesma tabela irá concentrar os registros das empresas e estabelecimentos contidos no ERP, no entanto, para isso ser possível, há duas colunas que permitirão identificar, em especial, esses papeis (verifique abaixo a documentação destas colunas):**

* membro_grupo (boolean)
* participante_matriz (uuid)

#### Propriedades comuns a todos os tipos de participante

| Propriedade               | Tipo        | Descrição                                                                                                                                                                                       | Not Null | Default | Domínio                                                                                                                                                                                                                                                                                                                                          |
| ------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| membro_grupo              | boolean     | Indica que este participante é um membro do grupo de participantes relacionado, pela coluna grupo_empresarial. Normalmente usado para atribuir uma empresa como membro de um grupo empresarial. | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |
| participante_matriz       | uuid        | ID do participante ao qual este está ligado como filial (normalmente para um estabelecimento filial de um empresa).                                                                             | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |
| codigo                    | string(30)  | Identificador único, do ponto de vista do usuário final.                                                                                                                                        | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| qualificacao              | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                                                                                                                              | Sim      | Vazio   | ["fisica", "juridica", "publica_federal", "publica_estadual", "publica_municipal", "cooperativa_credito", "cooperativa_agro", "cooperativa", "financeira", "seguradora_capitalizacao", "corretora_autonoma_seguro", "previdencia_complementar_aberta", "previdencia_complementar_fechada", "economia_mista", "outras_publica_federal", "outros"] |
| documento                 | string(14)  | CNPJ ou CPF do participante (só contendo números).                                                                                                                                              | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nome                      | string(150) | Nome do participante.                                                                                                                                                                           | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nome_fantasia             | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).                                                                                                                   | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  | observacao | string | Texto livre de observações sobre o participante. | Não | Vazio |  |
| propriedades_customizadas | json        | JSON de propriedades customizadas pelo cliente.                                                                                                                                                 | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| site                      | string(250) | Endereço eletrônico do participante.                                                                                                                                                            | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| inativo                   | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).                                                                                                      | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |
|                           |

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

| Propriedade           | Tipo        | Descrição                                                                     | Not Null | Default | LGPD     | Domínio                                                                                                                                                     |
| --------------------- | ----------- | ----------------------------------------------------------------------------- | -------- | ------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| caepf                 | string(18)  | Cadastro de Atividade Econômica da Pessoas Física.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nit                   | string(11)  | Número de Inscrição do Trabalhador.                                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nascimento            | date        | Data de nascimento do participante (se pessoa física).                        | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nacionalidade         | string(5)   | Código do país de nacionalidade.                                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| naturalidade          | string(2)   | UF de nascimento do participante.                                             | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| ibge_nascimento       | string(8)   | Código do município de nascimento do participante.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| obito                 | date        | Data de falescimento do participante (se pessoa física).                      | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| foto_url              | string(250) | URL para fotografia do participante (normalmente pessoal física).             | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| raca                  | string(10)  | Classificação de cor ou raça (conforme o IBGE).                               | Não      | Vazio   | Sensível | ["branca", "parda", "preta", "amarela", "indigena"] _(1)_                                                                                                   |
| grau_instrucao        | string(20)  | Classificação de grau de instrução (conforme o IBGE).                         | Não      | Vazio   | Sensível | ["sem_instrucao", "fundamental_incompleto", "fundamental_completo", "medio_incompleto", "medio_completo", "superior_incompleto", "superior_completo"] _(2)_ |
| sexo                  | string(10)  | Sexo do participante.                                                         | Não      | Vazio   | Sensível | ["homem", "mulher"] _(3)_                                                                                                                                   |
| estado_civil          | string(10)  | Estado civil do participante.                                                 | Não      | Vazio   | Sensível | ["casado", "desquitado_separado", "divorciado", "solteiro", "viuvo"] _(4)_                                                                                  |
| nome_pai              | string(250) | Nome do pai do participante.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nome_mae              | string(250) | Nome da mãe do participante.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| definiciente_visual   | boolean     | Indica que o participante é deficiente visual.                                | Não      | Vazio   | Sensível |                                                                                                                                                             |
| definiciente_auditivo | boolean     | Indica que o participante é deficiente auditivo.                              | Não      | Vazio   | Sensível |                                                                                                                                                             |
| definiciente_auditivo | boolean     | Indica que o participante é deficiente auditivo.                              | Não      | Vazio   | Sensível |                                                                                                                                                             |
| reabilitado           | boolean     | Indica que o participante é reabilitado.                                      | Não      | Vazio   | Sensível |                                                                                                                                                             |
| ctps                  | string(11)  | Número da Carteira de Trabalho e Previcência Social.                          | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| serie_ctps            | string(5)   | Série da Carteira de Trabalho e Previcência Social.                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_ctps               | string(2)   | Unidade Federativa de expedição da Carteira de Trabalho e Previcência Social. | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_ctps             | data        | Data de expedição da Carteira de Trabalho e Previcência Social.               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| ric                   | string(30)  | Número do Registro de Identidade Civil.                                       | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_ric             | string(20)  | Órgão emissor do Registro de Identidade Civil.                                | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_ric              | data        | Data de emissão do Registro de Identidade Civil.                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_ric                | string(2)   | UF do Registro de Identidade Civil.                                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cidade_ric            | string(60)  | Cidade de emissão do Registro de Identidade Civil.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| rg                    | string(30)  | Número do Registro Geral (identidade).                                        | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_rg              | string(20)  | Órgão emissor do Registro Geral (identidade).                                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_rg               | date        | Data de emissão do Registro Geral (identidade).                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_rg                 | string(2)   | Unidade Federativa de emissão do Registro Geral (identidade).                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cnh                   | string(30)  | Número da Carteira Nacional de Habilitação.                                   | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_cnh             | string(20)  | Órgão emissor da Carteira Nacional de Habilitação.                            | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_cnh              | date        | Data de emissão da Carteira Nacional de Habilitação.                          | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| validade_cnh          | date        | Data de Validade da Carteira Nacional de Habilitação.                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_primeira_cnh     | date        | Data de emissão da primeira Carteira Nacional de Habilitação.                 | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| categoria_cnh         | string(3)   | Categoria da Carteira Nacional de Habilitação.                                | Não      | Vazio   | Pessoal  | ["a", "b", "c", "d", "e", "ab", "ac", "ad", "ae"]                                                                                                           |
| pais_passaporte       | string(8)   | Código do país de emissão do Passaporte.                                      | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| passaporte            | string(30)  | Número do Passaporte.                                                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| orgao_passaporte      | string(20)  | Órgão emissor do Passaporte.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_passaporte         | string(2)   | Unidade Federativa de emissão do Passaporte.                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_passaporte       | date        | Data de emissão do Passaporte.                                                | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| validade_passaporte   | date        | Data de validade do Passaporte.                                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| passaporte            | string(30)  | Número do Passaporte.                                                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| nis                   | string(11)  | Número de Identificação Social.                                               | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| te                    | string(15)  | Número do Título de Eleitor.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| zona_te               | string(3)   | Zona do Título de Eleitor.                                                    | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| secao_te              | string(4)   | Sessão do Título de Eleitor.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_te                 | string(2)   | Unidade Federativa de emissão do Título de Eleitor.                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| atestado_obito        | string(32)  | Número do atestado de óbito.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| tipo_certidao         | string(10)  | Tipo da Certidão.                                                             | Não      | Vazio   | Pessoal  | ["nascimento", "casamento"]                                                                                                                                 |
| numero_certidao       | string(30)  | Número da Certidão.                                                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| livro_certidao        | string(15)  | Número do livro da Certidão.                                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| folha_certidao        | string(15)  | Número da folha do livro da Certidão.                                         | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| data_certidao         | date        | Data de expedição da Certidão.                                                | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cidade_certidao       | string(60)  | Cidade de expedição da Certidão.                                              | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| uf_certidao           | string(2)   | Unidade Federativa de expedição da Certidão.                                  | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
| cartorio_certidao     | string(30)  | Identificação do cartório de expedição da Certidão.                           | Não      | Vazio   | Pessoal  |                                                                                                                                                             |
 
_(1) As opções estão de acordo com as definições [oficiais do IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18319-cor-ou-raca.html#:~:text=De%20acordo%20com%20dados%20da,1%25%20como%20amarelos%20ou%20ind%C3%ADgenas.), inclusive há uma [reportagem da Gazeta](https://www.agazeta.com.br/es/gv/preto-ou-negro-ibge-explica-classificacao-de-cor-e-raca-em-pesquisas-1118), explicando o uso do termo "preta", e não "negro" (visto que "preto" é entendido como subconjunto de "negro", assim como "pardo")._

_(2) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://biblioteca.ibge.gov.br/visualizacao/livros/liv101736_informativo.pdf)._

_(3) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://brasilemsintese.ibge.gov.br/populacao/distribuicao-da-populacao-por-sexo.html)._

_(4) As opções estão de acordo com os [resultados divulgados pelo IBGE](https://cidades.ibge.gov.br/brasil/pesquisa/23/22714)._

#### Restrições

* unique: (tenant, grupo_empresarial, documento)
* unique: (tenant, grupo_empresarial, codigo)
* FK: (participante_matriz) aponta para `participante` coluna (id)

### Relacionamento Participantes

> Tabela: relacionamento_participante

Representa um relacionamento entre participantes contidos no BD.

| Propriedade             | Tipo    | Descrição                                                                                                                                                                               | Not Null | Default | Domínio |
| ----------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- | ------- |
| participante_origem     | uuid    | ID do participante origem do relacionamento (por exemplo, para um relacionamento com a flag "cliente" marcada, a origem está no contratante, ou cliente).                               | Sim      | Vazio   |         |
| participante_destino    | uuid    | ID do participante destino do relacionamento (por exemplo, para um relacionamento com a flag "cliente" marcada, o destino está no contratado, normalmente uma empresa membro do grupo). | Sim      | Vazio   |         |
| lead                    | boolean | Indica que o participante_origem é lead do participante_destino.                                                                                                                        | Sim      | False   |         |
| cliente                 | boolean | Indica que o participante_origem é Cliente do participante_destino.                                                                                                                     | Sim      | False   |         |
| fornecedor              | boolean | Indica que o participante_origem é Fornecedor do participante_destino.                                                                                                                  | Sim      | False   |         |
| tecnico                 | boolean | Indica que o participante_origem é Técnico do participante_destino.                                                                                                                     | Sim      | False   |         |
| vendedor                | boolean | Indica que o participante_origem é Vendedor do participante_destino.                                                                                                                    | Sim      | False   |         |
| transportadora          | boolean | Indica que o participante_origem é uma Transportadora do participante_destino.                                                                                                          | Sim      | False   |         |
| representante_comercial | boolean | Indica que o participante_origem é um Representante Comercial do participante_destino.                                                                                                  | Sim      | False   |         |
| representante_tecnico   | boolean | Indica que o participante_origem é um Representante Técnico do participante_destino.                                                                                                    | Sim      | False   |         |
| contato                 | boolean | Indica que o participante_origem é um Contato do participante_destino.                                                                                                                  | Sim      | False   |         |

_Obs. 1: Para os relacionamentos do tipo `lead, Cliente, Fornecedor, Técnico, Vendedor, Transportadora, Representante Comercial e Representante Técnico`, esta tabela de relacionamento será normalmente usada para indicar o relacionamento entre um Participante que figure como uma empresa de um dos grupo de participantes do sistema (marcado com a flag `empresa_membro` como true), e outro participante qualquer (geralmente com a flag `empresa_membro` como false). Nestes casos, a coluna `participante_destino` sempre será preenchida com a `empresa_membro`. Embora seja possível, não se planeja guardar o relacionamento (de cliente, fornecedor, etc) entre participantes onde ambos não sejam membros de um dos grupos de participantes do tenant._

_Obs. 2: Para os relacionamentos do tipo `Contato`, esta tabela de relacionamento será normalmente usada para indicar o relacionamento entre dois Participantes que não figuram como empresas membro de um dos grupos de participantes do sistema (marcados com a flag `empresa_membro` como false). Também pode-se gravar os contatos de uma `empresa_membro`, mas não se planeja esse como um uso normal._

#### Restrições

* unique: (tenant, grupo_participante, participante_origem, participante_destino)
* FK: (participante_origem) aponta para `participante` coluna (id)
* FK: (participante_destino) aponta para `participante` coluna (id)

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

* unique: (tenant, grupo_participante, participante, uf)
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

* unique: (tenant, grupo_participante, participante, ibge)
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

* unique: (tenant, grupo_participante, participante, email)
* unique: (tenant, grupo_participante, participante, principal)
* FK: (participante) aponta para `participante` coluna (id)

### Telefone

> Tabela: telefone

Representa um telefone de um participante.

| Propriedade  | Tipo        | Descrição                                                                                                                               | Not Null | Default | Domínio |
| ------------ | ----------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- | ------- |
| participante | uuid        | Identificador do participante relacionado.                                                                                              | Sim      | Vazio   |         |
| descricao    | string(150) | Descrição do telefone (de acordo com o ponto de vista do cliente).                                                                      | Não      | Vazio   |         |
| ddi          | string(2)   | DDI do telefone.                                                                                                                        | Sim      | Vazio   |         |
| ddd          | string(2)   | DDD do telefone.                                                                                                                        | Sim      | Vazio   |         |
| numero       | string(9)   | Número do telefone.                                                                                                                     | Sim      | Vazio   |         |
| principal    | boolean     | Indica se este é o e-mail principal do participante.                                                                                    | Não      | Vazio   |         |
| ramal        | string(12)  | Ramal do telefone.                                                                                                                      | Sim      | Vazio   |         |
| fax          | boolean     | Indica se é um telefone para remessa de fax.                                                                                            | Não      | Vazio   |
| financeiro   | boolean     | Indica se é um telefone para contato de natureza financeira (normalmente útil para telefones de contato não personificado).             | Não      | Vazio   |
| faturamento  | boolean     | Indica se é um telefone para contato de natureza fiscal, de faturamento (normalmente útil para telefones de contato não personificado). | Não      | Vazio   |
| comercial    | boolean     | Indica se é um telefone para contato de natureza comercial (normalmente útil para telefones de contato não personificado).              | Não      | Vazio   |
| ordem        | int         | Indica a ordem de importância deste telefone (de acordo com a ordenação do usuário).                                                    | Sim      | Vazio   |         |

#### Restrições

* unique: (tenant, grupo_participante, participante, ddi, ddd, telefone)
* unique: (tenant, grupo_participante, participante, principal)
* FK: (participante) aponta para `participante` coluna (id)

### Endereços

> Tabela: endereco

Representa um endereco de um participante.

| Propriedade     | Tipo        | Descrição                                                                            | Not Null | Default | Domínio                                                  |
| --------------- | ----------- | ------------------------------------------------------------------------------------ | -------- | ------- | -------------------------------------------------------- |
| participante    | uuid        | Identificador do participante relacionado.                                           | Sim      | Vazio   |                                                          |
| descricao       | string(150) | Descrição do endereço (de acordo com o ponto de vista do cliente).                   | Não      | Vazio   |                                                          |
| cep             | string(15)  | Código de Endereçamento Postal.                                                      | Sim      | Vazio   |                                                          |
| tipo_logradouro | string(10)  | Tipo do logradouro (rua, aveida, etc).                                               | Sim      | Vazio   | [Ver lista de tipos de logradouro.](tipos_logradouro.md) |
| logradouro      | string(250) | Nome da rua, avenida, etc.                                                           | Sim      | Vazio   |                                                          |
| numero          | string(10)  | Númeração dentro do logradouro.                                                      | Não      | Vazio   |                                                          |
| complemento     | string(250) | Complemento do endereço.                                                             | Não      | Vazio   |                                                          |
| bairro          | string(60)  | Bairro do endereço.                                                                  | Não      | Vazio   |                                                          |
| uf              | string(2)   | Unidade Federativa do endereço.                                                      | Não      | Vazio   |                                                          |
| pais            | string(5)   | Código do país (exemplo: Brasil = 1058).                                             | Sim      | Vazio   |                                                          |
| ibge            | string(250) | Código IBGE do município do endereço.                                                | Não      | Vazio   |                                                          |
| referencia      | string(250) | Referência para melhor localização do endereço.                                      | Não      | Vazio   |                                                          |
| local           | boolean     | Indica se este é o endereço de localização do participante.                          | Não      | Vazio   |                                                          |
| entrega         | boolean     | Indica se é um endereço para entrega de mercadoria.                                  | Não      | Vazio   |
| cobranca        | boolean     | Indica se é um endereço para cabrança financeira.                                    | Não      | Vazio   |
| comercial       | boolean     | Indica se é um endereço de referÊncia comercial do participante.                     | Não      | Vazio   |
| ordem           | int         | Indica a ordem de importância deste endereço (de acordo com a ordenação do usuário). | Sim      | Vazio   |                                                          |

#### Restrições

* unique: (tenant, grupo_participante, participante, local)
* FK: (participante) aponta para `participante` coluna (id)
