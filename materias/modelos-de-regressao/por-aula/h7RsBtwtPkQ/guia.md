> **Aula:** [EST5507 Modelos de Regressão 2026 10 16 MLGs: Estimação de parâmetros](https://www.youtube.com/watch?v=h7RsBtwtPkQ)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudo: Estimação de Parâmetros em Modelos Lineares Generalizados (MLGs)

Este guia de estudo foi elaborado para revisar e aprofundar os conhecimentos apresentados na aula de pós-graduação sobre estimação de parâmetros em Modelos Lineares Generalizados (MLGs), ministrada pela Professora Cibele Russo (disciplina EST5507). O conteúdo baseia-se na literatura de referência da área, como a obra de Gilberto Paula, além de textos complementares de Morettin & Singer e James et al.

#### 1. Objetivos da Aula

* **Compreender a estrutura de estimação em MLGs:** Entender por que a estimação dos parâmetros de regressão ($`\beta`$) em MLGs não possui solução em forma fechada como nos Modelos Lineares (ML) clássicos.
* **Obter a Função Score:** Desenvolver a derivada da log-verossimilhança em relação aos parâmetros $`\beta`$ utilizando a regra da cadeia para a Família Exponencial.
* **Diferenciar Métodos Iterativos:** Comparar os algoritmos numéricos de Newton-Raphson e *Fisher Scoring* (Mínimos Quadrados Reponderados Iterativos - IRLS/IWLS).
* **Compreender o Conceito e Aplicação da Função Desvio (** ***Deviance*** **):** Avaliar a qualidade do ajuste do modelo comparando-o ao modelo saturado e utilizar os resíduos componentes do desvio para diagnóstico.
* **Relacionar MLGs ao Contexto de Aprendizado de Máquina:** Identificar a aplicação de MLGs em tarefas de regressão e classificação (regressão logística), bem como reconhecer as métricas adequadas para avaliação de desempenho, especialmente em dados desbalanceados.

#### 2. Desenvolvimento Passo a Passo do Conteúdo

##### 2.1. Estrutura dos MLGs e Família Exponencial

Nos MLGs, a variável resposta $`Y_i`$ ($`i = 1, \dots, n`$) pertence à Família Exponencial de distribuições, cuja densidade ou função de massa de probabilidade é expressa por:
```math
f(y_i; \theta_i, \phi) = \exp \left\{ \phi y_i \theta_i - b(\theta_i) + c(y_i, \phi) \right\}
```
Onde:

* $`\theta_i`$ é o parâmetro canônico ou de localização.
* $`\phi`$ é o parâmetro de dispersão (inicialmente assumido como conhecido).
* $`b(\theta_i)`$ é uma função estritamente duas vezes diferenciável, na qual a primeira derivada fornece a média $`EY_i = \mu_i = b'(\theta_i)`$, e a segunda derivada relaciona-se com a variância: $`\text{Var}(Y_i) = \frac{b''(\theta_i)}{\phi} = \frac{V(\mu_i)}{\phi}`$, sendo $`V(\mu_i)`$ a função de variância.
* $`c(y_i, \phi)`$ é uma função de ponderação e ajuste que depende dos dados e do parâmetro de dispersão.

O preditor linear $`\eta_i`$ conecta as variáveis explicativas $`X_i`$ aos parâmetros do modelo por meio de uma função de ligação $`g(\cdot)`$:
```math
\eta_i = g(\mu_i) = x_i^T \beta = \beta_0 + \beta_1 x_{i1} + \dots + \beta_p x_{ip}
```
```math
\mu_i = g^{-1}(\eta_i)
```

##### 2.2. Estimação por Máxima Verossimilhança e a Função Score

Dado um conjunto de observações independentes $`Y_1, \dots, Y_n`$, a função de log-verossimilhança $`L(\theta, \phi; y)`$ é a soma das log-verossimilhanças individuais $`l_i`$:
```math
L(\theta, \phi; y) = \sum_{i=1}^n l_i(\theta_i, \phi) = \sum_{i=1}^n \left\{ \phi y_i \theta_i - b(\theta_i) + c(y_i, \phi) \right\}
```
Para estimar o vetor de parâmetros $`\beta`$, busca-se maximizar $`L(\theta, \phi; y)`$ resolvendo o sistema de equações $`U(\beta_j) = 0`$ para $`j = 0, 1, \dots, p`$, onde $`U(\beta_j) = \frac{\partial L}{\partial \beta_j}`$ é a **Função Score** .

###### *Aplicação da Regra da Cadeia*

Como o parâmetro $`\beta_j`$ está inserido indireitamente na verossimilhança através da cadeia de dependências $`\beta \to \eta \to \mu \to \theta \to L`$, utiliza-se a regra da cadeia:
```math
U(\beta_j) = \frac{\partial L}{\partial \beta_j} = \sum_{i=1}^n \frac{\partial l_i}{\partial \theta_i} \cdot \frac{\partial \theta_i}{\partial \mu_i} \cdot \frac{\partial \mu_i}{\partial \eta_i} \cdot \frac{\partial \eta_i}{\partial \beta_j}
```
Derivando cada componente:

1. $`\frac{\partial l_i}{\partial \theta_i} = \phi y_i - b'(\theta_i) = \phi (y_i - \mu_i)`$
2. $`\frac{\partial \theta_i}{\partial \mu_i} = \frac{1}{\frac{\partial \mu_i}{\partial \theta_i}} = \frac{1}{b''(\theta_i)} = \frac{1}{V(\mu_i)}`$
3. $`\frac{\partial \mu_i}{\partial \eta_i} = \frac{\partial g^{-1}(\eta_i)}{\partial \eta_i}`$ (depende da função de ligação adotada)
4. $`\frac{\partial \eta_i}{\partial \beta_j} = x_{ij}`$

Substituindo os termos na derivada, obtém-se a forma geral da Função Score para o parâmetro $`\beta_j`$:
```math
U(\beta_j) = \sum_{i=1}^n \phi \frac{(y_i - \mu_i)}{V(\mu_i)} \left( \frac{\partial \mu_i}{\partial \eta_i} \right) x_{ij}
```
Como as médias $`\mu_i`$ dependem não linearmente de $`\beta`$ por meio da função de ligação $`g(\cdot)`$, a equação $`U(\beta_j) = 0`$ não possui solução analítica direta (diferente da fórmula do Modelo Linear Clássico: $`\hat{\beta} = (X^T X)^{-1} X^T Y`$).

##### 2.3. Métodos Iterativos de Otimização Numérica

Para resolver $`U(\beta) = 0`$, recorre-se a procedimentos iterativos numéricos.

###### *A. Método de Newton-Raphson*

Utiliza a matriz de informação observada $`J(\beta)`$, que corresponde à matriz de derivadas segundas negativas da log-verossimilhança:
```math
J(\beta) = -\frac{\partial^2 L}{\partial \beta \partial \beta^T}
```
A atualização dos estimadores na iteração $`m+1`$ é dada por:
```math
\beta^{(m+1)} = \beta^{(m)} + J^{-1}(\beta^{(m)}) U(\beta^{(m)})
```

###### *B. Método Fisher Scoring*

Substitui a matriz observada $`J(\beta)`$ pela **matriz de informação esperada de Fisher** $`K(\beta) = EJ(\beta)`$. A atualização toma a forma:
```math
\beta^{(m+1)} = \beta^{(m)} + K^{-1}(\beta^{(m)}) U(\beta^{(m)})
```
Na Notação Matricial, definindo:

* $`W = \text{diag}(W_1, \dots, W_n)`$, onde os pesos são $`W_i = \left( \frac{\partial \mu_i}{\partial \eta_i} \right)^2 \frac{1}{V(\mu_i)}`$
* $`V = \text{diag}(V(\mu_1), \dots, V(\mu_n))`$
* $`X`$ como a matriz de planejamento contendo as variáveis preditoras

A matriz de informação esperada simplifica-se para $`K(\beta) = X^T W X`$. O algoritmo de *Fisher Scoring* equivale ao método de **Mínimos Quadrados Reponderados Iterativos (IRLS / IWLS)** :
```math
\beta^{(m+1)} = (X^T W^{(m)} X)^{-1} X^T W^{(m)} Z^{(m)}
```
Em que $`Z^{(m)}`$ representa a variável resposta ajustada/modificada na iteração $`m`$.

###### *Propriedades de Convergência*

* **Chute Inicial:** Pode-se adotar como valor inicial $`\mu_i^{(0)} = y_i`$.
* **Critério de Convergência:** O algoritmo atinge a convergência quando a variação relativa máxima nos parâmetros entre iterações consecutivas for menor do que um limite tolerável $`\epsilon`$ (por exemplo, $`10^{-2}`$ ou $`10^{-3}`$):

```math
\max_j \left| \frac{\beta_j^{(m+1)} - \beta_j^{(m)}}{\beta_j^{(m)}} \right| < \epsilon
```

* **Ligação Canônica:** O uso da função de ligação canônica (quando $`\eta_i = \theta_i`$) garante a unicidade da estimativa de máxima verossimilhança e facilita a convergência, pois as matrizes $`J(\beta)`$ e $`K(\beta)`$ coincidem.

##### 2.4. A Função Desvio ( *Deviance* ) e Análise de Diagnóstico

A **Função Desvio** mede o afastamento entre o modelo ajustado (com $`p+1`$ parâmetros) e o **modelo saturado** (modelo hipotético ideal no qual há um parâmetro para cada observação, isto é, $`\hat{\mu}_i = y_i`$, obtendo-se o ajuste perfeito).

###### *Definição*

O desvio escalonado ( *scaled deviance* ) é definido pelo teste da razão de verossimilhanças entre o modelo saturado e o modelo ajustado:
```math
D^*(y, \hat{\mu}) = 2 \left[ L(y, \phi; y) - L(\hat{\mu}, \phi; y) \right ]
```
A relação com o desvio não escalonado $`D(y, \hat{\mu})`$ é dada por $`D^*(y, \hat{\mu}) = \phi D(y, \hat{\mu})`$.

* **Valores Pequenos do Desvio:** Indicam que o modelo parcimonioso ajustado está próximo do modelo saturado, sinalizando um bom ajuste.
* **No Modelo Normal (Gaussiano):** O desvio equivale exatamente à Soma dos Quadrados dos Erros (SQE).

###### *Resíduos Componentes do Desvio*

A função desvio $`D(y, \hat{\mu})`$ é uma soma de $`n`$ parcelas individuais: $`D(y, \hat{\mu}) = \sum_{i=1}^n d_i^2`$.
O **resíduo componente do desvio** para a $`i`$ -ésima observação é dado por:
```math
r_{d,i} = \text{sinal}(y_i - \hat{\mu}_i) \sqrt{d_i^2}
```
Estes resíduos são amplamente empregados na análise de diagnóstico de MLGs (em substituição aos resíduos ordinários) e podem ser plotados em **gráficos de envelope simulado** para avaliar a adequação da distribuição assumida (ex.: comparar Poissonalidade versus Binomial Negativa).

##### 2.5. MLGs no Contexto de Aprendizado de Máquina ( *Machine Learning* )

Enquanto a abordagem estatística tradicional foca na estimação de parâmetros e interpretabilidade do modelo, o Aprendizado de Máquina (AM) foca primordialmente na capacidade preditiva em novos dados.

###### *Classificação Binária e Regressão Logística*

O modelo de Regressão Logística é um MLG com resposta Bernoulli ($`Y_i \in \{0, 1\}`$) e função de ligação logit $`\eta_i = \log\left(\frac{\pi_i}{1 - \pi_i}\right)`$, sendo a base fundamental para tarefas de classificação supervisionada (como identificação de e-mails em *spam* ou *não-spam* , inspirado em conceitos clássicos de discriminação como o Teste de Turing).

###### *Métricas de Avaliação de Desempenho*

Dependendo da natureza do problema e do equilíbrio das classes na resposta, diferentes métricas devem ser priorizadas:

| Tarefa | Métricas Recomendadas | Observação / Contexto |
|---|---|---|
| Regressão | Erro Absoluto Médio (MAE), Erro Quadrático Médio (MSE), Raiz do Erro Quadrático Médio (RMSE), Coeficiente de Determinação ($`R^2`$). | Respostas contínuas e quantitativas. |
| Classificação (Dados Balanceados) | Acurácia, Sensibilidade (Revocação), Especificidade, Área Sob a Curva ROC (AUC-ROC). | Percentual simples de acertos. |
| Classificação (Dados Desbalanceados) | Coeficiente de Correlação de Matthews (MCC), Média Geométrica ($`G\text{-mean}`$), Kappa de Cohen. | Atenção: Em dados desbalanceados (ex.: 95% de fraude na classe majoritária), a Acurácia gera uma falsa sensação de bom ajuste ao classificar tudo na classe majoritária. Métricas como MCC, $`G\text{-mean}`$ e Kappa de Cohen são mais robustas (conforme demonstrado na literatura por Cruz, Bazan e Russo). |


###### *Implementação Computacional*

* **Linguagem R:** Ajustado prioritariamente através da função nativa glm(). O desvio aparece diretamente nos relatórios de saída como deviance.
* **Linguagem Python:** Disponível na biblioteca statsmodels (módulo statsmodels.api.GLM), que oferece suporte a diversas famílias (Binomial, Poisson, Gamma, Gaussian, InverseGaussian, NegativeBinomial, Tweedie) e funções de ligação ( *logit* , *log* , *cloglog* , *power* , *cauchy* ).

#### 3. Questionário de Fixação (Quiz)

As dez perguntas a seguir possuem respostas curtas (de 2 a 3 frases) localizadas na seção **Gabarito** .

1. Por que não é possível obter uma expressão analítica direta em forma fechada para os estimadores $`\hat{\beta}`$ em MLGs como se faz em Modelos Lineares clássicos?
2. O que representa o parâmetro $`b(\theta_i)`$ na estrutura da Família Exponencial e como ele se relaciona com a média e a variância de $`Y_i`$ ?
3. Na derivação da Função Score $`U(\beta_j)`$, qual é o papel desempenhado pela Regra da Cadeia?
4. Qual é a diferença fundamental entre o algoritmo de Newton-Raphson e o algoritmo de *Fisher Scoring* para estimação de parâmetros?
5. O que são os Mínimos Quadrados Reponderados Iterativos (IRLS) e qual a sua relação com os MLGs?
6. O que é o modelo saturado e qual a sua utilidade no cálculo do desvio ( *deviance* )?
7. Como é interpretado um valor pequeno da função desvio em um modelo ajustado?
8. Qual a vantagem de utilizar o gráfico de envelope simulado na análise de resíduos de um MLG?
9. Por que a acurácia pode ser uma métrica enganosa na avaliação de um modelo de classificação ajustado sobre dados altamente desbalanceados?
10. Quais métricas de avaliação são mais recomendadas em problemas de classificação com dados desbalanceados segundo a literatura citada em aula?

#### 4. Gabarito das Questões do Quiz

1. **Por que não é possível obter uma expressão analítica direta em forma fechada para os estimadores $`\hat{\beta}`$em MLGs como se faz em Modelos Lineares clássicos?** *Resposta:* A não linearidade introduzida pela função de ligação $`g(\cdot)`$ faz com que os parâmetros $`\beta`$ fiquem contidos dentro de funções não lineares da média $`\mu_i`$. Dessa forma, ao igualar a função Score a zero, obtém-se um sistema de equações não lineares que não possui solução analítica explícita. Por isso, torna-se necessário o uso de métodos numéricos iterativos para encontrar as estimativas.
2. **O que representa o parâmetro $`b(\theta_i)`$na estrutura da Família Exponencial e como ele se relaciona com a média e a variância de $`Y_i`$?** *Resposta:* O parâmetro $`b(\theta_i)`$ é uma função estritamente diferenciável característica da distribuição pertencente à Família Exponencial. A sua primeira derivada em relação a $`\theta_i`$ fornece a média da variável resposta ($`EY_i = \mu_i = b'(\theta_i)`$). Já a sua segunda derivada define a função de variância da distribuição ($`\text{Var}(Y_i) = b''(\theta_i)/\phi = V(\mu_i)/\phi`$).
3. **Na derivação da Função Score $`U(\beta_j)`$, qual é o papel desempenhado pela Regra da Cadeia?** *Resposta:* A Regra da Cadeia é indispensável para decompor a derivada da log-verossimilhança em relação ao parâmetro de interesse $`\beta_j`$ através dos elos intermediários do modelo ($`\beta \to \eta \to \mu \to \theta \to L`$). Ela permite encadear a derivada da verossimilhança em relação a $`\theta_i`$, de $`\theta_i`$ em relação a $`\mu_i`$, de $`\mu_i`$ em relação ao preditor $`\eta_i`$, e deste em relação a $`\beta_j`$. O resultado final sintetiza esses componentes em uma expressão geral aplicável a qualquer MLG.
4. **Qual é a diferença fundamental entre o algoritmo de Newton-Raphson e o algoritmo de** ***Fisher Scoring*** **para estimação de parâmetros?** *Resposta:* O método de Newton-Raphson utiliza a matriz de informação observada $`J(\beta)`$, formada pelas derivadas segundas negativas da log-verossimilhança calculadas nos dados observados. O método de *Fisher Scoring* , por sua vez, substitui a matriz observada pela matriz de informação esperada $`K(\beta) = EJ(\beta)`$. Essa substituição comumente simplifica os cálculos computacionais e garante estabilidade ao processo iterativo.
5. **O que são os Mínimos Quadrados Reponderados Iterativos (IRLS) e qual a sua relação com os MLGs?** *Resposta:* Os Mínimos Quadrados Reponderados Iterativos (IRLS ou IWLS) correspondem à reescrita matricial do algoritmo de *Fisher Scoring* aplicado aos MLGs. A cada iteração, o procedimento resolve um problema de mínimos quadrados ponderados utilizando uma matriz de pesos $`W`$ e uma resposta ajustada $`Z`$, ambas reavaliadas a partir dos estimadores da iteração anterior. O processo prossegue iterativamente até que os parâmetros convirjam de acordo com o critério estabelecido.
6. **O que é o modelo saturado e qual a sua utilidade no cálculo do desvio (** ***deviance*** **)?** *Resposta:* O modelo saturado é um modelo hipotético ideal superparametrizado que possui tantos parâmetros quantas são as observações ($`n`$), ajustando perfeitamente os dados ($`\hat{\mu}_i = y_i`$). Ele serve como a referência máxima de verossimilhança alcançável. O desvio é construído comparando-se a log-verossimilhança do modelo saturado com a do modelo ajustado parcimonioso por meio da razão de verossimilhanças.
7. **Como é interpretado um valor pequeno da função desvio em um modelo ajustado?** *Resposta:* Um valor pequeno da função desvio indica que a log-verossimilhança do modelo ajustado está muito próxima da log-verossimilhança do modelo saturado. Isso significa que o modelo com poucos parâmetros ($`p+1`$) é capaz de explicar os dados com qualidade quase equivalente à do modelo com $`n`$ parâmetros. Trata-se de um forte indício de boa qualidade de ajuste e parcimônia.
8. **Qual a vantagem de utilizar o gráfico de envelope simulado na análise de resíduos de um MLG?** *Resposta:* O gráfico de envelope simulado constrói bandas de confiança empíricas para os resíduos (como os resíduos componentes do desvio) através de simulações. Ele permite avaliar visualmente se as suposições do modelo (como a distribuição de probabilidade adotada para a resposta) estão adequadas. Caso os resíduos fiquem contidos dentro das bandas do envelope, valida-se a adequação da distribuição escolhida.

| 9. **Por que a acurácia pode ser uma métrica enganosa na avaliação de um modelo de classificação ajustado sobre dados altamente desbalanceados?** *Resposta:* Em cenários desbalanceados (por exemplo | quando 95% das observações pertencem à classe majoritária) | um modelo ingênuo que preveja apenas a classe majoritária para todas as instâncias obterá uma acurácia de 95%. Essa taxa elevada cria uma falsa sensação de excelente desempenho do modelo. No entanto | o modelo falha completamente em identificar qualquer caso pertencente à classe minoritária de interesse. |
|---|---|---|---|
| 10. **Quais métricas de avaliação são mais recomendadas em problemas de classificação com dados desbalanceados segundo a literatura citada em aula?** *Resposta:* Segundo a literatura apresentada (Cruz | Bazan e Russo) | métricas baseadas na matriz de confusão que penalizam o desequilíbrio entre classes são superiores à acurácia e à curva ROC. Destacam-se o Coeficiente de Correlação de Matthews (MCC) | a Média Geométrica ($`G\text{-mean}`$) das sensibilidades/especificidades e o estatístico Kappa de Cohen. Essas medidas fornecem avaliações realistas do poder preditivo em ambas as classes. |


#### 5. Questões Dissertativas

*Instruções: Desenvolva respostas detalhadas e fundamentadas para as questões abaixo sem consultar um gabarito prévio, utilizando os conceitos debatidos na aula.*

1. Demonstre detalhadamente os passos matemáticos da Regra da Cadeia aplicados na obtenção do vetor Score $`U(\beta_j)`$ em um Modelo Linear Generalizado, explicitando como a estrutura da Família Exponencial simplifica o termo $`\frac{\partial l_i}{\partial \theta_i}`$.
2. Explique o papel da função de ligação canônica na estimação de parâmetros por máxima verossimilhança em MLGs. Quais são as consequências teóricas e práticas de sua utilização em termos de matrizes de informação e convergência de algoritmos iterativos?
3. Compare conceitualmente o modelo saturado, o modelo nulo e o modelo ajustado parcimonioso. Como a função desvio utiliza essas estruturas para realizar testes de hipótese e comparar modelos concorrentes?
4. Discuta a transição entre o enfoque puramente estatístico de estimação de parâmetros nos MLGs e o enfoque de Aprendizado de Máquina ( *Machine Learning* ). Em sua resposta, aborde como a escolha entre minimizar o desvio ( *deviance* ) ou otimizar métricas preditivas reflete as diferentes finalidades dessas áreas.
5. Em um problema real de detecção de fraudes financeiras com resposta binária e alta desproporção de classes, explique por que o ajuste de um MLG com ligação logit avaliado por acurácia pode levar a conclusões errôneas. Apresente métricas alternativas adequadas e justificativas teóricas para sua escolha com base no estudo citado na aula.

#### 6. Glossário de Termos-Chave

* **Acurácia:** Métrica de desempenho em classificação dada pelo percentual simples de acertos em relação ao total de observações. Pode induzir a diagnósticos incorretos em bases com classes altamente desbalanceadas.
* **Algoritmo de Newton-Raphson:** Método numérico iterativo de otimização que utiliza a matriz de derivadas segundas observadas (Matriz de Informação Observada) da função de log-verossimilhança para encontrar os zeros da função score.
* **Ajuste Saturado (Modelo Saturado):** Modelo hipotético limite que atribui um parâmetro para cada observação individual ($`\hat{\mu}_i = y_i`$), obtendo um ajuste perfeito e servindo de referência máxima de verossimilhança.
* **Cappa de Cohen (Kappa):** Métrica de concordância interobservadores/classificadores ajustada pelo acerto ao acaso. É recomendada em problemas de classificação com dados desbalanceados.
* **Coeficiente de Correlação de Matthews (MCC):** Métrica de avaliação para classificações binárias que leva em consideração os quatro quadrantes da matriz de confusão (verdadeiros positivos, verdadeiros negativos, falsos positivos e falsos negativos), produzindo um valor de $`-1`$ a $`+1`$. É altamente robusto para dados desbalanceados.
* **Desvio (** ***Deviance*** **):** Medida estatística de afastamento da qualidade do ajuste do modelo estudado em relação ao modelo saturado, calculada via teste da razão de verossimilhanças ($`2L(\text{saturado}) - L(\text{ajustado})`$).
* **Família Exponencial de Distribuições:** Classe flexível de distribuições de probabilidade (que inclui a Normal, Poisson, Binomial, Gamma, Binomial Negativa e Gaussiana Inversa) cujas densidades compartilham uma forma matemática padrão parametrizada por parâmetros de localização e dispersão.
* **Fisher Scoring:** Variação do algoritmo de Newton-Raphson que substitui a matriz de informação observada pela matriz de informação esperada de Fisher ($`K(\beta) = EJ(\beta)`$), facilitando o cálculo computacional.
* **Função de Ligação ($`g(\cdot)`$):** Função monotonicamente diferenciável que conecta o preditor linear ($`\eta_i = x_i^T \beta`$) ao valor esperado da variável resposta ($`\mu_i = EY_i`$).
* **Função de Ligação Canônica:** Função de ligação específica para a qual o preditor linear equivale diretamente ao parâmetro canônico ($`\eta_i = \theta_i`$), garantindo propriedades matemáticas convenientes como a unicidade do estimador de máxima verossimilhança.
* **Função de Variância ($`V(\mu_i)`$):** Componente do MLG derivada de $`b''(\theta_i)`$ que expressa a variância da resposta $`Y_i`$ puramente em termos de sua média $`\mu_i`$.
* **Função Score ($`U(\beta)`$):** Vetor das primeiras derivadas parciais da log-verossimilhança em relação aos parâmetros de regressão $`\beta`$. A estimação por máxima verossimilhança busca solucionar a equação $`U(\beta) = 0`$.
* **Gráfico de Envelope Simulado:** Técnica gráfica de diagnóstico que desenha bandas de confiança simuladas em torno dos resíduos ordenados para testar visualmente a adequação da distribuição de probabilidade assumida.
* **Matriz de Confusão:** Tabela cruzada utilizada em classificação que contrapõe as classes reais observadas com as classes preditas pelo modelo, discriminando Verdadeiros Positivos, Verdadeiros Negativos, Falsos Positivos e Falsos Negativos.
* **Matriz de Informação Esperada de Fisher ($`K`$):** Valor esperado da matriz de derivadas segundas negativas da log-verossimilhança. É equivalente a $`X^T W X`$ nos MLGs.
* **Matriz de Informação Observada ($`J`$):** Matriz contendo o negativo das derivadas segundas parciais da log-verossimilhança calculada nos valores amostrais observados.
* **Média Geométrica ($`G\text{-mean}`$):** Métrica calculada pela raiz quadrada do produto entre a Sensibilidade e a Especificidade. É indicada para medir o equilíbrio do desempenho preditivo em conjuntos desbalanceados.
* **Mínimos Quadrados Reponderados Iterativos (IRLS / IWLS):** Algoritmo iterativo empregado na estimação dos parâmetros dos MLGs no qual, a cada passo, resolve-se uma equação de mínimos quadrados ponderados reajustando-se os pesos $`W`$ e as respostas modificadas $`Z`$.
* **Parâmetro de Dispersão ($`\phi`$):** Parâmetro presente na Família Exponencial que escala a variância da variável resposta independentemente da média (na distribuição Poisson, por exemplo, $`\phi = 1`$).
* **Parâmetro de Localização / Locação ($`\theta_i`$):** Parâmetro canônico da Família Exponencial diretamente associado à posição ou média da distribuição.
* **Preditor Linear ($`\eta_i`$):** Combinação linear dos parâmetros do modelo com as variáveis explicativas ($`x_i^T \beta`$).
* **Regressão Logística:** Caso particular de MLG aplicável a variáveis de resposta binárias (Bernoulli), que utiliza a função de ligação *logit* ($`\log(\pi / (1-\pi))`$).
* **Resíduo Componente do Desvio:** Resíduo individual assintoticamente padronizado derivado da decomposição da função desvio total ($`D = \sum d_i^2`$), utilizado primordialmente para checagem de suposições e detecção de *outliers* .
* **statsmodels** **:** Biblioteca da linguagem Python que disponibiliza módulos para ajuste de MLGs (classe GLM), suportando variadas famílias e funções de ligação.
