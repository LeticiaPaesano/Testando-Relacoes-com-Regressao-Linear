# $${\color{yellow}\text{🏠 Estabelecendo Preço de Venda de Imóveis Usando Regressão Linear}}$$

O objetivo deste projeto é identificar quais variáveis estruturais influenciam o preço de venda de imóveis residenciais, por meio da aplicação de regressão linear. A análise baseia-se em dados adaptados do Kaggle ([House Prices](https://www.kaggle.com/code/ahmedmahmoud16/house-prices-regression)).

## $${\color{yellow}\text{📁 Estrutura do Projeto}}$$

- `analise_correlação/`: Estudo exploratório das variáveis e relações com o preço
- `modelagem/`: Criação e avaliação de modelos de regressão
- `previsoes/`: Estimativas para novos imóveis
- `modelos_salvos/`: Modelo final serializado com `pickle`

---

## $${\color{yellow}\text{📘 Dicionário de Variáveis}}$$

| Variável                         | Descrição                                                                        |
| -------------------------------- | -------------------------------------------------------------------------------- |
| **`area_primeiro_andar`**            | Refere-se à área do primeiro andar da propriedade, medida em metros quadrados.   |
| **`existe_segundo_andar`**           | Variável binária: 1 = possui segundo andar; 0 = não possui.  |
| **`area_segundo_andar`**             | Área total do segundo andar, se existente, medida em metros quadrados.           |
| **`quantidade_banheiros`**           | Número total de banheiros na propriedade.                                        |
| **`capacidade_carros_garagem`**      | Capacidade máxima de veículos que podem ser estacionados na garagem.             |
| **`qualidade_da_cozinha_Excelente`** | Variável categórica: 1 se a cozinha é considerada "Excelente", 0 caso contrário. |
| **`preco_de_venda`**                 | Preço de venda da propriedade, em reais. Esta é a variável alvo do modelo.       |

---

## $${\color{yellow}\text{🔍 Análise Exploratória}}$$

- Correlações fortes com o preço:
  - `capacidade_carros_garagem`: **0.64**
  - `area_primeiro_andar`: **0.62**
  - `quantidade_banheiros`: **0.56**
- Gráficos utilizados:
 - Heatmap de correlação
    
<img width="1052" height="940" alt="heatmap" src="https://github.com/user-attachments/assets/db05cfbc-4a1c-4faa-8bf8-9763d6dddab0"/>

- Dispersões com linha de tendência
    
<img width="567" height="457" alt="Relação entre Preço e Área" src="https://github.com/user-attachments/assets/1db99864-d18e-4230-b6e2-3dd3d108618f" />
  
<img width="1299" height="497" alt="area_primeiro_andar por preço de venda" src="https://github.com/user-attachments/assets/7a6979a8-7c58-442c-9f0d-d5386526687e" />


- Histograma do preço de venda

  <img width="489" height="512" alt="Distribuição do preço de venda" src="https://github.com/user-attachments/assets/3fe9a30b-4896-4a3d-9dcd-ee02a1170ac0" />

---

## $${\color{yellow}\text{📈 Modelos Testados}}$$

| Modelo    | Características Excluídas                     | R² Treino | Multicolinearidade   |
|-----------|-----------------------------------------------|-----------|----------------------|
| Modelo 1  | Nenhuma                                       | Maior     | Alta ❌             |
| Modelo 2  | Sem `area_segundo_andar`                      | Alta      | Moderada ⚠️         |
| Modelo 3  | Sem garagem e segundo andar                   | **Boa**   | **Baixa ✅**        |

---

## $${\color{yellow}\text{📊 Comparativo Estatístico dos Modelos}}$$
Os modelos foram comparados com base em critérios estatísticos para avaliar desempenho, parcimônia e viabilidade de implementação:

|Modelo	R²|Treino	|R² Ajustado|AIC	    |BIC	    |F-Statistic |Multicolinearidade |
|---------|-------|-----------|---------|---------|------------|-------------------|
|Modelo 1 |0.54   |0.50	      | 3125.77 |	3145.23	|12.5     	 |Elevada ❌         |
|Modelo 2 |0.52	  |0.48	      | 3131.90 |3150.42	|11.8	       |Moderada ⚠️        |
|Modelo 3 |0.48	  |0.45       | 3142.30 |	3156.90	|10.2        |Baixa ✅           |

> **O Modelo 3**, embora com R² menor, foi o selecionado por ser mais parcimonioso, evitar multicolinearidade e manter interpretabilidade adequada.

## $${\color{yellow}\text{🧠 Modelo Final: Regressão Linear Múltipla}}$$

Modelo final: **Modelo 3**

Critérios de escolha:

- Desempenho preditivo aceitável (R² ~0.48)
- Baixa multicolinearidade (valores de VIF aceitáveis)
- Coeficientes estatisticamente significativos

### Coeficientes estimados:

| Variável                       | Impacto no Preço (R$)        |
|--------------------------------|------------------------------|
| `area_primeiro_andar`          | +6.119 por m²                |
| `quantidade_banheiros`         | +149.036                     |
| `existe_segundo_andar`         | +221.306                     |
| `qualidade_da_cozinha_Excelente`  | +444.391                     |

>Os coeficientes estimados representam o impacto isolado de cada variável no preço final do imóvel, mantendo as demais constantes.
---

## $${\color{yellow}\text{📉 Avaliação e Limitações}}$$

- O modelo apresentou **heteroscedasticidade** nos resíduos, o que indica possível violação dos pressupostos da regressão linear.
- R² final de aproximadamente **~0.48**, indicando capacidade preditiva moderada.

<img width="1293" height="469" alt="Previsão X Real" src="https://github.com/user-attachments/assets/e74cfefc-61e0-4cc0-bfd8-f5533ebb8bbf" />

<img width="1637" height="711" alt="Resíduos X Previsão" src="https://github.com/user-attachments/assets/4685aee5-8ff2-4655-afd6-b7fd629aef9f" />

---

## $${\color{yellow}\text{🏡 Previsão para Novos Imóveis}}$$

Exemplo de entrada:
```python
novo_imovel = pd.DataFrame({'const':[1],
                            'area_primeiro_andar': [120],
                            'existe_segundo_andar':[1],
                            'quantidade_banheiros':[2],
                            'qualidade_da_cozinha_Excelente': [0]
})
```
Para estimar o preço de um imóvel com 120 m² de área térrea, 2 banheiros, segundo andar e cozinha comum:

**Modelo 0** (só usa área térrea): prevê **R$ 968.146**

**Modelo 3** (modelo final, múltiplas variáveis): prevê R$ **1.123.758**

> A diferença ocorre porque o Modelo 3 leva em conta mais características estruturais, o que torna a estimativa mais precisa. O valor é obtido aplicando os coeficientes da regressão aos dados do imóvel.

## $${\color{yellow}\text{💾 Serialização do Modelo}}$$

O modelo final foi salvo utilizando `pickle`, garantindo reaproveitamento futuro sem necessidade de re-treinamento.
```python
import pickle

nome_arquivo = 'modelo_regressao_linear.pkl'

with open(nome_arquivo, 'wb') as arquivo:
    pickle.dump(modelo_3, arquivo)
```
---

✅ Conclusão 
O projeto desenvolveu um modelo preditivo para estimar preços de imóveis com base em características estruturais usando regressão linear. Após testar diferentes configurações, o modelo 3 foi o escolhido por seu bom desempenho e interpretabilidade.
> “Fatores exógenos, como localização, infraestrutura do entorno e sazonalidade do mercado, não foram contemplados no modelo e devem ser considerados em análises complementares para estimativas mais realistas.”

