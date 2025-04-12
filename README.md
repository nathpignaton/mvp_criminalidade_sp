# Análise Geográfica e Temporal dos Crimes no Estado de São Paulo

## Objetivo

Este projeto tem como objetivo analisar registros de Boletins de Ocorrência (BOs) no estado de São Paulo entre os anos de 2022 e 2024. O foco está em entender a distribuição temporal, sazonal e geográfica dos crimes, além de gerar indicadores por município.

### Perguntas de negócio:

O trabalho tem a função de responder as perguntas abaixo, separadas em duas partes.

Perguntas relacionadas aos registros em todo o estado de São Paulo:

- Quais as 5 categorias de crime mais registrados no Estado?
- Esses 5 permaneceram os mesmos em 2022, 2023 e 2024?
- Houve sazonalidade? Com naturezas mais registradas ou menos registradas em determinadas épocas do ano? Ou eventos de aumento ou redução drásticas de determinados registros?
- Qual a distribuição geográfica das categorias de crime dentro do estado de São Paulo?

Perguntas relacionadas à distribuição dos tipos ou categorias de crime nos diferentes municípios do estado:

- Qual o percentual de registro de cada tipo de crime nos anos de 2022, 2023 e 2024 em cada município de São Paulo?
- Qual a distribuição geográfica das categorias de crime dentro de cada município?
- Qual a taxa de registro a cada 100 mil habitantes das 3 maiores categorias de crime de cada município?

## Evidências em vídeo
Links para o Google Drive:

[01_ingestao_bronze](https://drive.google.com/file/d/1i5HpQJtn6S74o4cFOrnMvVxkmsFLyJas/view?usp=share_link)

[02_tratamento_silver](https://drive.google.com/file/d/1GY3jBq1n1RrtVpbHidxarkgD04xVUhBP/view?usp=share_link)

03_analise_gold

## Sobre os Dados

Neste trabalho vamos processar os dados abertos de crimes do estado de São Paulo, dos anos de 2022, 2023 e 2024, disponibilizados no portal de transparência do estado, complementados pelo dicionário de dados fornecido pela Secretaria de Segurança Pública do estado de São Paulo.

Os dados completos podem ser encontrados na aba de [estatísticas](https://www.ssp.sp.gov.br/estatistica) do portal de transparência, mais especificamente na parte de [consultas](https://www.ssp.sp.gov.br/estatistica/consultas).

Quanto ao dicionário de dados, como eu já tinha a intenção de analisar esses dados e por esses dados serem oficiais do estado de São Paulo, solicitei no portal [Fala SP](https://fala.sp.gov.br/) informações sobre todos os atributos contidos neles.

### Estrutura dos dados:

- `df_bronze`: Dados brutos, conforme disponibilizados
- `df_silver`: Dados tratados e estruturados para análise
- `df_gold`: Dados prontos para visualização e resposta às perguntas

## Estrutura do Projeto

- `01_ingestao_bronze.ipynb`: Pipeline de carga e estruturação inicial dos dados
- `02_tratamento_silver.ipynb`: Tratamento de inconsistências, limpeza e enriquecimento
- `03_analise_gold.ipynb`: Análises, visualizações e discussão dos resultados

## Tecnologias Utilizadas

- **Plataforma**: Databricks Community Edition
- **Linguagem**: Python
- **Bibliotecas**: PySpark, pandas, matplotlib, seaborn

## Resultados

As principais análises foram realizadas no notebook Gold, com destaque para:
- As 5 categorias de crime mais registradas entre 2022 e 2024
- Sazonalidade nos registros

## Autoavaliação

Consegui atingir parcialmente os objetivos propostos para este trabalho. A principal dificuldade foi perceber tarde demais a complexidade do conjunto de análises que me propus a realizar. Ao tentar incorporar muitas visualizações e comparações complexas, acabei não respondendo a todas as perguntas delineadas inicialmente. A falta de foco na etapa de escopo tornou o fechamento do MVP mais desafiador.

O maior obstáculo técnico foi o uso da plataforma Databricks, especialmente no que diz respeito à manipulação de grandes volumes de dados com PySpark — uma biblioteca com a qual eu tinha menos familiaridade, em contraste com minha experiência prévia com pandas. Tive problemas com a importação de bibliotecas, organização de células e lentidão na execução, além da curva de aprendizado da própria plataforma.

Se eu tivesse mais tempo, faria uma análise aprofundada dos municípios do estado de São Paulo, criando dashboards com indicadores por município e cruzando essas informações com dados sociodemográficos do IBGE. Também separaria a cidade de São Paulo por zonas ou distritos e analisaria temporalidade, campos de texto dos boletins, e níveis de violência dos crimes.

Apesar disso, considero este trabalho um avanço significativo em relação à análise anterior que realizei apenas com dados de 6 meses da capital. A expansão para o estado inteiro me preparou para análises futuras mais complexas — que certamente pretendo realizar como parte do meu portfólio.

---

## Catálogo de Dados

### `df_gold`

| Coluna         | Tipo     | Descrição                                                                 | Domínio / Valores esperados                                                                 |
|----------------|----------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| `ano`          | Integer  | Ano de ocorrência do BO                                                   | 2022, 2023, 2024                                                                             |
| `mes`          | Integer  | Mês de ocorrência do BO                                                   | 1 a 12                                                                                       |
| `latitude`     | Double   | Latitude do local do crime                                                | Pode conter valores ausentes (374.713 vazios)                                               |
| `longitude`    | Double   | Longitude do local do crime                                               | Pode conter valores ausentes (374.712 vazios)                                               |
| `id_bo`        | String   | Identificador único criado para cada BO                                   | Sem repetições; identificador interno, não oficial                                           |
| `natureza`     | String   | Natureza apurada do crime segundo publicação oficial                      | 24 categorias únicas, valores textuais padronizados                                          |
| `municipio`    | String   | Nome do município onde ocorreu o crime                                    | 645 nomes únicos, todos em minúsculas, sem acento ou caracteres especiais (normalizados)    |
| `categoria`    | String   | Agrupamento de crimes definido pelo autor do projeto                      | 8 categorias únicas                                                                         |
| `tipo_crime`   | String   | Classificação mais ampla segundo o Código Penal                           | 7 classificações únicas, abrangentes, oficialmente definidas                                 |

---

### `df_municipio`

| Coluna            | Tipo     | Descrição                                                                                      | Domínio / Observações                                                                 |
|-------------------|----------|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| `municipio`       | String   | Nome do município                                                                               | 645 municípios, nomes normalizados (minúsculas, sem acento ou caractere especial)     |
| `codigo_ibge`     | Integer  | Código IBGE oficial do município                                                               | Completo para todos os municípios                                                     |
| `taxa_100k_22`    | Double   | Taxa de BOs por 100 mil habitantes em 2022                                                     | Calculada com total de BOs de 2022 dividido pela população_22                         |
| `taxa_100k_23`    | Double   | Taxa de BOs por 100 mil habitantes em 2023                                                     | Calculada com total de BOs de 2023 dividido pela população_23                         |
| `taxa_100k_24`    | Double   | Taxa de BOs por 100 mil habitantes em 2024                                                     | Calculada com total de BOs de 2024 dividido pela população_24                         |
| `populacao_22`    | Integer  | População oficial do município em 2022, segundo o Censo                                        | Fonte: IBGE                                                                            |
| `populacao_23`    | Double   | Estimativa da população em 2023, calculada como média de 2022 e 2024                          | Cálculo próprio a partir das populações oficial e estimada                            |
| `populacao_24`    | Integer  | Estimativa oficial da população em 2024, segundo o IBGE                                        | Fonte: IBGE                                                                            |

