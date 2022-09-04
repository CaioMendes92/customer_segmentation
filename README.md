# Insiders Clustering

**Disclaimer**: Todos o contexto deste projeto é completamente fictício.

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/programa-de-fidelidade-capa.jpg)

# 1. Contexto do negócio
A empresa All in One Place é uma empresa Outlet Multimarcas, ou seja, ela comercializa produtos de segunda linha de várias marcas a um preço menor, através de um e-commerce.

Em pouco mais de 1 anos de operação, o time de marketing percebeu que alguns clientes da sua base, compram produtos mais caros, com alta frequência e acabam contribuindo com uma parcela significativa do faturamento da empresa.

Baseado nessa percepção, o time de marketing vai lançar um programa de fidelidade para os melhores clientes da base, chamado Insiders. Mas o time não tem um conhecimento avançado em análise de dados para eleger os participantes do programa.

Por esse motivo, o time de marketing requisitou ao time de dados uma seleção de clientes elegíveis ao programa, usando técnicas avançadas de manipulação de dados.

# 2. Problema de Negócio
Encontrar os clientes mais elegíveis para o programa de fidelidade Insiders. Além disso, algumas perguntas precisarão ser respondidas ao longo do projeto:
1. Quem são as pessoas elegíveis para participar do programa de Insiders ?
2. Quantos clientes farão parte do grupo?
3. Quais as principais características desses clientes ?
4. Qual a porcentagem de contribuição do faturamento, vinda do Insiders ?
5. Qual a garantia que o programa Insiders é melhor que o restante da base ?
6. Quais ações o time de marketing pode realizar para aumentar o faturamento?

Para responder estas perguntas, utiliza-se a base de dados vindas do Kaggle: https://www.kaggle.com/vik2012kvs/high-value-customers-identification

## 2.1 Descrição dos atributos
* **InvoiceNo** - Indicador único de cada transação;
* **Stock Code Product** - Código do produto.
* **Description Product**: Nome do produto.
* **Quantity**: Quantidade de cada produto comprado por transação.
* **Invoice Date**: Dia em que a compra ocorreu.
* **Unit Price**: Preço do produto por unidade.
* **Customer ID**: Identificador único do cliente.
* **Country**: Nome do país que o cliente reside.

# 3. Suposições do Negócio

* Compras com quantity negativo serão tratadas separadamente como produtos retornados;
* Compras com unit price zero serão removidas;
* O atributo quantity negativo será considerado como devolução do produto e, consequentemente, a empresa precisará reembolsar o cliente;
* Itens cujo stock code possuem apenas caracteres alfabéticos (por exemplo: 'POST', 'D', 'DOT', 'M', 'S', 'AMAZONFEE', 'm', 'DCGSSBOY', entre outros) parece não possuir um produto relacionado, sendo assim, serão desconsiderados;
* A unidade monetária utilizada foi o dólar americano.

# 4. Estratégia para desenvolver a solução
A partir do pedido da equipe de marketing e da análise dos dados, é possível observar que esse é um claro problema de clusterização. Para solucioná-lo, será usado um modelo de Machine Learning. Depois dos clusters formados, será respondido as questões levantadas pela equipe de marketing e montado um total de 8 clusters separando os clientes.

Será utilizado o método cíclico CRISP-DM (Cross-Industry Process), que é um metodo de gerenciamento de projetos para ciência de dados. A vantagem desse método é que se entrega o valor de uma forma mais rápida. Foi realizado dez ciclos do CRISP para chegar na solução final do projeto, passando de uma solução de ponta-a-ponta, sem tratamento nas variáveis até a solução final, com 8 clusters segmentando os clientes.

Para a separação dos clientes, usou-se um espaço de embedding, sendo construído por três formas diferentes: Utilizando UMAP, t-SNE e uma Randon Forest. A melhor solução encontrada foi utilizando a RF.

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/random_forest_clustering.png)

É possível ver uma clara separação dos clientes a partir do espaço de embedding. Vale salientar que a utilização deste espaço pode trazer um melhor resultado final, mas, em contrapartida, perde-se a explicabilidade do modelo.

# 5. Modelos de Machine Learning Aplicados

Em cima do espaço de embedding, foi aplicado os seguintes modelos: KMeans, Hierarchical Cluster e Gaussian Mixture Model para vários valores de K e calculado a Silhouette Score como métrica de comparação.

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/modelos.png)

A cor verde indica qual o valor de K retornou o melhor valor da Silhouette Score. Em uma rápida análise, observa-se que todos os modelos o maior valor da SS para valor de K > 13, o que dá a impressão que seria possível crescer ainda mais o valor de K e obter melhores resultados. Entretanto, pensando do ponto de vista do negócio, a equipe de marketing teria que acompanhar mais de 15 grupos diferentes, o que precisaria ser muito bem alinhado com eles para que o projeto não seja inviabilizado. Além disso, vale salientar que o KMeans é um modelo muito mais simples de implementar que qualquer um dos outros. Por estes pontos, foi escolhido o KMeans com K = 9, sendo assim, mais rápido de realizar o projeto e mais viável a utilização pela equipe de Marketing.

# 6. Performance dos Modelos de Machine Learning
Como dito anteriormente, a análise foi feita utilizando como métrica o Silhouette Score. A performance para o KMeans com K = 9 foi de 0.656453.

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/silhouette_score.png)

A visualização da silhoueta por cluster também foi realizada

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/silhouette_score_image.png)

É possível observar que o tamanho da silhueta é semelhante, não tem nenhuma muito maior que as outras, o que é um indício que a quantidade de pontos nos clusters são parecidas. Porém, alguns clusters são menores, o que é um indício que seria ideal diminuir o tamanho dos outros clusters, aumentando o K, de forma que os tamanhos sejam semelhantes. Além disso, observa-se que a queda da "ponta" das silhuetas é brusca, de forma que seja um indício de que os clusters não sejam tão compactos. Estes pontos já podia ser visto a partir da análise dos valores de K maiores feitos na seção anterior. Aumentando o valor de K seria possível obter resultados mais eficazes, entretanto, para a viabilidade do projeto, é melhor perder um pouco da métrica e tornar o projeto viável.

Além disso, os clusters ficaram separados da seguinte forma


![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/clusters.png)

O que corrobora o que já foi mostrado até aqui: O resultado da clusterização foi boa para o resultado do projeto esperado.

# 7. Resultados de negócio

Como resultado, tem um conjunto de 9 clusters, que, agrupados a partir do gross revenue (quantidade*preço) é possível definir qual será o cluster dos Insiders

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/conjunto_de_clusters.png)

Assim, o cluster 5 é o considerado cluster insider, cujas características são:

**Cluster Insider**
- Número de customers: 373 (12.56% dos customers)
- Faturamento Médio: $10503,06
- Recência Média: 19 dias
- Média de Produtos Comprados: 475 produtos
- Frequência de Produtos Comprados: 0.11 produtos/dia

# 8. Respostas as perguntas de Negócio

**1.** Quem são as pessoas elegíveis para participar do programa de Insiders?
As pessoas elegíveis são encontradas a partir de seus IDs

![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/customers_id.png)

**2.** Quantos clientes farão parte do grupo?
Um total de 330 clientes.

**3.** Quais as principais características desses clientes?
**Cluster Insider**
- Número de customers: 373 (12.56% dos customers)
- Faturamento Médio: $10503,06
- Recência Média: 19 dias
- Média de Produtos Comprados: 475 produtos
- Frequência de Produtos Comprados: 0.11 produtos/dia

**4.** Qual a porcentagem de contribuição do faturamento, vinda do Insiders?
![alt text](https://github.com/CaioMendes92/customer_segmentation/blob/main/images/gross_revenue_insiders.png)

**5.** Qual a garantia que o programa Insiders é melhor que o restante da base ?
Baseado na resposta 4, é possível ver que o faturamento do programa Insiders é de quase a metade da contribuição do faturamento da loja. Sendo assim, é uma parcela que trás um retorno financeiro muito grande para empresa, mesmo sendo um grupo menor de pessoas.

**6.** Quais ações o time de marketing pode realizar para aumentar o faturamento?
As ações serão divididas por grupos e quais ações são **sugeridas**. Vale salientar que são apenas sugestões, a decisão final não é da equipe de ciência de dados.

* Cluster 5 - Cluster Insiders: É possível observar que este cluster é o com maior receita, entretanto, tem uma baixa frequencia de compras e uma alta quantidade de retornos. Uma sugestão seria limitar a quantidade de produtos retornados, evitando assim prejuízos para a loja.
* Cluster 4: Uma sugestão seria aumentar a quantidade de produtos vendidos, uma vez que, em relação ao cluster insiders, a quantidade de produtos compradas é menos da metade. Uma sugestão é utilizar cross-sell, sugerindo alguns produtos juntamente com os que estão sendo comprados.
* Cluster 3: Este é um grupo de pessoas que talvez seja interessante aumentar a frequência de compras. Como sugestão, enviar cupons de desconto para atrair os clientes mais vezes.
* Cluster 1: Neste caso, um aumento na quantidade de compras seria interessante, uma vez que a quantidade de compras é a mais baixa para grupos com Gross Revenue acima de $1k. Cross-sell unido a cupons de desconto podem ajudar este grupo a comprar mais.
* Cluster 8: Aumentar a quantidade de vezes compradas (recency) é o ideal para alavancar ainda mais este grupo. Estar sempre enviando e-mail com as principais ofertas pode ajudar.
* Cluster 7, 2, 0 e 6: Todos os quatro grupos estão com um gross-revenue abaixo de $1k, ou seja, é importante aumentar o quanto é gasto na loja. Isto pode ser feito melhorando o tempo entre as compras ou aumentando a quantidade de produtos comprados.

# 9. Conclusões
* O cluster Insider é constituído de pessoas que gastam o equivalente a quase 50% do faturamento da loja;
* É possível desenvolver uma estratégia específica para cada cluster;
* Promoções e cupons de desconto podem ser enviados de formas estratégias para alavancar as vendas da loja.

# 10. Lições Aprendidas
* Apesar de estar sendo feito modelos de Machine Learning, no fim, é necessário responder questionamento de outras áreas, sendo assim, mais importante que o modelo escolhido seja um que seja utilizável, mesmo que com um resultado mais baixo, do que um com resultados maiores e que seria descartado.
* Nem sempre por um modelo ser melhor que o outro ele será utilizado. O custo computacional no dia-a-dia precisa ser levado em conta.
* Algoritmos não-supervisionados são difíceis de ter uma precisão do resultado, é necessário um acompanhamento diário para ter certeza que não é necessário um re-treino, mesmo tendo as métricas para ter uma noção.

# 11. Próximo Passos
* Colocar o modelo em produção em sistemas cloud como AWS.
* Discutir com a equipe de Marketing para saber se outros valores de K inviabilizaria o projeto.
* Gerar outras variáveis explicativas do modelo.
