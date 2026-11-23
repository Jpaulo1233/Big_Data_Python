# Violência contra a Mulher no Estado do Rio de Janeiro

Análise de dados sobre **feminicídio** e **tentativa de feminicídio** no Estado do Rio de Janeiro (nov/2016 a jul/2026), feita em Python como trabalho da disciplina **Tópicos de Big Data em Python** (Estácio Niterói, Profª. Simone Gama).

## Sobre o projeto

O objetivo é transformar uma base grande de registros policiais em informação clara para conscientizar a população e os órgãos públicos sobre a violência contra a mulher. O trabalho foi desenvolvido individualmente, com foco em limpeza, exploração e visualização dos dados.

## Fonte dos dados

- **ISP-RJ** (Instituto de Segurança Pública do Rio de Janeiro): https://www.ispdados.rj.gov.br/estatistica.html
- Arquivo: `BaseFeminicidioEvolucaoMensalCisp.csv` (separador `;`, codificação `latin-1`)
- Base: 17.165 linhas × 9 colunas, 138 CISPs, 41 AISPs, 7 RISPs, 92 municípios, sem valores nulos.

## Tecnologias

Python · Google Colab · Pandas · NumPy · Matplotlib · Seaborn · SciPy · Scikit-learn

## Etapas do trabalho

1. **Definição do tema** e escolha da fonte.
2. **Pipeline de limpeza e extração**: leitura do CSV, checagem de nulos e tipos.
3. **Análise exploratória**: totais mensais e anuais, ranking de municípios, estatísticas descritivas.
4. **Outliers (boxplot)**: regra de Tukey (Q3 + 1,5·IQR).
5. **Regressão linear** entre feminicídios e tentativas.
6. **Apresentação** dos resultados.

## Principais resultados

- **877 feminicídios** e **2.937 tentativas** no período.
- Média mensal no Estado: **7,5 feminicídios** e **25,1 tentativas**.
- A cidade do **Rio de Janeiro** concentra **308 feminicídios (35,1%)**.
- Feminicídios por ano: 68 (2017) → 111 (2022) → 105 (2025).
- Cerca de **3,3 tentativas para cada feminicídio**.
- Regressão mensal: R² = 0,039 (relação linear fraca). A regressão anual com os anos completos (2017–2025) tem R² = 0,37 e p = 0,083, ou seja, não é estatisticamente significativa.

### Observação metodológica sobre o boxplot

Em ~95% das linhas CISP×mês o valor é 0, então Q1 = mediana = Q3 = 0 e todo valor positivo vira outlier (caixa "achatada"). Por isso o boxplot usa os **totais mensais do Estado** (117 meses), que mostram a distribuição de forma legível.

## Como executar

1. Abra um notebook no Google Colab e envie o CSV.
2. Rode:

```python
import pandas as pd, numpy as np
import seaborn as sns, matplotlib.pyplot as plt
from scipy import stats

df = pd.read_csv("BaseFeminicidioEvolucaoMensalCisp.csv", sep=";", encoding="latin-1")
colunas = ["Feminicídio", "Tentativa de feminicídio"]

# Totais mensais no Estado
mes = df.groupby(["Ano", "Mês"])[colunas].sum().reset_index()

# Boxplot
sns.boxplot(data=mes[colunas])
plt.show()

# Regressão linear
reg = stats.linregress(mes["Feminicídio"], mes["Tentativa de feminicídio"])
print(reg.rvalue**2, reg.pvalue)
```

## Limitações

- Os valores são **contagens absolutas**, não taxas por habitante.
- A base reúne registros policiais; subnotificação não é medida.
- Comparações anuais usam apenas anos completos (2017–2025).
- A análise **não mede o efeito de penas** ou políticas públicas.
- Significado da coluna `Fase` não confirmado.

## Autor

**João Paulo Cardoso de Almeida** — Estácio Niterói
Disciplina: Tópicos de Big Data em Python · Profª. Simone Gama · 2026
