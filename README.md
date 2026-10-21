# Análise Macroeconômica de Valória

Diagnóstico estatístico dos indicadores econômicos da República de Valória (país fictício), feito em Python, com estatística descritiva, testes de hipóteses, ANOVA e regressão linear, e encerrado com um informe executivo em formato de *storytelling* com dados.

Projeto desenvolvido para a disciplina de *Inferência Estatística e Análise de Dados* do Centro Universitário Impacta (IMPACTA).

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Gsqn9leUs9H8zAUm0pdryUBChN66wi-y#scrollTo=c90ed22e)

> **Aviso:** Valória é fictícia e a base é simulada. Os resultados descrevem apenas este conjunto de dados e servem como exercício metodológico, não como evidência sobre um país real. Todas as relações encontradas são associações, não causalidade.

---

## Contexto

O Conselho de Política Econômica do Banco Central de Valória solicitou um diagnóstico estatístico rigoroso sobre o comportamento dos indicadores nacionais. O notebook responde a quatro perguntas de negócio:

1. Os indicadores são estáveis ou mostram tendência ao longo dos anos?
2. As regiões e as orientações políticas diferem de forma relevante?
3. O que explica o desemprego e o investimento estrangeiro?
4. Que decisões as evidências sustentam?

## Base de dados

| Item | Descrição |
|---|---|
| Arquivo | `dados_macroeconomicos_valoria.xlsx` |
| Registros | 100 (2000 a 2024, sem valores ausentes) |
| Geração | O próprio notebook cria o arquivo na primeira célula, sem upload manual |

| Variável | Tipo | Descrição |
|---|---|---|
| `ID`, `Ano` | Identificador / temporal | Identificação e ano do registro |
| `Regiao` | Qualitativa nominal | Norte, Sul, Leste ou Oeste |
| `Orientacao_Politica` | Qualitativa binária | Desenvolvimentista ou Liberal |
| `Crescimento_PIB_Pct` | Quantitativa contínua | Variação percentual do PIB |
| `Inflacao_Pct` | Quantitativa contínua | Taxa de inflação (%) |
| `Taxa_Juros_Selic` | Quantitativa contínua | Taxa básica de juros (%) |
| `Desemprego_Pct` | Quantitativa contínua | Desemprego (% da população economicamente ativa) |
| `Investimento_Estrangeiro_Milhoes` | Quantitativa contínua | Capital estrangeiro direto (US$ milhões) |
| `Indice_Confianca_Consumidor` | Ordinal (1 a 5) | Confiança do consumidor |

## Estrutura do notebook

| Seção | Conteúdo |
|---|---|
| 0. Geração do arquivo | Cria o `.xlsx` com a base oficial |
| 1 a 3. Configuração e dados | Bibliotecas, leitura com `pd.read_excel`, verificação de ausentes e duplicatas |
| 4. Estatística descritiva | Média, mediana, desvio-padrão, variância, IQR, mínimo, máximo, assimetria e curtose; histogramas com KDE, boxplots por região e heatmap de correlação; teste de tendência temporal (Pearson e Spearman contra `Ano`) |
| 5. Normalidade e aderência | Shapiro-Wilk (PIB e inflação) e qui-quadrado de aderência entre as regiões |
| 6. Testes para 2 grupos | Levene, t de Student e Mann-Whitney U |
| 7. ANOVA de um fator | Desemprego por região |
| 8. ANOVA de dois fatores | Investimento estrangeiro por região × orientação política, com interação |
| 9. Regressão linear simples | Desemprego em função do PIB |
| 10. Regressão linear múltipla | Investimento estrangeiro em função de PIB, inflação, Selic e desemprego, com R² ajustado e VIF |
| 11. Diagnóstico de resíduos | Gráfico de resíduos e QQ-plot |
| 13. Informe executivo | Relatório ao Ministro da Economia: introdução, diagnóstico, validação das políticas públicas, modelo preditivo e limitações |

Todos os testes usam nível de significância de **5%**, com H₀ e H₁ formalizadas antes de cada resultado.

## Principais resultados

| Análise | Resultado |
|---|---|
| Shapiro-Wilk | PIB compatível com a Normal (p = 0,358); inflação rejeita normalidade (p = 0,019) |
| Qui-quadrado (regiões) | Distribuição uniforme não rejeitada (χ² = 0,88; p = 0,830) |
| Tendência temporal | Nenhum indicador tem correlação significativa com o ano (todos os p > 0,31) |
| Levene (PIB × orientação) | Variâncias iguais (p = 0,918) |
| t de Student (PIB × orientação) | Diferença significativa (t = 3,65; p = 0,0004): PIB médio de +0,67% no grupo Desenvolvimentista e −0,09% no Liberal |
| Mann-Whitney (inflação × confiança) | Sem diferença significativa (p = 0,540) |
| ANOVA 1 fator (desemprego × região) | Sem diferença (F = 0,356; p = 0,785) |
| ANOVA 2 fatores (investimento) | Região, orientação e interação não significativas (p = 0,650; 0,303; 0,541) |
| Regressão simples | Desemprego = 12,038 − 0,827 × PIB; R² = 0,754; F = 299,8 |
| Regressão múltipla | Nenhum coeficiente significativo; R² ajustado = 0,002; F global p = 0,380; VIFs abaixo de 5 |

**Em resumo:** o crescimento do PIB explica cerca de três quartos da variação do desemprego; a orientação Desenvolvimentista coincide com maior crescimento; e nenhum dos indicadores analisados explica o investimento estrangeiro, que deve ser investigado com outras variáveis.

## Adaptações em relação ao enunciado

A base fornecida não contém `Periodo_Politica` nem `Meta_Inflacao_Cumprida`. As comparações correspondentes foram adaptadas:

- **Levene e t de Student:** comparam o PIB entre `Orientacao_Politica` (Desenvolvimentista × Liberal), a variável binária de política disponível.
- **Mann-Whitney U:** compara a inflação entre grupos de confiança do consumidor (baixa: índice 1 ou 2; alta: índice 4 ou 5; o nível 3 fica de fora).

## Como executar

### No Google Colab

1. Clique no botão **Abrir no Colab** no topo deste README.
2. Execute **Ambiente de execução → Executar tudo**.

### Localmente

```bash
git clone https://github.com/RichardMendes49/Analise_Econometrica.git
cd Analise_Econometrica
pip install pandas numpy scipy statsmodels seaborn matplotlib openpyxl jupyter
jupyter notebook Projeto_Analise_Macroeconomica.ipynb
```

## Tecnologias

Python 3 · pandas · NumPy · SciPy · statsmodels · seaborn · Matplotlib

## Estrutura do repositório

```
.
├── Projeto_Analise_Macroeconomica_Valoria_final.ipynb   # notebook com análise e informe executivo
├── dados_macroeconomicos_valoria.xlsx                    
└── README.md
```

## Autor

**Richard Bernardino Mendes**
Centro Universitário Impacta, disciplina de Inferência Estatística e Análise de Dados
Professor: Msc. Kyung Moo Kim
