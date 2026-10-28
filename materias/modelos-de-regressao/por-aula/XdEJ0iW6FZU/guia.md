> **Aula:** [EST5507 Modelos de Regressão 2026 10 23 Função desvio em MLGs e exemplos de proporção e contagem](https://www.youtube.com/watch?v=XdEJ0iW6FZU)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudo: Função Desvio e Modelos Lineares Generalizados (MLGs) para Proporção e Contagem

#### 1. Objetivos da Aula

* **Compreender o conceito e a formulação da Função Desvio (** ***Deviance*** **)** : Entender a função desvio como uma medida de qualidade do ajuste em Modelos Lineares Generalizados (MLGs), derivada da diferença de log-verossimilhanças entre o modelo saturado e o modelo ajustado.
* **Analisar as propriedades amostrais do desvio** : Estudar o comportamento da distribuição do desvio sob diferentes suposições de variância (conhecida versus estimada pelo quadrado médio do resíduo) na distribuição normal e em distribuições discretas (Bernoulli e Poisson).
* **Avaliar a significância do modelo via Teste de Razão de Verossimilhanças** : Comparar as discrepâncias entre o modelo nulo, o modelo ajustado e o modelo saturado por meio da redução do desvio e testes de hipóteses associados à distribuição $`\chi^2`$.
* **Realizar diagnósticos de ajuste e de resíduos** : Utilizar os resíduos componentes do desvio, pontos de alavanca ( *leverage* ), distância de Cook e gráficos de envelopes simulados para identificar pontos influentes e desalinhamentos do modelo.
* **Aplicar MLGs em dados de proporção e contagem** :
* **Proporção/Binomial** : Ajustar modelos de regressão logística, interpretar coeficientes por meio da Razão de Chances ( *Odds Ratio* ) e explorar funções de ligação alternativas.
* **Contagem** : Ajustar modelos de Poisson e Binomial Negativa, identificando a presença de sobredispersão nos dados e aplicando o modelo adequado.

#### 2. Desenvolvimento Passo a Passo e Fórmulas

##### 2.1 Conceito Geral da Função Desvio ( *Deviance* )

A função desvio é utilizada em MLGs como uma medida da qualidade do ajuste do modelo aos dados observados, funcionando como uma generalização da soma de quadrados dos resíduos da regressão linear clássica.
Ela é definida formalmente como duas vezes a diferença entre a log-verossimilhança do **modelo saturado** ($`l(\tilde{\mu}; y)`$) e a log-verossimilhança do **modelo ajustado** ($`l(\hat{\mu}; y)`$):
```math
D(y; \hat{\mu}) = 2 \left[ l(\tilde{\mu}; y) - l(\hat{\mu}; y) \right ]
```
Onde:

* **Modelo Saturado** : É um modelo superparametrizado no qual se estima um parâmetro $`\mu_i`$ para cada observação $`y_i`$ ($`n`$ parâmetros para $`n`$ observações), obtendo um ajuste perfeito onde $`\tilde{\mu}_i = y_i`$.
* **Modelo Ajustado** : É o modelo parcimonioso proposto, no qual os $`n`$ valores de $`\mu_i`$ são estimados a partir de $`p`$ coeficientes $`\beta`$ ($`\hat{\mu}_i = g^{-1}(x_i^T \beta)`$), com $`p < n`$.

Se o valor do desvio for elevado, a log-verossimilhança do modelo ajustado está distante da do modelo saturado, o que indica que o modelo proposto não retém a informação necessária e deve ser rejeitado.

##### 2.2 Função Desvio no Modelo Normal

Suponha observações independentes $`Y_1, \dots, Y_n`$ tais que $`Y_i \sim N(\mu_i, \sigma^2)`$.

###### *Caso 1: Variância $`\sigma^2`$ Conhecida*

A log-verossimilhança do modelo saturado estimando $`\mu_i = y_i`$ e do modelo ajustado estimando $`\hat{\mu}_i`$ resulta na seguinte expressão para o desvio não escalonado (soma de quadrados dos resíduos):
```math
D = \sum_{i=1}^n (y_i - \hat{\mu}_i)^2
```
O **desvio escalonado** é dado por $`D^* = \frac{D}{\sigma^2}`$. Quando $`\sigma^2`$ é conhecido e a função de ligação é a canônica (identidade), a distribuição exata do desvio escalonado é uma qui-quadrado com $`n-p`$ graus de liberdade:
```math
\frac{D}{\sigma^2} \sim \chi^2_{n-p}
```

* **Valor Esperado** : $`ED = (n-p)\sigma^2`$
* **Variância** : $`Var(D) = 2(n-p)\sigma^4`$

###### *Caso 2: Variância $`\sigma^2`$ Desconhecida e Estimada*

Se $`\sigma^2`$ for estimado pelo quadrado médio do resíduo (MQRes):
```math
\hat{\sigma}^2 = \frac{\sum_{i=1}^n (y_i - \hat{\mu}_i)^2}{n-p} = \frac{D}{n-p}
```
Ao substituir essa estimativa na expressão do desvio escalonado, obtém-se:
```math
\frac{D}{\hat{\sigma}^2} = \frac{D}{\frac{D}{n-p}} = n - p
```
A quantidade resulta em uma constante fixa ($`n-p`$), deixando de seguir uma distribuição de probabilidade aleatória $`\chi^2`$. Se for utilizado o estimador de máxima verossemilhança ($`\hat{\sigma}_{ML}^2 = D/n`$), a razão também resulta na constante $`n`$.
Apesar dessa limitação teórica quando $`\sigma^2`$ é estimado, autores de referência na área (como Gilberto Paula e McCullagh & Nelder) recomendam a comparação do desvio observado com os quantis da distribuição $`\chi^2`$ para a tomada de decisão sobre a adequação do modelo.

##### 2.3 Função Desvio para as Distribuições Bernoulli e Poisson

###### *Distribuição Bernoulli*

Para $`Y_i \sim \text{Bernoulli}(\mu_i)`$, a log-verossimilhança é:
```math
l(\mu; y) = \sum_{i=1}^n \left[ y_i \ln(\mu_i) + (1 - y_i) \ln(1 - \mu_i) \right ]
```
No modelo saturado, definindo $`\tilde{\mu}_i = y_i`$ (com $`y_i \in \{0, 1\}`$), adota-se a convenção de que $`0 \ln(0) = 0`$. Com isso, $`l(\tilde{\mu}; y) = 0`$.
A expressão da **função desvio na Bernoulli** simplifica-se para:
```math
D(y; \hat{\mu}) = 2 \sum_{i=1}^n \left[ y_i \ln\left(\frac{y_i}{\hat{\mu}_i}\right) + (1 - y_i) \ln\left(\frac{1 - y_i}{1 - \hat{\mu}_i}\right) \right ]
```
Onde o valor ajustado $`\hat{\mu}_i`$ para a ligação logito é obtido invertendo a função de ligação via expressão *expit* :
```math
\hat{\mu}_i = \frac{1}{1 + e^{-x_i^T \hat{\beta}}}
```

###### *Distribuição de Poisson*

Para observações de contagem $`Y_i \sim \text{Poisson}(\mu_i)`$, substituindo o estimador saturado $`\tilde{\mu}_i = y_i`$ e o ajustado $`\hat{\mu}_i = e^{x_i^T \hat{\beta}}`$, a **função desvio na Poisson** é dada por:
```math
D(y; \hat{\mu}) = 2 \sum_{i=1}^n \left[ y_i \ln\left(\frac{y_i}{\hat{\mu}_i}\right) - (y_i - \hat{\mu}_i) \right ]
```

##### 2.4 Comparação entre Modelo Nulo, Modelo Ajustado e Modelo Saturado

Para testar a hipótese de que o conjunto de preditoras incluído no modelo traz ganho de informação significativo, comparam-se três modelos aninhados:

1. **Modelo Saturado** : Contém $`n`$ parâmetros (log-verossimilhança $`l_{sat}`$).
2. **Modelo Ajustado** : Contém $`p`$ parâmetros/coeficientes $`\beta`$ (log-verossimilhança $`l_{ajust}`$).
3. **Modelo Nulo** : Contém apenas o parâmetro de intercepto ($`1`$ parâmetro, log-verossimilhança $`l_{nulo}`$).

Modelo,Número de Parâmetros,Log-Verossimilhança,Desvio Calculado
Saturado,$`n`$,Maior ($`l_{sat}`$),$`D_{sat} = 0`$
Ajustado,$`p`$,Intermediária ($`l_{ajust}`$),$`D_{ajust} = 2(l_{sat} - l_{ajust})`$
Nulo,$`1`$,Menor ($`l_{nulo}`$),$`D_{nulo} = 2(l_{sat} - l_{nulo})`$
A **Redução no Desvio** (obtida via Teste de Razão de Verossimilhanças) reflete o ganho do modelo ajustado sobre o modelo nulo:
```math
\Delta D = D_{nulo} - D_{ajust} = 2 \left[ l_{ajust} - l_{nulo} \right ]
```
Sob a hipótese nula de que os coeficientes associados às preditoras são nulos ($`H_0: \beta_1 = \dots = \beta_{p-1} = 0`$), essa diferença segue assintoticamente uma distribuição qui-quadrado:
```math
\Delta D \sim \chi^2_{(n-1) - (n-p)} \implies \Delta D \sim \chi^2_{p-1}
```
Se o p-valor associado a $`\Delta D`$ for pequeno (ex.: $`p < 0{,}05`$), rejeita-se o modelo nulo em favor do modelo ajustado.

##### 2.5 Resíduos da Componente do Desvio e Diagnóstico

O desvio total é a soma das contribuições individuais de cada observação: $`D = \sum_{i=1}^n d_i^2`$.
O **resíduo componente do desvio** ($`r_{D,i}`$) é definido extraindo a raiz quadrada da $`i`$ -ésima componente $`d_i^2`$ e atribuindo o sinal da diferença entre o valor observado $`y_i`$ e o valor ajustado $`\hat{\mu}_i`$:
```math
r_{D,i} = \text{sinal}(y_i - \hat{\mu}_i) \sqrt{d_i^2}
```

* **Função Sinal** : Garante que se o modelo subestimou a observação ($`y_i > \hat{\mu}_i`$), o resíduo seja positivo; se superestimou ($`y_i < \hat{\mu}_i`$), o resíduo seja negativo.

###### *Ferramentas Complementares de Diagnóstico*

* **Pontos de Alavanca (** ***Leverage*** **- $`h_i`$)** : Medem o afastamento das preditoras da observação $`i`$ em relação ao centro do espaço das covariáveis (obtido da matriz hat/pesos via get_matrix()).
* **Distância de Cook** : Identifica pontos influentes que alteram individualmente as estimativas do vetor de parâmetros $`\hat{\beta}`$.
* **Envelope Simulado** : Banda de confiança construída por simulação para verificar se os resíduos ordenados se mantêm dentro dos limites esperados para a distribuição teórica assumida.

##### 2.6 Exemplo Prático 1: MLG para Proporção/Binomial (Regressão Logística e Razão de Chances)

Em um banco de dados bancário simulado ($`n=500`$) voltado à predição de inadimplência (inadimplente $`\in \{0, 1\}`$), ajustou-se um MLG binomial com ligação logito ($`g(\mu) = \ln(\frac{\mu}{1-\mu})`$).

###### *Funções de Ligação Suportadas na Família Binomial*

Além da ligação canônica **Logito** , a biblioteca estatística permite ligações como **Probito** , **Cauchy** e **Complemento Log-Log (cloglog)** .

###### *Interpretação dos Coeficientes e Razão de Chances ( Odds Ratio )*

Para uma variável quantitativa contínua como salario, suponha um coeficiente estimado $`\hat{\beta}_{salario} = -0{,}0011`$.
A **Razão de Chances (** ***Odds Ratio*** **- OR)** é calculada pela exponencial do coeficiente:
```math
\text{OR} = e^{\hat{\beta}_{salario}} = e^{-0{,}0011} \approx 0{,}9989
```
**Interpretação** : A cada incremento de $`1`$ unidade monetária no salário do cliente, a chance de inadimplência diminui em aproximadamente $`0{,}11\%`$ ($`1 - 0{,}9989 = 0{,}0011`$), mantendo as demais variáveis constantes.

##### 2.7 Exemplo Prático 2: MLG para Dados de Contagem (Poisson vs. Binomial Negativa)

Em uma aplicação de entregas com $`n=100`$ pedidos, busca-se modelar o numero_de_produtos (variável de contagem $`Y \ge 0`$) em função de preditoras como categoria de produto, dia da semana, canal de venda e presença de promoção.

###### *Modelo de Poisson e o Problema da Sobredispersão*

A distribuição de Poisson impõe a restrição de equidispersão:
```math
EY = Var(Y) = \mu
```
Ao ajustar o modelo de Poisson e construir o gráfico de **envelope simulado** para os resíduos componentes do desvio, observou-se que diversos pontos extrapolaram as bandas de confiança teóricas. Além disso, a razão entre o desvio do modelo e os graus de liberdade do resíduo ($`\frac{D}{n-p}`$) apresentou valor superior a $`1`$. Esse padrão indica a presença de **sobredispersão** ($`Var(Y) > EY`$).

###### *Solução via Modelo Binomial Negativo*

Para acomodar a variância superior à média, ajustou-se um modelo da **Binomial Negativa** , que adiciona um parâmetro de dispersão $`\alpha`$:
```math
Var(Y) = \mu + \alpha \mu^2
```
O gráfico de envelope simulado do modelo Binomial Negativo manteve os resíduos dentro dos limites das bandas de confiança, confirmando a adequação superior dessa distribuição para os dados de contagem sobredispersos.

#### 3. Questionário de Fixação (Quiz)

1. Como é definida formalmente a função desvio ( *deviance* ) em Modelos Lineares Generalizados e qual é o seu objetivo?
2. O que caracteriza o modelo saturado e qual é o número de parâmetros estimados por ele em relação ao tamanho da amostra $`n`$ ?
3. Sob quais condições o desvio escalonado em um modelo normal apresenta distribuição exata $`\chi^2`$ ?
4. O que ocorre com a quantidade $`D/\hat{\sigma}^2`$ quando a variância $`\sigma^2`$ é estimada pelo quadrado médio dos resíduos (MQRes)?
5. Por que a log-verossimilhança do modelo saturado para a distribuição Bernoulli é igual a zero?
6. Qual é a expressão matemática da função desvio para a distribuição de Poisson?
7. O que representa a métrica de "redução do desvio" ($`\Delta D`$) e qual teste estatístico é utilizado para avaliar sua significância?
8. Como é calculado o resíduo componente do desvio e qual a função da aplicação da função sinal em sua fórmula?
9. Como se obtém e qual é a interpretação da Razão de Chances ( *Odds Ratio* ) a partir do coeficiente estimado $`\hat{\beta}`$ em uma regressão logística?
10. Como o gráfico de envelope simulado dos resíduos auxilia no diagnóstico de sobredispersão em modelos de contagem de Poisson?

#### 4. Chave de Respostas (Gabarito do Quiz)

1. **Resposta** : A função desvio é definida como duas vezes a diferença entre a log-verossimilhança do modelo saturado e a do modelo ajustado, $`D = 2l(\tilde{\mu}; y) - l(\hat{\mu}; y)`$. Seu objetivo é fornecer uma medida global da qualidade do ajuste do modelo, avaliando o quanto o modelo proposto se afasta de um ajuste perfeito.
2. **Resposta** : O modelo saturado é um modelo superparametrizado no qual se assume uma média individual para cada observação ($`\tilde{\mu}_i = y_i`$). Por essa razão, ele estima exatamente $`n`$ parâmetros para um conjunto de dados de tamanho $`n`$.
3. **Resposta** : O desvio escalonado $`D/\sigma^2`$ segue uma distribuição qui-quadrado exata com $`n-p`$ graus de liberdade ($`\chi^2_{n-p}`$) quando os dados possuem distribuição normal, a variância $`\sigma^2`$ é assumida conhecida e fixa, e é utilizada a função de ligação canônica (identidade).
4. **Resposta** : Quando $`\sigma^2`$ é estimado pelo quadrado médio dos resíduos, $`s^2 = D/(n-p)`$, a razão $`D/\hat{\sigma}^2`$ resulta na constante fixa $`n-p`$. Consequentemente, essa quantidade deixa de ser uma variável aleatória com distribuição de probabilidade $`\chi^2`$.
5. **Resposta** : Para a distribuição Bernoulli, a log-verossimilhança do modelo saturado substitui $`\mu_i`$ por $`y_i \in \{0, 1\}`$. Pela convenção de que $`0 \ln(0) = 0`$, os termos $`y_i \ln(y_i)`$ e $`(1-y_i)\ln(1-y_i)`$ anulam-se para todas as observações, resultando em $`l(\tilde{\mu}; y) = 0`$.
6. **Resposta** : A função desvio para a distribuição de Poisson é dada pela fórmula $`D(y; \hat{\mu}) = 2 \sum_{i=1}^n \left[ y_i \ln\left(\frac{y_i}{\hat{\mu}_i}\right) - (y_i - \hat{\mu}_i) \right ]`$, onde $`\hat{\mu}_i`$ representa o valor ajustado do número de contagens.
7. **Resposta** : A redução do desvio ($`\Delta D = D_{nulo} - D_{ajust}`$) mede o ganho de informação e a melhoria de ajuste obtidos ao incluir as covariáveis do modelo ajustado em relação ao modelo nulo. Sua significância é avaliada pelo Teste de Razão de Verossimilhanças, comparando $`\Delta D`$ com a distribuição $`\chi^2_{p-1}`$.
8. **Resposta** : O resíduo componente do desvio é calculado por $`r_{D,i} = \text{sinal}(y_i - \hat{\mu}_i) \sqrt{d_i^2}`$, onde $`d_i^2`$ é a contribuição da observação $`i`$ para o desvio total. A função sinal preserva a informação de sobre-estimativa ou sub-estimativa do valor observado pelo modelo.
9. **Resposta** : A Razão de Chances é obtida calculando a exponencial do coeficiente estimado, $`\text{OR} = e^{\hat{\beta}}`$. Ela indica como a chance da ocorrência do evento de interesse ($`Y=1`$) é multiplicada a cada aumento de uma unidade na variável explicativa correspondente.
10. **Resposta** : O gráfico de envelope simulado exibe bandas de confiança para os resíduos ordenados sob a suposição do modelo. Se os resíduos do modelo Poisson extrapolarem os limites dessas bandas, fica evidenciada a sobredispersão, indicando a necessidade de uma distribuição com maior flexibilidade de variância, como a Binomial Negativa.

#### 5. Questões para Desenvolvimento (Discursivas)

1. Discuta as implicações teóricas e práticas de utilizar o quantil da distribuição $`\chi^2`$ para testar a adequação de um MLG quando o parâmetro de dispersão precisa ser estimado, contrastando com o caso em que a variância é conhecida.
2. Deduza matematicamente a expressão da função desvio para a distribuição de Poisson a partir da diferença entre as log-verossimilhanças do modelo saturado e do modelo ajustado.
3. Explique detalhadamente o procedimento de análise de diagnóstico em MLGs via envelope simulado para resíduos do desvio. Como o gráfico de envelope auxilia no reconhecimento de falhas de especificação do modelo, como a sobredispersão?
4. No contexto da regressão logística para concessão de crédito, conceitue Razão de Chances ( *Odds Ratio* ). Demonstre como o parâmetro $`\hat{\beta}`$ se transforma em razão de chances e interprete o impacto prático de um coeficiente negativo associado ao salário do cliente.
5. Compare a modelagem de dados de contagem via distribuição Poisson e Binomial Negativa. Em quais situações a suposição de equidispersão da Poisson é violada em dados reais e de que forma a distribuição Binomial Negativa acomoda essa variabilidade adicional?

#### 6. Glossário Exaustivo de Termos-Chave

* **Desvio Escalonado (** ***Scaled Deviance*** **)** : Métrica dada pela divisão da função desvio pelo parâmetro de dispersão ou variância ($`\frac{D}{\sigma^2}`$). Em modelos normais com variância conhecida, possui distribuição qui-quadrado.
* **Distância de Cook** : Medida de diagnóstico que avalia o impacto global da remoção de uma observação específica sobre o conjunto de coeficientes estimados do modelo.
* **Envelope Simulado** : Técnica gráfica de diagnóstico baseada em simulações de Monte Carlo que cria bandas de confiança em torno dos resíduos ordenados para checar a adequação do modelo postulado.
* **Equidispersão** : Propriedade intrínseca da distribuição de Poisson na qual a média teórica é exatamente igual à sua variância ($`EY = Var(Y) = \mu`$).
* **Função Desvio (** ***Deviance*** **)** : Estatística de qualidade de ajuste calculada como $`2l(\text{saturado}) - l(\text{ajustado})`$. Quantifica a perda de informação do modelo ajustado em relação a um modelo perfeito.
* **Função Expit** : Inversa da função de ligação logito, definida por $`\text{expit}(x) = \frac{1}{1 + e^{-x}}`$. É utilizada para converter o preditor linear em probabilidade no intervalo $`(0, 1)`$.
* **Função de Ligação Canônica** : Função $`g(\mu)`$ que conecta a média $`\mu`$ do modelo ao preditor linear $`\eta = X\beta`$, derivada diretamente da forma natural da família exponencial da distribuição (ex.: ligação identidade para Normal, logito para Bernoulli, logaritmo para Poisson).
* **Modelo Ajustado** : Modelo parcimonioso proposto pelo pesquisador que utiliza $`p`$ parâmetros (coeficientes $`\beta`$) associados a covariáveis para explicar a resposta de $`n`$ observações, com $`p < n`$.
* **Modelo Nulo** : Modelo de referência simplificado que não contém variáveis explicativas, apenas o parâmetro de intercepto (1 parâmetro), servindo como base para testes de ganho de ajuste.
* **Modelo Saturado** : Modelo baseline hipotético superparametrizado que contém $`n`$ parâmetros para $`n`$ observações, ajustando-se perfeitamente aos dados observados ($`\tilde{\mu}_i = y_i`$).
* **Pontos de Alavanca (** ***Leverage*** **)** : Observações que possuem valores extremos no espaço das variáveis explicativas (avaliados pelos elementos da diagonal da matriz hat/pesos $`h_{ii}`$), com potencial desproporcional de atrair a reta/superfície de ajuste.
* **Razão de Chances (** ***Odds Ratio*** **)** : Razão entre a chance de ocorrência de um evento em um grupo e a chance do mesmo evento em outro grupo. Em regressão logística com ligação logito, é calculada por $`e^{\hat{\beta}}`$.
* **Resíduo Componente do Desvio** : Valor assinalado referente à contribuição individual da observação $`i`$ para o desvio total, calculado por $`r_{D,i} = \text{sinal}(y_i - \hat{\mu}_i) \sqrt{d_i^2}`$.
* **Sobredispersão (** ***Overdispersion*** **)** : Fenômeno em dados de contagem em que a variância observada é superior à média teórica estimada ($`Var(Y) > EY`$), violando as suposições da distribuição de Poisson.
* **Teste de Razão de Verossimilhanças** : Teste estatístico baseado na diferença entre os logaritmos das verossimilhanças de dois modelos aninhados ($`\Delta D = 2l_{ajust} - l_{nulo}`$), comparado a uma distribuição $`\chi^2`$.
