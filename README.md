# Mini-projeto de Análise Exploratória de Dados — Varejo

**Aluno:** Maximiliano Markowsky

**Turma:** T8

**Módulo:** Análise de Dados com Python — Módulo 1

## Objetivo

Realizar uma análise exploratória de dados de varejo usando Python e pandas. O projeto inspeciona e trata a base, resume registros de produtos por categoria, período e perfil de cliente e exporta uma cópia tratada dos dados.

## Arquivos do projeto

```text
.
├── Mini_Projeto_Avaliativo_MaximilianoMarkowsky_T8.ipynb  # notebook com diagnóstico, limpeza e análise
├── README.md
└── bases/
    ├── base_varejo.csv                     # dados de entrada
    └── base_varejo_limpo.csv               # dados tratados e exportados
```

## Origem e período dos dados

A base utilizada está em [`bases/base_varejo.csv`](bases/base_varejo.csv). A referência indicada para a base Varejo é o [Kaggle — Base Varejo](https://www.kaggle.com/datasets/namespaiva/base-varejo/data). O arquivo mantido neste repositório é a cópia local utilizada no projeto: contém 830.000 registros, com datas de 1º de fevereiro de 2019 a 19 de agosto de 2022.

O CSV original usa ponto e vírgula (`;`) como separador. Cada linha representa um registro de produto associado a uma compra, e não necessariamente uma compra ou um cliente único. Por isso, as contagens apresentadas neste README são contagens de registros/itens; não representam faturamento, pois a base não contém preço ou valor de venda.

## Colunas

O arquivo original contém dez campos de dados e quatro colunas vazias geradas na origem. As quatro colunas vazias foram descartadas durante a limpeza. O arquivo tratado contém os dez campos de dados e duas colunas derivadas:

| Coluna | Significado |
| --- | --- |
| `DATA` | Data do registro da compra |
| `CO_ID` | Identificador da compra |
| `CL_ID` | Identificador do cliente |
| `CL_GENERO` | Gênero informado para o cliente |
| `CL_EC` | Código de estado civil |
| `CL_FHL` | Quantidade de filhos informada para o cliente |
| `CL_SEG` | Código do segmento do cliente |
| `PR_ID` | Identificador do produto |
| `PR_CAT` | Categoria do produto |
| `PR_NOME` | Nome do produto |
| `CL_TIPO` | Classificação derivada no notebook a partir do número de registros associados ao cliente: eventual (até 500), recorrente (501 a 1.499) ou VIP (1.500 ou mais) |
| `ANO` | Ano derivado da coluna `DATA` |

Os códigos de `CL_EC` e `CL_SEG` são mantidos como códigos; a base disponível não fornece uma legenda para decodificá-los.

## Processo de leitura, limpeza e gravação

No notebook, a leitura é feita com `pandas.read_csv()` usando `sep=';'`. A limpeza trabalha sobre uma cópia do DataFrame original: remove as quatro colunas vazias, converte os marcadores `#N/D` em ausentes e descarta as linhas sem categoria ou nome de produto, remove duplicatas exatas mantendo a primeira ocorrência, padroniza textos (minúsculas, espaços externos, acentos e caracteres especiais) e converte `DATA` para data. Não são preenchidos valores ausentes. Em seguida, a análise cria `CL_TIPO` e `ANO` e salva o resultado em CSV sem índice (`index=False`).

Das 830.000 linhas originais, 3.650 tinham `#N/D` nos campos de categoria/nome do produto. Depois de removê-las, foram descartadas 96.131 linhas duplicadas exatas. Assim, a base tratada tem 730.219 registros — 99.781 a menos que a original. As colunas vazias não continham dados. O arquivo tratado incluído no repositório não apresenta células vazias nem linhas duplicadas exatas.

## Como executar

O notebook utiliza recursos do Google Colab (`google.colab.files`) para enviar o CSV e baixar os resultados. A forma prevista de execução é:

1. Abra `Mini_Projeto_Avaliativo_MaximilianoMarkowsky_T8.ipynb` no Google Colab.
2. No repositório, baixe `bases/base_varejo.csv` e envie esse arquivo quando a primeira célula de upload solicitar. Mantenha o nome `base_varejo.csv`.
3. Execute as células do notebook em ordem. O notebook faz leituras, limpeza, diagnósticos e análises por etapas; algumas etapas também baixam arquivos intermediários.
4. Na última etapa, baixe `base_varejo_limpo_final.csv`. Para manter a organização deste repositório, coloque-o em `bases/` com o nome `base_varejo_limpo.csv`.

A versão tratada já está disponível em [`bases/base_varejo_limpo.csv`](bases/base_varejo_limpo.csv). O notebook pode ser consultado e executado no Colab; a execução local direta não está configurada, pois o fluxo de upload e download depende das ferramentas do Colab.

## Resultados e conclusões

Os números abaixo foram conferidos em `bases/base_varejo_limpo.csv`:

1. Após o tratamento, restaram **730.219 registros**, partindo de 830.000; **99.781 linhas (12,02%)** foram removidas por dados inválidos do produto ou duplicidade exata.
2. **Alimentos** é a categoria com mais registros (**384.197; 52,61%**) e **acessórios** é a categoria com menos (**12.871; 1,76%**).
3. **Presunto cozido** é o produto com mais registros (**12.719**).
4. A coluna `CL_FHL` tem **730.219 valores**; a média é **1,15 filho por registro**, com mínimo de 0 e máximo de 4. Como o atributo do cliente se repete nas linhas de produtos, essa média é ponderada por registros e não por clientes únicos.
5. O ano com mais registros é **2021 (215.850)**. Essa contagem é de registros de produtos, não de compras únicas. Além disso, os períodos de 2019 e 2022 estão incompletos no arquivo, então a comparação entre anos deve ser interpretada com cautela.

**Recomendação:** investigar o mix e a disponibilidade de produtos nas categorias com maior e menor volume de registros antes de decidir ações comerciais. Uma decisão sobre estoque, preço ou rentabilidade exigiria também dados de quantidade vendida, preço e margem, que não estão nesta base.

**Limitação:** a análise descreve registros de produtos e não permite medir faturamento nem comparar clientes únicos sem agregações específicas por `CO_ID` ou `CL_ID`. A cobertura de datas também não corresponde a anos-calendário completos para 2019 e 2022.
