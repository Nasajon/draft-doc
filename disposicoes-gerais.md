# Disposições Gerais das APIs do ERP Nasajon (ERP 4.0)

Nesta sessão, são apresentados os padrões técnicos comuns a maioria das APIs disponíveis no ERP Nasajon.

Sugere-se a leitura preliminar desta sessão, antes mesmo do estudo de qualquer rota em particular.

## Das Entidades

A maioria das entidades tratadas pelas APIs do ERP Nasajon apresentam as seguintes propriedades comuns, presentes em qualquer representação expandida dos respectivos dados:

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| criado_em | date-time | Data e hora de cadastro do registro. |
| criado_por | uuid | Identificador único interno, do usuário responsável pelo cadastro. |
| atualizado_em | date-time | Data e hora da última atualização do registro. |
| atualizado_por | uuid | Identificador único interno, do usuário responsável pela última atualização. |

## Das Rotas

### Parâmetro Fields

A maioria das rotas de recuperação de recursos (normalmente ligadas ao método HTTP GET), suporta o parêmtro ```fields```, o qual funciona como um indicador das propriedades que se desejam acessar para os objetos.

Em resumo, as APIs do ERP Nasajon trabalham com um paradigma de retorno enxuto, de modo que as representações expandidas dos objetos, em sua maioria, abrangem apenas as propriedades de identificações de tais objetos.

Caso se deseje uma propriedade complementar, é necessário especificar a mesma por meio do parâmetro ```fields``` (passado euquanto _query parameter_ nas chamadas).

Esse parâmetro recebe uma lista de propriedades separadas por vírgula, e se for necessário especificar uma propriedades de uma entidade relacionada, deve-se adicionar a mesma entre parênteses, ou usando o padrão de propriedades com ".". Ambas as sintaxes são igualmente aceitas. Ver exemplo abaixo (possível na recuperação de um Produto):

> fields=ncm,preco_venda,unidade_padrao(casas_decimais,padrao,razao_conversao)

> fields=ncm,preco_venda,unidade_padrao.casas_decimais,unidade_padrao.padrao,unidade_padrao.razao_conversao

### Parâmetro Expand

A maioria das rotas de recuperação de recursos (normalmente ligadas ao método HTTP GET), suporta o parâmetro ```expand```, o qual indica se os relacionamentos devem ou não ser expandidos em um JSON aninhado ao recursos pedido.

Essa parâmetro é opcional e pode ser passado como _query parameter_ nas chamadas.

Se o parâmetro ```fields``` também for passado, e contiver indicação a uma propriedade de uma entidade relacionada, tal entidade será expandida independente do uso do parâmetro _expand_.

A sintaxe padrão de uso é:

> expand=propriedade1,propriedade2,propriedade3

E, os valores false ou null são tratados de modo equivalente.

### Padrão de Resposta para Erros

Todos os retornos com erro seguem o seguinte padrão de reposta:

```http
{
    "errors": [
        {
            "code": "<CÓDIGO MOPE DO ERRO - MOPE_CODE-E000)>",
            "msg": "<MENSAGEM DESCRITIVA DO ERRO OCORRIDO>"
        }
    ]
}
```

Todos os erros ocorridos numa requisição são retornados numa lista, de modo que o a aplicação cliente poderá tratar os erros numa única interação com o servidor do ERP Nasajon.

Além disso, o código dos erros segue o padrão:

> <NÚMERO DO PROCESSO MOPE>-E<INTEIRO INCREMENTAL COM 3 DÍGITOS>

Por exemplo:

> 4311-E001

### Paginação das Respostas

A maioria das rotas de recuperação de listagem de objetos (HTTP GET, sem especificiação de identificador de objeto), apresenta um padrão de retorno paginado. Deste modo, as APIS do ERP Nasajon são pouco suscetíveis a ocorrência de timeouts (por demora do servidor) em suas chamadas.

Este padrão de implementação prevê os seguintes parâmetros para as rotas:

| Parâmetro | Tipo | Descrição |
| -- | -- | -- |
| limit | integer | Indica o número máximo de registros a retornar (limitado a 250). |
| offset | string | Indica o salto inicial na consulta dos registros. É o Identificador do último item da página anterior da listagem. |

Além disso, o próprio formato JSON das respostas também é padronizado, de forma a facilitar o tratamento do retorno por parte da aplicação cliente:

```http
{
    "count": <NÚMERO DE REGISTROS RETORNADOS NA LISTA "results">,
    "next": <URL PARA RETORNO DA PRÓXIMA PÁGINA>,
    "results": [
        {
            <JSON DO OBJETO EM SI>
        },
        ...
    ]
}

Obs.: Este formato de retorno se inspira no padrão REST navegável, para facilitar o uso das APIs do ERP Nasajon.