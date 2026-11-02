> **Aula:** [EST5507 Modelos de Regressão - Qualidade do ajuste em MLGs](https://www.youtube.com/watch?v=p4psoyyaOG4)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudos: EST5507 – Qualidade do Ajuste em Modelos Lineares Generalizados (MLGs)

#### Objetivos da Aula

Ao final do estudo deste material, o estudante de pós-graduação deverá ser capaz de:

1. Compreender a distinção fundamental entre a avaliação da qualidade do ajuste do ponto de vista estatístico e a modelagem orientada a desempenho preditivo.
2. Definir conceitualmente o modelo saturado e o modelo proposto (reduzido) no contexto dos Modelos Lineares Generalizados (MLGs).
3. Derivar formalmente a função desvio ( *deviance* ) a partir da razão de log-verossimilhanças para a família exponencial.
4. Aplicar e simplificar as expressões da função desvio e da *deviance* para distribuições específicas, como a Normal e a Poisson.
5. Entender o papel da distribuição Qui-Quadrado ($`\chi^2_{n-p}`$) no teste de hipóteses para avaliação do desvio.
6. Compreender o funcionamento do método de Mínimos Quadrados Reponderados Iterativamente (IWLS) via algoritmos de Newton-Raphson e Score de Fisher para estimação de parâmetros em MLGs (modelos Bernoulli e Poisson).

#### Desenvolvimento Passo a Passo e Fórmulas

##### 1. Ajuste Estatístico vs. Desempenho Preditivo

Na modelagem estatística tradicional via MLGs, o foco principal reside na **qualidade do ajuste** para verificar se o modelo proposto é adequado aos dados efetivamente observados, além de priorizar a interpretação dos parâmetros e a extrapolação controlada.
Diferentemente da modelagem preditiva (que divide bases em treino/teste, utiliza validação cruzada e avalia métricas como Erro Quadrático Médio - EQM ou Erro Absoluto Médio - EAM), a análise de qualidade do ajuste em MLGs utiliza toda a base de dados observada. A avaliação é realizada por meio da **função desvio** e do estudo dos resíduos componentes do desvio.

##### 2. O Modelo Saturado vs. O Modelo Proposto (Reduzido)

Para avaliar se o modelo ajustado retém informação suficiente dos dados, faz-se uma comparação estruturada entre dois modelos:

* **Modelo Saturado (Superparametrizado):** Assume a estimação de cada parâmetro de média $`\mu_i`$ pela própria observação $`y_i`$ ($`\hat{\mu}_i = y_i`$). Possui $`n`$ parâmetros para $`n`$ observações. Representa o ajuste perfeito e fornece o valor máximo absoluto possível para a log-verossimilhança.
* **Modelo Proposto/Postulado (Reduzido):** Utiliza um número menor de parâmetros $`p`$ (onde $`p < n`$), estruturados via vetor de covariáveis e coeficientes $`\boldsymbol{\beta}`$ ($`\beta_0, \beta_1, \dots, \beta_{p-1}`$).

##### 3. Derivação Matemática da Função Desvio na Família Exponencial

A função de densidade (ou massa) de probabilidade para uma observação $`y_i`$ pertencente à família exponencial é dada por:
```math
f(y_i; \theta_i, \phi) = \exp \left\{ \phi y_i \theta_i - b(\theta_i) + c(y_i, \phi) \right\}
```
Aplicando a função logaritmo, a $`i`$ -ésima componente da log-verossimilhança é expressa por:
```math
\ell_i(\mu_i | y_i) = \phi y_i \theta_i - b(\theta_i) + c(y_i, \phi)
```
Para a amostra completa de $`n`$ observações independentes, a log-verossimilhança é a soma de suas parcelas:
```math
L(\boldsymbol{\mu}; \mathbf{y}) = \sum_{i=1}^{n} \left\{ \phi y_i \theta_i - b(\theta_i) + c(y_i, \phi) \right\}
```

###### *Maximização no Modelo Saturado*

Para encontrar o estimador de máxima verossimilhança de $`\mu_i`$ no modelo saturado, deriva-se a $`i`$ -ésima parcela da log-verossimilhança em relação a $`\mu_i`$ aplicando a regra da cadeia:
```math
\frac{d \ell_i}{d \mu_i} = \frac{d \ell_i}{d \theta_i} \cdot \frac{d \theta_i}{d \mu_i}
```
Sabendo que $`\frac{d b(\theta_i)}{d \theta_i} = \mu_i`$ e que a função de variância é dada por $`V(\mu_i) = \frac{d \mu_i}{d \theta_i}`$ (portanto, $`\frac{d \theta_i}{d \mu_i} = \frac{1}{V(\mu_i)}`$), tem-se:
```math
\frac{d \ell_i}{d \mu_i} = \phi \left( y_i - \frac{d b(\theta_i)}{d \theta_i} \right) \cdot \frac{d \theta_i}{d \mu_i} = \phi (y_i - \mu_i) \cdot \frac{1}{V(\mu_i)} = \frac{\phi}{V(\mu_i)} (y_i - \mu_i)
```
Igualando a derivada a zero:
```math
\frac{\phi}{V(\mu_i)} (y_i - \hat{\mu}_i) = 0 \implies \hat{\mu}_i = y_i
```
A derivada de segunda ordem é:
```math
\frac{d^2 \ell_i}{d \mu_i^2} = -\frac{\phi}{V(\mu_i)} < 0
```
O que confirma que $`\hat{\mu}_i = y_i`$ é o ponto de máximo absoluto da log-verossimilhança no modelo saturado.

###### *Definição da Função Desvio ($`D^*`$) e Deviance ($`d`$)*

A função desvio ($`D^*`$) surge da razão de verossimilhanças entre o modelo saturado e o modelo proposto, multiplicada por $`2`$ ao aplicar a diferença dos logs:
```math
D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \left[ L(\mathbf{y}; \mathbf{y}) - L(\hat{\boldsymbol{\mu}}; \mathbf{y}) \right ]
```
Quando o parâmetro de dispersão $`\phi`$ é fixado ou isolado, define-se a *deviance* (ou desvio reduzido) $`d(\mathbf{y}, \hat{\boldsymbol{\mu}})`$ como:
```math
D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = \phi \cdot d(\mathbf{y}, \hat{\boldsymbol{\mu}})
```

##### 4. Teste de Hipóteses e Distribuição da Função Desvio

Para decidir se o modelo proposto é adequado, formula-se o teste de hipóteses:

* $`H_0`$: O modelo proposto/postulado está correto.
* $`H_1`$: O modelo saturado é necessário (o modelo proposto é inadequado).
* **Regra de Decisão:** Rejeita-se $`H_0`$ se o valor observado de $`D^*`$ for excessivamente grande (maior que um crítico $`C`$). Um desvio pequeno indica bom ajuste (o modelo reduzido retém quase tanta informação quanto o saturado); um desvio grande indica mau ajuste.
* **Distribuição de Referência:** Em geral, a distribuição exata de $`D^*`$ é desconhecida. Contudo, a literatura indica a comparação dos valores observados de $`D^*`$ com os quantis de uma distribuição Qui-Quadrado com $`n - p`$ graus de liberdade ($`\chi^2_{n-p}`$) para um nível de significância $`\alpha`$.
* **Valor Esperado:** Se $`D^* \sim \chi^2_{n-p}`$, então o seu valor esperado é igual aos seus graus de liberdade:

```math
ED^* = n - p
```

##### 5. Casos Particulares: Aplicação aos Modelos Normal e Poisson

###### *A. Modelo Normal*

Para o modelo Normal com variância $`\sigma^2`$ (onde $`\theta_i = \mu_i`$, $`b(\theta_i) = \frac{\theta_i^2}{2} = \frac{\mu_i^2}{2}`$ e $`\phi = \frac{1}{\sigma^2}`$):

* **Deviance:**

```math
d(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^{n} \left [\left( y_i y_i - \frac{y_i^2}{2} \right) - \left( y_i \hat{\mu}_i - \frac{\hat{\mu}_i^2}{2} \right) \right ] = \sum_{i=1}^{n} (y_i - \hat{\mu}_i)^2
```

* **Função Desvio Integrada:**

```math
D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = \frac{1}{\sigma^2} \sum_{i=1}^{n} (y_i - \hat{\mu}_i)^2
```

No caso Normal, a função desvio dividida por $`\sigma^2`$ segue exatamente uma distribuição $`\chi^2_{n-p}`$. Se $`\sigma^2`$ for desconhecido, ele pode ser estimado via quadrado médio do resíduo.

###### *B. Modelo Poisson*

Para o modelo Poisson (onde $`\phi = 1`$, $`\theta_i = \log(\mu_i)`$, $`b(\theta_i) = e^{\theta_i} = \mu_i`$):

* No modelo saturado: $`\theta_i = \log(y_i)`$ e $`b(\theta_i) = y_i`$.
* No modelo proposto: $`\theta_i = \log(\hat{\mu}_i)`$ e $`b(\theta_i) = \hat{\mu}_i`$.
* **Função Desvio / Deviance:**

```math
D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^{n} \left[ y_i \log(y_i) - y_i - \left( y_i \log(\hat{\mu}_i) - \hat{\mu}_i \right) \right ]
```

```math
D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^{n} \left[ y_i \log\left(\frac{y_i}{\hat{\mu}_i}\right) + \hat{\mu}_i - y_i \right ]
```

##### 6. Estimação de Parâmetros por Mínimos Quadrados Reponderados Iterativamente (IWLS)

A estimação dos parâmetros $`\boldsymbol{\beta}`$ em MLGs não possui solução analítica em forma fechada na maioria dos casos, exigindo procedimentos iterativos como Newton-Raphson ou Score de Fisher.

1. **Vetor Score $`U(\boldsymbol{\beta})`$:** Representa o gradiente do log da verossimilhança:

```math
U(\boldsymbol{\beta}) = X^T W (Y - \boldsymbol{\mu})
```

2. **Matriz de Informação de Fisher / Hessiana:** No método Score de Fisher, utiliza-se a esperança da matriz Hessiana, simplificando os termos pela propriedade $`EY - \boldsymbol{\mu} = 0`$.
3. **Processo Iterativo:**

```math
\boldsymbol{\beta}^{(k+1)} = \boldsymbol{\beta}^{(k)} + \left( X^T W^{(k)} X \right)^{-1} U\left(\boldsymbol{\beta}^{(k)}\right)
```

###### *Exemplos de Funções de Ligação Canônica:*

* **Modelo Bernoulli (Regressão Logística):**
* Ligação Logito: $`\eta_i = \log\left(\frac{\mu_i}{1 - \mu_i}\right) = \mathbf{x}_i^T \boldsymbol{\beta}`$.
* Estimador de Média (Inversa da Ligação): $`\hat{\mu}_i = \frac{1}{1 + \exp(-\mathbf{x}_i^T \hat{\boldsymbol{\beta}})}`$.
* $`\hat{\mu}_i`$ representa a probabilidade estimada de sucesso para o indivíduo $`i`$.
* **Modelo Poisson:**
* Ligação Logarítmica: $`\eta_i = \log(\mu_i) = \mathbf{x}_i^T \boldsymbol{\beta}`$.
* Estimador de Média (Inversa da Ligação): $`\hat{\mu}_i = \exp(\mathbf{x}_i^T \hat{\boldsymbol{\beta}})`$.

#### Questionário de Avaliação (Quiz de Respostas Curtas)

1. Qual é a diferença fundamental entre a avaliação da qualidade do ajuste estatístico e a avaliação de modelos sob o ponto de vista preditivo?
2. Como é definido conceitualmente o modelo saturado no contexto dos MLGs?
3. Qual é a expressão matemática genérica da função desvio $`D^*(\mathbf{y}, \hat{\boldsymbol{\mu}})`$ em termos de log-verossimilhanças?
4. Por que o fator $`2`$ é utilizado na construção da função desvio?
5. Ao derivar a log-verossimilhança de uma componente da família exponencial em relação a $`\mu_i`$, qual ponto de máximo é obtido no modelo saturado?
6. Como se correlacionam a *deviance* $`d(\mathbf{y}, \hat{\boldsymbol{\mu}})`$ e a função desvio $`D^*(\mathbf{y}, \hat{\boldsymbol{\mu}})`$ na presença do parâmetro de dispersão $`\phi`$ ?
7. Qual é a regra de decisão do teste de hipóteses para a qualidade do ajuste baseado na função desvio?
8. Qual é a forma simplificada da *deviance* para o caso de um Modelo Linear Generalizado Normal?
9. Qual é a expressão da função desvio para o modelo com distribuição Poisson?
10. Como se obtém a estimativa da média $`\hat{\mu}_i`$ para um indivíduo específico em um modelo de regressão logística após a convergência dos estimadores $`\hat{\boldsymbol{\beta}}`$ ?

#### Chave de Respostas do Quiz

1. **Resposta:** A avaliação do ajuste estatístico busca verificar se o modelo proposto descreve adequadamente todos os dados observados, focando na interpretação dos parâmetros e na função desvio. Já a abordagem preditiva prioriza a capacidade de generalização para novos dados, utilizando métricas como erro quadrático médio (EQM) e divisão da base em treino e teste.
2. **Resposta:** O modelo saturado é um modelo superparametrizado no qual se assume um parâmetro de média para cada observação ($`\hat{\mu}_i = y_i`$). Ele não realiza redução de dimensionalidade ($`n`$ parâmetros para $`n`$ observações) e fornece o valor máximo absoluto alcançável pela função de log-verossimilhança.
3. **Resposta:** A função desvio é dada por $`D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \left[ L(\mathbf{y}; \mathbf{y}) - L(\hat{\boldsymbol{\mu}}; \mathbf{y}) \right ]`$. Ela calcula duas vezes a diferença entre a log-verossimilhança do modelo saturado $`L(\mathbf{y}; \mathbf{y})`$ e a do modelo proposto $`L(\hat{\boldsymbol{\mu}}; \mathbf{y})`$.
4. **Resposta:** O fator $`2`$ deriva da construção do teste de razão de verossimilhanças, cuja estatística assintótica fundamenta-se em $`-2 \log(\Lambda)`$. Essa multiplicação facilita a obtenção e a aproximação para distribuições assintóticas conhecidas, como a Qui-Quadrado.
5. **Resposta:** A derivada resulta em $`\frac{\phi}{V(\mu_i)}(y_i - \mu_i)`$, cuja igualdade a zero resulta no estimador $`\hat{\mu}_i = y_i`$. A segunda derivada ser $`-\frac{\phi}{V(\mu_i)} < 0`$ confirma que $`\hat{\mu}_i = y_i`$ é o ponto de máximo único da log-verossimilhança.
6. **Resposta:** A relação é dada por $`D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = \phi \cdot d(\mathbf{y}, \hat{\boldsymbol{\mu}})`$. A *deviance* $`d(\mathbf{y}, \hat{\boldsymbol{\mu}})`$ representa a parte da função desvio puramente vinculada às observações e médias ajustadas, com o parâmetro de dispersão $`\phi`$ fatorado.
7. **Resposta:** Sob $`H_0`$ (modelo proposto adequado), rejeita-se a hipótese nula se o valor de $`D^*`$ observado for superior ao quantil crítico da distribuição Qui-Quadrado com $`n - p`$ graus de liberdade ($`\chi^2_{n-p}`$) para um nível $`\alpha`$. Um desvio elevado indica que o modelo proposto perde muita informação em comparação ao modelo saturado.
8. **Resposta:** No modelo Normal, a *deviance* simplifica-se para a soma dos quadrados dos resíduos: $`d(\mathbf{y}, \hat{\boldsymbol{\mu}}) = \sum_{i=1}^{n} (y_i - \hat{\mu}_i)^2`$. A função desvio completa é obtida dividindo essa soma pela variância $`\sigma^2`$.
9. **Resposta:** Para o modelo Poisson, a função desvio é dada por $`D^*(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^{n} \left[ y_i \log\left(\frac{y_i}{\hat{\mu}_i}\right) + \hat{\mu}_i - y_i \right ]`$.
10. **Resposta:** A média estimada é obtida aplicando-se a inversa da função de ligação logito (canônica) ao preditor linear. A fórmula final é $`\hat{\mu}_i = \frac{1}{1 + \exp(-\mathbf{x}_i^T \hat{\boldsymbol{\beta}})}`$, que representa a probabilidade estimada de sucesso para a observação $`i`$.

#### Questões para Ensaio / Dissertativas

1. Discuta detalhadamente as diferenças filosóficas e metodológicas entre a modelagem focada em inferência/ajuste estatístico e a modelagem preditiva. Explique por que a divisão de dados em treino/teste não é a abordagem central na disciplina de MLGs.
2. Demonstre passo a passo a derivação da componente da log-verossimilhança para a família exponencial, provando formalmente por que o ponto de máximo no modelo saturado ocorre quando $`\hat{\mu}_i = y_i`$.
3. Explique o conceito da função desvio ($`Deviance`$) e como a aproximação pela distribuição Qui-Quadrado com $`n - p`$ graus de liberdade é utilizada no teste de hipóteses para validação do ajuste de um MLG.
4. Compare detalhadamente as expressões da função desvio obtidas para a distribuição Normal e para a distribuição Poisson, destacando o impacto do parâmetro de dispersão $`\phi`$ em cada uma delas.
5. Descreva a estrutura teórica do método de Mínimos Quadrados Reponderados Iterativamente (IWLS). Explique a diferença entre a matriz Hessiana usada no algoritmo de Newton-Raphson e a Matriz de Informação Esperada usada no algoritmo Score de Fisher.

#### Glossário de Termos-Chave

* **Ajuste Estatístico:** Avaliação da adequação do modelo aos dados observados através de medidas de verossimilhança e desvio, com foco principal na interpretação dos parâmetros e na estrutura inferencial do modelo.
* **Desempenho Preditivo:** Abordagem de modelagem focada na capacidade do modelo de prever novas observações com precisão, utilizando métricas de erro (como EQM e EAM) e técnicas de validação (como divisão treino/teste).
* **Modelo Saturado:** Modelo conceitual ideal com o número de parâmetros igual ao número de observações ($`n`$), em que cada média estimada é igual ao próprio valor observado ($`\hat{\mu}_i = y_i`$). Fornece o teto absoluto da log-verossimilhança.
* **Modelo Proposto (ou Reduzido):** O modelo simplificado e parcimonioso em estudo, que utiliza $`p`$ parâmetros ($`\beta_0, \beta_1, \dots, \beta_{p-1}`$, com $`p < n`$) associados a covariáveis para explicar a resposta.
* **Função Desvio ($`D^*`$):** Estatística baseada em duas vezes a diferença entre a log-verossimilhança do modelo saturado e a do modelo proposto. Mede o montante de informação perdido ao se adotar o modelo reduzido.
* *Deviance* ($`d`$): Expressão simplificada do desvio onde o parâmetro de dispersão $`\phi`$ é isolado/fatorado ($`D^* = \phi \cdot d`$). É a métrica usualmente fornecida por pacotes estatísticos.
* **Mínimos Quadrados Reponderados Iterativamente (IWLS):** Algoritmo iterativo empregado na estimação de parâmetros em MLGs, que atualiza repetidamente os vetores de coeficientes e matrizes de pesos até a convergência.
* **Função Score $`U(\boldsymbol{\beta})`$:** Vetor gradiente das primeiras derivadas da função de log-verossimilhança em relação aos parâmetros do modelo.
* **Matriz de Informação de Fisher:** A esperança do valor negativo da matriz de segundas derivadas (Hessiana) da log-verossimilhança. Utilizada no método Score de Fisher para garantir convergência estável.
* **Função de Ligação Canônica:** Função que relaciona diretamente o parâmetro canônico da família exponencial ($`\theta_i`$) ao preditor linear ($`\eta_i = \mathbf{x}_i^T \boldsymbol{\beta}`$).
* **Parâmetro de Dispersão ($`\phi`$):** Componente da família exponencial associado à escala ou variância dos dados (por exemplo, $`\phi = 1/\sigma^2`$ na distribuição Normal e $`\phi = 1`$ na Poisson).
