> **Aula:** [EST5507 Modelos de Regressão 20261009 Análise de diagnóstico em ML e Modelos Lineares Generalizados](https://www.youtube.com/watch?v=6ZzfysjboyE)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudo: Análise de Diagnóstico em Modelos Lineares e Introdução aos Modelos Lineares Generalizados (MLG)

#### Objetivos de Aprendizagem

* **Diferenciar Modelagem Estatística de Aprendizado de Máquina:** Compreender o foco da modelagem estatística na interpretação inferencial e na validação de suposições teóricas, em contraste com a priorização preditiva.
* **Avaliar Suposições em Modelos Lineares Gerais:** Identificar e testar desvios de linearidade, independência, normalidade e homocedasticidade nos erros aleatórios após o ajuste do modelo.
* **Distinguir Tipos de Observações Problemáticas:** Conceituar e diferenciar observações atípicas ( *outliers* ), pontos de alavanca ( *leverage* ), pontos aberrantes e pontos influentes.
* **Aplicar Métricas e Ferramentas de Diagnóstico:** Utilizar a matriz chapéu, resíduos estudentizados (internos e externos), Distância de Cook, DFFITS, DFBETAS, análise confirmatória ( *case deletion* ) e gráficos de envelope para detectar anomalias.
* **Compreender a Estrutura dos Modelos Lineares Generalizados (MLG):** Formular a Família Exponencial de distribuições, identificar seus parâmetros (localização e dispersão), definir a função de variância e mapear as funções de ligação canônicas.

#### Desenvolvimento Passo a Passo do Conteúdo

##### 1. Modelagem Estatística vs. Aprendizado de Máquina e Validação do Modelo

A modelagem estatística busca interpretar a relação entre variáveis preditoras e a variável resposta, dependendo diretamente de suposições sobre a distribuição dos dados e dos erros. Diferente de aplicações de aprendizado de máquina voltadas exclusivamente à predição, a inferência estatística exige a verificação *a posteriori* do ajuste do modelo.
No Modelo Linear Geral:
```math
Y = X\beta + \epsilon
```
onde $`Y`$ é o vetor de respostas $`n \times 1`$, $`X`$ é a matriz de delineamento (ou planejamento) $`n \times p`$, $`\beta`$ é o vetor de parâmetros $`p \times 1`$, e $`\epsilon`$ é o vetor de erros aleatórios.
As suposições clássicas estabelecem que $`E\epsilon = 0`$ e $`Var(\epsilon) = \sigma^2 I_n`$.

* **Homocedasticidade:** A variância dos erros ($`\sigma^2`$) é constante para todas as observações.
* **Não-correlação/Independência:** A matriz identidade $`I_n`$ indica ausência de covariância entre erros distintos ($`Cov(\epsilon_i, \epsilon_j) = 0, \forall i \neq j`$). A não-correlação implica independência apenas sob a suposição de normalidade.

O estimador de Mínimos Quadrados Ordinários (MQO), que coincide com o Estimador de Máxima Verossimilhança (EMV) sob a suposição de normalidade dos erros, é dado por:
```math
\hat{\beta} = (X^T X)^{-1} X^T Y
```
com distribuição amostral $`\hat{\beta} \sim N_p(\beta, \sigma^2 (X^T X)^{-1})`$.
Se $`X^T X`$ for singular (matriz de posto incompleto), adotam-se inversas generalizadas e funções estimáveis. Caso a matriz de variância não seja escalar ($`\sigma^2 V`$), recorre-se a mínimos quadrados ponderados ou generalizados.

###### *Coeficiente de Determinação ($`R^2`$)*

O ajuste geral é frequentemente resumido pelo coeficiente de determinação:
```math
R^2 = \frac{SQ_{Reg}}{SQ_{Tot}} = 1 - \frac{SQ_R}{SQ_{Tot}}
```
Em notação matricial, utilizando a matriz chapéu $`H = X(X^T X)^{-1} X^T`$:
```math
R^2 = 1 - \frac{Y^T (I - H) Y}{SQ_{Tot}}
```
O $`R^2`$ ajustado penaliza a inclusão desnecessária de covariáveis, favorecendo a parsimónia:
```math
R^2_{adj} = 1 - \left [\frac{(1 - R^2)(n - 1)}{n - p} \right ]
```
*Nota de contexto em Ciência de Dados:* Praticantes de *machine learning* por vezes calculam o $`R^2`$ entre os valores ajustados ($`\hat{Y}`$) e os observados ($`Y`$). Como $`\hat{Y} = HY`$ já é uma transformação linear de $`Y`$ no modelo linear simples, essa medida reflete a proximidade linear entre o estimado e o observado, equivalendo ao quadrado da correlação linear de Pearson.

##### 2. Análise de Diagnóstico em Modelos Lineares

A análise de diagnóstico busca identificar falhas nas suposições e detectar observações com comportamento anômalo.

###### *Tipologia dos Pontos Diagnosticados*

* **Observações Atípicas (** ***Outliers*** **):** Dados discrepantes no espaço da resposta ou das preditoras, que não necessariamente alteram as conclusões inferenciais.
* **Pontos de Alavanca (** ***Leverage Points*** **):** Observações afastadas da massa de dados no espaço das variáveis explicativas ($`X`$). Apresentam grande potencial de atração sobre o seu próprio valor ajustado.
* **Pontos Aberrantes:** Observações que apresentam resíduos elevados (distantes da massa principal de resíduos).
* **Pontos Influentes:** Observações cuja remoção altera significativamente as estimativas dos parâmetros ($`\hat{\beta}`$), a significância estatística das preditoras ou as conclusões do modelo.

Visualização Conceitual da Relação entre os Pontos:
```text
+-------------------------------------------------------------------+
| Espaço dos X (Matriz de Covariáveis)                              |
| -> Distanciamento do centro dos dados = PONTO DE ALAVANCA (h_i)   |
+-------------------------------------------------------------------+
                                 |
                                 v
+-------------------------------------------------------------------+
| Espaço do Erro/Resíduo (Variável Resposta Y)                      |
| -> Resíduo elevado em magnitude = PONTO ABERRANTE (e_i)           |
+-------------------------------------------------------------------+
                                 |
                                 v
+-------------------------------------------------------------------+
| Alteração nas estimativas dos parâmetros (Exclusão do Ponto)      |
| -> Mudança substancial em beta/p-valor = PONTO INFLUENTE          |
+-------------------------------------------------------------------+
```

###### *Ferramentas de Detecção de Alavanca e Resíduos*

* **Matriz Chapéu ($`H`$):**

```math
H = X(X^T X)^{-1} X^T
```

É uma matriz de projeção ortogonal, simétrica e idempotente ($`H^2 = H`$). O $`i`$ -ésimo elemento da diagonal principal, $`h_i = h_{ii} = x_i^T (X^T X)^{-1} x_i`$, mede o grau de alavancagem da $`i`$ -ésima observação ($`0 \le h_i \le 1`$).
* *Critério de corte:* Uma observação é considerada ponto de alavanca se $`h_i > 2 \bar{h}`$, onde a média é $`\bar{h} = \frac{p}{n}`$.
* **Resíduo Ordinário:**

```math
e_i = y_i - \hat{y}_i \quad \Rightarrow \quad e = (I - H)Y
```

Possui variância não constante: $`Var(e_i) = \sigma^2 (1 - h_i)`$. A estimativa não viesada de $`\sigma^2`$ é dada pelo Erro Quadrático Médio ($`s^2 = \frac{SQ_R}{n - p}`$).
* **Resíduos Estudentizados Interna e Externamente:** Para padronizar o resíduo e permitir comparações, divide-se pelo seu erro padrão:
* *Internamente Estudentizado:*

```math
t_i = \frac{e_i}{\hat{\sigma} \sqrt{1 - h_i}}
```

* *Externamente Estudentizado (via técnica de Jackknife):*

```math
t_i^* = \frac{e_i}{\hat{\sigma}_{(i)} \sqrt{1 - h_i}}
```

onde $`\hat{\sigma}_{(i)}`$ é a estimativa da variância calculada sem a $`i`$ -ésima observação, impedindo que um *outlier* inflacione o denominador e disfarce sua própria discrepância.

###### *Medidas de Influência Global e Local*

* **Distância de Cook ($`D_i`$):** Avalia o impacto da remoção da $`i`$ -ésima observação sobre todo o vetor de parâmetros ajustados $`\hat{\beta}`$:

```math
D_i = \frac{(\hat{\beta} - \hat{\beta}_{(i)})^T X^T X (\hat{\beta} - \hat{\beta}_{(i)})}{p \hat{\sigma}^2} = \frac{t_i^2}{p} \left( \frac{h_i}{1 - h_i} \right)
```

Valores elevados (comparados aos quantis de uma distribuição $`F_{p, n-p}`$) indicam alta influência.
* **DFFITS e DFBETAS:** Medem, respectivamente, a alteração no valor ajustado individual e a variação percentual em cada coeficiente $`\beta_j`$ específico quando a $`i`$ -ésima observação é excluída.
* **Análise Confirmatória / Influência Global (** ***Case Deletion*** **):** Procedimento sistemático de remoção individual de casos para observar variações percentuais nas estimativas dos parâmetros e nos p-valores dos testes de hipótese estatística.
* **Gráfico de Envelope:** Técnica baseada em simulações de Monte Carlo que projeta bandas de confiança para os resíduos estudentizados ordenados, permitindo verificar visualmente se os resíduos aderem à distribuição teórica esperada.

##### 3. Exemplo Prático de Aplicação (Venda de Telhados)

Considerando o estudo de caso citado em aula, proveniente da literatura do Prof. Gilberto Paula:

* **Problema:** Explicar o total de metros quadrados de telhados de madeira vendidos ($`Y`$) em $`n = 26`$ filiais em função do número de clientes cadastrados ($`X`$).
* **Ajuste:** Modelo de regressão linear simples $`Y = \beta_0 + \beta_1 X + \epsilon`$.
* **Resultados do Diagnóstico:**
* A análise dos resíduos estudentizados e do **gráfico de envelope** não indicou violações graves da normalidade.
* O gráfico do elemento $`h_i`$ da diagonal da matriz $`H`$ identificou a **observação 23** ultrapassando o ponto de corte ($`2p/n`$), sendo classificada como ponto de alavanca (valor reduzido de clientes e de vendas).
* A **Distância de Cook** e a métrica **DFFITS** destacaram as **observações 5, 9 e 25** como altamente influentes. A observação 9 correspondia ao valor máximo da variável resposta $`Y`$, enquanto a observação 5 acumulava alto valor da preditora $`X`$ combinado a um valor intermediário de resposta.
* **Ação Corretiva:** Reestimar os parâmetros com a exclusão temporária das observações e avaliar a estabilidade dos coeficientes $`\hat{\beta}_0`$ e $`\hat{\beta}_1`$.

##### 4. Introdução aos Modelos Lineares Generalizados (MLG)

Propostos por Nelder e Wedderburn (1972), os MLGs ampliam a regressão linear clássica para situações em que a variável resposta não possui distribuição normal (e.g., dados de contagem, dados binários ou proporções).

###### *A Família Exponencial de Distribuições*

Uma variável aleatória $`Y_i`$ pertence à Família Exponencial se sua função de densidade (ou massa) de probabilidade puder ser escrita na forma:
```math
f(y_i; \theta_i, \phi) = \exp \left\{ \phi \left[ y_i \theta_i - b(\theta_i) \right ] + c(y_i, \phi) \right\}
```
onde:

* $`\theta_i`$ é o **parâmetro canônico** (relacionado à localização).
* $`\phi`$ é o **parâmetro de dispersão** (conhecido ou estimado).
* $`b(\theta_i)`$ é uma função estritamente diferenciável.

###### *Propriedades da Família Exponencial*

* **Média (Esperança):**

```math
EY_i = \mu_i = b'(\theta_i) = \frac{d b(\theta_i)}{d \theta_i}
```

* **Variância:**

```math
Var(Y_i) = \frac{b''(\theta_i)}{\phi} = \frac{V(\mu_i)}{\phi}
```

onde $`V(\mu_i)`$ é a **função de variância** , que estabelece como a variância de $`Y_i`$ depende de sua média $`\mu_i`$.

###### *Os Três Componentes do MLG*

1. **Componente Aleatório:** Variáveis respostas independentes $`Y_1, \dots, Y_n`$ com distribuição pertencente à Família Exponencial.
2. **Componente Sistemático:** O preditor linear $`\eta_i`$, que combina linearmente as covariáveis:

```math
\eta_i = x_i^T \beta = \beta_0 + \beta_1 x_{i1} + \dots + \beta_p x_{ip}
```

3. **Função de Ligação ($`g`$):** Uma função monótona e diferenciável que conecta a média da resposta $`\mu_i`$ ao preditor linear $`\eta_i`$:

```math
\eta_i = g(\mu_i)
```

###### *Funções de Ligação Canônicas e Funções de Variância*

A ligação é dita **canônica** quando o preditor linear é igualado diretamente ao parâmetro canônico da distribuição ($`\eta_i = \theta_i`$).

| Distribuição | Parâmetro de Dispersão ($`\phi`$) | Função de Variância $`V(\mu)`$ | Função de Ligação Canônica $`g(\mu) = \eta`$ | Nome da Ligação |
|---|---|---|---|---|
| Normal | $`1/\sigma^2`$ | $`1`$ | $`g(\mu) = \mu`$ | Identidade |
| Poisson | $`1`$ | $`\mu`$ | $`g(\mu) = \ln(\mu)`$ | Logarítmica |
| Binomial / Bernoulli | $`1`$ | $`\mu(1 - \mu)`$ | $`g(\mu) = \ln\left(\frac{\mu}{1 - \mu}\right)`$ | Logito |
| Gama | $`\nu`$ (parâmetro de forma) | $`\mu^2`$ | $`g(\mu) = \mu^{-1}`$ | Recíproca |
| Normal Inversa | $`\lambda`$ | $`\mu^3`$ | $`g(\mu) = \mu^{-2}`$ | Recíproca Quadrática |


#### Quiz: Teste de Verificação de Leitura

##### Perguntas

1. Qual é a principal diferença conceitual entre o foco da modelagem estatística tradicional e o foco do aprendizado de máquina?
2. O que caracteriza um ponto de alavanca ( *leverage point* ) em um modelo linear e qual ferramenta é utilizada para sua identificação?
3. Qual é a utilidade da versão externa do resíduo estudentizado em comparação com o resíduo ordinário?
4. O que mede a Distância de Cook e como seus valores elevados devem ser interpretados?
5. Como a análise confirmatória (ou influência global/exclusão de casos) é conduzida no diagnóstico de um modelo?
6. Qual é a função do gráfico de envelope na análise de diagnóstico de modelos lineares e generalizados?
7. Quais são os três componentes essenciais que definem a estrutura de um Modelo Linear Generalizado (MLG)?
8. Como se definem a esperança e a variância de uma variável resposta pertencente à Família Exponencial sob a parametrização adotada?
9. O que caracteriza uma função de ligação canônica em um MLG?
10. Quais são as funções de ligação canônicas e as funções de variância associadas às distribuições Poisson e Binomial?

##### Gabarito do Quiz

1. **Resposta:** A modelagem estatística visa interpretar a relação estrutural e inferencial entre preditoras e a resposta, exigindo a validação rigorosa de suposições probabilísticas. O aprendizado de máquina foca primordialmente no poder preditivo do algoritmo em novos dados, sem prezar necessariamente pela interpretabilidade dos parâmetros ou verificação de hipóteses distributivas.
2. **Resposta:** Um ponto de alavanca é uma observação que se afasta do centro de massa dos dados no espaço das variáveis explicativas ($`X`$). Ele é identificado através dos elementos da diagonal principal ($`h_i`$) da Matriz Chapéu $`H`$, sendo classificado como alavanca quando $`h_i > 2p/n`$.
3. **Resposta:** A versão externa do resíduo estudentizado calcula a estimativa da variância ($`\hat{\sigma}_{(i)}`$) reamostrando os dados sem a $`i`$ -ésima observação (técnica de Jackknife). Isso evita que uma observação com resíduo excessivamente alto inflacione a estimativa da variância do erro no denominador e mascarar sua própria atipicidade.
4. **Resposta:** A Distância de Cook mede o impacto global da remoção da $`i`$ -ésima observação sobre todo o vetor de coeficientes estimados ($`\hat{\beta}`$). Valores elevados indicam que o ponto possui forte influência na estimação do modelo, podendo alterar conclusões inferenciais.
5. **Resposta:** A análise confirmatória consiste no procedimento iterativo de excluir observações suspeitas uma a uma (ou em pequenos grupos) e reajustar o modelo. Avalia-se a variação percentual resultante nos coeficientes $`\hat{\beta}`$ e no p-valor dos testes de hipótese de cada preditora.
6. **Resposta:** O gráfico de envelope constrói bandas de confiança geradas por simulação para os resíduos ordenados sob a suposição de que o modelo está correto. Ele permite identificar visualmente se os resíduos observados seguem a distribuição teórica esperada ou se há desvios de especificação.
7. **Resposta:** Os três componentes são: o componente aleatório (variável resposta $`Y`$ pertencente à Família Exponencial), o componente sistemático (preditor linear $`\eta = X\beta`$) e a função de ligação $`g(\mu)`$, que conecta a média esperada da resposta ($`\mu`$) ao preditor linear.
8. **Resposta:** Para uma variável na Família Exponencial expressa por $`f(y_i; \theta_i, \phi) = \exp\{\phi y_i \theta_i - b(\theta_i) + c(y_i, \phi)\}`$, a esperança é dada pela primeira derivada $`EY_i = \mu_i = b'(\theta_i)`$. A variância é expressa por $`Var(Y_i) = \frac{b''(\theta_i)}{\phi} = \frac{V(\mu_i)}{\phi}`$, sendo $`V(\mu_i)`$ a função de variância.
9. **Resposta:** A função de ligação é dita canônica quando ela iguala diretamente o preditor linear ao parâmetro canônico de localização da distribuição, ou seja, $`\eta_i = \theta_i`$.
10. **Resposta:** Para a distribuição Poisson, a função de variância é $`V(\mu) = \mu`$ e a ligação canônica é a logarítmica ($`g(\mu) = \ln(\mu)`$). Para a distribuição Binomial, a função de variância é $`V(\mu) = \mu(1 - \mu)`$ e a ligação canônica é a logito ($`g(\mu) = \ln(\mu / (1 - \mu))`$).

#### Questões Discursivas (Formato Ensaio)

1. **Discussão sobre Conceitos de Diagnóstico:** Compare e contraste pontos de alavanca, pontos aberrantes e pontos influentes. Discuta sob quais condições um ponto de alavanca pode não se tornar um ponto influente em um modelo de regressão linear.
2. **Resíduos e Validação de Suposições:** Explique por que o resíduo ordinário $`e_i = y_i - \hat{y}_i`$ não é ideal para diagnosticar discrepâncias e justifique matematicamente a necessidade de utilização do resíduo estudentizado externo em processos automatizados de diagnóstico.
3. **Métricas Combinadas e Tomada de Decisão:** Um analista de dados encontra uma observação com alto valor de $`h_i`$, porém com uma Distância de Cook próxima de zero. Explique a interpretação geométrica e estatística desse achado e proponha a conduta adequada em relação a esse dado.
4. **Generalização dos Modelos Lineares:** Justifique a necessidade histórica e teórica do surgimento dos Modelos Lineares Generalizados (MLG) para além do Modelo Linear Geral. Como a introdução da função de ligação e da função de variância resolve limitações relativas a variáveis de contagem e binárias?
5. **A Família Exponencial de Distribuições:** Deduza formalmente a média $`EY_i`$ e a variância $`Var(Y_i)`$ de uma distribuição da Família Exponencial a partir das propriedades de derivados da função de log-verossimilhança em relação ao parâmetro canônico $`\theta_i`$.

#### Glossário Abrangente

* **Análise Confirmatória (** ***Case Deletion*** **/ Exclusão de Casos):** Método de diagnóstico estatístico baseado na exclusão sistemática de observações individuais ou em bloco para mensurar a sensibilidade e estabilidade das estimativas dos parâmetros do modelo.
* **DFBETAS:** Métrica de diagnóstico individual que mede a alteração padronizada no valor de um coeficiente de regressão específico ($`\hat{\beta}_j`$) ocasionada pela remoção de determinada observação.
* **DFFITS:** Métrica de diagnóstico que quantifica a variação padronizada no valor ajustado ($`\hat{y}_i`$) quando a $`i`$ -ésima observação é excluída da estimação do modelo.
* **Distância de Cook ($`D_i`$):** Medida agregada de influência que avalia o deslocamento conjunto de todas as estimativas do vetor $`\hat{\beta}`$ decorrente da omissão de uma observação específica.
* **Família Exponencial de Distribuições:** Classe flexível de distribuições de probabilidade (incluindo Normal, Poisson, Binomial, Gama e Normal Inversa) cujas funções de densidade ou massa podem ser expressas através de uma estrutura matemática comum parametrizada por parâmetros de localização e dispersão.
* **Função de Ligação ($`g(\mu)`$):** Função matemática monótona e diferenciável que relaciona a média condicional da variável resposta ($`\mu = EY`$) ao preditor linear ($`\eta = X\beta`$).
* **Função de Variância ($`V(\mu)`$):** Componente de um MLG que expressa a relação funcional exata entre a variância da variável resposta e a sua média esperada.
* **Gráfico de Envelope:** Ferramenta gráfica de diagnóstico baseada em simulação amostral que constrói limites teóricos de variação para os resíduos ordenados, auxiliando na verificação global da especificação do modelo.
* **Matriz Chapéu (** ***Hat Matrix*** **- $`H`$):** Matriz de projeção $`X(X^T X)^{-1} X^T`$ que mapeia o vetor de valores observados $`Y`$ no vetor de valores ajustados $`\hat{Y}`$.
* **Parâmetro Canônico ($`\theta`$):** Parâmetro de localização natural associado às distribuições da Família Exponencial; quando igualado diretamente ao preditor linear ($`\theta = \eta`$), define a função de ligação canônica.
* **Ponto Aberrante:** Observação cujo valor de resposta difere acentuadamente da previsão do modelo, gerando um resíduo de elevada magnitude.
* **Ponto de Alavanca (** ***Leverage Point*** **):** Observação que apresenta valores extremados ou isolados no espaço das variáveis explicativas ($`X`$), medido pelo elemento $`h_i`$ da diagonal da matriz chapéu.
* **Ponto Influente:** Observação cuja presença afeta desproporcionalmente os coeficientes da regressão, os testes de hipótese ou as inferências derivadas do modelo ajustado.
* **Preditor Linear ($`\eta`$):** Combinação linear dos parâmetros e das variáveis explicativas ($`\eta = X\beta`$) utilizada em modelos de regressão.
* **Resíduo Estudentizado Externamente:** Resíduo padronizado cuja estimativa da variância residual é calculada com a exclusão da própria observação analisada, reduzindo o viés do *outlier* sobre o erro padrão.
