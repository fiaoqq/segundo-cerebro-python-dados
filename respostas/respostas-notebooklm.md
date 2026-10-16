### 1. Fluxo básico para analisar um dataset em Python

O fluxo de trabalho (*data pipeline*) para análise de dados consiste em etapas encadeadas em que a saída de uma fase serve como entrada para a seguinte:

1. **Coleta de Dados:** Ingestão de dados brutos a partir de fontes como arquivos CSV, planilhas Excel, bancos de dados SQL ou consumo de APIs.
2. **Limpeza e Tratamento (*Data Preparation / Wrangling*):** Adequação e higienização dos dados, incluindo identificação e tratamento de valores ausentes, conversão de tipos de variáveis (*dtypes*), remoção de duplicatas, tratamento de *outliers* e escalonamento.
3. **Análise Exploratória de Dados (AED):** Investigação de padrões, cálculo de estatísticas descritivas (como média, mediana, desvio padrão e quantis) e geração de gráficos univariados e multivariados.
4. **Modelagem e Extração de Insights:** Aplicação de algoritmos estatísticos e de *Machine Learning* (como regressão, classificação ou agrupamento) para validar hipóteses, prever tendências e comunicar descobertas.

---

### 2. Como lidar com valores ausentes no pandas

No **pandas**, valores faltantes são representados por `NaN`, `None`, `NaT` ou `<NA>`. A abordagem para tratá-los envolve três passos principais:

* **Identificação e Quantificação:**
  * `df.info()`: Exibe a contagem de valores não nulos por coluna.
  * `df.isna().sum()`: Retorna a quantidade absoluta de nulos em cada coluna.
  * `df.isna().mean()`: Calcula a proporção de valores ausentes por coluna.

* **Remoção de Dados Ausentes:**
  * `df.dropna()`: Remove linhas (ou colunas) que contenham qualquer valor ausente. É recomendada apenas quando a perda de dados for pequena e não introduzir viés de amostragem.

* **Imputação de Dados:**
  * **Imputação Simples (`fillna`):**
    * **Média:** `df['coluna'].fillna(df['coluna'].mean())` — adequada para variáveis numéricas simétricas e sem *outliers*.
    * **Mediana:** `df['coluna'].fillna(df['coluna'].median())` — resistente a *outliers*, ideal para distribuições assimétricas.
    * **Moda:** `df['coluna'].fillna(df['coluna'].mode())` — opção padrão para preencher variáveis categóricas.
  * **Interpolação (`interpolate`):** Estimativa baseada em pontos vizinhos, muito utilizada em séries temporais ordenadas. Pode-se utilizar `method="linear"` ou `method="time"` (quando o espaçamento temporal for irregular).
  * **Imputação Multivariada (`KNNImputer`):** Utiliza o algoritmo de \(k\)-vizinhos mais próximos do Scikit-learn para estimar o valor faltante com base na similaridade das demais variáveis.

---

### 3. Qual a diferença entre `loc` e `iloc`

A seleção e filtragem no pandas diferem na forma como referenciam linhas e colunas:

* **`loc` (Seleção baseada em Rótulos / Labels):**
  * Acessa linhas e colunas utilizando seus **rótulos nominais** (`df.loc[linhas, colunas]`).
  * **Inclusão do limite final:** No fatiamento (*slicing*), tanto o ponto inicial quanto o ponto final são **incluídos**. Por exemplo, `df.loc[0:8]` seleciona os rótulos de 0 até 8 (totalizando 9 linhas).
  * Aceita máscaras booleanas para filtragem (ex: `df.loc[df['popularidade'] >= 50]`).

* **`iloc` (Seleção baseada em Posição Numérica Inteira):**
  * Acessa elementos com base em suas **posições inteiras** baseadas em zero (`df.iloc[linhas, colunas]`).
  * **Exclusão do limite final:** Segue a convenção padrão do Python em que o limite final é **excluído**. Por exemplo, `df.iloc[0:8]` seleciona as posições de 0 a 7 (totalizando 8 linhas).

---

### 4. Como criar visualizações simples com Matplotlib e Seaborn

A visualização de dados revela tendências e distribuições que estatísticas resumo isoladas não capturam.

* **Matplotlib:** Biblioteca fundamental que oferece controle detalhado sobre eixos, rótulos e figuras.
  ```python
  import matplotlib.pyplot as plt

  plt.figure(figsize=(8, 4))
  plt.plot(df["mes"], df["vendas"], marker="o", label="Vendas")
  plt.title("Evolução Mensal de Vendas")
  plt.xlabel("Mês")
  plt.ylabel("Unidades Vendidas")
  plt.legend()
  plt.show()
