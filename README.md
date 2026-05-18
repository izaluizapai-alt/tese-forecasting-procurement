# Aplicação de Modelos de Previsão para Apoiar Decisões de Procurement visando a Redução de Custos

**Dissertação de Mestrado em Logística**  
ISCAP — Instituto Superior de Contabilidade e Administração do Porto / APNOR  
**Autora:** Izabella Santos  
**Orientadora:** Prof.ª Doutora Ana Paula Lopes  
**Ano:** 2026

---

## Descrição

Este repositório contém o código Python desenvolvido no âmbito da dissertação de mestrado, que testa empiricamente a hipótese de que a aplicação de algoritmos de Machine Learning ao *forecasting* da procura produz ganhos de precisão mensuráveis face aos métodos estatísticos clássicos, com impacto direto na eficiência do aprovisionamento estratégico.

O modelo é aplicado ao dataset **DataCo Smart Supply Chain** (180.519 registos, 52 variáveis, Jan 2015 – Set 2017) e compara cinco modelos de previsão numa série temporal diária de 1.003 observações.

O impacto financeiro dos resultados, stock de segurança em USD por modelo e por categoria, é calculado e visualizado no dashboard Power BI disponível no Anexo III da dissertação.

---

## Dataset

| Atributo | Valor |
|---|---|
| Nome | DataCo Smart Supply Chain Dataset |
| Registos | 180.519 |
| Variáveis | 52 |
| Período | Janeiro 2015 – Setembro 2017 |
| Observações diárias | 1.003 dias |
| Divisão treino/teste | 80% / 20% (802 / 201 dias) |
| Fonte | Kaggle — DataCo Global |

> **Nota:** Os dados de outubro a dezembro de 2017 foram excluídos por apresentarem uma quebra abrupta de ~80% no volume — indicativa de registos incompletos e não de uma alteração real da procura.

---

## Modelos Implementados

| Modelo | Tipo | Hiperparâmetros |
|---|---|---|
| Naïve | Benchmark | — |
| SARIMA | Estatístico | (1,0,1)(1,0,1)[7] |
| Holt-Winters | Estatístico | trend='add', seasonal='add', s=7 |
| Random Forest | Machine Learning | n_estimators=200, random_state=42 |
| XGBoost | Machine Learning | n_estimators=200, lr=0.05, max_depth=4, subsample=0.8 |

---

## Resultados — Série Total

| Rank | Modelo | RMSE | MAPE | Δ MAPE vs. Naïve |
|---|---|---|---|---|
| 🥇 **1.º** | **Random Forest** | **35,87** | **8,24%** | **−3,56 pp** |
| 2.º | XGBoost | 38,35 | 8,84% | −2,96 pp |
| 3.º | Holt-Winters | 43,63 | 10,64% | −1,16 pp |
| 4.º | SARIMA | 43,73 | 10,69% | −1,11 pp |
| 5.º (ref.) | Naïve | 47,50 | 11,80% | — |

**Modelo vencedor: Random Forest**

A redução do RMSE de 47,50 (Naïve) para 35,87 (Random Forest) representa uma diminuição de **24,5%** que, pela fórmula SS = z × σ × √L (Silver et al., 1998) com z=1,65 e L=7 dias, se propaga diretamente ao stock de segurança necessário para manter o nível de serviço de 95% — equivalendo a **6.779 USD de capital imobilizado libertado** por período de análise.

---

## Resultados — 7 Categorias Classe A (81% do volume total)

| Categoria | MAPE Naïve | MAPE RF | Redução | Poupança SS (USD) |
|---|---|---|---|---|
| Indoor/Outdoor Games | 45,04% | 23,71% | −47% | 1.804 USD |
| Cardio Equipment | 47,61% | 30,84% | −35% | 1.355 USD |
| Fishing | 24,50% | 20,25% | −17% | 856 USD |
| Cleats | 20,04% | 19,29% | −4% | 711 USD |
| Women's Apparel | 31,07% | 26,01% | −16% | 454 USD |
| Shop By Sport | 30,63% | 31,34% | +0,7% | 225 USD |
| Men's Footwear | 18,18% | 18,20% | ≈0 | 176 USD |

> A poupança em SS (USD) foi calculada com base nos preços reais do dataset (variável *Product Price*) e é visualizada dinamicamente no dashboard Power BI.

---

## Diagnóstico de Estacionaridade

| Teste | Estatística | p-value | Conclusão |
|---|---|---|---|
| ADF (H₀: não estacionária) | −28,53 | 0,000 | Rejeita H₀ → série estacionária |
| KPSS (H₀: estacionária) | 0,18 | 0,100 | Não rejeita H₀ → confirma |

**Conclusão:** d = 0 — série estacionária em nível, sem necessidade de diferenciação.

---

## Engenharia de Variáveis (Feature Engineering)

| Variável | Descrição |
|---|---|
| day_of_week | Dia da semana (0=segunda … 6=domingo) |
| day_of_month | Dia do mês (1–31) |
| month_of_year | Mês do ano (1–12) |
| day_num | Índice sequencial do dia |
| lag_1 | Procura do dia anterior |
| lag_7 | Procura do mesmo dia na semana anterior |
| lag_14 | Procura do mesmo dia há duas semanas |
| rolling_mean_7 | Média móvel dos últimos 7 dias |
| rolling_mean_14 | Média móvel dos últimos 14 dias |

A variável mais importante: **rolling_mean_7** (~26% no Random Forest, ~21% no XGBoost).

---

## Estrutura do Repositório

```
tese-forecasting-procurement/
│
├── modelo_forecasting_procurement.ipynb   # Notebook principal com todos os modelos
├── README.md                              # Este ficheiro
└── DataBase/
    └── DataCoSupplyChainDataset.csv       # Dataset (não incluído — ver fonte)
```

---

## Como Executar

```bash
# 1. Instalar dependências
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn xgboost

# 2. Colocar o dataset na pasta DataBase/
# Fonte: https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis

# 3. Executar o notebook
jupyter notebook modelo_forecasting_procurement.ipynb
```

---

## Análise de Impacto Financeiro

O cálculo do stock de segurança em USD — por modelo e por categoria — é realizado no **dashboard Power BI** através de medidas DAX dinâmicas que integram os valores de RMSE obtidos pelo modelo Python com os preços reais do dataset (variável *Product Price*).

**Fórmula aplicada:** SS = z × RMSE × √L  
com z = 1,65 (nível de serviço 95%) e L = 7 dias (lead time médio do fornecedor).

Dashboard disponível em:  
🔗 https://app.powerbi.com/view?r=eyJrIjoiODcwYzgzOTUtYThlNi00MDc2LWI2YmItM2UxZTFhZjZjNGFmIiwidCI6IjVlOWUzODBkLTQ3ZjAtNGE5NC04N2JkLWJmM2U2NDgzZWEyZSJ9

---

## Referências

- Silver, E. A., Pyke, D. F., & Peterson, R. (1998). *Inventory Management and Production Planning and Scheduling*. John Wiley & Sons.
- Makridakis, S., Spiliotis, E., & Assimakopoulos, V. (2018). Statistical and Machine Learning Forecasting Methods: Concerns and Ways Forward. *PLoS ONE*, 13(3), e0194889.
- Hyndman, R. J., & Athanasopoulos, G. (2021). *Forecasting: Principles and Practice* (3.ª ed.). OTexts. https://otexts.com/fpp3/
- Chen, T., & Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *KDD 2016*, 785–794.
- Christopher, M. (2016). *Logistics & Supply Chain Management* (5.ª ed.). Pearson Education.

---

## Licença

Este código é disponibilizado para fins académicos no âmbito da dissertação de mestrado.  
© 2026 Izabella Santos — ISCAP / APNOR
