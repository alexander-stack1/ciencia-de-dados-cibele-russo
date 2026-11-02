> **Aula:** [EST5507 Modelos de Regressão 20261006 Modelo linear com matriz de planejamento de posto incompleto](https://www.youtube.com/watch?v=ftE7EAmoaaY)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudo: Modelos Lineares com Matriz de Planejamento de Posto Incompleto

Este guia de estudo destina-se a revisar e consolidar os conceitos teóricos e práticos apresentados sobre modelos lineares gerais em que a matriz de planejamento não possui posto colunar pleno. O conteúdo é estritamente fundamentado no material da disciplina EST5507 (Modelos de Regressão).

#### 1. Objetivos da Aula

Ao final do estudo deste material, o estudante de pós-graduação deverá ser capaz de:

1. **Compreender a natureza do posto incompleto:** Identificar as causas estruturais e de parametrização (como *one-hot encoding* e modelos de ANOVA superparametrizados) que levam a matriz de planejamento $`X`$ a ter posto $`R < P`$.
2. **Analisar as consequências algébricas:** Entender a inexistência da inversa usual $`(X^T X)^{-1}`$, a não identificabilidade individual dos parâmetros do vetor $`\beta`$ e a presença de infinitas soluções nas equações normais.
3. **Dominar o conceito de Inversa Generalizada:** Aplicar a definição $`A A^- A = A`$ e utilizar inversas generalizadas $`(X^T X)^-`$ para obter soluções para o sistema de equações normais.
4. **Demonstrar invariâncias do modelo:** Provar que os valores ajustados ($`\hat{Y}`$), a matriz chapéu ($`H`$), a Soma de Quadrados dos Resíduos ($`SQRes`$) e a estimativa da variância ($`\hat{\sigma}^2`$) são únicos e invariantes à escolha da inversa generalizada.
5. **Formular e avaliar Funções Estimáveis:** Identificar combinações lineares $`L^T \beta`$ estimáveis via critério do espaço linha de $`X`$ ($`L^T = T^T X`$) e aplicar o Teorema de Gauss-Markov para modelos de posto incompleto.
6. **Avaliar Hipóteses Testáveis:** Compreender as condições sob as quais hipóteses da forma $`H_0: K^T \beta = M`$ podem ser testadas e como a estatística $`F`$ é construída.
7. **Propor e comparar soluções práticas:** Avaliar alternativas para contornar o problema do posto incompleto ou de quase-singularidade, como reparametrização (casela de referência, desvios médios), seleção de variáveis, redução de dimensionalidade e métodos de regularização (Ridge, Lasso, Elastic Net).

#### 2. Desenvolvimento Passo a Passo da Matéria

##### 2.1 O Modelo Linear Geral e a Definição de Posto Incompleto

O modelo linear geral é expresso na forma matricial por:
```math
Y = X\beta + \varepsilon
```
Onde:

* $`Y`$ é o vetor de respostas de dimensão $`N \times 1`$.
* $`X`$ é a matriz de planejamento (ou matriz de modelo) de dimensão $`N \times P`$.
* $`\beta`$ é o vetor de parâmetros de dimensão $`P \times 1`$.
* $`\varepsilon`$ é o vetor de erros aleatórios de dimensão $`N \times 1`$, assumido com distribuição normal multivariada: $`\varepsilon \sim N(0, \sigma^2 I_N)`$.

Em cenários padrão (posto completo), assume-se que $`Rank(X) = P`$ (com $`P < N`$). No entanto, em diversas aplicações — como em modelos de Análise de Variância (ANOVA) com fatores categóricos superparametrizados —, o posto da matriz $`X`$ é estritamente menor que o número de colunas:
```math
Rank(X) = R < P < N
```

###### *Causas do Posto Incompleto:*

1. **Dependência Linear Exata:** Uma variável preditora é combinação linear exata de outras preditoras (ou múltiplo exato).
2. **Superparametrização (** ***One-Hot Encoding*** **):** Inclusão de um intercepto conjuntamente com todas as $`K`$ colunas indicadoras para os $`K`$ níveis de uma variável categórica, em vez de utilizar $`K-1`$ colunas.

##### 2.2 Consequências Matemáticas e a Inversa Generalizada

Pelas propriedades da álgebra matricial, o posto da matriz de coeficientes das equações normais é igual ao posto da matriz de planejamento:
```math
Rank(X^T X) = Rank(X) = R < P
```
Como $`Rank(X^T X) = R < P`$, a matriz $`X^T X`$ (de ordem $`P \times P`$) possui determinante igual a zero ($`\det(X^T X) = 0`$). Consequentemente:

1. Não existe a inversa usual $`(X^T X)^{-1}`$.
2. As equações normais dadas por $`X^T X \beta = X^T Y`$ admitem **infinitas soluções** .
3. Os parâmetros do vetor $`\beta`$ **não são identificáveis individualmente** (existem múltiplos conjuntos de parâmetros que geram exatamente o mesmo ajuste do modelo).

###### *A Inversa Generalizada*

Para obter soluções para o sistema de equações normais, define-se a inversa generalizada de uma matriz $`A`$ (denotada por $`A^-`$ ou $`A^\sim`$) como qualquer matriz que satisfaça a condição:
```math
A A^- A = A
```
Uma solução geral para o sistema de equações normais em termos de uma inversa generalizada $`(X^T X)^-`$ e de um vetor arbitrários $`Z`$ é dada por:
```math
\hat{\beta}^* = (X^T X)^- X^T Y + \left[ I - (X^T X)^- X^T X \right ] Z
```
Como $`Z`$ pode assumir qualquer valor vetorial, confirma-se a existência de infinitas estimativas para $`\hat{\beta}^*`$.

##### 2.3 Propriedades de Invariância

Apesar de o vetor de estimadores $`\hat{\beta}^*`$ não ser único, os principais resultados de predição e ajuste do modelo permanecem totalmente invariantes à escolha da inversa generalizada utilizada:

1. **Vetor de Valores Ajustados ($`\hat{Y}`$):**

```math
\hat{Y} = X \hat{\beta}^* = X (X^T X)^- X^T Y
```

O valor de $`\hat{Y}`$ é único e idêntico para qualquer inversa generalizada $`(X^T X)^-`$.

2. **Matriz Chapéu / Projeção Ortogonal ($`H`$):**

```math
H = X (X^T X)^- X^T
```

A matriz $`H`$ é uma matriz de projeção ortogonal invariante à escolha de $`(X^T X)^-`$.

3. **Soma de Quadrados dos Resíduos ($`SQRes`$):**

```math
SQRes = (Y - \hat{Y})^T (Y - \hat{Y}) = Y^T (I - H) Y
```

Permanecendo a mesma independentemente do estimador $`\hat{\beta}^*`$ adotado.

4. **Estimador da Variância do Erro ($`\hat{\sigma}^2`$):** O número de graus de liberdade associados aos resíduos ajusta-se para $`N - R`$ (onde $`R = Rank(X)`$). O estimador não viesado da variância é:

```math
\hat{\sigma}^2 = \frac{SQRes}{N - R} = Quadrado\ Médio\ do\ Resíduo\ (QMR)
```

##### 2.4 Funções Estimáveis e o Teorema de Gauss-Markov

Como os parâmetros individuais $`\beta_j`$ não podem ser estimados de forma única, busca-se identificar combinações lineares do vetor de parâmetros que possuam estimadores únicos e não viesados.

###### *Definição de Função Estimável*

Uma função linear dos parâmetros $`L^T \beta`$ é dita **estimável** se existir um estimador linear $`T^T Y`$ tal que seu valor esperado seja identicamente igual a $`L^T \beta`$:
```math
ET^T Y = L^T \beta
```

###### *Teorema e Condição de Estimabilidade*

A combinação linear $`L^T \beta`$ é estimável se, e somente se, o vetor $`L^T`$ pertencer ao espaço linha da matriz de planejamento $`X`$. Ou seja, se existir um vetor $`T`$ tal que:
```math
L^T = T^T X
```
Equivalentemente, a condição necessária e suficiente para estimabilidade em termos da inversa generalizada é:
```math
L^T (X^T X)^- X^T X = L^T
```

* **Número Máximo de Funções Estimáveis L.I.:** O número máximo de funções estimáveis linearmente independentes é igual ao posto da matriz de planejamento, ou seja, $`R = Rank(X)`$.
* **Caso de Posto Completo:** Se $`R = P`$, todo parâmetro individual $`\beta_j`$ é estimável, assim como qualquer combinação linear $`L^T \beta`$.

###### *Teorema de Gauss-Markov (Posto Incompleto)*

Se $`L^T \beta`$ for uma função estimável, então:

1. O estimador dado por $`L^T \hat{\beta}^* = L^T (X^T X)^- X^T Y`$ é **único** e **invariante** a qualquer escolha de inversa generalizada $`(X^T X)^-`$.
2. $`L^T \hat{\beta}^*`$ é o **Melhor Estimador Linear Não Viesado** (BLUE - *Best Linear Unbiased Estimator* ) de $`L^T \beta`$.
3. A variância do estimador é dada por:

```math
Var(L^T \hat{\beta}^*) = \sigma^2 L^T (X^T X)^- L
```

A qual também é estritamente invariante à escolha de $`(X^T X)^-`$.

##### 2.5 Hipóteses Testáveis

Em modelos de posto incompleto, não se pode testar hipóteses sobre parâmetros individuais isolados. Só é possível testar hipóteses nulas formuladas sobre funções estimáveis.
Uma hipótese da forma $`H_0: K^T \beta = M`$ é dita **testável** se, e somente se, cada linha da matriz $`K^T`$ for uma combinação linear das linhas da matriz $`X`$ (isto é, existe $`T`$ tal que $`K^T = T^T X`$).
Sob a validade de uma hipótese testável $`H_0`$, a estatística do teste F é construída a partir da Soma de Quadrados da Regressão sob $`H_0`$ dividida por $`\hat{\sigma}^2`$, mantendo a distribuição $`F`$ sob a hipótese nula.

##### 2.6 Exemplo Numérico Detalhado: Estudo sobre Peso de Plantas

Considere um estudo didático para avaliar o peso (em gramas) de $`N = 6`$ plantas submetidas a $`3`$ tipos categóricos: Normal, Atípica e Aberrante.

###### *Dados Observados ($`Y`$):*

* Plantas do tipo **Normal** ($`3`$ réplicas): $`101, 105, 94`$
* Plantas do tipo **Atípica** ($`2`$ réplicas): $`84, 88`$
* Plantas do tipo **Aberrante** ($`1`$ réplica): $`32`$

O vetor de respostas é:
```math
Y = \begin{bmatrix} 101 \\ 105 \\ 94 \\ 84 \\ 88 \\ 32 \end{bmatrix}
```

###### *Modelo Superparametrizado:*

```math
y_{ij} = \mu + \beta_1 X_{1ij} + \beta_2 X_{2ij} + \beta_3 X_{3ij} + \varepsilon_{ij}
```
O vetor de parâmetros possui $`P = 4`$ elementos: $`\beta = \begin{bmatrix} \mu & \beta_1 & \beta_2 & \beta_3 \end{bmatrix}^T`$.
A matriz de planejamento $`X`$ ($`6 \times 4`$) é estruturada como:

```math
X = \begin{bmatrix} 1 & 1 & 0 & 0 \\ 1 & 1 & 0 & 0 \\ 1 & 1 & 0 & 0 \\ 1 & 0 & 1 & 0 \\ 1 & 0 & 1 & 0 \\ 1 & 0 & 0 & 1 \end{bmatrix}
```


###### *Análise do Posto de $`X`$:*

Observe que a primeira coluna de $`X`$ (referente ao intercepto $`\mu`$) é a soma exata da 2ª, 3ª e 4ª colunas. Portanto, $`Rank(X) = 3 < 4`$.
A matriz $`X^T X`$ ($`4 \times 4`$) é calculada por:

```math
X^T X = \begin{bmatrix} 6 & 3 & 2 & 1 \\ 3 & 3 & 0 & 0 \\ 2 & 0 & 2 & 0 \\ 1 & 0 & 0 & 1 \end{bmatrix}
```

Como a primeira coluna/linha de $`X^T X`$ é a soma das três últimas, $`\det(X^T X) = 0`$. Não existe $`(X^T X)^{-1}`$ usual.

###### *Exemplo de Inversa Generalizada $`(X^T X)^-`$:*

Uma solução possível para a inversa generalizada obtida por algoritmos que atribuem zeros às componentes associadas à dependência linear é:

```math
(X^T X)^- = \begin{bmatrix} 0 & 0 & 0 & 0 \\ 0 & 1/3 & 0 & 0 \\ 0 & 0 & 1/2 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
```


###### *Funções Estimáveis do Exemplo:*

Os parâmetros $`\mu, \beta_1, \beta_2, \beta_3`$ isoladamente **não são estimáveis** . Contudo, as seguintes funções lineares são estimáveis e formam uma base de dimensão $`R = 3`$:

1. $`\mu + \beta_1`$ (Média populacional do grupo Normal)
2. $`\mu + \beta_2`$ (Média populacional do grupo Atípico)
3. $`\mu + \beta_3`$ (Média populacional do grupo Aberrante)

Para estimar, por exemplo, $`\mu + \beta_1`$, define-se $`L^T = \begin{bmatrix} 1 & 1 & 0 & 0 \end{bmatrix}`$. Multiplicando pelas matrizes do modelo:
```math
L^T \hat{\beta}^* = L^T (X^T X)^- X^T Y = \frac{101 + 105 + 94}{3} = 100
```
A estimativa resulta na média amostral exata do grupo Normal ($`100`$ g), demonstrando que o valor da função estimável é único e independente da escolha da inversa generalizada.

##### 2.7 Soluções Práticas em Regressão

Para evitar os problemas numéricos e teóricos decorrentes do posto incompleto ou da quase-singularidade (determinante muito próximo de zero), adotam-se na prática as seguintes abordagens:

| Abordagem | Mecanismo | Aplicação / Exemplo |
|---|---|---|
| 1. Reparametrização | Reduz o número de colunas da matriz $`X`$ eliminando redundâncias. | Casela de Referência: Omite-se a coluna do nível de referência (ex: $`K-1`$ dummies ). Desvios Médios: Aplica-se restrições de contrastes aos parâmetros (ex: $`\sum \beta_j = 0`$). |
| 2. Redução de Dimensionalidade / Seleção | Elimina variáveis preditoras altamente correlacionadas ou redundantes. | Remoção direta de preditoras duplicadas ou uso de Componentes Principais (PCA), embora PCA dificulte a interpretação dos parâmetros originais. |
| 3. Métodos de Regularização | Adiciona uma penalização à matriz $`X^T X`$, garantindo que ela se torne estritamente invertível. | Regressão Ridge: Adiciona $`\lambda I`$ à matriz ($`X^T X + \lambda I`$), resolvendo a singularidade. Lasso / Elastic Net: Realiza seleção e regularização simultânea. |


#### 3. Quiz de Resposta Curta (10 Questões)

1. O que significa afirmar que uma matriz de planejamento $`X`$ possui posto incompleto?
2. Quais são as consequências diretas do posto incompleto sobre o determinante da matriz $`X^T X`$ e a resolução das equações normais?
3. Como é definida matematicamente a inversa generalizada de uma matriz $`A`$ ?
4. O vetor de parâmetros estimados $`\hat{\beta}^*`$ obtido em um modelo de posto incompleto é único? Explique.
5. Quais quantidades estimadas no modelo linear permanecem estritamente invariantes à escolha da inversa generalizada?
6. O que é uma função estimável no contexto de modelos lineares gerais?
7. Qual é a condição necessária e suficiente expressa via matriz de planejamento $`X`$ para que $`L^T \beta`$ seja uma função estimável?
8. Qual é o número máximo de funções estimáveis linearmente independentes que podem ser obtidas em um modelo com matriz $`X_{N \times P}`$ de posto $`R`$ ?
9. O que estabelece o Teorema de Gauss-Markov para modelos lineares de posto incompleto?
10. Cite duas alternativas práticas recomendadas para resolver o problema de posto incompleto sem necessidade de calcular inversas generalizadas.

##### Gabarito / Chave de Respostas

1. **Resposta:** Significa que o posto colunar da matriz $`X_{N \times P}`$ é $`R < P`$, indicando que existe ao menos uma dependência linear exata entre suas colunas. Isso ocorre frequentemente por superparametrização, como no uso de *one-hot encoding* para variáveis categóricas sem remoção da categoria de referência.
2. **Resposta:** A matriz $`X^T X`$ torna-se singular com determinante igual a zero ($`\det(X^T X) = 0`$), impossibilitando a obtenção da inversa usual $`(X^T X)^{-1}`$. Consequentemente, o sistema de equações normais passa a admitir infinitas soluções, tornando os parâmetros do vetor $`\beta`$ não identificáveis individualmente.
3. **Resposta:** A inversa generalizada de uma matriz $`A`$, denotada por $`A^-`$, é definida como qualquer matriz que satisfaz a relação algébrica $`A A^- A = A`$. Ela permite encontrar soluções gerais para sistemas de equações lineares cujas matrizes de coeficientes não possuem inversa usual.
4. **Resposta:** Não, o vetor $`\hat{\beta}^*`$ não é único e depende da escolha da inversa generalizada $`(X^T X)^-`$ e de um vetor arbitrário $`Z`$. Por essa razão, os parâmetros individuais $`\beta_j`$ não são identificáveis separadamente.
5. **Resposta:** Permanecem invariantes o vetor de valores ajustados ($`\hat{Y} = X \hat{\beta}^*`$), a matriz chapéu ($`H`$), a Soma de Quadrados dos Resíduos ($`SQRes`$) e o estimador da variância do erro ($`\hat{\sigma}^2`$). Todos esses elementos produzem exatamente os mesmos resultados numéricos independentemente da inversa generalizada adotada.
6. **Resposta:** Uma função estimável é uma combinação linear dos parâmetros, representada por $`L^T \beta`$, para a qual existe pelo menos um estimador linear e não viesado da forma $`T^T Y`$. Nesses casos, o valor esperado de $`T^T Y`$ é identicamente igual a $`L^T \beta`$.
7. **Resposta:** A condição é que o vetor $`L^T`$ deve pertencer ao espaço linha da matriz de planejamento $`X`$, ou seja, deve existir um vetor $`T`$ tal que $`L^T = T^T X`$. Alternativamente, isso equivale à condição $`L^T (X^T X)^- X^T X = L^T`$.
8. **Resposta:** O número máximo de funções estimáveis linearmente independentes é igual ao próprio posto da matriz de planejamento $`X`$, representado por $`R = Rank(X)`$. Esse conjunto de funções forma uma base do espaço das funções estimáveis do modelo.
9. **Resposta:** O teorema estabelece que, se $`L^T \beta`$ for uma função estimável, o seu estimador $`L^T \hat{\beta}^* = L^T (X^T X)^- X^T Y`$ é único, invariante à escolha da inversa generalizada e representa o Melhor Estimador Linear Não Viesado (BLUE) de $`L^T \beta`$.
10. **Resposta:** As principais alternativas são a reparametrização do modelo (por exemplo, utilizando a parametrização por casela de referência ou por desvios médios) e o uso de métodos de regularização (como a Regressão Ridge, que adiciona um termo à diagonal de $`X^T X`$ para torná-la invertível).

#### 4. Questões para Ensaio (Dissertativas)

*Nota: As questões a seguir destinam-se ao aprofundamento analítico e reflexão teórica; não acompanham respostas.*

1. Discuta detalhadamente como o problema de identificabilidade de parâmetros se manifesta em modelos de Análise de Variância (ANOVA) com parametrização completa versus parametrização por casela de referência. Explique os impactos dessas escolhas na interpretação teórica e prática dos coeficientes estimados.
2. Analise analiticamente a demonstração da invariância do vetor de valores ajustados $`\hat{Y} = X \hat{\beta}^*`$ frente à escolha da inversa generalizada $`(X^T X)^-`$. Por que a projeção ortogonal gerada pela matriz $`H = X(X^T X)^- X^T`$ garante a unicidade de $`\hat{Y}`$ mesmo quando $`\hat{\beta}^*`$ tem infinitas soluções?
3. Compare as abordagens de reparametrização, seleção de variáveis (ou PCA) e regularização (Ridge/Lasso) no tratamento de matrizes de planejamento que apresentam quase-singularidade (determinante próximo de zero em decorrência de alta multicolinearidade). Quais são as vantagens e limitações de cada método em termos de viés, variância e interpretabilidade?
4. Considere um modelo linear em que uma variável preditora categórica de $`K`$ níveis é codificada via *one-hot encoding* sem intercepto versus com intercepto. Deduza a estrutura da matriz $`X^T X`$ em ambos os casos, determinando o posto de $`X`$ e identificando quais funções lineares dos parâmetros são estimáveis em cada configuração.
5. Em testes de hipóteses lineares sob a estrutura $`H_0: K^T \beta = M`$, explique rigorosamente por que a condição de testabilidade exige que cada linha de $`K^T`$ seja estimável. O que ocorre com a estatística do teste $`F`$ e com o p-valor se uma hipótese não testável for submetida ao teste?

#### 5. Glossário Completo

* **Casela de Referência (** ***Reference Cell*** **):** Técnica de reparametrização para variáveis categóricas na qual uma das categorias é omitida (definida como nível base ou referência), atribuindo-se colunas indicadoras ( *dummies* ) apenas às $`K-1`$ categorias restantes para garantir posto pleno na matriz de planejamento.
* **Desvios Médios ( parametrização por contrastes):** Forma de reparametrização em modelos categóricos que impõe restrições aos parâmetros (como $`\sum \beta_j = 0`$), permitindo interpretar os parâmetros como desvios em relação à média geral do experimento.
* **Espaço Linha de $`X`$:** O subespaço vetorial gerado pelas linhas da matriz de planejamento $`X`$. Um vetor $`L^T`$ pertence ao espaço linha de $`X`$ se puder ser escrito como combinação linear das linhas de $`X`$.
* **Função Estimável:** Combinação linear dos parâmetros $`L^T \beta`$ que possui um estimador linear não viesado $`T^T Y`$, garantindo estimativa única e invariante em modelos de posto incompleto.
* **Hipótese Testável:** Hipótese nula da forma $`H_0: K^T \beta = M`$ em que todas as combinações lineares definidas pelas linhas da matriz $`K^T`$ constituem funções estimáveis.
* **Identificabilidade de Parâmetros:** Propriedade de um modelo estatístico na qual diferentes valores dos parâmetros resultam em distribuições de probabilidade distintas para os dados observados. Em modelos de posto incompleto, os parâmetros são não identificáveis individualmente.
* **Inversa Generalizada ($`A^-`$ou $`A^\sim`$):** Matriz que satisfaz a condição $`A A^- A = A`$, utilizada em álgebra linear para resolver sistemas de equações lineares singulares ou indeterminados.
* **Matriz Chapéu ($`H`$):** Matriz de projeção ortogonal definida por $`H = X(X^T X)^- X^T`$ que mapeia o vetor de observações $`Y`$ no subespaço estimado dos valores ajustados $`\hat{Y}`$.
* **Matriz de Planejamento ($`X`$):** Matriz de dimensão $`N \times P`$ contendo os valores das variáveis explicativas (ou indicadoras) para cada uma das $`N`$ observações do modelo.
* **One-Hot Encoding:** Método de codificação no qual uma variável categórica de $`K`$ níveis é convertida em $`K`$ variáveis binárias (indicadoras). Quando combinado com um vetor intercepto, gera dependência linear exata na matriz $`X`$.
* **Posto Colunar ($`Rank`$):** Número máximo de colunas linearmente independentes de uma matriz.
* **Quase-Singularidade (Multicolinearidade Severa):** Condição na qual as colunas de $`X`$ não são exatamente combinações lineares umas das outras, mas possuem altíssima correlação, fazendo com que $`\det(X^T X)`$ seja muito próximo de zero e causando instabilidade numérica e altas variâncias nas estimativas.
* **Regressão Ridge (Crista):** Método de regularização que adiciona uma penalidade aos coeficientes somando uma constante $`\lambda`$ à diagonal principal de $`X^T X`$ (resultando em $`X^T X + \lambda I`$), eliminando a singularidade da matriz.
