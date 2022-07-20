# Disposições Gerais do BD do ERP4

Nesta sessão, constam as disposições gerais a serem observadas em todas as partes da modelagem do banco de dados do ERP4.

## Entidades Base das Instalações

Os entidades abaixo são base para o ERP4, tendo suas referências presentes em todas as demais tabelas do banco de dados, o que permitirá fácil segmentação de dados, bem como importação, exportação e exclusão em massa.

Além disso, as configurações gerais do sistema estão, via de regra, associadas também a tais entidades.

### Tenant

> Tabela: tenant

Tabela que representa uma instalação (ou provisionamento) de ERP, para um cliente final (contratante do sistema).

| Propriedade | Tipo        | Descrição                                   | Not Null | Default |
| ----------- | ----------- | ------------------------------------------- | -------- | ------- |
| tenant      | bigint      | Número identificador único do tenant (PK).  | Sim      | Vazio   |
| descricao   | string(250) | Descrição textual (user friendly) do tenant | Sim      | Vazio   |

#### Restrições

* primary key: (tenant)

### Grupo empresarial

> Tabela: grupo_empresarial

Tabela que representa um grupo empresarial de um tenant. A ideia aqui é que os dados de uma instalação são contidos nos tenants, mas, seguimentados, obrigatoriamente, por grupos empresariais (permitindo a exportaçao e importação de grupos inteiros).

| Propriedade | Tipo        | Descrição                                                                                          | Not Null | Default | Domínio |
| ----------- | ----------- | -------------------------------------------------------------------------------------------------- | -------- | ------- | ------- |
| id          | uuid        | Identificador único da tabela, usado como PK, e para controle de FKs, mas, não expôsto pelas APIs. | Sim      | Vazio   |         |
| codigo      | string(30)  | Código único do grupo (chave primária, do ponto de visto do usuário final).                        | Sim      | Vazio   |         |
| descricao   | string(250) | Descrição textual (user friendly) do grupo empresarial.                                            | Sim      | Vazio   |         |
| tenant      | bigint      | Identificador do tenant ao qual o dado pertence.                                                   | Sim      | Vazio   |         |

#### Restrições

* primary key: (id)
* unique: (tenant, codigo)

## Propriedades comuns a todas as demais entidades (que representam dados de uma instalação)

| Propriedade       | Tipo        | Descrição                                                                                            | Not Null | Default | Domínio |
| ----------------- | ----------- | ---------------------------------------------------------------------------------------------------- | -------- | ------- | ------- |
| id                | uuid        | Identificador único da tabela, usado como PK, e para referências de FKs, mas, não expôsto pelas APIs | Sim      | Vazio   |         |
| criado_em         | datetime    | Indica data e hora de cadastro do registro.                                                          | Não      | now()   |         |
| criado_por        | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela criação do registro.                     | Sim      | Vazio   |         |
| atualizado_em     | datetime    | Indica data e hora da última atualização do registro.                                                | Não      | Vazio   |         |
| atualizado_por    | string(150) | Indica e-mail da conta Nasajon, do usuário responsável pela última atualização do registro.          | Sim      | Vazio   |         |
| apagado_em        | datetime    | Indica data e hora quando o registro sofreu exclusão lógica ("soft delete"), se ocorrer.             | Não      | Vazio   |         |
| grupo_empresarial | uuid        | Identificador do grupo empresarial ao qual o registro pertence.                                      | Sim      | Vazio   |         |
| tenant            | bigint      | Identificador do tenant ao qual o registro pertence.                                                 | Sim      | Vazio   |         |

#### Restrições

* primary key: (tenant)
