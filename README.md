# House Rocket Project

![shutterstock_50097079-2ec9cc](https://user-images.githubusercontent.com/86979717/211115641-4c509c9e-84c5-4675-bb7c-2e30baeb4152.jpg)

Esse projeto é fictício, e busca retirar insights e propôr soluções de negócios para uma empresa de venda de imóveis, também fictícia, chamada House Rocket.

Portfólio criado a partir da proposta do curso "Python do ZERO ao DS", da comunidade DS. Os dados podem ser encontrados no <a href="https://www.kaggle.com/datasets/harlfoxem/housesalesprediction">Kaggle</a>.

O dashboard desenvolvido para visualização dos dados foi colocado em produção no Streamlit:

<a href="https://joaorange-streamlit-dashboard-dashboard-wf8rh4.streamlit.app/">![](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)</a>.

[![forthebadge made-with-python](http://ForTheBadge.com/images/badges/made-with-python.svg)](https://www.python.org/)

# 1. Descrição

A empresa House Rocket é uma firma do ramo imobiliário que tem como natureza a compra de imóveis estratégicos em tempos de baixa no mercado e a
revenda em períodos e por preços mais adequados. Eles decidiram contratar um cientista de dados para,
por meio de buscas nos padrões nos dados, indicar à empresa quais imóveis comprar e por qual período e preço devem ser revendidos, além de
tentar retirar insights relevantes para os times de negócio.

# 2. Resultados da Estratégia de Negócios

Dos 21435 imóveis disponíveis, assumindo as premissas de negócio, 10505 poderiam ser adquiridos pela House Rocket, que, caso revendidos, poderiam gerar um lucro de US$481 milhões.

Valor Máximo Investido: US$ 4.079.586.744,00

Valor máximo Retornado: US$ 4.560.966.465,00

Lucro Máximo Esperado: US$ 481.379.721,00

# 3. Premissas de negócio e Atributos dos dados

Foram consideradas as seguintes premissas para tratamento dos dados:

* Os valores de "bathrooms" e "bedrooms" foram arredondados;
* As linhas em que a coluna "id" está duplicada foram removidas, deixando as linhas com datas das últimas compras, visto que é o que interessa à House Rocket;
* Foi retirada uma linha com valor muito alto na quantidade de quartos, de 33, que pode ter sido erro de preenchimento. Abaixo esse outlier sendo mostrado em um boxplot:

 ![image](https://user-images.githubusercontent.com/86979717/212213579-6a2da23f-5ea2-4a9b-bc61-f1d616130cc5.png)
 


Para responder as perguntas de negócio, sobre que imóvel comprar e por que preço, foram adotadas as seguintes estratégias:

* Foi calculada a mediana de preço dos imóveis agrupados por zipcode. Se o imóvel tem preço abaixo da mediana de seu zipcode, e possui "condition", ou condição, maior ou igual a 3, ele poderia ser comprado pela House Rocket;
* Para definir o preço de revenda, primeiro, foi definida uma estação do ano para os dias em que os imóveis foram vendidos, seguindo os seguintes critérios: 
  * Spring (primavera) - Março a maio.
  * Summer (verão) - Junho a agosto.
  * Autumn (outuno)- Setembro a novembro.
  * Winter (inverno) - Dezembro a favereiro.

* Após, os dados foram agrupados por mediana tanto por zipcode quanto por estação, isto é, cada zipcode teria 4 medianas de preços, uma para cada estação. Com base nisso, foi definido que o preço de revenda seria o preço em que os imóveis foram comprados multiplicado por 1.3 caso esse preço fosse menor que a mediana de preço por zipcode e estação do ano, e 1.1 se esse preço fosse maior.


As variáveis do conjunto de dados, e suas respectivas descrições, são as seguintes:

| Variável | Descrição |
| ------------- | :-------------: |
| id  | Identificação única de cada imóvel  |
| date | Data que o imóvel foi vendido |
| price  | Preço de venda de cada imóvel |
| bedrooms  | Número de quartos |
| bathrooms  | Número de banheiros  |
| sqft_living  | Medida, em pés quadrados, do espaço interior dos apartamentos |
| sqft_lot  | Medida, em pés quadrados, de todo o espaço terrestre do imóvel |
| floors  | Número de andares  |
| waterfront  | "1" se possui vista para a água e "0" se não  |
| view  | Índice que quantifica, de 0 a 4, o quão boa é a vista da propriedade |
| condition  | Índice que quantifica, de 1 a 5, a condição do apartamento  |
| grade  | Um índice que quantifica, de 1 a 13, o nivel da construção e do design do edifício. 1-3 = baixo, 7 = médio e 11-13 = alto
| sqft_above  | Medida, em pés quadrados, do espaço interior acima do solo  |
| sqft_basement  | Medida, em pés quadrados, do espaço interior abaixo do solo |
| yr_built  | Ano que o imóvel foi construído  |
| yr_renovated  | Ano em que a casa foi renovada pela última vez  |
| zipcode  | Código ZIP, ou CEP, do imóvel  |
| lat  | Latitude |
| long  | Longitude  |
| sqft_living15  | Medida, em pés quadrado, do espaço interno de habitação dos 15 imóveis vizinhos mais próximos |
| sqft_lot15 | Medida, em pés quadrado, do espaço terrestre dos 15 imóveis vizinhos mais próximos |

# 4. Hipóteses de Negócio

Foram desenvolvidas 5 hipóteses de negócio para serem validadas ou não pela análise dos dados. 

* Hipótese 1: Imóveis que possuem vista para água, são 30% mais caros, na média.

![image](https://user-images.githubusercontent.com/86979717/212220553-82bec9d2-fd67-41dd-86e6-4f2b557dd42b.png)

Hipótese verdadeira, visto que o preço médio do pé quadrado, ou square foot, do lote é 35% maior nos imóveis com vista para a água. Seria interessante, então, priorizar imóveis com vista para a água.


* Hipótese 2: Imóveis com data de construção menor que 1955, são 50% mais baratos, na média.

![image](https://user-images.githubusercontent.com/86979717/212220863-25e6ea9b-47e1-4595-a27f-c72b701bb0a8.png)

Hipótese falsa. Imóveis construídos após 1955 tem, em média, preço por pé quadrado apenas 9% maior; ou seja, não há necessidade de descartar ou diminuir o potencial de ganho com imóveis muito antigos.


* Hipótese 3: Imóveis sem porão possuem sqft_lot 50% maiores do que com porão.

![image](https://user-images.githubusercontent.com/86979717/212221211-cdf6802b-4932-421a-a5aa-585f9a57fab3.png)

Hipótese falsa. Imóveis sem porão tem, em média, apenas 22% a mais de pés quadrados em seus lotes.


* Hipótese 4: A quantidade de quartos tem maior correlação com o preço do que a quantidade de banheiros.

![image](https://user-images.githubusercontent.com/86979717/212221500-48aaad8a-ad18-42a3-8f23-bfed74dc4742.png)

Hipótese falsa. A correlação do preço por pé quadrado com quantidade de quartos é quase 0. A com quantidade de banheiros é maior; o que traduz para um interesse comercial maior por imóveis com maior quantidade de banheiros.


* Hipótese 5: H5: O preço dos imóveis aumenta ao menos 5% ano a ano (YoY) após a construção.


![image](https://user-images.githubusercontent.com/86979717/212221866-9245dbd9-0343-408c-906e-1a25ce6cc09d.png)

Hipótese falsa: Como pode ser visto no gráfico da esquerda, o preço médio por pé quadrado abaixou de imóveis construídos no início do século XX para os construídos nos anos 40 a 90, tendo novamente uma trajetória de crescimento para os construídos nos anos 2000.

