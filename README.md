# Análise Exploratória de Dados de Churn de Clientes

Este notebook realiza uma Análise Exploratória de Dados (AED) em um conjunto de dados de clientes de uma empresa de telecomunicações. O objetivo é identificar padrões, tendências e fatores que influenciam o *churn* (cancelamento de serviço) dos clientes, fornecendo insights para estratégias de retenção.

## 📌 Extração

Os dados foram extraídos de um arquivo JSON (`TelecomX_Data.json`). O conjunto de dados original continha informações de clientes, mas com alguns campos aninhados, como detalhes do cliente, telefone, internet e conta.

## 🔧 Transformação

Nesta etapa, os dados foram transformados para facilitar a análise:

1.  **Normalização de Dados Aninhados:** Os campos aninhados (`customer`, `phone`, `internet`, `account`) foram normalizados usando `pd.json_normalize()` para extrair as informações e criar novas colunas no DataFrame principal.
2.  **Concatenação de DataFrames:** Os DataFrames normalizados foram concatenados com o DataFrame original, removendo as colunas aninhadas originais.
3.  **Conversão de Tipo de Dados:** A coluna `Charges.Total` foi convertida para o tipo numérico (`float64`) usando `pd.to_numeric`. O parâmetro `errors='coerce'` foi utilizado para tratar quaisquer valores que não pudessem ser convertidos, substituindo-os por `NaN`.

## 📊 Carga e Análise

Os dados transformados foram carregados em um DataFrame pandas para análise.

### Análise Descritiva das Variáveis Numéricas

Foram calculadas estatísticas descritivas (média, mediana, desvio padrão, mínimo, máximo, quartis) para as colunas numéricas relevantes: `tenure`, `Charges.Monthly` e `Charges.Total`. Isso fornece um resumo da distribuição central, dispersão e alcance dessas variáveis.

### Visualização das Variáveis Numéricas

Para visualizar a distribuição das variáveis numéricas e entender sua relação com o churn, foram gerados:

*   **Histogramas:** Plotados para `tenure`, `Charges.Monthly` e `Charges.Total` para mostrar a frequência de valores em diferentes intervalos. A linha KDE (Kernel Density Estimate) foi adicionada para visualizar a distribuição de probabilidade.
*   **Box Plots:** Gerados para `tenure`, `Charges.Monthly` e `Charges.Total` em relação à variável `Churn`. Isso permite comparar a distribuição dessas variáveis entre clientes que churnaram (Yes) e aqueles que não churnaram (No), identificando possíveis diferenças significativas.

### Visualização das Variáveis Categóricas

Para analisar a relação entre as variáveis categóricas e o churn, foram criados gráficos de contagem (`countplot`) para cada coluna categórica (excluindo `customerID` e `Churn`), com a variável `Churn` como `hue`. Isso mostra a contagem de clientes em cada categoria, dividida por status de churn. As colunas categóricas analisadas incluem: `gender`, `SeniorCitizen`, `Partner`, `Dependents`, `PhoneService`, `MultipleLines`, `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`, `Contract`, `PaperlessBilling`, e `PaymentMethod`.

## 📄 Relatório Final

Com base na análise exploratória, foram identificados os seguintes insights principais relacionados ao churn:

*   Clientes que churnaram tendem a ter um tempo de serviço (`tenure`) significativamente menor do que aqueles que não churnaram.
*   Clientes que churnaram apresentam cobranças mensais (`Charges.Monthly`) geralmente mais altas em comparação com os que não churnaram.
*   Clientes idosos (`SeniorCitizen` = 1) parecem ter uma taxa de churn maior do que clientes não idosos.
*   Clientes sem parceiro e sem dependentes parecem ter uma maior probabilidade de churnar.
*   Clientes com serviço de internet Fibra Óptica (`InternetService` = Fiber optic) apresentam uma taxa de churn consideravelmente maior.
*   Clientes que não assinam serviços adicionais de internet (Segurança Online, Backup Online, Proteção de Dispositivo, Suporte Técnico) têm uma taxa de churn significativamente mais alta.
*   Clientes com contrato Mês a Mês (`Contract` = Month-to-month) têm uma taxa de churn muito mais alta do que aqueles com contratos de Um Ano ou Dois Anos.
*   Clientes que utilizam Cheque Eletrônico (`PaymentMethod` = Electronic check) como método de pagamento apresentam a maior taxa de churn.

### Insights e Próximos Passos

A análise sugere que fatores como tempo de serviço, tipo de contrato, tipo de serviço de internet, serviços adicionais e método de pagamento estão fortemente associados ao churn. Os próximos passos podem incluir:

*   Investigar as causas subjacentes da alta rotatividade entre clientes com serviço de internet Fibra Óptica e aqueles que utilizam Cheque Eletrônico, possivelmente através de pesquisas de satisfação ou análise de dados de suporte.
*   Desenvolver e implementar estratégias de retenção direcionadas para segmentos de clientes de alto risco, como aqueles com baixo tempo de serviço, contrato Mês a Mês e que não utilizam serviços adicionais de internet. Isso pode incluir ofertas de incentivos para migração para contratos de longo prazo ou a adição de serviços, além de melhorias na qualidade do serviço de Fibra Óptica e na experiência de pagamento com Cheque Eletrônico.
*   Construir um modelo preditivo de churn utilizando as variáveis identificadas nesta AED para prever quais clientes têm maior probabilidade de churnar, permitindo ações proativas de retenção.
