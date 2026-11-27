> Material derivado das aulas da Profa. Cibele Russo (ICMC/USP), organizado com auxílio do NotebookLM. [↩ Voltar à trilha](README.md)

# Guia de Estudos: Análise Multivariada e Aprendizado Não Supervisionado

## 1. Questionário de Avaliação (Quiz)

1. Qual é a diferença fundamental entre técnicas de aprendizado supervisionado (como a Análise Discriminante Linear - LDA) e aprendizado não supervisionado (como a Análise de Componentes Principais - PCA)?
2. Como se define a estatística $`T^2`$ de Hotelling para uma única amostra e qual é a sua distribuição exata sob a hipótese nula $`H_0: \boldsymbol{\mu} = \boldsymbol{\mu}_0`$ em populações com distribuição normal multivariada?
3. No contexto do teste de hipóteses para comparação de vetores de médias de duas populações, qual é a distinção metodológica no tratamento de amostras pareadas (correlacionadas) versus amostras independentes?
4. O que é a Distância de Mahalanobis e por que ela é preferível à distância euclidiana simples na análise de dados multivariados com variáveis de diferentes magnitudes e variâncias?
5. Como se realiza a decomposição da matriz de somas de quadrados e produtos cruzados na Análise de Variância Multivariada (MANOVA)?
6. Em Análise de Componentes Principais (PCA), qual é a consequência prática de extrair as componentes a partir da matriz de variâncias e covariâncias (S) em comparação com a matriz de correlações (R)?
7. Qual é o objetivo principal da Análise de Correlações Canônicas (CCA) e como se determina o número máximo de pares de variáveis canônicas possíveis de serem obtidos?
8. Em Análise de Agrupamentos (Cluster Analysis), qual é a diferença entre os métodos hierárquicos aglomerativos de Ligação Simples (Single Linkage) e de Ligação Completa (Complete Linkage)?
9. Como o Teorema do Limite Central (TLC) fundamenta a aplicação dos testes de hipóteses baseados na estatística $`T^2`$ de Hotelling em conjuntos de dados cujas variáveis individuais violam a suposição de normalidade (como dados dicotômicos)?
10. O que representa o Lambda de Wilks ($`\Lambda^*`$) na MANOVA e como a aproximação de Bartlett permite realizar a inferência estatística sobre essa quantidade em amostras grandes?

## 2. Gabarito do Questionário

1. O aprendizado supervisionado utiliza variáveis respostas acompanhadas de rótulos ou classificações prévias para treinar modelos preditivos ou de discriminação, a exemplo da LDA. Por outro lado, o aprendizado não supervisionado opera em dados sem rótulos ou variáveis respostas definidas, buscando identificar estruturas ocultas, padrões de associação ou reduções de dimensionalidade, como visto na PCA e na Análise de Agrupamentos.

2. A estatística $`T^2`$ de Hotelling é uma forma quadrática construída a partir do vetor de médias amostrais $`\mathbf{\bar{x}}`$, do vetor sob hipótese nula $`\boldsymbol{\mu}_0`$ e da inversa da matriz de covariâncias amostrais S. Sob a hipótese nula em uma população normal p-variada, ela se relaciona diretamente com a distribuição F de Snedecor por meio da transformação $`\dfrac{n-p}{(n-1)p} T^2 \sim F_{p, n-p}`$.

3. Para amostras pareadas ou correlacionadas, calcula-se a diferença vetorial individual $`D_j = X_{1j} - X_{2j}`$ para reduzir o problema a um teste de amostra única do vetor de médias das diferenças contra o vetor nulo. Em amostras independentes de duas populações, avalia-se a diferença direta das médias amostrais ($`\mathbf{\bar{x}}_1 - \mathbf{\bar{x}}_2`$) empregando a matriz de covariâncias combinada ($`S_{pooled}`$) sob a suposição de igualdade das matrizes de covariância populacionais.

4. A Distância de Mahalanobis é uma métrica de afastamento entre vetores ponderada pela inversa da matriz de variâncias e covariâncias. Ela realiza uma padronização estatística, impedindo que atributos com amplitudes numéricas ou variâncias elevadas dominem o cálculo da distância e levando em conta a estrutura de correlação entre as variáveis.

5. A MANOVA decompõe a matriz de somas de quadrados e produtos cruzados total (T) na soma da matriz entre tratamentos/grupos (B, between) com a matriz dentro dos tratamentos/resíduos (W, within). Essa estrutura matricial estende a ANOVA univariada para avaliar simultaneamente se os vetores de médias de G populações normais independentes são idênticos.

6. A extração baseada na matriz de covariâncias S preserva as escalas originais, fazendo com que variáveis com maior variância exerçam peso desproporcional sobre as componentes principais resultantes. O uso da matriz de correlações R padroniza previamente todas as variáveis para variância unitária, garantindo que diferenças nas unidades de medida originais não distorçam a contribuição relativa de cada variável.

7. A Análise de Correlações Canônicas visa maximizar a correlação linear entre combinações lineares de dois conjuntos distintos de variáveis, denotados por U e V. O número máximo de pares de variáveis canônicas que podem ser formados equivale ao mínimo entre a quantidade de variáveis do primeiro conjunto (p) e do segundo conjunto (q), ou seja, $`\min(p, q)`$.

8. No critério de Ligação Simples, a distância entre dois agrupamentos é definida como a menor distância verificada entre qualquer elemento do primeiro grupo e qualquer elemento do segundo grupo. Por sua vez, o critério de Ligação Completa define a distância entre grupos como a máxima distância observada entre quaisquer pares de elementos pertencentes a cada agrupamento.

9. O Teorema do Limite Central garante que a distribuição do vetor de médias amostrais $`\mathbf{\bar{x}}`$ aproxima-se de uma distribuição normal multivariada à medida que o tamanho da amostra n cresce suficientemente em relação ao número de variáveis p. Com isso, os procedimentos inferenciais baseados na estatística $`T^2`$ de Hotelling mantêm sua validade assintótica mesmo se os dados individuais apresentarem distribuições não normais ou variáveis dicotômicas.

10. O Lambda de Wilks ($`\Lambda^*`$) é uma estatística de teste da MANOVA calculada como a razão dos determinantes da matriz residual e da matriz total, dada por $`\Lambda^* = \dfrac{|W|}{|W+B|}`$. Para amostras grandes, a transformação baseada no logaritmo dessa estatística é aproximada por uma distribuição Qui-Quadrado via teste de Bartlett com $`p(G-1)`$ graus de liberdade sob a hipótese nula de igualdade dos vetores de médias.

## 3. Questões para Desenvolvimento (Ensaio)

1. Discuta a relação de equivalência entre testes de hipóteses multivariados para o vetor de médias utilizando a estatística $`T^2`$ de Hotelling e a construção de regiões de confiança elipsoidais. Em sua resposta, analise o papel da matriz de covariâncias e da forma quadrática na definição da geometria dessas superfícies de confiança.

2. Compare criticamente as técnicas de Análise de Componentes Principais (PCA) e Análise de Correlações Canônicas (CCA) quanto aos seus objetivos de otimização, restrições matriciais e formas de interpretação. Em quais cenários práticos a aplicação da CCA se mostra mais vantajosa do que a PCA isolada?

3. Avalie o impacto das suposições teóricas da Análise de Variância Multivariada (MANOVA) — especificamente normalidade multivariada, homogeneidade das matrizes de covariância e independência das observações — sobre a validade da inferência. Quais alternativas metodológicas ou adaptações devem ser consideradas pelo pesquisador diante de violações graves dessas premissas?

4. Analise as diferenças conceituais e algorítmicas entre abordagens de agrupamento hierárquicas (aglomerativas com diferentes critérios de ligação, como Ward, Ligação Simples e Centróide) e não hierárquicas (como o método das k-médias). Como a padronização prévia dos dados e a escolha da métrica de distância influenciam os agrupamentos finais formados?

5. Explique a fundamentação assintótica dos autovalores e autovetores amostrais obtidos na PCA e discuta a sua importância para a inferência estatística, destacando como esses resultados assintóticos permitem a construção de intervalos de confiança para as variâncias das componentes principais.

## 4. Glossário Abrangente de Termos-Chave

| Termo | Definição |
|---|---|
| Algoritmo das K-Médias (K-Means) | Método de agrupamento não hierárquico que particiona n observações em k grupos pré-fixados, onde cada observação pertence ao grupo com o centróide (vetor de médias) mais próximo, atualizando iterativamente as posições dos centróides. |
| Análise de Componentes Principais (PCA) | Técnica de aprendizado não supervisionado que transforma linearmente um conjunto de p variáveis correlacionadas em um conjunto de componentes não correlacionadas ordenadas de forma decrescente pela variância explicada, permitindo a redução de dimensionalidade. |
| Análise de Correlações Canônicas (CCA) | Técnica multivariada criada por Hotelling para identificar e quantificar a relação linear entre dois conjuntos distintos de variáveis aleatórias (X e Y), construindo pares de combinações lineares (U e V) que maximizam a correlação entre si. |
| Análise de Variância Multivariada (MANOVA) | Extensão da ANOVA para cenários com múltiplas variáveis respostas quantitativas dependentes simultâneas, utilizada para testar a igualdade dos vetores de médias de G populações independentes. |
| Análise Discriminante Linear (LDA) | Técnica de aprendizado supervisionado voltada para a classificação de observações em grupos pré-definidos e redução de dimensionalidade, encontrando combinações lineares de características que melhor separam duas ou mais classes. |
| Cargas Canônicas (Canonical Loadings) | Correlações entre as variáveis originais de um conjunto e as variáveis canônicas resultantes, utilizadas para interpretar a contribuição de cada variável individual na estrutura de associação canônica. |
| Decomposição Espectral | Teorema da álgebra linear que decompõe uma matriz simétrica e positiva definida (como a matriz de covariâncias ou de correlações) em um produto de seus autovalores e autovetores ortonormais, constituindo a base matemática da PCA. |
| Dendrograma | Diagrama em árvore estruturado em graus de distância ou dissimilaridade que ilustra a sequência de fusões ou divisões ocorridas ao longo de um processo de agrupamento hierárquico. |
| Distância de Mahalanobis | Métrica de distância entre dois vetores populacionais ou amostrais ponderada pela inversa da matriz de variâncias e covariâncias, levando em consideração a escala e a correlação existente entre os atributos. |
| Estatística $`T^2`$ de Hotelling | Forma quadrática utilizada para testar hipóteses sobre vetores de médias populacionais no caso multivariado, estendendo a estatística t de Student univariada. |
| Lambda de Wilks ($`\Lambda^*`$) | Estatística multivariada utilizada em MANOVA para testar a hipótese nula de igualdade de vetores de médias entre grupos, calculada a partir da razão dos determinantes $`\|W\| / \|W + B\|`$. |
| Matriz de Covariâncias Combinada ($`S_{pooled}`$) | Média ponderada pelas respetivas margens de graus de liberdade das matrizes de covariância amostrais de duas ou mais populações, empregada sob a suposição de homogeneidade das matrizes de covariância. |
| Matriz de Somas de Quadrados e Produtos Cruzados | Matriz $`p \times p`$ que armazena as somas de quadrados das variáveis na diagonal principal e os produtos cruzados entre pares de variáveis fora da diagonal, utilizada para decompor a variabilidade em MANOVA ($`T = B + W`$). |
| Região de Confiança Elipsoidal | Extensão multivariada do intervalo de confiança, correspondente ao conjunto de vetores de parâmetros no espaço p-dimensional que satisfazem uma determinada restrição quadrática associada à estatística $`T^2`$ de Hotelling. |
| Scree Plot (Gráfico de Cotovelo) | Representação gráfica dos autovalores em ordem decrescente em função do número de componentes principais, empregada como critério visual para determinar a quantidade ideal de componentes a reter no problema. |
| Variáveis Canônicas | Combinações lineares não aleatórias $`U_k = \mathbf{a}_k' X`$ e $`V_k = \mathbf{b}_k' Y`$ derivadas para apresentar correlação máxima entre si, mantendo-se não correlacionadas com os outros pares de variáveis canônicas formados. |
