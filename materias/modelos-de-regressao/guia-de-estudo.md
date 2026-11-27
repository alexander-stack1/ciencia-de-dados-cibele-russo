> Material derivado das aulas da Profa. Cibele Russo (ICMC/USP), organizado com auxílio do NotebookLM. [↩ Voltar à trilha](README.md)

# Guia de Estudo: Inferência em Modelos Lineares e Modelos Lineares Generalizados (MLGs)

## 1. Questionário de Avaliação (Quiz)

1. Qual é a condição necessária e suficiente descrita no Teorema do Caso 3 para que o estimador de mínimos quadrados ordinários ($`\hat{\beta}`$) seja equivalente ao estimador de mínimos quadrados generalizados ($`\tilde{\beta}`$)?
2. Como se caracteriza o Caso 4 dos modelos de regressão linear e quais são as limitações inferenciais decorrentes da ausência de normalidade nos erros?
3. Como é definida a estrutura geral da função de densidade ou massa de probabilidade de uma variável resposta Y pertencente à família exponencial de distribuições?
4. O que é a função de variância $`V(\mu)`$ no contexto dos Modelos Lineares Generalizados (MLGs) e como ela se relaciona com o parâmetro de dispersão $`\phi`$?
5. De que forma o preditor linear $`\eta_i`$ e a função de ligação $`g(\mu_i)`$ se articulam para estruturar um Modelo Linear Generalizado?
6. O que caracteriza uma função de ligação canônica em um MLG e quais são as ligações canônicas associadas às distribuições Normal, Poisson e Bernoulli?
7. Qual é a definição conceitual e matemática da função desvio (deviance) e de que maneira ela compara o modelo ajustado ao modelo saturado?
8. Por que a acurácia se mostra uma métrica inadequada para diagnosticar a qualidade de modelos de regressão logística em cenários de dados desbalanceados e quais métricas alternativas são recomendadas?
9. Como se diagnostica o fenômeno da sobredispersão em modelos de regressão Poisson e qual distribuição é indicada para contornar essa limitação?
10. Qual é a interpretação prática do coeficiente exponenciado $`\exp(\hat{\beta}_j)`$ em um modelo Poisson com ligação logarítmica comparada à sua interpretação em um modelo de regressão logística?

## 2. Gabarito do Questionário

1. O estimador de mínimos quadrados ordinários é equivalente ao estimador de mínimos quadrados generalizados no Caso 3 se e somente se existir uma matriz F não singular tal que $`\Sigma X = XF`$. Essa condição permite utilizar a expressão de mínimos quadrados ordinários — fundamentada unicamente nos dados observados — sem depender dos parâmetros desconhecidos contidos na matriz de variâncias e covariâncias cheia $`\Sigma`$. Dessa forma, viabiliza-se a estimação consistente de $`\beta`$ quando a matriz $`\Sigma`$ é desconhecida, porém positiva definida.

2. O Caso 4 caracteriza-se por erros com distribuição desconhecida (sem a suposição de normalidade), embora mantendo a suposição de ausência de correlação e variância constante. Nesse cenário, é possível obter os estimadores de mínimos quadrados de $`\beta`$ e empregar o quadrado médio do resíduo para estimar $`\sigma^2`$. Contudo, perdem-se os resultados exatos de distribuição (como a distribuição qui-quadrado para a razão da variância residual), limitando certos testes de hipóteses exatos.

3. A família exponencial de distribuições abrange modelos cuja densidade ou massa de probabilidade assume a forma $`f(y; \theta, \phi) = \exp\{\phi [y\theta - b(\theta)] + c(y, \phi)\}`$, na qual $`\theta`$ representa o parâmetro de localização/natural e $`\phi`$ o parâmetro de dispersão. Nessa formulação, $`b(\theta)`$ e $`c(y, \phi)`$ são funções conhecidas e diferenciáveis, com $`\phi > 0`$. O valor esperado da variável resposta é derivado diretamente por $`E[Y] = \mu = b'(\theta)`$, estabelecendo a ligação funcional entre $`\theta`$ e a média $`\mu`$.

4. A função de variância $`V(\mu)`$ expressa a dependência funcional direta entre a variância da variável resposta e a sua média $`\mu`$, sendo matematicamente definida pela derivada segunda $`V(\mu) = b''(\theta) = d\mu/d\theta`$. A variância total de Y é dada pela relação $`\operatorname{Var}(Y) = \phi^{-1} V(\mu)`$, na qual $`\phi`$ atua como o parâmetro de precisão ou dispersão. Por exemplo, na distribuição Poisson tem-se $`V(\mu) = \mu`$, enquanto na distribuição Bernoulli obtém-se $`V(\mu) = \mu(1-\mu)`$.

5. O preditor linear $`\eta_i`$ é a combinação linear das variáveis explicativas e dos parâmetros desconhecidos, expresso por $`\eta_i = X_i^T \beta = \beta_0 + \beta_1 X_{1i} + \dots + \beta_p X_{pi}`$. A função de ligação $`g(\mu_i)`$ é uma função monótona e diferenciável que conecta a média esperada da resposta $`\mu_i`$ ao preditor linear, tal que $`\eta_i = g(\mu_i)`$. Essa estrutura permite modelar respostas com restrições de suporte (como contagens ou proporções) sem violar os limites do espaço paramétrico.

6. A função de ligação canônica ocorre quando a função de ligação $`g(\mu_i)`$ iguala diretamente o preditor linear $`\eta_i`$ ao parâmetro natural $`\theta_i`$ da família exponencial. Para a distribuição Normal, a ligação canônica é a identidade ($`\eta = \mu`$); para a Poisson, é a logarítmica ($`\eta = \ln \mu`$); e para a Bernoulli/Binomial, é a função logito ($`\eta = \ln(\mu / (1-\mu))`$). O uso da ligação canônica simplifica as derivadas da verossimilhança e garante a unicidade do estimador de máxima verossimilhança.

7. A função desvio (deviance) é definida como duas vezes a diferença entre a log-verossimilhança do modelo saturado e a log-verossimilhança do modelo ajustado, isto é, $`D^* = 2[L(y; y) - L(\hat{\mu}; y)]`$. O modelo saturado aloca um parâmetro para cada observação ($`\hat{\mu}_i = y_i`$), representando o ajuste ideal com o máximo de informação possível. Assim, valores pequenos da função desvio indicam que o modelo proposto, com menos parâmetros, é capaz de explicar os dados de forma equivalente ao modelo superparametrizado.

8. Em bases desbalanceadas, onde a classe majoritária domina a amostra, um classificador simplório que atribua todas as observações à classe majoritária obterá uma acurácia elevada, mas falhará em identificar os eventos da classe minoritária de interesse. Essa métrica gera uma falsa sensação de adequação do ajuste estatístico em problemas como análise de fraude ou inadimplência. Para contornar essa limitação, recomendam-se métricas baseadas na matriz de confusão como o Coeficiente de Correlação de Matthews (MCC), a Média Geométrica (G-Mean) e o índice Kappa de Cohen.

9. A sobredispersão em modelos Poisson ocorre quando a variância empírica dos dados excede a média esperada, sendo identificada por resíduos desalinhados fora das bandas de confiança do gráfico de envelope ou por razões elevadas entre o desvio e os graus de liberdade. Esse desajuste indica que a suposição de igualdade entre média e variância ($`Var(Y) = \mu`$) foi violada. Para corrigi-lo, adota-se o modelo Binomial Negativo, que introduz um parâmetro adicional de dispersão $`\phi`$, expandindo a variância para $`\operatorname{Var}(Y) = \mu + \mu^2 \phi`$.

10. No modelo Poisson com ligação logarítmica, a exponenciação $`\exp(\hat{\beta}_j)`$ representa a Razão de Taxas, indicando que o incremento de uma unidade na covariável $`X_j`$ multiplica a resposta média esperada $`\mu`$ pelo fator $`\exp(\hat{\beta}_j)`$. No modelo de regressão logística, $`\exp(\hat{\beta}_j)`$ corresponde à Razão de Chances (odds ratio), representando o fator multiplicativo sobre a chance de sucesso $`\mu`$/(1-$`\mu`$). Dessa forma, enquanto na Poisson o efeito é multiplicativo direto sobre a taxa esperada, na regressão logística ele atua sobre a razão entre probabilidades de sucesso e fracasso.

## 3. Questões para Desenvolvimento (Ensaio)

1. Discuta a transição teórica e prática dos Modelos Lineares Ordinários para os Modelos Lineares Generalizados (MLGs). Na sua resposta, aborde como a flexibilização do pressuposto de normalidade e a introdução da função de ligação e da família exponencial expandem a capacidade de modelagem estatística para variáveis discretas, contagens e respostas estritamente positivas.

2. Compare o método de estimação de parâmetros por Mínimos Quadrados Ordinários em modelos lineares ao método de Máxima Verossimilhança via algoritmo Fisher Scoring (Mínimos Quadrados Reponderados Iterativos - IWLS) em MLGs. Detalhe o papel da matriz de pesos W, a necessidade do processo iterativo e o impacto da escolha da função de ligação canônica sobre a convergência do algoritmo e a unicidade dos estimadores.

3. Avalie o papel da Função Desvio (Deviance) e da análise de resíduos (componente do desvio e gráficos de envelope) no diagnóstico da qualidade do ajuste em MLGs. Explique como os testes de razão de verossimilhanças baseados na redução do desvio permitem comparar modelos aninhados, exemplificando com a comparação entre o modelo nulo e o modelo proposto.

4. Analise o problema da sobredispersão em modelos para dados de contagem. Explique as limitações teóricas do modelo Poisson diante da sobredispersão, como a distribuição Binomial Negativa estrutura a sua função de variância para contornar essa deficiência e quais são as consequências inferenciais de se ignorar a sobredispersão em uma análise estatística.

5. Em contextos de classificação binária com dados desbalanceados (como na modelagem de risco de crédito ou detecção de fraudes), discuta por que métricas tradicionais baseadas na acurácia falham. Sintetize as métricas alternativas de desempenho (MCC, G-Mean, Kappa) e discuta os impactos da escolha de funções de ligação alternativas (como probito ou complemento log-log) em relação ao modelo logito convencional.

## 4. Glossário Abrangente de Termos-Chave

| Termo | Definição |
|---|---|
| Acurácia | Proporção total de acertos de um modelo de classificação em relação ao total de observações. Revela-se uma métrica enganosa em conjuntos de dados desbalanceados. |
| Caso 1 (Regressão Linear) | Modelo linear de posto completo com erros normais, independentes e identicamente distribuídos, com média zero e matriz de variâncias e covariâncias $`\sigma^2 I_n`$. |
| Caso 2 (Regressão Linear) | Modelo linear com erros normais, média zero e matriz de variâncias e covariâncias $`\sigma^2`$ V, onde V é uma matriz conhecida que reflete erros correlacionados ou heterocedásticos. Resolvido via Mínimos Quadrados Ponderados ou Generalizados. |
| Caso 3 (Regressão Linear) | Modelo linear com erros normais, média zero e matriz de variâncias e covariâncias $`\Sigma`$ desconhecida e positiva definida. O estimador de mínimos quadrados ordinários equivale ao generalizado se e somente se existir F não singular tal que $`\Sigma X = XF`$. |
| Caso 4 (Regressão Linear) | Modelo linear no qual a distribuição dos erros é desconhecida (sem suposição de normalidade), mas com erros não correlacionados. Permite obter estimadores de mínimos quadrados de $`\beta`$, mas perde propriedades inferenciais exatas de distribuição. |
| Coeficiente de Determinação ($`R^2`$) | Proporção da variabilidade total da variável resposta explicada pelo modelo de regressão. Em MLGs ou aprendizado de máquina, costuma ser avaliado pelo ajuste linear entre os valores observados e os ajustados. |
| Condições de Regularidade | Conjunto de propriedades teóricas da função de log-verossimilhança que garantem que o valor esperado do vetor score seja zero (E[U($`\theta`$)] = 0) e que a variância do score seja igual à matriz de informação de Fisher. |
| Desvio Nulo (Null Deviance) | Valor da função desvio obtido ao se ajustar o modelo nulo (contendo apenas o termo de intercepto), servindo como base de comparação para medir o ganho de informação trazido pelas covariáveis no modelo proposto. |
| Envelope de Resíduos (Gráfico de Envelope) | Técnica de diagnóstico baseada em simulações de Monte Carlo sob o modelo ajustado, construindo bandas de confiança de 95% para os resíduos ordenados a fim de verificar a adequação da distribuição especificada. |
| Estimador de Mínimos Quadrados Generalizados (MQG) | Estimador utilizado quando a matriz de covariâncias dos erros não é escalar ($`\sigma^2 V`$). Exige a transformação dos dados ou o uso da ponderação pela matriz $`V^{-1}`$ ou $`\Sigma^{-1}`$. |
| Família Exponencial de Distribuições | Classe de distribuições de probabilidade cujas funções de densidade ou massa compartilham a forma $`f(y; \theta, \phi) = \exp\{\phi [y\theta - b(\theta)] + c(y, \phi)\}`$. Inclui as distribuições Normal, Poisson, Bernoulli, Binomial, Gama e Normal Inversa. |
| Fisher Scoring (IWLS) | Método iterativo numérico empregado para estimar os parâmetros $`\beta`$ em MLGs por Máxima Verossimilhança, utilizando a matriz de informação de Fisher esperada ($`K = X^T W X`$) no processo de atualização de Newton-Raphson. |
| Função de Ligação ($`g(\mu)`$) | Função monótona e diferenciável que relaciona a média esperada da variável resposta $`\mu_i`$ ao preditor linear $`\eta_i`$, permitindo acomodar restrições no suporte da variável resposta. |
| Função de Ligação Canônica | Função de ligação para a qual o preditor linear equivale diretamente ao parâmetro natural da família exponencial ($`\eta_i = \theta_i`$). Garante boas propriedades algébricas e a unicidade das estimativas de máxima verossimilhança. |
| Função de Variância ($`V(\mu)`$) | Expressão matemática dada por b''($`\theta`$) que define como a variância da variável resposta depende da sua média esperada $`\mu`$. |
| Função Desvio (Deviance) | Estatística de qualidade do ajuste definida por $`2[L(y; y) - L(\hat{\mu}; y)]`$, que mede o quão distante a log-verossimilhança do modelo proposto está da log-verossimilhança do modelo saturado. |
| Matriz Chapéu (H) | Matriz de projeção ortogonal dada por $`H = X(X^T X)^{-1} X^T`$ que mapeia o vetor de respostas observadas Y no vetor de valores ajustados $`\hat{Y}`$. Seus elementos diagonais $`h_{ii}`$ medem a alavancagem das observações. |
| Modelo Binomial Negativo | Modelo para dados de contagem que estende a distribuição Poisson ao incorporar um parâmetro extra de dispersão, tornando a variância uma função quadrática da média ($`\operatorname{Var}(Y) = \mu + \mu^2 \phi`$) e corrigindo a sobredispersão. |
| Modelo de Regressão Logística | MLG voltado para respostas binárias (Bernoulli/Binomial) que adota a função de ligação logito ($`\ln(\mu/(1-\mu))`$). Os coeficientes exponenciados $`\exp(\hat{\beta}_j)`$ possuem interpretação em termos de Razão de Chances. |
| Modelo Linear Generalizado (MLG) | Extensão dos modelos lineares que permite modelar variáveis resposta pertencentes à família exponencial, conectando a média $`\mu`$ ao preditor linear $`\eta`$ mediante uma função de ligação $`g(\cdot)`$. |
| Modelo Nulo | Modelo simplificado que contém unicamente a constante de intercepto ($`\beta_0`$), assumindo que nenhuma das covariáveis explicativas influencia a média da variável resposta. |
| Modelo Poisson | MLG para dados de contagem que assume a distribuição Poisson para a resposta e utiliza a função de ligação logarítmica, impondo a restrição de equidispersão (Var(Y) = E[Y] = $`\mu`$). |
| Modelo Saturado | Modelo conceitual superparametrizado que aloca um parâmetro para cada observação individual ($`\hat{\mu}_i = y_i`$), alcançando o ajuste perfeito e a máxima log-verossimilhança possível para o conjunto de dados. |
| Pontos Aberrantes (Outliers) | Observações que apresentam resíduos substancialmente elevados em valor absoluto em relação ao modelo ajustado, destacando-se da massa geral dos dados. |
| Pontos de Alavanca (Leverage Points) | Observações que possuem valores extremos ou afastados no espaço das variáveis explicativas (X), caracterizados por altos valores na diagonal da matriz chapéu ($`h_{ii}`$). |
| Pontos Influentes | Observações cuja remoção do conjunto de dados provoca alterações substanciais nas estimativas dos parâmetros $`\hat{\beta}`$ ou nas conclusões inferenciais do modelo. |
| Preditor Linear ($`\eta`$) | Estrutura linear definida pela combinação dos parâmetros e covariáveis, dada por $`\eta_i = X_i^T \beta`$. |
| Razão de Chances (Odds Ratio) | Medida de efeito obtida por $`\exp(\hat{\beta}_j)`$ na regressão logística, que expressa como a chance de ocorrência de um evento ($`\mu/(1-\mu)`$) é multiplicada ao se incrementar uma unidade na covariável $`X_j`$. |
| Razão de Taxas | Medida de efeito obtida por $`\exp(\hat{\beta}_j)`$ no modelo Poisson com ligação logarítmica, indicando o fator multiplicativo sobre a resposta média esperada $`\mu`$ por unidade de incremento na covariável $`X_j`$. |
| Resíduo Componente do Desvio | Tipo de resíduo em MLGs calculado a partir da contribuição individual de cada observação para a estatística de desvio total, sendo amplamente utilizado no diagnóstico de pontos discrepantes e na construção do envelope. |
| Sobredispersão (Superdispersão) | Fenômeno verificado quando a variância empírica dos dados observados excede a variância teórica prevista pelo modelo probabilístico assumido (frequente em modelos Poisson). |
