---

## Como Executar
```bash
# 1. Instalar dependências
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn xgboost

# 2. Colocar o dataset na pasta DataBase/
# Fonte: Constante, F. J. N., Silva, F., & Pereira, A. C. (2019).
# DataCo Smart Supply Chain for Big Data Analysis. Mendeley Data.
# https://doi.org/10.17632/8gx2fvg2k6.5
# (disponível também em: https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)

# 3. Executar o notebook
jupyter notebook modelo_forecasting_procurement.ipynb
```

---

## Análise de Impacto Financeiro
O cálculo do stock de segurança em USD — por modelo e por categoria — é realizado no **dashboard Power BI** através de medidas DAX dinâmicas que integram os valores de RMSE obtidos pelo modelo Python com os preços reais do dataset (variável *Product Price*).

**Fórmula aplicada:** SS = z × σ × √L (Silver et al., 1998), onde σ é empiricamente aproximado pelo RMSE quando os erros são centrados em zero — aproximação metodologicamente aceite nesta condição, verificada nesta investigação.  
Com z = 1,65 (nível de serviço 95%) e L = 7 dias (prazo de aprovisionamento fixo — pressuposto metodológico).

Dashboard disponível em:  
🔗 https://app.powerbi.com/view?r=eyJrIjoiODcwYzgzOTUtYThlNi00MDc2LWI2YmItM2UxZTFhZjZjNGFmIiwidCI6IjVlOWUzODBkLTQ3ZjAtNGE5NC04N2JkLWJmM2U2NDgzZWEyZSJ9

---

## Referências
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5-32.
- Chen, T., & Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 785-794.
- Chopra, S., & Meindl, P. (2016). *Supply Chain Management: Strategy, Planning, and Operation* (6.a ed.). Pearson.
- Christopher, M. (2016). *Logistics & Supply Chain Management* (5.a ed.). Pearson Education.
- Constante, F. J. N., Silva, F., & Pereira, A. C. (2019). *DataCo Smart Supply Chain for Big Data Analysis* [Dataset]. Mendeley Data. https://doi.org/10.17632/8gx2fvg2k6.5
- Glas, A. H., & Kleemann, F. C. (2016). The impact of Industry 4.0 on procurement and supply management. *Journal of Procurement & Supply Management*, 22(4), 347-363.
- Hohenstein, N. O., Feisel, E., Hartmann, E., & Giunipero, L. (2015). Research on the phenomenon of supply chain resilience: A systematic review and paths for further investigation. *International Journal of Physical Distribution & Logistics Management*, 45(1/2), 90-117.
- Hyndman, R. J., & Athanasopoulos, G. (2021). *Forecasting: Principles and Practice* (3.a ed.). OTexts. https://otexts.com/fpp3/
- Januschowski, T., Gasthaus, J., Park, Y., Salinas, D., Flunkert, V., Seeger, M., & Smola, A. J. (2020). Criteria for classifying forecasting methods. *International Journal of Forecasting*, 36(1), 167-177.
- Lim, B., Arik, S. O., Loeff, N., & Pfister, T. (2021). Temporal Fusion Transformers for interpretable multi-horizon time series forecasting. *International Journal of Forecasting*, 37(4), 1748-1764.
- Makridakis, S., Spiliotis, E., & Assimakopoulos, V. (2018). Statistical and Machine Learning Forecasting Methods: Concerns and Ways Forward. *PLoS ONE*, 13(3), e0194889.
- Makridakis, S., Spiliotis, E., & Assimakopoulos, V. (2022). M5 accuracy competition: Results, findings, and conclusions. *International Journal of Forecasting*, 38(4), 1346-1364.
- Silver, E. A., Pyke, D. F., & Peterson, R. (1998). *Inventory Management and Production Planning and Scheduling* (3.a ed.). John Wiley & Sons.
- Waller, M. A., & Fawcett, S. E. (2013). Data Science, Predictive Analytics, and Big Data: A Revolution That Will Transform Supply Chain Design and Management. *Journal of Business Logistics*, 34(2), 77-84.

---

## Licença
Este código é disponibilizado para fins académicos no âmbito da dissertação de mestrado.  
© 2026 Izabella Santos — ISCAP / APNOR
