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

* empresa_membro (boolean)
* empresa_matriz (uuid)

| Propriedade               | Tipo        | Descrição                                                                                                           | Not Null | Default | Domínio                                                                                                                                                                                                                                                                                                                                          |
| ------------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------- | -------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| codigo                    | string(30)  | Identificador único, do ponto de vista do usuário final.                                                            | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| qualificacao              | string(11)  | Tipo de participante perante a lei (pessoa física, jurídica, etc).                                                  | Sim      | Vazio   | ["fisica", "juridica", "publica_federal", "publica_estadual", "publica_municipal", "cooperativa_credito", "cooperativa_agro", "cooperativa", "financeira", "seguradora_capitalizacao", "corretora_autonoma_seguro", "previdencia_complementar_aberta", "previdencia_complementar_fechada", "economia_mista", "outras_publica_federal", "outros"] |
| documento                 | string(14)  | CNPJ ou CPF do participante (só contendo números).                                                                  | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nome                      | string(150) | Nome do participante.                                                                                               | Sim      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nome_fantasia             | string(150) | Nome fantasia do participante (ou qualquer tipo de nomenclatura alternativa).                                       | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| caepf                     | string(18)  | Cadastro de Atividade Econômica da Pessoas Física.                                                                  | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| rntrc                     | string(14)  | Registro Nacional de Transportadores Rodoviários de Cargas.                                                         | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| nit                       | string(11)  | Número de Inscrição do Trabalhador.                                                                                 | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| observacao                | string      | Texto livre de observações sobre o participante.                                                                    | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| propriedades_customizadas | json        | JSON de propriedades customizadas pelo cliente.                                                                     | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| site                      | string(250) | Endereço eletrônico do participante.                                                                                | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| inativo                   | boolean     | Indica que um participante está inativo (não excluído, apenas oculo por opção do usuário).                          | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |
| cnae_principal            | string(7)   | Classificação Nacional de Atividade Econômica principal.                                                            | Não      | False   |                                                                                                                                                                                                                                                                                                                                                  |
| nascimento                | date        | Data de nascimento do participante (se pessoa física).                                                              | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| inicio_atividades         | date        | Data de início das atividades (se pessoa jurídica ou similar).                                                      | Não      | Vazio   |                                                                                                                                                                                                                                                                                                                                                  |
| empresa_membro            | boolean     | Indica que este participante é uma empresa membro do grupo empresarial relacionado (pela coluna grupo_empresarial). | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |
| empresa_matriz            | uuid        | ID da empresa matriz à qual este participante está ligado como filial.                                              | Sim      | False   |                                                                                                                                                                                                                                                                                                                                                  |

#### Restrições

* unique: (tenant, grupo_empresarial, documento)
* unique: (tenant, grupo_empresarial, codigo)
* FK: (empresa_matriz) aponta para `participante` coluna (id)

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

_Obs. 1: Para os relacionamentos do tipo `lead, Cliente, Fornecedor, Técnico, Vendedor, Transportadora, Representante Comercial e Representante Técnico`, esta tabela de relacionamento será normalmente usada para indicar o relacionamento entre um Participante que figure como uma empresa de um dos grupos empresariais do sistema (marcado com a flag `empresa_membro` como true), e outro participante qualquer (geralmente com a flag `empresa_membro` como false). Nestes casos, a coluna `participante_destino` sempre será preenchida com a `empresa_membro`. Embora seja possível, não se planeja guardar o relacionamento (de cliente, fornecedor, etc) entre participantes onde ambos não sejam membros de um dos grupos empresariais do tenant._

_Obs. 2: Para os relacionamentos do tipo `Contato`, esta tabela de relacionamento será normalmente usada para indicar o relacionamento entre dois Participantes que não figuram como empresas membro de um dos grupos empresariais do sistema (marcados com a flag `empresa_membro` como false). Também pode-se gravar os contatos de uma `empresa_membro`, mas não se planeja esse como um uso normal._

#### Restrições

* unique: (tenant, grupo_empresarial, participante_origem, participante_destino)
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

* unique: (tenant, grupo_empresarial, participante, ddi, ddd, telefone)
* unique: (tenant, grupo_empresarial, participante, principal)
* FK: (participante) aponta para `participante` coluna (id)
