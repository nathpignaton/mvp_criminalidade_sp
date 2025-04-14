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

## Evidências em imagens no arquivo PDF
[Evidências no GitHub]()

## Sobre os Dados

Neste trabalho vamos processar os dados abertos de crimes do estado de São Paulo, dos anos de 2022, 2023 e 2024, disponibilizados no portal de transparência do estado, complementados pelo dicionário de dados fornecido pela Secretaria de Segurança Pública do estado de São Paulo.

Os dados completos podem ser encontrados na aba de [estatísticas](https://www.ssp.sp.gov.br/estatistica) do portal de transparência, mais especificamente na parte de [consultas](https://www.ssp.sp.gov.br/estatistica/consultas).

Quanto ao dicionário de dados, como eu já tinha a intenção de analisar esses dados e por esses dados serem oficiais do estado de São Paulo, solicitei no portal [Fala SP](https://fala.sp.gov.br/) informações sobre todos os atributos contidos neles.

## Modelagem dos dados

O modelo dimensional adotado neste projeto segue a estrutura clássica de estrela, uma abordagem comum em soluções de engenharia de dados voltadas para análise. Nesse modelo, os dados transacionais ficam concentrados em uma tabela fato (neste caso, a fato_boletim_ocorrencia, `fato_bo`), enquanto os atributos descritivos são organizados em tabelas dimensão, como `dim_tempo`, `dim_municipio` e `dim_crime`. Essa separação permite consultas analíticas rápidas, reduz a redundância e facilita a criação de relatórios, dashboards e visualizações orientadas a múltiplos eixos (tempo, localização e natureza criminal). A modelagem estrela foi escolhida por sua clareza, escalabilidade e compatibilidade com ferramentas de BI e ambientes de consulta SQL.

## Estrutura do Projeto

- `01_ingestao_bronze.ipynb`: Pipeline de carga e estruturação inicial dos dados, link do Databricks [aqui](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3811145726725248/3094996477823788/5963056565390150/latest.html)
- `02_tratamento_silver.ipynb`: Tratamento de inconsistências, limpeza e enriquecimento, link do Databricks [aqui](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3811145726725248/2953575381780261/5963056565390150/latest.html)
- `03_analise_gold.ipynb`: Análises, visualizações e discussão dos resultados, link do Databricks [aqui](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3811145726725248/3450117763868196/5963056565390150/latest.html)

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

### `fato_bo`
| Coluna         | Tipo     | Descrição                                                                 |
|----------------|----------|---------------------------------------------------------------------------
| `id_bo`          | String  | Identificador único do boletim de ocorrência (chave primária)            |
| `id_tempo`          | Integer  | Chave estrangeira para a dimensão tempo                              | 
| `codigo_ibge`     | Integer   | Latitude do local do crime                                             |  
| `id_crime`    | String   | Chave estrangeira para a dimensão crime                                      | 
| `latitude`        | Double   | Identificador único criado para cada BO                                 |
| `longitude`     | Double   | Natureza apurada do crime segundo publicação oficial                      |

---

### `dim_tempo`
| Coluna         | Tipo     | Descrição                                                                 |
|----------------|----------|---------------------------------------------------------------------------
| `id_tempo`          | Integer  | Identificador único do tempo          |
| `ano`          | Integer  | Ano do registro                              | 
| `mes`     | Integer   | Mês do registro                                             |  

---

### `dim_municipio`
| Coluna         | Tipo     | Descrição                                                                 |
|----------------|----------|---------------------------------------------------------------------------
| `municipio`          | String  | Nome do município (normalizado, sem acentos)      |
| `codigo_ibge`          | Integer  | Código oficial IBGE do município (chave primária)           | 
| `pop_censo_22`     | Integer   | População do censo de 2022                                            |  
| `pop_estimada_23`          | Float  | População estimada para 2023          |
| `pop_estimada_24`          | Integer  | População estimada para 2024                              | 

---

### `dim_crime`
| Coluna         | Tipo     | Descrição                                                                 |
|----------------|----------|---------------------------------------------------------------------------
| `id_crime`          | String  | Identificador único do tipo de crime      |
| `natureza`          | String  | Natureza apurada do crime (conforme boletim oficial)           | 
| `categoria`     | String   | Categoria agregada de crime definida no projeto                                 |  
| `tipo_crime`          | String  | Classificação ampla conforme Código Penal          |
