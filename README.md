# ☕ Análise Sensorial do Café com Machine Learning
 
---
 
## 1. Introdução
 
O café é uma das bebidas mais consumidas no mundo, e sua qualidade é avaliada por especialistas a partir de uma série de atributos sensoriais como aroma, sabor, acidez, corpo e equilíbrio. Esses atributos, quando analisados em conjunto, permitem determinar a pontuação final de um café e classificá-lo dentro de padrões de excelência reconhecidos internacionalmente.
 
Este projeto utiliza a base de dados `df_arabica_clean.csv`, que reúne avaliações sensoriais de cafés arábica provenientes de diversas regiões do mundo. O conjunto de dados contém informações sobre a origem dos grãos, o método de processamento e as notas atribuídas por provadores especializados a cada característica sensorial.
 
O objetivo central é demonstrar como técnicas de Análise de Dados e Machine Learning podem ser aplicadas sobre esse tipo de base para responder a diferentes perguntas: *Quais atributos mais influenciam a qualidade final do café? É possível prever automaticamente se um café é Premium? Existem perfis sensoriais naturais entre os cafés avaliados?*
 
Para isso, o projeto é organizado em três etapas principais: análise exploratória dos dados, aplicação de modelos de machine learning e interpretação dos resultados.
 
---
 
## 2. Desenvolvimento
 
### 2.1 Base de Dados
 
A base de dados é composta pelas seguintes variáveis sensoriais:
 
| Variável           | Descrição                          |
|--------------------|------------------------------------|
| `Aroma`            | Nota de aroma                      |
| `Flavor`           | Nota de sabor                      |
| `Aftertaste`       | Nota de retrogosto                 |
| `Acidity`          | Nota de acidez                     |
| `Body`             | Nota de corpo                      |
| `Balance`          | Nota de equilíbrio                 |
| `Total Cup Points` | Pontuação final (variável alvo)    |
 
A maioria dos cafés apresenta notas sensoriais entre **7,5 e 8,5** e pontuações finais concentradas entre **82 e 86 pontos**.
 
---
 
### 2.2 Análise Exploratória
 
A análise exploratória foi conduzida com três visualizações principais:
 
- **Histograma** da variável `Total Cup Points`, que revelou uma distribuição concentrada entre 82 e 86 pontos, indicando que a maioria dos cafés avaliados possui qualidade elevada.
- **Gráfico de dispersão** entre `Flavor` e `Total Cup Points`, que evidenciou uma forte correlação linear positiva — quanto maior o sabor, maior a nota final.
- **Matriz de correlação**, que confirmou que atributos como `Flavor`, `Aroma` e `Aftertaste` possuem alta correlação com a pontuação total, sendo os principais candidatos como preditores nos modelos.
---
 
### 2.3 Modelos de Machine Learning
 
Foram aplicadas três abordagens diferentes de aprendizado de máquina sobre a mesma base de dados.
 
#### Modelo 1 — Clustering (K-Means)
 
O K-Means foi utilizado para identificar perfis sensoriais naturais entre os cafés, sem considerar a nota final como referência. Os dados foram padronizados antes do treinamento, e o algoritmo foi configurado para encontrar 3 grupos distintos.
 
| Cluster | Perfil        | Aroma | Flavor | Acidity |
|---------|---------------|-------|--------|---------|
| 0       | Inferior      | 7,38  | 7,37   | 7,35    |
| 2       | Intermediário | 7,66  | 7,70   | 7,66    |
| 1       | Superior      | 7,99  | 8,01   | 7,92    |
 
O **Silhouette Score** obtido foi de `0,3266`, indicando uma separação moderada entre os grupos — resultado esperado para dados sensoriais contínuos, nos quais diferentes perfis de café podem compartilhar características próximas.
 
#### Modelo 2 — Classificação (Regressão Logística)
 
A Regressão Logística foi aplicada para classificar os cafés como **Premium** (nota ≥ 83 pontos) ou **Padrão** (nota < 83 pontos), com base apenas nas suas características sensoriais.
 
| Métrica   | Classe 0 — Padrão | Classe 1 — Premium |
|-----------|-------------------|---------------------|
| Precisão  | 1,00              | 0,96                |
| Revocação | 0,94              | 1,00                |
| F1-score  | 0,97              | 0,98                |
 
O modelo alcançou **98% de acurácia**, classificando corretamente 41 dos 42 cafés presentes no conjunto de teste. Os resultados demonstram alta capacidade de distinção entre as duas categorias com baixíssimo índice de erros.
 
#### Modelo 3 — Regressão (Regressão Linear Múltipla)
 
A Regressão Linear Múltipla foi empregada para prever o valor numérico exato da pontuação final dos cafés. A forte correlação linear identificada na etapa exploratória justificou a escolha deste modelo.
 
| Métrica | Valor  | Interpretação                              |
|---------|--------|--------------------------------------------|
| MSE     | 0,0116 | Diferença quadrática média muito baixa     |
| MAE     | 0,0858 | Erro médio de apenas ~0,09 pontos          |
| R²      | 0,9954 | 99,54% da variação explicada pelo modelo   |
 
O modelo apresentou desempenho excelente, com previsões extremamente próximas dos valores reais. A análise dos coeficientes revelou que `Flavor` é o atributo com maior impacto positivo sobre a pontuação final.
 
---
 
## 3. Conclusão
 
Os resultados obtidos demonstram que as técnicas de Machine Learning aplicadas foram eficazes para analisar a qualidade dos cafés arábica a partir de suas características sensoriais.
 
O **K-Means** mostrou que, mesmo sem utilizar a nota final como referência, é possível segmentar os cafés em grupos com perfis sensoriais distintos — o que pode ser útil para categorização e direcionamento de produtos no mercado. A **Regressão Logística** provou ser uma ferramenta poderosa para classificação automática de cafés Premium, atingindo 98% de acurácia e demonstrando que as notas sensoriais são altamente discriminativas. Já a **Regressão Linear Múltipla** entregou previsões com erro médio inferior a 0,09 pontos e R² de 0,9954, confirmando que a pontuação final pode ser quase inteiramente explicada pelas variáveis sensoriais disponíveis.
 
De forma geral, o projeto evidencia que atributos como `Flavor`, `Aroma` e `Aftertaste` são os principais determinantes da qualidade do café. Esses insights têm aplicação direta para produtores, avaliadores e empresas do setor cafeeiro, que podem utilizar modelos preditivos como este para orientar decisões relacionadas à melhoria de qualidade, precificação e posicionamento de produtos.
 