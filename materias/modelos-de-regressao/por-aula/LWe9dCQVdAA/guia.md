> **Aula:** [EST5507 Modelos de Regressão 2026 10 13 Modelos Lineares Generalizados - Condições de regularidade](https://www.youtube.com/watch?v=LWe9dCQVdAA)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudos: Modelos Lineares Generalizados e Condições de Regularidade

Este guia de estudos detalhado foi elaborado para a fixação de conceitos avançados da disciplina de pós-graduação em Modelos de Regressão, abordando os fundamentos da Família Exponencial de Distribuições, a definição formal dos Modelos Lineares Generalizados (MLGs), as condições de regularidade da função de verossimilhança e o estudo de funções de ligação.

#### Objetivos de Aprendizagem

1. **Definir a Família Exponencial de Distribuições** e identificar seus componentes: o parâmetro natural ou canônico ($`\theta`$), o parâmetro de dispersão ($`\phi`$), a função de interesse $`b(\theta)`$ e o termo de suporte $`c(y, \phi)`$.
2. **Aplicar as Condições de Regularidade** da função de verossimilhança para derivar rigorosamente a esperança matemática $`EY`$ e a variância $`\text{Var}(Y)`$ de qualquer distribuição pertencente à família exponencial.
3. **Estruturar os Modelos Lineares Generalizados (MLGs)** compreendendo a função dos componentes aleatório e sistemático, os pressupostos sobre as covariáveis e a finalidade da função de ligação $`g(\mu)`$.
4. **Deduzir as Ligações Canônicas** e expressar distribuições clássicas (Poisson e Binomial) na forma da família exponencial.
5. **Avaliar Implicações Práticas e Computacionais** , abordando o tratamento de dados desbalanceados, escolhas de funções de ligação alternativas e implementação no pacote statsmodels de Python.

#### Desenvolvimento Passo a Passo e Formulação Matemática

##### 1. A Família Exponencial de Distribuições

Uma variável aleatória $`Y`$ pertence à família exponencial de distribuições se sua função de densidade de probabilidade (fdp) ou função massa de probabilidade (fmp) puder ser parametrizada e escrita na seguinte forma geral:
```math
f(y; \theta, \phi) = \exp \left\{ \phi \left[ y\theta - b(\theta) \right ] + c(y, \phi) \right\}
```
Em que:

* $`\theta`$ é o **parâmetro natural** (ou canônico) de interesse, situado em um espaço paramétrico $`\Theta`$.
* $`\phi`$ é o **parâmetro de dispersão** , satisfazendo a restrição $`\phi > 0`$.
* $`b(\cdot)`$ e $`c(\cdot, \cdot)`$ são funções diferenciáveis. A função $`b(\theta)`$ desempenha papel central no cálculo dos momentos de $`Y`$.

##### 2. Condições de Regularidade e Derivação dos Momentos

A estimativa e as propriedades dos MLGs apoiam-se nas condições de regularidade aplicadas à função de log-verossimilhança. Para uma observação individual $`y_i`$, a contribuição para a log-verossimilhança $`L_i(\theta, \phi) = \log f(y_i; \theta, \phi)`$ é dada por:
```math
L_i(\theta, \phi) = \phi \left[ y_i \theta - b(\theta) \right ] + c(y_i, \phi)
```

###### *Primeira Condição de Regularidade (Esperança do Escore)*

A função ecore ( *score function* ) é a derivada primeira da log-verossimilhança em relação ao parâmetro $`\theta`$. A primeira condição de regularidade estabelece que o valor esperado da função escore é nulo:
```math
E \left [\frac{d L_i}{d\theta} \right ] = 0
```
Derivando $`L_i(\theta, \phi)`$ em relação a $`\theta`$:
```math
\frac{d L_i}{d\theta} = \phi \left[ y_i - b'(\theta) \right ]
```
Aplicando a esperança matemática:
```math
E \left\{ \phi \left[ Y_i - b'(\theta) \right ] \right\} = 0 \implies \phi \left\{ EY_i - b'(\theta) \right\} = 0
```
Como $`\phi > 0`$, conclui-se que o valor esperado (média) de $`Y_i`$ é a primeira derivada da função $`b(\theta)`$:
```math
EY_i = \mu_i = b'(\theta)
```

###### *Segunda Condição de Regularidade (Informação de Fisher)*

A segunda condição de regularidade relaciona o valor esperado da derivada segunda da log-verossimilhança com a esperança do quadrado do escore:
```math
E \left [\frac{d^2 L_i}{d\theta^2} \right ] = - E \left [\left( \frac{d L_i}{d\theta} \right)^2 \right ]
```
Calculando a derivada segunda de $`L_i(\theta, \phi)`$ em relação a $`\theta`$:
```math
\frac{d^2 L_i}{d\theta^2} = -\phi b''(\theta)
```
Aplicando o valor esperado no lado esquerdo da igualdade:
```math
E \left [\frac{d^2 L_i}{d\theta^2} \right ] = -\phi b''(\theta)
```
Para o lado direito da igualdade, utilizando a expressão do escore e sabendo que $`b'(\theta) = EY_i`$:
```math
- E \left [\left( \frac{d L_i}{d\theta} \right)^2 \right ] = - E \left\{ \left( \phi \left[ Y_i - b'(\theta) \right ] \right)^2 \right\} = -\phi^2 E \left[ (Y_i - EY_i)^2 \right] = -\phi^2 \text{Var}(Y_i)
```
Igualando as duas expressões sob a condição de regularidade:
```math
-\phi b''(\theta) = -\phi^2 \text{Var}(Y_i) \implies \text{Var}(Y_i) = \frac{b''(\theta)}{\phi}
```
Definindo a **função de variância** $`V(\mu) = b''(\theta) = \frac{d\mu}{d\theta}`$, a variância de $`Y_i`$ reescreve-se como:
```math
\text{Var}(Y_i) = \phi^{-1} V(\mu_i)
```

##### 3. Definição e Estrutura dos Modelos Lineares Generalizados (MLGs)

Um MLG estende o modelo linear clássico ao permitir respostas cuja distribuição pertença à família exponencial e ao associar a média da resposta ao preditor linear por meio de uma função não linear.

###### *Componentes Formais de um MLG:*

* **Componente Aleatório** : As variáveis resposta $`Y_1, \dots, Y_n`$ são independentes, com cada $`Y_i`$ distribuído segundo a família exponencial com média $`\mu_i`$ e parâmetro de dispersão $`\phi`$.
* *Nota* : As variáveis são independentes, mas **não identicamente distribuídas** , pois cada $`Y_i`$ possui sua própria média $`\mu_i`$ vinculada às covariáveis da $`i`$ -ésima unidade experimental.
* **Componente Sistemático (Preditor Linear)** : O preditor linear $`\eta_i`$ é formado pela combinação linear de $`p`$ covariáveis conhecidas e não aleatórias:

```math
\eta_i = \beta_0 + \beta_1 x_{1i} + \dots + \beta_p x_{pi} = X_i^T \beta
```
Em que $`X_i = (1, x_{1i}, \dots, x_{pi})^T`$ é o vetor de covariáveis da $`i`$ -ésima unidade e $`\beta = (\beta_0, \beta_1, \dots, \beta_p)^T`$ é o vetor de coeficientes desconhecidos a serem estimados. 3. **Função de Ligação** : A função $`g(\cdot)`$ é uma função monótona e diferenciável que conecta o valor esperado $`\mu_i`$ ao preditor linear $`\eta_i`$:
```math
\eta_i = g(\mu_i)
```

###### *Função de Ligação Canônica*

A ligação canônica ocorre quando o parâmetro natural $`\theta_i`$ da distribuição equivale diretamente ao preditor linear $`\eta_i`$:
```math
\theta_i = \eta_i = g(\mu_i)
```

##### 4. Estudo de Casos Práticos: Poisson e Binomial

###### *Exemplo 1: Distribuição de Poisson*

Seja $`Y \sim \text{Poisson}(\mu)`$, com função massa de probabilidade dada por:
```math
f(y; \mu) = \frac{e^{-\mu} \mu^y}{y!}, \quad y = 0, 1, 2, \dots
```
Para reescrever $`f(y; \mu)`$ no formato da família exponencial, aplica-se a exponencial e o logaritmo:
```math
f(y; \mu) = \exp \left\{ \log \left( \frac{e^{-\mu} \mu^y}{y!} \right) \right\} = \exp \left\{ y \log(\mu) - \mu - \log(y!) \right\}
```
Ajustando os termos à forma $`\exp\{\phi y\theta - b(\theta) + c(y, \phi)\}`$:
```math
f(y; \mu) = \exp \left\{ 1 \cdot \left[ y \log(\mu) - \mu \right ] - \log(y!) \right\}
```

* **Identificação dos Parâmetros** :
* $`\phi = 1`$
* $`\theta = \log(\mu) \implies \mu = e^\theta`$
* $`b(\theta) = \mu = e^\theta`$
* $`c(y, \phi) = -\log(y!)`$
* **Verificação dos Momentos** :
* $`EY = b'(\theta) = \frac{d}{d\theta}(e^\theta) = e^\theta = \mu`$
* $`b''(\theta) = \frac{d^2}{d\theta^2}(e^\theta) = e^\theta = \mu \implies V(\mu) = \mu`$
* $`\text{Var}(Y) = \frac{V(\mu)}{\phi} = \frac{\mu}{1} = \mu`$
* **Ligação Canônica** : Definindo $`\theta = \eta`$, obtemos $`\log(\mu) = \eta`$. Portanto, a ligação canônica para o modelo Poisson é a **função logarítmica** ($`\text{modelo log}`$).

###### *Exemplo 2: Distribuição Binomial (Modelagem de Proporções)*

Considere $`Y^*`$ como a proporção de sucessos $`Y^* = \frac{Y}{n}`$ em $`n`$ ensaios independentes de Bernoulli com probabilidade de sucesso $`\mu`$. A fmp de $`N Y^* = Y`$ é:
```math
P(Y^* = y^*) = \binom{n}{n y^*} \mu^{n y^*} (1-\mu)^{n (1-y^*)}
```
Aplicando a transformação exponencial do logaritmo:
```math
P(Y^* = y^*) = \exp \left\{ \log \binom{n}{n y^*} + n y^* \log(\mu) + (n - n y^*) \log(1-\mu) \right\}
```
Agrupando os termos que contêm $`n y^*`$:
```math
P(Y^* = y^*) = \exp \left\{ n y^* \left [\log(\mu) - \log(1-\mu) \right ] + n \log(1-\mu) + \log \binom{n}{n y^*} \right\}
```
```math
P(Y^* = y^*) = \exp \left\{ n \left[ y^* \log\left( \frac{\mu}{1-\mu} \right) - \left(-\log(1-\mu)\right) \right ] + \log \binom{n}{n y^*} \right\}
```

* **Identificação dos Parâmetros** :
* $`\phi = n`$
* $`\theta = \log\left( \frac{\mu}{1-\mu} \right) = \text{logit}(\mu)`$
* Invertendo $`\theta(\mu)`$: $`e^\theta = \frac{\mu}{1-\mu} \implies \mu = \frac{e^\theta}{1 + e^\theta}`$
* $`b(\theta) = -\log(1-\mu) = -\log\left( 1 - \frac{e^\theta}{1 + e^\theta} \right) = \log(1 + e^\theta)`$
* $`c(y^*, \phi) = \log \binom{n}{n y^*}`$
* **Verificação dos Momentos** :
* $`EY^* = b'(\theta) = \frac{e^\theta}{1 + e^\theta} = \mu`$
* $`b''(\theta) = \frac{d}{d\theta} \left( \frac{e^\theta}{1 + e^\theta} \right) = \frac{e^\theta (1 + e^\theta) - e^\theta (e^\theta)}{(1 + e^\theta)^2} = \frac{e^\theta}{(1 + e^\theta)^2} = \left(\frac{e^\theta}{1 + e^\theta}\right) \left(\frac{1}{1 + e^\theta}\right) = \mu(1-\mu)`$
* A função de variância é $`V(\mu) = \mu(1-\mu)`$
* $`\text{Var}(Y^*) = \frac{V(\mu)}{\phi} = \frac{\mu(1-\mu)}{n}`$
* **Ligação Canônica** : Definindo $`\theta = \eta`$, obtemos $`\text{logit}(\mu) = \eta`$. Assim, a ligação canônica para a Binomial é a **função logito** ($`\text{modelo de regressão logística}`$).

##### Resumo das Ligações Canônicas por Distribuição

| Distribuição | Parâmetro Canônico ($`\theta`$) | Função de Ligação Canônica $`g(\mu) = \eta`$ | Nome do Modelo |
|---|---|---|---|
| Normal | $`\mu`$ | Identidade: $`g(\mu) = \mu`$ | Modelo Linear Clássico |
| Poisson | $`\log(\mu)`$ | Log: $`g(\mu) = \log(\mu)`$ | Modelo Log / Poisson |
| Binomial | $`\log\left(\frac{\mu}{1-\mu}\right)`$ | Logito: $`g(\mu) = \log\left(\frac{\mu}{1-\mu}\right)`$ | Regressão Logística |
| Gama | $`-\mu^{-1}`$ | Recíproca / Negativa: $`g(\mu) = -\mu^{-1}`$ | Modelo Gama |
| Normal Inversa | $`-\frac{1}{2\mu^2}`$ | Recíproca do Quadrado: $`g(\mu) = -\frac{1}{2\mu^2}`$ | Modelo Normal Inverso |


##### 5. Ligações Alternativas, Dados Desbalanceados e Ajuste Computacional

###### *Funções de Ligação Alternativas para Resposta Binária:*

* **Probito** : $`g(\mu) = \Phi^{-1}(\mu)`$, onde $`\Phi(\cdot)`$ é a função de distribuição acumulada da Normal Padrão $`\mathcal{N}(0,1)`$.
* **Complemento Log-Log (c-log-log)** : $`g(\mu) = \log(-\log(1-\mu))`$.

###### *Modelagem de Eventos Raros (Dados Desbalanceados)*

Em áreas de aplicação como detecção de fraudes em cartão de crédito ou inadimplência, a proporção da classe de interesse (sucesso $`1`$) é extremamente baixa. O uso do modelo logístico tradicional assume classes aproximadamente balanceadas.
Para contornar o problema do desbalanceamento sem recorrer a técnicas de perda ou manipulação de informação como *undersampling* ou *oversampling* , abordagens modernas sugerem:

1. **Utilização de Ligações Assimétricas** : Adotar funções de ligação mais flexíveis e assimétricas (como o c-log-log ou ligações assimétricas generalizadas).
2. **Adoção de Métricas de Avaliação Adequadas** : A acurácia global torna-se inútil sob desbalanceamento severo (pois prever sempre a classe majoritária gera acurácia inflacionada). Recomendam-se métricas como a Média Geométrica (G-mean), o Coeficiente de Correlação de Matthews (MCC) e o Kappa de Cohen.

###### *Implementação Computacional em Python*

No ecossistema Python, os MLGs são ajustados por meio da biblioteca statsmodels, especificamente pela classe GLM presente nos módulos statsmodels.api ou statsmodels.formula.api. Caso o usuário especifique a família da distribuição (ex: statsmodels.api.families.Poisson()) sem declarar explicitamente o argumento da função de ligação, o pacote adota automaticamente a **ligação canônica** correspondente.

#### Questionário de Fixação (10 Perguntas)

1. Qual é a expressão geral da fdp ou fmp de uma distribuição pertencente à família exponencial de distribuições e o que representa o parâmetro $`\phi`$ ?
2. Como se define a ligação canônica em um Modelo Linear Generalizado?
3. Quais são as três suposições estruturais que definem formalmente um MLG em relação aos dados $`Y_i`$ e às covariáveis $`X_i`$ ?
4. Como a primeira condição de regularidade da verossimilhança é utilizada para demonstrar que $`EY_i = b'(\theta)`$ ?
5. De que forma a segunda condição de regularidade estabelece a relação analítica entre a variância de $`Y_i`$ e a segunda derivada $`b''(\theta)`$ ?
6. Por que as variáveis resposta $`Y_1, \dots, Y_n`$ em um MLG são consideradas independentes, mas não identicamente distribuídas?
7. Qual é a função de variância $`V(\mu)`$ e o parâmetro de dispersão $`\phi`$ para uma variável resposta com distribuição de Poisson?
8. Ao parametrização a distribuição Binomial para a proporção de sucessos $`Y^*`$, como se expressam seu parâmetro de dispersão $`\phi`$ e sua função $`b(\theta)`$ ?
9. Por que a acurácia clássica não é uma métrica apropriada para avaliar MLGs em contextos de dados fortemente desbalanceados?
10. Quais são as implicações negativas associadas à prática de *undersampling* e *oversampling* no ajuste de modelos de regressão logística?

#### Gabarito do Questionário (Respostas de Curta Extensão)

1. A expressão geral é $`f(y; \theta, \phi) = \exp\{\phi y\theta - b(\theta) + c(y, \phi)\}`$. O parâmetro $`\phi`$ representa o parâmetro de dispersão da distribuição, devendo assumir valores estritamente positivos ($`\phi > 0`$).
2. A ligação canônica ocorre quando o parâmetro natural $`\theta_i`$ da família exponencial é igualado diretamente ao preditor linear do modelo ($`\theta_i = \eta_i`$). Dessa forma, a função de ligação $`g(\mu_i)`$ é a função que transforma a média $`\mu_i`$ exatamente no parâmetro natural $`\theta_i`$.
3. As três suposições são: variáveis resposta $`Y_1, \dots, Y_n`$ independentes pertencentes à família exponencial; preditor linear $`\eta_i = X_i^T \beta`$ baseado em covariáveis conhecidas e não aleatórias; e uma função de ligação $`g(\mu)`$ monótona e diferenciável conectando a média ao preditor linear.
4. A primeira condição de regularidade estabelece que o valor esperado da derivada da log-verossimilhança em relação a $`\theta`$ é zero, ou seja, $`Ed L_i / d\theta = 0`$. Desenvolvendo a derivada $`\phi(y_i - b'(\theta))`$ e aplicando a esperança, obtém-se $`\phi(EY_i - b'(\theta)) = 0`$, o que resulta diretamente em $`EY_i = b'(\theta)`$.
5. A segunda condição iguala a esperança da derivada segunda da log-verossimilhança ao negativo do valor esperado do quadrado do escore: $`Ed^2 L_i / d\theta^2 = -E(d L_i / d\theta)^2`$. Substituindo as derivadas, obtém-se $`-\phi b''(\theta) = -\phi^2 \text{Var}(Y_i)`$, do que decorre que $`\text{Var}(Y_i) = b''(\theta) / \phi`$.
6. Elas são independentes devido ao delineamento amostral, contudo não são identicamente distribuídas porque o valor esperado $`\mu_i`$ de cada observação depende do vetor de covariáveis $`X_i`$, que varia entre as unidades experimentais. Assim, embora sigam a mesma família de distribuições, os parâmetros de localização de cada $`Y_i`$ diferem.
7. Na distribuição de Poisson, a função de variância é dada por $`V(\mu) = \mu`$, significando que a variância é idêntica à média. O parâmetro de dispersão é fixo e igual a unidade ($`\phi = 1`$).
8. Na modelagem da proporção de sucessos $`Y^*`$ em $`n`$ ensaios de Bernoulli, o parâmetro de dispersão equivale ao número de ensaios ($`\phi = n`$). A função $`b(\theta)`$ é dada por $`b(\theta) = \log(1 + e^\theta)`$, cuja primeira derivada fornece a média $`\mu`$.
9. Em conjuntos de dados desbalanceados, a classe majoritária domina o volume de observações, fazendo com que um modelo ingênuo que preveja apenas a classe majoritária obtenha uma acurácia elevada. Esse fenômeno oculta a incapacidade do modelo de detectar a classe minoritária de interesse, tornando a acurácia uma métrica enganosa.
10. A prática de *undersampling* descarta dados observados da classe majoritária, resultando em perda de informação valiosa para a estimação. Por outro lado, o *oversampling* duplica ou sintetiza observações da classe minoritária, podendo introduzir vício, distorcer a estrutura original dos dados e alterar artificialmente a calibração das probabilidades estimadas.

#### Questões Dissertativas (Sem Gabarito)

1. Demonstre analiticamente, passo a passo, a dedução das expressões da esperança $`EY_i`$ e da variância $`\text{Var}(Y_i)`$ utilizando as condições de regularidade de primeira e segunda ordem sobre a log-verossimilhança da família exponencial.
2. Formule as diferenças conceituais e estruturais entre os Modelos Lineares Clássicos e os Modelos Lineares Generalizados, discutindo como o componente de ligação e a família exponencial ampliam a capacidade de modelagem estatística.
3. Apresente a parametrização completa da distribuição Binomial na forma da família exponencial para o número de sucessos $`Y`$ e para a proporção de sucessos $`Y^*`$. Derive detalhadamente a função $`b(\theta)`$, a função de variância $`V(\mu)`$ e justifique por que o logito é a sua ligação canônica.
4. Em um problema de detecção de fraudes financeiras com presença severa de desbalanceamento entre as classes de resposta, critique a aplicação do modelo de regressão logística convencional e proponha soluções analíticas baseadas em funções de ligação alternativas e métricas de desempenho adequadas.
5. Construa uma tabela comparativa detalhando a parametrização na família exponencial para as distribuições Normal, Poisson, Binomial e Gama, explicitando para cada uma o parâmetro natural $`\theta`$, a função $`b(\theta)`$, a função de variância $`V(\mu)`$, o parâmetro de dispersão $`\phi`$ e a respectiva função de ligação canônica.

#### Glossário de Termos-Chave

* **Família Exponencial de Distribuições** : Classe de distribuições de probabilidade cuja densidade ou função massa pode ser reescrita em uma forma exponencial específica envolvendo o parâmetro natural $`\theta`$ e o parâmetro de dispersão $`\phi`$.
* **Parâmetro Canônico ($`\theta`$)** : Também chamado de parâmetro natural, é a transformação do parâmetro de localização da distribuição que se associa diretamente ao termo linear em modelos da família exponencial.
* **Parâmetro de Dispersão ($`\phi`$)** : Parâmetro estritamente positivo que escala a variabilidade da distribuição de probabilidade, podendo ser fixo (como na Poisson e Binomial) ou estimado (como na Normal e Gama).
* **Função de Variância ($`V(\mu)`$)** : Função equivalente a $`b''(\theta)`$ que expressa a dependência funcional entre a variância e a média da variável resposta na família exponencial.
* **Modelo Linear Generalizado (MLG)** : Classe flexível de modelos de regressão que conecta a média de uma resposta da família exponencial a um preditor linear por meio de uma função de ligação monótona e diferenciável.
* **Preditor Linear ($`\eta`$)** : Combinação linear dos parâmetros do modelo com as covariáveis conhecidas, expressa por $`\eta_i = X_i^T \beta`$.
* **Função de Ligação ($`g(\mu)`$)** : Função matemática monótona e diferenciável que transforma a média esperada da variável resposta $`\mu_i`$ para a escala do preditor linear $`\eta_i`$.
* **Função de Ligação Canônica** : Função de ligação $`g(\mu)`$ obtida ao igualar o preditor linear diretamente ao parâmetro natural da distribuição ($`\eta_i = \theta_i`$).
* **Função Escore** : Derivada primeira da função de log-verossimilhança em relação aos parâmetros, cujo valor esperado sob condições de regularidade é igual a zero.
* **Informação de Fisher** : Medida da quantidade de informação que uma variável aleatória observada carrega sobre um parâmetro desconhecido, derivada da esperança do quadrado da função escore ou do negativo da esperança da derivada segunda da log-verossimilhança.
* **Razão de Chances (** ***Odds Ratio*** **)** : Medida de efeito associada exclusivamente ao modelo de regressão logística (resposta binária com ligação logito), que quantifica a alteração relativa na chance de ocorrência do evento de interesse por variação unitária na covariável.
* **Dados Desbalanceados** : Cenário de modelagem em que as categorias da variável resposta qualitativa apresentam frequências extremamente díspares, exigindo cuidados na escolha da função de ligação e das métricas de avaliação do modelo.
