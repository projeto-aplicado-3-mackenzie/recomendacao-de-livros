# Sistema de Recomendação de Livros

Projeto desenvolvido na disciplina Projeto Aplicado III, do curso de Banco de Dados da Universidade Presbiteriana Mackenzie.

O sistema recomenda livros a partir da semelhança entre suas sinopses, ajudando leitores a descobrir títulos relacionados a uma obra de seu interesse.

**[Acessar a aplicação](https://sistema-recomendacao-livros.streamlit.app/)**

**[Consultar o notebook](https://github.com/projeto-aplicado-3-mackenzie/recomendacao-de-livros/blob/main/projeto-aplicado-III.ipynb)**
## Objetivo

Desenvolver um sistema de recomendação baseado em conteúdo, utilizando processamento de texto para comparar sinopses e sugerir novas leituras.

A proposta está relacionada ao **ODS 4 — Educação de Qualidade**, pelo incentivo à leitura e à descoberta de conteúdos de interesse dos usuários.

## Funcionamento

1. O usuário informa um livro disponível no catálogo.
2. O sistema utiliza a representação TF-IDF das sinopses para comparar os livros.
3. Os resultados são ordenados por similaridade.
4. São apresentados dez livros semelhantes, excluindo o próprio registro selecionado.

As recomendações incluem título, autor, gênero, sinopse e pontuação de similaridade.

## Base de dados

Foi utilizada a base **Livros Skoob**, disponibilizada por Victor Stein no Kaggle.

**[Consultar o dataset](https://www.kaggle.com/datasets/victorstein/livros-skoob)**

| Característica | Quantidade |
|---|---:|
| Registros na base original | 11.975 |
| Registros sem sinopse removidos | 657 |
| Livros utilizados pelo modelo | 11.318 |

## Tecnologias utilizadas

- **Python:** desenvolvimento do modelo.
- **pandas:** carregamento e tratamento dos dados.
- **scikit-learn:** representação TF-IDF e cálculo de similaridade.
- **kagglehub:** obtenção da base de dados.
- **Google Colab:** desenvolvimento e realização dos experimentos.
- **Streamlit:** disponibilização da aplicação.

## Modelo de recomendação

A abordagem utiliza filtragem baseada em conteúdo. O texto das sinopses é transformado em vetores por meio do `TfidfVectorizer`.

A comparação é realizada com `linear_kernel`. Como os vetores TF-IDF são normalizados, o resultado corresponde à similaridade de cosseno.

Foram comparadas duas configurações:

- **Original:** palavras individuais e combinações de duas palavras, com `ngram_range=(1, 2)`.
- **Ajustada:** somente palavras individuais, com `ngram_range=(1, 1)`.

Os demais parâmetros foram mantidos: conversão para letras minúsculas, `min_df=2`, limite de 30.000 características e ausência de uma lista específica de stopwords.

## Resultados

A avaliação utilizou uma amostra de **50 livros**, com **dez recomendações por livro**, totalizando **500 recomendações analisadas em cada configuração**.

O critério de correspondência foi a presença de pelo menos um gênero em comum entre o livro de partida e o recomendado.

| Métrica | Modelo original | Modelo ajustado |
|---|---:|---:|
| Precisão por gênero@10 | 24,80% | 25,60% |
| Hit@10 por gênero | 54,00% | 54,00% |
| Recomendações com gênero em comum | 124 | 128 |

O ajuste apresentou um ganho de **0,8 ponto percentual** na precisão, sem alteração no Hit@10.

Em um teste pontual com o livro *A Guerra Pela Uber*, o cálculo de similaridade e a seleção das dez recomendações levaram **0,0409 segundos no Google Colab**. Esse valor corresponde ao teste realizado, não a uma média de tempo da aplicação.

## Limitações

- A qualidade das recomendações depende das informações disponíveis nas sinopses.
- Livros sem sinopse não participam do modelo.
- O TF-IDF compara termos textuais e pode não identificar relações expressas com palavras diferentes.
- A avaliação por gênero indica correspondência temática, mas não mede diretamente a satisfação dos leitores.
- Os resultados apresentados correspondem à amostra avaliada.
 
## Integrantes

- David Wildson Gonçalves Santos
- Ricardo Koozo Taira
- Wanderley de Oliveira Junior

**Universidade Presbiteriana Mackenzie — 2026**
