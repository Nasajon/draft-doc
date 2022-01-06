# 4311 - Gerenciar cadastro de peças e materiais

Esta atividade tem por objetivo garantir que os cadastros de peças e materiais estão corretamente identificados, e não existem duplicações nem faltas para as necessidades dos produtos e serviços comercializados ou suportados pela empresa.

## Entidades

### Produto

Representa um material cadastrado no ERP, utilizado como insumo para diversos outros processos (como Compras, Vendas, Insumos para Prestação de Serviços, etc).

#### Propriedades

##### Identificação

Propriedades contidas em qualquer representação expandida dos produtos:

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| id | uuid | Representa o identificar único interno de um produto.
| codigo | string | Representa o identificador único do ponto de vista do usuário. |
| descricao | string | Descrição textual explicativa do produto (para o usuário final). |

Obs.: Além destas, os produtos apresentam também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizaqdo_por]. [Ver a sessão de disposições gerais das APIs do ERP Nasajon.](../../../disposicoes-gerais.md)

##### Complementares

| Propriedade | Tipo | Descrição | Domínio |
| -- | -- | -- | -- |
| codigo_barras | string | Código de barras do produto (normalmente comum ao registro nacional). |  |
| ncm | string | Nomenclatura Comum do Mercosul. |  |
| preco_venda | number(20,4) | Preço de venda praticado. |  |
| peso_bruto | number(20,4) | Peso bruto do produto. |  |
| peso_liquido | number(20,4) | Peso líquido do produto. |  |
| origem_mercadoria | string | Identifica a origem da mercadoria. | <ul><li>_"nacional":_ Fabricação Nacional.</li><li>_"estrageira_direta":_ Estrangeira - Importação Direta.</li><li>_"estrangeira_indireta":_ Estrangeira - Importada Adquirida no Mercado Interno.</li><li>_"nacional_40_70":_ Mercadoria ou bem com Conteúdo de Importação superior a 40% e inferior ou igual a 70% (setenta por cento).</li><li>_"nacional_legislacao_ajustes":_ Nacional, cuja produção tenha sido feita em conformidade com os processos produtivos básicos de que tratam as legislações citadas nos ajustes.</li><li>_"nacional_40":_ Mercadoria ou bem com conteúdo de importação inferior ou igual a 40%.</li><li>_"estrangeira_direta_camex":_ Importação direta, sem similar nacional, constante em lista de resolução CAMEX e gás natural.</li><li>_"estrangeira_indireta_camex":_ Adquirida no mercado interno, sem similar nacional, constante em lista de resolução CAMEX e gás natural.</li><li>_"nacional_70":_ Mercadoria ou bem com Conteúdo de Importação superior a 70% (setenta por cento).</li></ul> |
| grupo_inventario | string | Identifica o grupo de inventário do material. | <ul><li>_"mercadorias":_ Mercadorias</li><li>_"materias_primas":_ Matérias-Primas</li><li>_"produtos_intermediarios":_ Produtos Intermediários</li><li>_"materiais_embalagem":_ Material de Embalagem</li><li>_"manufaturados":_ Produtos Acabados ou Manufaturados</li><li>_"em_fabricacao":_ Produtos em Fase de Fabricação</li><li>_"bens_terceiros":_ Bens de Terceiros</li><li>_"ativo":_ Ativo Permanente</li><li>_"uso_consumo":_ Uso e Consumo</li><li>_"outros":_ Outros</li><li>_"bens_poder_terceiros":_ Bens em Poder de Terceiros</li><li>_"bens_terceiros_poder_terceiros":_ Bens de Terceiros em Poder de Terceiros</li><li>_"subprodto":_ Subproduto</li><li>_"outros_insumos":_ Outros insumos</li><li>_"gorjeta":_ Gorjeta</li></ul> |
| familia | [Familia](#família) | Objeto representado a família do produto (relacionada com o registro). |  |
| unidades | [Unidade[]](#unidade)) | Lista de objetos representado as unidades disponíves para o produto (relacionadas com o registro). |  |
| fabricante | [Fabricante](TODO) | Objeto representado o Fabricante do produto (relacionado com o registro). |  |
| categoria | [Categoria](#categoria) | Objeto representado a Categoria do produto (relacionado com o registro). |  |
| tributacao | [TributacaoProduto](#tributacaoproduto) | Objeto representando as normas de tributação aplicáveis ao Produto (relacionado com o registro). |  |

### Unidade

Representa uma unidade aplicável a um produto usado no ERP.

#### Propriedades

##### Identificação

Propriedades contidas em qualquer representação expandida de uma unidade:

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| id | uuid | Representa o identificar único interno de uma unidade.
| codigo | string | Representa o identificador único do ponto de vista do usuário. |
| descricao | string | Descrição textual explicativa da unidade (para o usuário final). |

Obs.: Além destas, as unidades apresentam também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizaqdo_por]. [Ver a sessão de disposições gerais das APIs do ERP Nasajon.](../../../disposicoes-gerais.md)

##### Complementares

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| casas_decimais | integer | Número de casas decimais aplicáveis a unidade (exemplo, 2 casasa para litros). |
| padrao | boolean | Identifica se esta é a unidade padrão de medida do produto relacionado. |
| embalagem | boolean | Identifica se esta unidade corresponde a uma forma de embalagem do produto no ERP. |
| embalagem_id | uuid | Identificador único da embalagem correspondente no ERP (para unidades que representem embalagens agrupadoras de um produto). |
| razao_conversao | integer | Multiplicador usado como fator de conversão entre a unidade em questão, e a unidade padrão do produto. |

### Família

Representa um agrupamento não hierárquico de materiais usados no ERP (com base em suas características comuns).

#### Propriedades

##### Identificação

Propriedades contidas em qualquer representação expandida de uma família:

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| id | uuid | Representa o identificar único interno de uma família.
| codigo | string | Representa o identificador único do ponto de vista do usuário. |
| descricao | string | Descrição textual explicativa da família (para o usuário final). |

Obs.: Além destas, as famílias apresentam também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizaqdo_por]. [Ver a sessão de disposições gerais das APIs do ERP Nasajon.](../../../disposicoes-gerais.md)

### Categoria

Representa um agrupamento hierárquico dos materiais usados no ERP (com base em suas características comuns, e permitindo a construção de um agrupamento em árvore).

#### Propriedades

##### Identificação

Propriedades contidas em qualquer representação expandida de uma categoria:

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| id | uuid | Representa o identificar único interno de uma categoria.
| codigo | string | Representa o identificador único do ponto de vista do usuário. |
| descricao | string | Descrição textual explicativa da categoria (para o usuário final). |

Obs.: Além destas, as categorias apresentam também as propriedades resumo herdadas: [criado_em, criado_por, atualizado_em, atualizaqdo_por]. [Ver a sessão de disposições gerais das APIs do ERP Nasajon.](../../../disposicoes-gerais.md#das_entidades)

##### Complementares

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| categoria_pai | uuid | Identificador único da categoria pai, permitindo a construção da árvore de agrupamento. |

### TributacaoProduto

Representa o conjunto de informações tributárias aplicáveis a um determinado produto.

#### Propriedades

Propriedades contidas em qualquer representação expandida de uma tributação de produto:

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| cest | string | Código Especificador da Substituição Tributária. |

## Rotas

### Listar Produtos

> GET dados-mestre/4311/produtos

#### Parâmetros da rota

| Parâmetro | Tipo | Obrigatório | Descrição |
| -- | -- | -- | -- |
| grupo_empresarial | uuid | Sim | Identificador único interno de grupo_empresarial desejado. |
| criado_apos | date |  | Permite filtrar produtos criados após uma determinada data (condição: maior que). |
| atualizado_apos | date |  | Permite filtrar produtos atualizados após uma determinada data (condição: maior que). |

#### Parâmetros herdados

| Parâmetro | Tipo | Descrição |
| -- | -- | -- |
| fields | uuid | Listagem das [propriedades complementares](#complementares) a serem retornadas na chamada. Consulte as [disposições gerais](../../../disposicoes-gerais.md#parâmetro-fields) para mais informações. |
| expand | boolean | Indica se os relacionamentos devem ou não ser expandidos em JSON aninhado. Consulte as [disposições gerais](../../../disposicoes-gerais.md#parâmetro-expand) para mais informações. |
| limit | integer | Indica o número máximo de registros a retornar (limitado a 250). Consulte as [disposições gerais](../../../disposicoes-gerais.md#paginação-das-respostas) para mais informações. |
| offset | integer | Indica o salto inicial na consulta dos registros. Consulte as [disposições gerais](../../../disposicoes-gerais.md#paginação-das-respostas) para mais informações. |

#### Formato de Retorno

| HTTP Status | Descrição |
| -- | -- |
| 200 | Ok (ver exemplos a seguir) |
| 400 | Requisição inválida, causa provável: parâmetro incorreto. **(*)** |
| 401 | Proibido acesso, causa provável: falha na autenticação. **(*)** |
| 403 | Proibida ação, causa provável: falha na autorização. **(*)** |
| 500 | Erro desconhecido (ver detalhes no corpo da resposta). **(*)** |

* **(*):** Ver [disposições gerais sobre erros](../../../disposicoes-gerais.md#padrão-de-resposta-para-erros), para mais informações.

##### Exemplos de retorno

###### Listagem resumida

```http
GET dados-mestre/4311/produtos?grupo_empresarial=5df5c57d-c0d8-4a79-a5fc-c3cfa3a20a66

{
    "count": 1,
    "next": null,
    "prev": null,
    "results": [
        {
            "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "codigo": "FARINHA",
            "descricao": "Farinha de trigo sem fermento.",
            "criado_em": "2021-12-09T23:58:49",
            "criado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "atualizado_em": "2021-12-09T23:58:49",
            "atualizado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
        }
    ]
}
```

###### Listagem com fields
```http
GET dados-mestre/4311/produtos?grupo_empresarial=5df5c57d-c0d8-4a79-a5fc-c3cfa3a20a66&fields=ncm,preco_venda,unidades

{
    "count": 1,
    "next": null,
    "prev": null,
    "results": [
        {
            "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "codigo": "FARINHA",
            "descricao": "Farinha de trigo sem fermento.",
            "criado_em": "2021-12-09T23:58:49",
            "criado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "atualizado_em": "2021-12-09T23:58:49",
            "atualizado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "ncm": "85015210",
            "ncm": "85015210",
            "preco_venda": 5.76,
            "unidades": ["8105a0a7-7132-4349-a488-1e4a9333ca79", "8eeb2b3d-de9d-4f59-ab0c-10a5ab81bf31"]
        }
    ]
}
```


###### Listagem com fields e expand
```http
GET /4311/material-peca/produtos?grupo_empresarial=5df5c57d-c0d8-4a79-a5fc-c3cfa3a20a66&fields=ncm,preco_venda,unidades&expand=true

{
    "count": 1,
    "next": null,
    "prev": null,
    "results": [
        {
            "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "codigo": "FARINHA",
            "descricao": "Farinha de trigo sem fermento.",
            "criado_em": "2021-12-09T23:58:49",
            "criado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "atualizado_em": "2021-12-09T23:58:49",
            "atualizado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "ncm": "85015210",
            "ncm": "85015210",
            "preco_venda": 5.76,
            "unidades": [
                {
                    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
                    "codigo": "UN",
                    "descricao": "Unidade",
                    "criado_em": "2021-12-09T23:58:49",
                    "criado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
                    "atualizado_em": "2021-12-09T23:58:49",
                    "atualizado_por": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
                }
            ]
        }
    ]
}
```

###### Retorno com erro

```http
GET dados-mestre/4311/produtos

{
    "errors": [
        {
            "code": "4311-E001",
            "msg": "Faltando parâmetro "grupo_empresarial" na chamada."
        }
    ]
}
```

##### Códigos internos de erro

| Codigo | Descrição |
| - | - |
| 4311-E001 | Faltando parâmetro "grupo_empresarial" na chamada. |

TODO Complementar com códigos de erro
