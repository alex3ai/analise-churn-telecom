# Análise Preditiva de Churn - Clientes de Telecom (Telco)

## 🎯 Visão Geral do Projeto

Este é um projeto de portfólio de Data Science focado em **classificação** para prever o *churn* (cancelamento) de clientes em uma empresa de telecomunicações.

O objetivo principal é construir um modelo de machine learning robusto, desde a limpeza e análise exploratória dos dados até a modelagem e interpretação dos resultados. O projeto demonstra habilidades em:

  * **Análise Exploratória de Dados (EDA)** para identificar os principais perfis de risco.
  * **Pré-processamento robusto** com `Scikit-learn Pipelines` para evitar vazamento de dados (*data leakage*) e garantir um código pronto para produção.
  * **Modelagem Comparativa** (Regressão Logística, Random Forest, LightGBM).
  * **Avaliação de Métricas** focada em `ROC AUC`, ideal para datasets desbalanceados.
  * **Interpretação de Modelos** (Feature Importance) para extrair insights de negócio.

-----

## 1\. 📊 O Desafio: Entendendo o Churn

O primeiro passo foi entender o problema de negócio. A análise da variável alvo mostrou que **26.54% dos clientes** na base de dados cancelaram seus serviços. Essa é uma taxa de churn significativa, justificando a criação de um modelo preditivo para identificar clientes em risco e permitir ações de retenção.

-----

## 2\. 🔍 Análise Exploratória (EDA): O Perfil do Risco

A análise visual dos dados revelou que o perfil de churn é muito mais **comportamental** do que demográfico.

  * **Contrato (Alto Risco):** Clientes com contratos **"Mês a Mês"** (Month-to-month) têm uma taxa de churn **4x maior** que clientes com contratos de 1 ou 2 anos.
  * **Método de Pagamento (Alto Risco):** Clientes que utilizam **"Cheque Eletrônico"** (Electronic check) como forma de pagamento têm o **dobro** de probabilidade de cancelar.
  * **Demografia (Baixo Risco):** Clientes com **parceiros** (Partner) e/ou **dependentes** (Dependents) são significativamente mais leais e menos propensos ao churn.
  * **Neutro:** O gênero do cliente não demonstrou ter impacto relevante na decisão de cancelamento.

-----

## 3\. ⚙️ Metodologia e Modelagem com Pipelines

Para garantir que o modelo seja robusto, escalável e livre de vazamento de dados (*data leakage*), todo o processo de pré-processamento e modelagem foi encapsulado em **Pipelines** do Scikit-learn.

### Pré-processamento

Foi utilizado um `ColumnTransformer` para aplicar transformações específicas a cada tipo de dado:

  * **Colunas Numéricas (`CLTV`, `MonthlyCharges`, etc.):** Padronização com `StandardScaler`.
  * **Colunas Categóricas (`Contract`, `PaymentMethod`, etc.):** Codificação com `OneHotEncoder` (configurado com `handle_unknown='ignore'` para resiliência a novas categorias).

### Treinamento e Avaliação

1.  Os dados foram divididos em conjuntos de treino (80%) e teste (20%) utilizando **estratificação (`stratify=y`)**, o que é crucial para garantir que a proporção de churn (26.5%) seja mantida em ambas as amostras.
2.  Três modelos de classificação foram treinados e comparados dentro do Pipeline:
      * **Regressão Logística** (Baseline)
      * **Random Forest**
      * **LightGBM (LGBMClassifier)**

-----

## 4\. 📈 Resultados e Interpretação

### Performance dos Modelos

Todos os modelos apresentaram excelente performance, com scores de **ROC AUC em torno de 0.84**, muito superiores ao baseline aleatório (0.50). O LightGBM e o Random Forest tiveram um leve destaque, como visto na Curva ROC (imagem no topo deste README).

  * **LightGBM: AUC = 0.843**
  * **Random Forest: AUC = 0.842**
  * **Regressão Logística: AUC = 0.836**

### Fatores Mais Importantes (Feature Importance)

Para entender *o que* o modelo (LightGBM) aprendeu, foi extraída a importância das features. As variáveis financeiras e contratuais são as mais decisivas:

1.  **num\_\_CLTV** (Valor do Tempo de Vida do Cliente)
2.  **num\_\_MonthlyCharges** (Cobranças Mensais)
3.  **num\_\_TotalCharges** (Cobranças Totais)
4.  **num\_\_TenureMonths** (Tempo de Contrato em Meses)
5.  **cat\_\_Contract\_Month-to-month** (Contrato Mês a Mês)

-----

## 5\. 💡 Conclusão

O projeto entregou um modelo de classificação robusto (AUC \~0.84) capaz de prever o churn de clientes.

A análise exploratória (EDA) e a interpretação do modelo (Feature Importance) são consistentes e apontam para a mesma conclusão: o risco de churn está concentrado em clientes com **contratos mensais** e que pagam com **cheque eletrônico**, sendo as **variáveis financeiras** (CLTV, cobranças) os preditores mais fortes.

-----

## 🛠️ Tecnologias Utilizadas

  * Python
  * Pandas (para manipulação de dados)
  * Scikit-learn (para Pipelines, ColumnTransformer e Modelagem)
  * LightGBM (para o modelo de boosting)
  * Matplotlib & Seaborn (para visualização)
  * Jupyter Notebook

-----

## 🚀 Como Executar o Projeto

1.  Clone o repositório:
    ```bash
    git clone [URL-DO-SEU-REPOSITORIO]
    ```
2.  Crie um ambiente virtual e instale as dependências:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn lightgbm openpyxl
    ```
3.  Coloque o arquivo `Telco_customer_churn.xlsx` na mesma pasta do notebook.
4.  Execute o Jupyter Notebook: `Análise_churn_telco_customer.ipynb`.