> Material derivado das aulas da Profa. Cibele Russo (ICMC/USP), organizado com auxílio do NotebookLM. [↩ Voltar à trilha](README.md)

# Guia de Estudos: Trilha Didática Avançada sobre Modelos de Regressão

## 0. Sumário Geral da Trilha e Visão Pedagógica

Esta trilha didática foi concebida sob o rigor matemático e a intuição estocástica necessários para a formação avançada em modelagem estatística em nível de pós-graduação. O objetivo central é conduzir o estudante desde a intuição da regressão linear simples até o domínio teórico e aplicado dos Modelos Lineares Generalizados (MLGs), consolidando a formulação matricial, distribuições amostrais multivariadas, estimação por Mínimos Quadrados Ordinários (MQO) e Máxima Verossimilhança (EMV), Análise de Variância (ANOVA), testes de hipóteses lineares e diagnósticos residuais.

Ao longo de 15 aulas encadeadas, projetadas para uma dedicação individual de 10 a 15 minutos por aula, você desenvolverá a capacidade de formalizar, estimar, testar e diagnosticar modelos estatísticos, estabelecendo a distinção clara entre a inferência estatística interpretável e o foco estritamente preditivo do Aprendizado de Máquina.

#### Estrutura Didática dos Módulos

| Módulo | Descrição do Módulo | Objetivos Centrais | Qtd. Aulas | Tempo Estimado Total |
|---|---|---|---|---|
| Módulo 1 | Introdução e Regressão Linear Simples | Compreender o papel das componentes estocásticas e determinísticas, o ajuste por MQO e a transição para a notação matricial. | 3 | 30 – 45 min |
| Módulo 2 | Álgebra Matricial e Fundamentos Probabilísticos | Dominar a Distribuição Normal Multivariada, as distribuições amostrais derivativas ($`\chi^2, t`$, F) e o Teorema de Cochran. | 3 | 30 – 45 min |
| Módulo 3 | Modelo Linear Geral de Posto Completo e Estimação | Formalizar o modelo $`\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}`$, derivar estimadores de MQO e EMV e analisar a geometria da Matriz Chapéu. | 3 | 30 – 45 min |
| Módulo 4 | Inferência, ANOVA e Diagnóstico em Modelos Lineares | Construir a Tabela de ANOVA, aplicar o Teste de Hipótese Linear Geral e contornar erros correlacionados via Mínimos Quadrados Generalizados (GLS). | 3 | 30 – 45 min |
| Módulo 5 | Modelos Lineares Generalizados (MLG) e Regressão Logística | Estender a resposta para a Família Exponencial, diagnosticar via Função Desvio (Deviance) e aplicar a Regressão Logística Binária. | 3 | 30 – 45 min |

> [!NOTE]
> Transição Narrativa: Nossa jornada começa desmistificando a relação entre variáveis no Módulo 1, compreendendo como a aleatoriedade e o comportamento médio se articulam para descrever os fenômenos do mundo real.

## Módulo 1: Introdução e Regressão Linear Simples

### Aula 1: O que é um Modelo de Regressão? (Variável Resposta vs. Preditora, Intuição e Componentes Estocásticos)

#### Objetivo Didático

Compreender a transição entre mecanismos determinísticos e estocásticos, identificando a variável resposta $`(Y_i)`$, a variável preditora $`(X_i)`$ e a natureza do erro aleatório $`(\epsilon_i)`$.

#### Intuição e Analogias

Pense na relação entre anos de experiência profissional e salário, ou tempo dedicado aos estudos e nota na prova. Se a natureza fosse puramente determinística, como a fórmula da física clássica $`F = m \cdot`$ a, duas pessoas com exatamente 5 anos de experiência ganhariam rigorosamente o mesmo salário até o último centavo. Na prática, porém, observamos uma variabilidade natural: os pontos flutuam ao redor de uma tendência central.

#### O modelo de regressão traduz essa realidade dividindo o fenômeno em duas componentes:

1. Componente Determinística: A tendência média do comportamento da resposta dada a preditora, expressa por $`E(Y_i \vert  X_i) = \beta_0 + \beta_1 X_i`$.
2. Componente Estocástica $`(\epsilon_i)`$: O erro aleatório não observável que capta flutuações individuais, variáveis omitidas e ruídos inerentes ao processo de amostragem.

#### Exemplo Numérico Passo a Passo

Considere um conjunto de dados brutos minimizado (n=4) relacionando Anos de Experiência $`(X_i)`$ e Salário em milhares de reais $`(Y_i)`$:

| Indivíduo (i) | Experiência $`(X_i)`$ | Salário $`(Y_i)`$ |
|---|---|---|
| 1 | 1 | 2 |
| 2 | 2 | 3 |
| 3 | 4 | 5 |
| 4 | 5 | 6 |

#### Cálculo das Médias Amostrais:

* $`\bar{X} = \frac{\sum_{i=1}^{4} X_i}{n} = \frac{1 + 2 + 4 + 5}{4} = \frac{12}{4} = 3{,}0`$
* $`\bar{Y} = \frac{\sum_{i=1}^{4} Y_i}{n} = \frac{2 + 3 + 5 + 6}{4} = \frac{16}{4} = 4{,}0`$

O centro de gravidade dos dados, dado pelo ponto centroidal $`(\bar{X}, \bar{Y})`$, é $`(3{,}0; 4{,}0)`$. Qualquer reta ajustada por mínimos quadrados passará obrigatoriamente por este ponto.

#### Erros Comuns de Iniciantes

* Confundir Correlação com Causalidade: Assumir que a associação linear entre $`X_i`$ e $`Y_i`$ implica relação de causa e efeito. A regressão modela a dependência funcional esperada, mas a causalidade exige um desenho experimental rigoroso.
* Tratar $`X_i`$ como Variável Aleatória no Modelo Clássico: No modelo linear clássico, assume-se que as preditoras $`X_i`$ são fixas ou controladas pelo pesquisador, estando a aleatoriedade concentrada estritamente no termo de erro $`\epsilon_i`$.
* Ignorar o Erro Estocástico $`(\epsilon_i)`$: Tentar forçar uma reta a passar exatamente por todos os pontos observados, esquecendo que o erro representa ruído não sistemático.

#### Resumo Conceitual

1. O modelo de regressão decompõe a variável resposta em uma componente determinística e um termo de erro estocástico $`\epsilon_i`$.
2. A variável resposta $`Y_i`$ é aleatória e observável, enquanto a preditora $`X_i`$ é tratada como fixa ou controlada.
3. O erro aleatório possui média zero $`(E(\epsilon_i) = 0)`$, representando desvios não observados que se anulam em média.

#### Exercícios Resolvidos

1. Exercício 1: Um pesquisador deseja modelar a quantidade de combustível consumida por um veículo com base na distância percorrida. Identifique a variável resposta (Y) e a variável preditora (X).
  * Gabarito Comentado: A variável resposta (dependente) é o Consumo de Combustível (Y), pois é o fenômeno cuja variação se deseja explicar. A variável preditora (independente ou explicativa) é a Distância Percorrida (X), fixada pelo trajeto analisado.
2. Exercício 2: Explique a diferença teórica entre o termo de erro populacional $`(\epsilon_i)`$ e o resíduo amostral $`(e_i)`$.
  * Gabarito Comentado: O erro populacional $`\epsilon_i = Y_i - (\beta_0 + \beta_1 X_i)`$ é não observável e representa o desvio real do indivíduo em relação à verdadeira reta populacional. O resíduo amostral $`e_i = Y_i - \hat{Y}_i = Y_i - (\hat{\beta}_0 + \hat{\beta}_1 X_i)`$ é calculável e mede o desvio entre o valor observado e a estimativa obtida pela amostragem.
3. Exercício 3: Se o modelo linear assume $`E(\epsilon_i \vert  X_i) = 0`$, qual é a expectativa condicional $`E(Y_i \vert  X_i)`$ para $`Y_i = \beta_0 + \beta_1 X_i + \epsilon_i?`$
  * Gabarito Comentado: Aplicando a expectativa condicional: $`E(Y_i \vert  X_i) = E(\beta_0 + \beta_1 X_i + \epsilon_i \vert  X_i) = \beta_0 + \beta_1 X_i + E(\epsilon_i \vert  X_i)`$. Como $`\beta_0, \beta_1`$ são parâmetros constantes e $`X_i`$ é fixo, com $`E(\epsilon_i \vert  X_i) = 0`$, obtemos $`E(Y_i \vert  X_i) = \beta_0 + \beta_1 X_i`$.

#### Referência Audiovisual

* EST5507 Modelos de Regressão - Apresentação da disciplina (PIPGEs) & Introdução à Inferência Estatística - Regressão Linear Simples (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Compreendida a natureza estocástica do modelo, precisamos de um critério matemático rigoroso para encontrar os melhores estimadores dos coeficientes que traçam a reta de tendência, tema desenvolvido na Aula 2.

### Aula 2: Ajuste da Reta por Mínimos Quadrados Simples (Estimadores $`\hat{\beta}_0`$ e $`\hat{\beta}_1`$, Interpretação e Exemplo Prático)

#### Objetivo Didático

Derivar e calcular os estimadores de Mínimos Quadrados Ordinários (MQO) para os parâmetros de intercepto $`(\beta_0)`$ e inclinação $`(\beta_1)`$, interpretando fisicamente seus valores numéricos.

#### Intuição e Analogias

Imagine fixar pregos em um plano cartesiano nas coordenadas de cada ponto amostral $`(X_i, Y_i)`$. Se passarmos uma régua rígida por entre esses pregos e conectarmos cada prego à régua por elásticos verticais idênticos, a régua se posicionará naturalmente na trajetória que minimiza a força de tensão total. Como a energia elástica acumulada em cada mola é proporcional ao quadrado da deformação, a régua busca minimizar a soma dos quadrados das distâncias verticais. Esse é o princípio do Método dos Mínimos Quadrados Ordinários (MQO): minimizar a Soma de Quadrados dos Resíduos $`(SQR = \sum e_i^2)`$.

#### Exemplo Numérico Passo a Passo

Utilizando os mesmos dados da Aula 1 (n=4), com $`\bar{X} = 3{,}0`$ e $`\bar{Y} = 4{,}0`$, estruturamos os desvios e produtos cruzados:

| i | $`X_i`$ | $`Y_i`$ | $`(X_i - \bar{X})`$ | $`(Y_i - \bar{Y})`$ | $`(X_i - \bar{X})^2`$ | $`(X_i - \bar{X})(Y_i - \bar{Y})`$ |
|---|---|---|---|---|---|---|
| 1 | 1 | 2 | -2 | -2 | 4 | 4 |
| 2 | 2 | 3 | -1 | -1 | 1 | 1 |
| 3 | 4 | 5 | 1 | 1 | 1 | 1 |
| 4 | 5 | 6 | 2 | 2 | 4 | 4 |
| Soma | 12 | 16 | 0 | 0 | 10 | 10 |

Cálculo da Inclinação $`(\hat{\beta}_1): \hat{\beta}_1 = \frac{\sum_{i=1}^{n} (X_i - \bar{X})(Y_i - \bar{Y})}{\sum_{i=1}^{n} (X_i - \bar{X})^2} = \frac{10}{10} = 1{,}0`$

Cálculo do Intercepto $`(\hat{\beta}_0): \hat{\beta}_0 = \bar{Y} - \hat{\beta}_1 \bar{X} = 4{,}0 - (1{,}0 \cdot 3{,}0) = 1{,}0`$

#### Equação Ajustada: $`\hat{Y}_i = 1{,}0 + 1{,}0 \cdot X_i`$

Interpretação: Para cada aumento de 1 ano na experiência $`(X_i)`$, o salário médio estimado $`(\hat{Y}_i)`$ aumenta em $`1{,}0`$ mil R\$. O intercepto indica que um indivíduo com zero anos de experiência possui um salário base estimado de $`1{,}0`$ mil R\$.

#### Erros Comuns de Iniciantes

* Extrapolação Indevida: Interpretar o intercepto $`\hat{\beta}_0`$ em cenários onde $`X_i = 0`$ não possui significado físico (ex: peso de um indivíduo com altura zero) ou realizar predições fora do domínio de amostragem de X.
* Confundir Parâmetros e Estimadores: Escrever o modelo populacional utilizando estimadores $`(\hat{\beta}_0, \hat{\beta}_1)`$ ou omitir o termo de erro estocástico ao definir a relação teórica.
* Minimizar Distâncias Ortogonais: O método de MQO clássico minimiza os desvios estritamente verticais na direção do eixo $`Y_i (Y_i - \hat{Y}_i)`$, e não a distância ortogonal perpendicular dos pontos à reta.

#### Resumo Conceitual

1. Os estimadores de MQO minimizam a soma dos resíduos quadráticos: $`SQR = \sum (Y_i - \hat{\beta}_0 - \hat{\beta}_1 X_i)^2`$.
2. A inclinação estimada $`\hat{\beta}_1`$ mede a taxa de variação média esperada na resposta por unidade de variação da preditora.
3. A reta ajustada passa obrigatoriamente pelo centroide dos dados $`(\bar{X}, \bar{Y})`$.

#### Exercícios Resolvidos

1. Exercício 1: Dado um conjunto de dados com $`\sum (X_i - \bar{X})(Y_i - \bar{Y}) = 45, \sum (X_i - \bar{X})^2 = 15, \bar{X} = 5`$ e $`\bar{Y} = 12`$, calcule os coeficientes $`\hat{\beta}_1`$ e $`\hat{\beta}_0`$.
  * Gabarito Comentado: $`\hat{\beta}_1 = \frac{45}{15} = 3{,}0 \hat{\beta}_0 = \bar{Y} - \hat{\beta}_1 \bar{X} = 12 - (3{,}0 \cdot 5) = 12 - 15 = -3{,}0`$ A reta ajustada é $`\hat{Y} = -3{,}0 + 3{,}0 X`$.
2. Exercício 2: Interprete o coeficiente $`\hat{\beta}_1 = 2{,}5`$ em um modelo onde Y é o peso de uma planta (gramas) e X é a dosagem de fertilizante (litros).
  * Gabarito Comentado: O valor $`\hat{\beta}_1 = 2{,}5`$ indica que, em média, para cada litro adicional de fertilizante aplicado, o peso esperado da planta aumenta em 2,5 gramas.
3. Exercício 3: Utilizando o modelo ajustado $`\hat{Y} = 1{,}0 + 1{,}0 X`$, determine a previsão pontual do salário $`(\hat{Y})`$ para um profissional com 3 anos de experiência.
  * Gabarito Comentado: Substituindo $`X = 3: \hat{Y} = 1{,}0 + 1{,}0(3) = 4{,}0 (4`$ mil R\$). O valor previsto coincide com a média amostral $`\bar{Y}`$, pois $`X = \bar{X} = 3`$.

#### Referência Audiovisual

* Introdução à Inferência Estatística - Regressão Linear Simples & Modelo linear simples - matricialmente SME0812 (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Manipular equações algébricas escalares torna-se inviável ao adicionar múltiplas preditoras. Na Aula 3, escalaremos a formulação do modelo para a Notação Matricial.

### Aula 3: Formulação Matricial do Modelo Linear (Vetores $`\mathbf{y}`$, Matriz de Planejamento $`\mathbf{X}, \mathbf{\beta}`$ e Erros)

#### Objetivo Didático

Traduzir um sistema de equações lineares individuais de qualquer dimensão para a representação matricial compacta $`\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}`$, dominando as dimensões e estruturas das componentes.

#### Intuição e Analogias

Pense na notação matricial como uma "planilha de dados inteligente" conectada a um motor de cálculo vetorial. Em vez de escrever n equações escalares separadas no papel, empilhamos todas as respostas em uma coluna vertical $`(\mathbf{y})`$ e organizamos todas as características observadas em colunas paralelas em um bloco retangular (a Matriz de Planejamento $`\mathbf{X}`$). Com uma única operação matricial, processamos simultaneamente milhares de observações e dezenas de variáveis explicativas.

#### Exemplo Numérico Passo a Passo

Considere n=3 observações e p=1 variável preditora $`(X_1)`$. O modelo escalar individual é $`Y_i = \beta_0 + \beta_1 X_{1i} + \epsilon_i`$, para i=1, 2, 3. Dados brutos: $`(X_{11}=1, Y_1=3), (X_{12}=2, Y_2=5), (X_{13}=3, Y_3=6)`$.

#### Montagem de Vetores e Matrizes:

* Vetor Resposta $`(\mathbf{y}_{3 \times 1}): \mathbf{y} = \begin{bmatrix} 3 \\ 5 \\ 6 \end{bmatrix}`$
* Matriz de Planejamento $`(\mathbf{X}_{3 \times 2})`$: A primeira coluna é composta por 1s para incorporar o intercepto $`\beta_0. \mathbf{X} = \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix}`$
* Vetor de Parâmetros $`(\mathbf{\beta}_{2 \times 1}): \mathbf{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \end{bmatrix}`$
* Vetor de Erros $`(\mathbf{\epsilon}_{3 \times 1}): \mathbf{\epsilon} = \begin{bmatrix} \epsilon_1 \\ \epsilon_2 \\ \epsilon_3 \end{bmatrix}`$

Cálculo da Matriz $`\mathbf{X}^T \mathbf{X}: \mathbf{X}^T = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix}`$

```math
\mathbf{X}^T \mathbf{X} = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix} \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix} = \begin{bmatrix} 1+1+1 & 1+2+3 \\ 1+2+3 & 1^2+2^2+3^2 \end{bmatrix} = \begin{bmatrix} 3 & 6 \\ 6 & 14 \end{bmatrix}
```

#### Erros Comuns de Iniciantes

* Omitir a Coluna de Unidades (1s) na Matriz $`\mathbf{X}`$: Esquecer de inserir a primeira coluna de 1s na Matriz de Planejamento ao ajustar modelos que possuem intercepto $`(\beta_0)`$.
* Incompatibilidade de Dimensões: Tentar multiplicar matrizes cujas dimensões internas não coincidem. Para multiplicar $`\mathbf{A}_{m \times n} \times \mathbf{B}_{n \times p}`$, o número de colunas de $`\mathbf{A}`$ deve ser igual ao número de linhas de $`\mathbf{B}`$.
* Confundir Vetores Linha e Coluna: Tratar o vetor de respostas $`\mathbf{y}`$ como um vetor linha $`(1 \times n)`$ em vez do vetor coluna $`(n \times 1)`$.

#### Resumo Conceitual

1. A representação matricial do modelo linear geral é $`\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}`$.
2. Para n observações e p preditoras (com intercepto), a dimensão da Matriz de Planejamento $`\mathbf{X}`$ é $`n \times (p+1)`$.
3. O produto $`\mathbf{X}^T \mathbf{X}`$ resulta em uma matriz quadrada de ordem $`(p+1) \times (p+1)`$, sempre simétrica.

#### Exercícios Resolvidos

1. Exercício 1: Monte o vetor $`\mathbf{y}`$ e a matriz $`\mathbf{X}`$ para um conjunto de n=2 observações e p=2 preditoras ($`X_1`$ e $`X_2`$): Indivíduo 1: Y=10, $`X_1=2, X_2=5`$; Indivíduo 2: Y=15, $`X_1=4, X_2=8`$.
  * Gabarito Comentado: $`\mathbf{y} = \begin{bmatrix} 10 \\ 15 \end{bmatrix}_{2 \times 1}, \quad \mathbf{X} = \begin{bmatrix} 1 & 2 & 5 \\ 1 & 4 & 8 \end{bmatrix}_{2 \times 3}`$
2. Exercício 2: Para uma matriz de planejamento $`\mathbf{X}`$ de dimensão $`100 \times 4`$, quais são as dimensões do vetor $`\mathbf{\beta}`$, da matriz $`\mathbf{X}^T \mathbf{X}`$ e do vetor $`\mathbf{y}?`$
  * Gabarito Comentado: Com n=100 e p+1=4:
    * Vetor de parâmetros $`\mathbf{\beta}`$: dimensão $`4 \times 1`$.
    * Matriz $`\mathbf{X}^T \mathbf{X}`$: dimensão $`(4 \times 100) \times (100 \times 4) = 4 \times 4`$.
    * Vetor de resposta $`\mathbf{y}`$: dimensão $`100 \times 1`$.
3. Exercício 3: Expanda a segunda linha do sistema matricial $`\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}`$ considerando $`\mathbf{X} = \begin{bmatrix} 1 & X_{11} \\ 1 & X_{12} \end{bmatrix}`$ e $`\mathbf{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \end{bmatrix}`$.
  * Gabarito Comentado: Multiplicando a segunda linha de $`\mathbf{X}`$ por $`\mathbf{\beta}`$ e somando o erro correspondente: $`Y_2 = \begin{bmatrix} 1 & X_{12} \end{bmatrix} \begin{bmatrix} \beta_0 \\ \beta_1 \end{bmatrix} + \epsilon_2 \implies Y_2 = \beta_0 + \beta_1 X_{12} + \epsilon_2`$

#### Referência Audiovisual

* EST5507 Modelos de Regressão - Notação Matricial, Vetores aleatórios (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Dominada a estrutura matricial dos dados, precisamos construir a base probabilística multivariada para realizar testes de hipóteses sobre os parâmetros. Iniciaremos o Módulo 2 abordando a Distribuição Normal Multivariada.

## Módulo 2: Álgebra Matricial e Fundamentos Probabilísticos

### Aula 4: A Distribuição Normal Multivariada (Vetor de Médias $`\mathbf{\mu}`$, Matriz $`\mathbf{\Sigma}`$ e Propriedades)

#### Objetivo Didático

Apresentar a extensão da distribuição Normal para p dimensões, detalhando o vetor de médias $`\mathbf{\mu}`$, a matriz de variâncias-covariâncias $`\mathbf{\Sigma}`$ e suas propriedades geométricas e algébricas.

#### Intuição e Analogias

Enquanto a distribuição Normal univariada possui a forma de uma curva em sino bidimensional, a Normal Bivariada se assemelha a uma montanha de areia tridimensional sobre um plano. Cortando essa montanha com planos horizontais paralelos ao chão, observamos curvas de nível elípticas. O pico da montanha é centrado no Vetor de Médias $`\mathbf{\mu}`$. A dispersão e a orientação geográfica dessa superfície dependem das variâncias e das covariâncias contidas na Matriz $`\mathbf{\Sigma}`$.

```text
           Visão Tridimensional (3D)                     Visão Superior (Curvas de Nível)
                   /\                                               .---.
                  /  \                                            .'     '.
                 /    \                                          /   (µ)   \
               /        \                                        '.       .'
             /            \                                        '---'
    --------'--------------'--------                            Contornos Elípticos
```

#### Exemplo Numérico Passo a Passo

Considere um vetor aleatório bivariado $`\mathbf{x} = \begin{bmatrix} X_1 \\ X_2 \end{bmatrix} \sim N_2(\mathbf{\mu}, \mathbf{\Sigma})`$, com: $`\mathbf{\mu} = \begin{bmatrix} 2 \\ 5 \end{bmatrix}, \quad \mathbf{\Sigma} = \begin{bmatrix} 4 & 1 \\ 1 & 9 \end{bmatrix}`$

#### Extração de Parâmetros:

* Média de $`X_1: E(X_1) = \mu_1 = 2`$
* Média de $`X_2: E(X_2) = \mu_2 = 5`$
* Variância de $`X_1: Var(X_1) = \sigma_1^2 = 4 \implies \sigma_1 = \sqrt{4} = 2`$
* Variância de $`X_2: Var(X_2) = \sigma_2^2 = 9 \implies \sigma_2 = \sqrt{9} = 3`$
* Covariância entre $`X_1`$ e $`X_2: Cov(X_1, X_2) = \sigma_{12} = 1`$

Cálculo da Correlação $`(\rho_{12}): \rho_{12} = \frac{\sigma_{12}}{\sigma_1 \cdot \sigma_2} = \frac{1}{2 \cdot 3} = \frac{1}{6} \approx 0{,}1667`$

#### Erros Comuns de Iniciantes

* Generalizar Covariância Nula para Qualquer Distribuição: Na Normal Multivariada, $`Cov(X_i, X_j) = 0`$ implica independência probabilística. Contudo, essa equivalência não se aplica a distribuições não gaussianas genéricas.
* Utilizar Matrizes Não Positivas Definidas: Tentar definir $`\mathbf{\Sigma}`$ com determinante nulo ou negativo, o que impede a inversão $`(\mathbf{\Sigma}^{-1})`$ necessária na densidade Normal Multivariada.

#### Resumo Conceitual

1. A densidade Normal Multivariada de $`\mathbf{x}_{p \times 1}`$ é completamente especificada pelo vetor de médias $`\mathbf{\mu}_{p \times 1}`$ e pela matriz de variâncias-covariâncias $`\mathbf{\Sigma}_{p \times p}`$.
2. Superfícies de densidade constante formam elipsoides centrados em $`\mathbf{\mu}`$, definidos pelos autovalores e autovetores de $`\mathbf{\Sigma}`$.
3. Se $`\mathbf{x} \sim N_p(\mathbf{\mu}, \mathbf{\Sigma})`$, qualquer transformação linear $`\mathbf{A}\mathbf{x} + \mathbf{b}`$ também segue distribuição Normal Multivariada.

#### Exercícios Resolvidos

1. Exercício 1: Dada a matriz $`\mathbf{\Sigma} = \begin{bmatrix} 16 & -4 \\ -4 & 25 \end{bmatrix}`$, calcule o coeficiente de correlação $`\rho_{12}`$.
  * Gabarito Comentado: $`\sigma_1 = \sqrt{16} = 4, \sigma_2 = \sqrt{25} = 5, \sigma_{12} = -4. \rho_{12} = \frac{-4}{4 \cdot 5} = \frac{-4}{20} = -0{,}20`$
2. Exercício 2: Seja $`\mathbf{x} \sim N_2(\mathbf{\mu}, \mathbf{\Sigma})`$. Se $`Y = X_1 + X_2`$, determine a distribuição de Y.
  * Gabarito Comentado: Definindo $`\mathbf{A} = \begin{bmatrix} 1 & 1 \end{bmatrix}`$, temos $`Y = \mathbf{A}\mathbf{x}`$.
    * Média: $`E(Y) = \mathbf{A}\mathbf{\mu} = \mu_1 + \mu_2`$.
    * Variância: $`Var(Y) = \mathbf{A}\mathbf{\Sigma}\mathbf{A}^T = \begin{bmatrix} 1 & 1 \end{bmatrix} \begin{bmatrix} \sigma_1^2 & \sigma_{12} \\ \sigma_{12} & \sigma_2^2 \end{bmatrix} \begin{bmatrix} 1 \\ 1 \end{bmatrix} = \sigma_1^2 + \sigma_2^2 + 2\sigma_{12}`$. Como é uma transformação linear, $`Y \sim N(\mu_1 + \mu_2, \sigma_1^2 + \sigma_2^2 + 2\sigma_{12})`$.
3. Exercício 3: Se $`\mathbf{x} \sim N_3(\mathbf{\mu}, \mathbf{\Sigma})`$ possui matriz $`\mathbf{\Sigma}`$ diagonal, qual é a relação entre as componentes $`X_1, X_2, X_3?`$
  * Gabarito Comentado: Uma matriz $`\mathbf{\Sigma}`$ diagonal possui covariâncias fora da diagonal nulas $`(\sigma_{ij} = 0, \forall i \neq j)`$. Sob a Normal Multivariada, covariância zero garante independência mútua entre $`X_1, X_2`$ e $`X_3`$.

#### Referência Audiovisual

* EST5507 Modelos de Regressão - Distribuição Normal Multivariada & Propriedades e resultados da Distribuição Normal Multivariada (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Com a distribuição Normal Multivariada consolidada, podemos derivar as distribuições amostrais quadráticas ($`\chi^2, t`$, F) indispensáveis para a inferência estatística, abordadas na Aula 5.

### Aula 5: Distribuições Amostrais e Inferenciais (Chi-quadrado, t de Student, F de Snedecor Centrais e Não-Centrais)

#### Objetivo Didático

Apresentar a construção e as inter-relações entre as distribuições Chi-quadrado $`(\chi^2), t`$ de Student e F de Snedecor, introduzindo os parâmetros de não-centralidade $`(\lambda)`$ para avaliações sob a hipótese alternativa $`(H_1)`$.

#### Intuição e Analogias

#### Pense nessas distribuições como instrumentos de medição:

* Chi-quadrado $`(\chi^2)`$: É o "acumulador de ruídos quadráticos". Somando os quadrados de variáveis normais padronizadas independentes, a variável resultante acumula energia segundo uma distribuição $`\chi^2`$.
* t de Student: É a distribuição ideal para pequenas amostras quando a verdadeira variância populacional é desconhecida e estimada pela amostra.
* F de Snedecor: É a "balança de comparação de variâncias". Ela mede a razão entre duas variáveis $`\chi^2`$ independentes divididas pelos seus graus de liberdade. Se o sinal explicativo superação o ruído residual, a balança F pende para a direita.

> [!NOTE]
> Contexto Histórico: A distribuição t foi desenvolvida por William Sealy Gosset em 1908 enquanto trabalhava como químico e estatístico na cervejaria Guinness em Dublin. Como a empresa proibia a publicação de segredos industriais por funcionários, Gosset publicou seu trabalho científico sob o pseudônimo "Student", resolvendo o problema prático de controle de qualidade em pequenas amostras de cevada quando o desvio padrão populacional $`\sigma`$ era desconhecido.

#### Exemplo Numérico Passo a Passo

Sejam $`U_1 \sim \chi^2(3)`$ e $`U_2 \sim \chi^2(10)`$ duas variáveis aleatórias independentes com distribuições Chi-quadrado centrais. Definimos a variável W: $`W = \frac{U_1 / 3}{U_2 / 10}`$

Por definição, $`W \sim F(3`$, 10), apresentando $`\nu_1 = 3`$ graus de liberdade no numerador e $`\nu_2 = 10`$ graus de liberdade no denominador.

#### Propriedades dos Graus de Liberdade e Esperança:

| Propriedade | Numerador $`(U_1)`$ | Denominador $`(U_2)`$ | Estatística $`W \sim F(3`$, 10) |
|---|---|---|---|
| Graus de Liberdade $`(\nu)`$ | $`\nu_1 = 3`$ | $`\nu_2 = 10`$ | F(3, 10) |
| Valor Esperado Indiv. (E[U]) | E[U_1] = 3 | E[U_2] = 10 | E[W] = $`\frac{\nu_2}{\nu_2 - 2} = \frac{10}{8} = 1{,}25`$ (para $`\nu_2 > 2`$) |

#### Erros Comuns de Iniciantes

* Inverter Graus de Liberdade no Teste F: Trocar os graus de liberdade do numerador $`(\nu_1)`$ com os do denominador $`(\nu_2)`$, alterando quantis críticos e p-valores.
* Negligenciar o Parâmetro de Não-Centralidade $`(\lambda)`$: Assumir que sob $`H_1`$ as estatísticas mantêm distribuições centrais, ignorando que o parâmetro $`\lambda`$ desloca a distribuição e determina o poder estatístico do teste $`(1 - \beta)`$.

#### Resumo Conceitual

1. A soma dos quadrados de n variáveis N(0,1) independentes segue uma distribuição Chi-quadrado central $`\chi^2(n)`$.
2. O quadrado de uma variável t de Student com $`\nu`$ graus de liberdade é idêntico a uma variável F de Snedecor com 1 e $`\nu`$ graus de liberdade: $`t^2(\nu) = F(1, \nu)`$.
3. Sob $`H_1`$, as estatísticas seguem distribuições Não-Centrais, onde o parâmetro $`\lambda`$ desvia a massa de probabilidade para a direita.

#### Exercícios Resolvidos

1. Exercício 1: Se $`Z_1, Z_2, Z_3, Z_4 \stackrel{i.i.d.}{\sim} N(0,1)`$, qual é a distribuição da variável $`V = \sum_{i=1}^{4} Z_i^2?`$
  * Gabarito Comentado: Sendo a soma do quadrado de 4 variáveis Normal Padrão independentes, V segue rigorosamente uma distribuição Chi-quadrado central com 4 graus de liberdade: $`V \sim \chi^2(4)`$.
2. Exercício 2: Mostre que se $`T \sim t(\nu)`$, então $`T^2 \sim F(1, \nu)`$.
  * Gabarito Comentado: Por definição, $`T = \frac{Z}{\sqrt{U/\nu}}`$, com $`Z \sim N(0,1)`$ e $`U \sim \chi^2(\nu)`$ independentes. Elevando ao quadrado: $`T^2 = \frac{Z^2 / 1}{U / \nu}`$. Como $`Z^2 \sim \chi^2(1), T^2`$ é a razão de duas variáveis $`\chi^2`$ independentes divididas pelos seus graus de liberdade. Logo, $`T^2 \sim F(1, \nu)`$.
3. Exercício 3: Determine o valor esperado E(X) e a variância Var(X) de uma variável $`X \sim \chi^2(\nu)`$.
  * Gabarito Comentado: Para uma distribuição Chi-quadrado central com $`\nu`$ graus de liberdade, o valor esperado é $`E(X) = \nu`$ e a variância é igual ao dobro dos graus de liberdade, $`Var(X) = 2\nu`$.

#### Referência Audiovisual

* EST5507 Modelos de Regressão - Distribuições de probabilidades úteis para modelos regressão (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Para decompor a variabilidade total na ANOVA, precisamos de ferramentas matriciais para somas quadráticas: as Formas Quadráticas e o Teorema de Cochran, estudados na Aula 6.

### Aula 6: Formas Quadráticas e o Teorema de Cochran (Propriedades de Matrizes Idempotentes e Independência)

#### Objetivo Didático

Representar matricialmente formas quadráticas $`\mathbf{y}^T \mathbf{A} \mathbf{y}`$, compreender o papel das matrizes idempotentes $`(\mathbf{A}^2 = \mathbf{A})`$ e aplicar o Teorema de Cochran na decomposição independente de somas de quadrados.

#### Intuição e Analogias

Imagine a variância total de um conjunto de dados como a energia de um feixe de luz branca. O Teorema de Cochran funciona como um prisma de cristal: ele decompõe essa luz em feixes coloridos separados (somas de quadrados como Regressão e Erro). O teorema garante que a soma das energias dos feixes coloridos equivale à energia total original e que os feixes são ortogonais (probabilisticamente independentes), impedindo sobreposição de informações.

#### Exemplo Numérico Passo a Passo

Considere a matriz simétrica $`\mathbf{A}`$ de ordem $`2 \times 2: \mathbf{A} = \begin{bmatrix} 0{,}5 & 0{,}5 \\ 0{,}5 & 0{,}5 \end{bmatrix}`$

1. Verificação da Idempotência $`(\mathbf{A}^2 = \mathbf{A}): \mathbf{A}^2 = \begin{bmatrix} 0{,}5 & 0{,}5 \\ 0{,}5 & 0{,}5 \end{bmatrix} \begin{bmatrix} 0{,}5 & 0{,}5 \\ 0{,}5 & 0{,}5 \end{bmatrix} = \begin{bmatrix} 0{,}25 + 0{,}25 & 0{,}25 + 0{,}25 \\ 0{,}25 + 0{,}25 & 0{,}25 + 0{,}25 \end{bmatrix} = \begin{bmatrix} 0{,}5 & 0{,}5 \\ 0{,}5 & 0{,}5 \end{bmatrix} = \mathbf{A}`$ Como $`\mathbf{A}^2 = \mathbf{A}`$, a matriz é idempotente.

2. Traço e Graus de Liberdade: $`\text{tr}(\mathbf{A}) = a_{11} + a_{22} = 0{,}5 + 0{,}5 = 1`$ Para matrizes idempotentes, o posto equivale exatamente ao seu traço: $`\text{posto}(\mathbf{A}) = \text{tr}(\mathbf{A}) = 1`$. Se $`\mathbf{y} \sim N_2(\mathbf{0}, \mathbf{I}_2)`$, a forma quadrática $`\mathbf{y}^T \mathbf{A} \mathbf{y}`$ segue uma distribuição $`\chi^2(1)`$.

#### Erros Comuns de Iniciantes

* Esquecer que o Posto de uma Matriz Idempotente equivale ao seu Traço: Tentar calcular postos por eliminação gaussiana complexa em matrizes de alta dimensão, ignorando a identidade $`\text{posto}(\mathbf{A}) = \text{tr}(\mathbf{A})`$.
* Aplicar Cochran sem Verificar Ortogonalidade: Assumir independência entre duas formas quadráticas $`\mathbf{y}^T \mathbf{A} \mathbf{y}`$ e $`\mathbf{y}^T \mathbf{B} \mathbf{y}`$ sem provar a condição de ortogonalidade matricial $`\mathbf{A} \cdot \mathbf{B} = \mathbf{0}`$.

#### Resumo Conceitual

1. Uma matriz é idempotente se $`\mathbf{A}^2 = \mathbf{A}`$. Sendo simétrica e idempotente, $`\text{posto}(\mathbf{A}) = \text{tr}(\mathbf{A})`$.
2. Se $`\mathbf{y} \sim N_n(\mathbf{0}, \mathbf{I}_n)`$ e $`\mathbf{A}`$ é idempotente de traço r, então $`\mathbf{y}^T \mathbf{A} \mathbf{y} \sim \chi^2(r)`$.
3. Pelo Teorema de Cochran, duas formas quadráticas $`\mathbf{y}^T \mathbf{A} \mathbf{y}`$ e $`\mathbf{y}^T \mathbf{B} \mathbf{y}`$ são independentes se e somente se $`\mathbf{A} \mathbf{B} = \mathbf{0}`$.

#### Exercícios Resolvidos

1. Exercício 1: Verifique se a matriz $`\mathbf{B} = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}`$ é idempotente e calcule seu traço.
  * Gabarito Comentado: $`\mathbf{B}^2 = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix} \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix} = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix} = \mathbf{B}`$ A matriz é idempotente. Seu traço é $`\text{tr}(\mathbf{B}) = 1 + 0 = 1`$.
2. Exercício 2: Se $`\mathbf{y} \sim N_3(\mathbf{0}, \mathbf{I}_3)`$ e $`\mathbf{A}`$ é idempotente de ordem $`3 \times 3`$ com $`\text{tr}(\mathbf{A}) = 2`$, qual é a distribuição de $`Q = \mathbf{y}^T \mathbf{A} \mathbf{y}?`$
  * Gabarito Comentado: Como $`\mathbf{y}`$ segue distribuição Normal Padrão multivariada com matriz identidade $`\mathbf{I}_3`$ e $`\mathbf{A}`$ é simétrica e idempotente de traço 2, a forma quadrática $`Q \sim \chi^2(2)`$.
3. Exercício 3: Sejam $`\mathbf{A} = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}`$ e $`\mathbf{B} = \begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix}`$. Prove que as formas quadráticas $`\mathbf{y}^T \mathbf{A} \mathbf{y}`$ e $`\mathbf{y}^T \mathbf{B} \mathbf{y}`$ são independentes.
  * Gabarito Comentado: Avaliando o produto matricial: $`\mathbf{A} \mathbf{B} = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix} \begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}`$ Como $`\mathbf{A} \mathbf{B} = \mathbf{0}`$, as formas quadráticas são independentes pelo Teorema de Cochran.

#### Referência Audiovisual

* Modelos Lineares - Distribuição de formas quadráticas (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Com o arcabouço probabilístico e matricial sedimentado, iniciamos o Módulo 3 formalizando o Modelo Linear Geral sob a condição clássica de Posto Completo.

## Módulo 3: Modelo Linear Geral de Posto Completo e Estimação

### Aula 7: Modelo Linear Geral de Posto Completo (Estrutura Matricial, Suposições Clássicas e o Intercepto)

#### Objetivo Didático

Formalizar a estrutura do Modelo Linear Geral sob a condição de posto completo da Matriz de Planejamento $`\mathbf{X} (\text{posto}(\mathbf{X}) = p+1 \le n)`$, consolidando as suposições clássicas de Gauss-Markov.

#### Intuição e Analogias

Pense em uma mesa de três pernas ajustáveis. Para que a mesa fique perfeitamente estável e firme sobre o chão em uma posição única (solução matemática única), nenhuma perna pode estar posicionada na mesma linha exata de outra. A condição de "Posto Completo" garante que nenhuma coluna da matriz $`\mathbf{X}`$ seja combinação linear perfeita de outras colunas, permitindo a inversão da matriz $`\mathbf{X}^T \mathbf{X}`$.

#### Exemplo Numérico Passo a Passo

Considere a matriz de planejamento $`\mathbf{X}`$ com n=4 observações e p=1 preditora, incluindo a coluna de 1s: $`\mathbf{X} = \begin{bmatrix} 1 & 2 \\ 1 & 4 \\ 1 & 6 \\ 1 & 8 \end{bmatrix}_{4 \times 2}`$

#### Verificação do Posto:

* A dimensão de $`\mathbf{X}`$ é $`4 \times 2`$. O posto máximo é $`\min(n, p+1) = \min(4, 2) = 2`$.
* A coluna $`1 (\mathbf{c}_1 = [1`$, 1, 1, 1]^T) e a coluna $`2 (\mathbf{c}_2 = [2`$, 4, 6, 8]^T) são linearmente independentes (não existe escalar k tal que $`\mathbf{c}_2 = k \cdot \mathbf{c}_1`$).
* Portanto, $`\text{posto}(\mathbf{X}) = 2`$ (Posto Completo).

#### Erros Comuns de Iniciantes

* Armadilha da Variável Dummy (Dummy Variable Trap): Incluir todas as categorias de uma variável qualitativa junto com a coluna de 1s do intercepto (ex: incluir dummys para "Masculino" e "Feminino" simultaneamente), gerando colinearidade perfeita e tornando $`\mathbf{X}^T \mathbf{X}`$ singular.
* Incluir Preditoras Redundantes: Tentar ajustar modelos contendo preditoras perfeitamente dependentes, como "Renda em Reais" e "Renda em Milhares de Reais".

#### Resumo Conceitual

1. A condição de Posto Completo exige que $`\text{posto}(\mathbf{X}) = p+1`$, garantindo que a matriz $`\mathbf{X}^T \mathbf{X}`$ seja não singular e invertível.
2. As suposições de Gauss-Markov exigem $`E(\mathbf{\epsilon}) = \mathbf{0}, Var(\mathbf{\epsilon}) = \sigma^2 \mathbf{I}_n`$ (homocedasticidade e não-correlação).
3. Sob erros gaussianos, assume-se que $`\mathbf{\epsilon} \sim N_n(\mathbf{0}, \sigma^2 \mathbf{I}_n)`$, exigindo n > p+1.

#### Exercícios Resolvidos

1. Exercício 1: Avalie se a matriz $`\mathbf{X} = \begin{bmatrix} 1 & 3 & 6 \\ 1 & 4 & 8 \\ 1 & 5 & 10 \end{bmatrix}`$ possui posto completo.
  * Gabarito Comentado: Observando as colunas de $`\mathbf{X}`$, a coluna $`3 (\mathbf{c}_3 = [6`$, 8, 10]^T) é o dobro da coluna $`2 (\mathbf{c}_2 = [3`$, 4, 5]^T), isto é, $`\mathbf{c}_3 = 2 \cdot \mathbf{c}_2`$. Há dependência linear perfeita, logo $`\text{posto}(\mathbf{X}) = 2 < 3`$. A matriz é de posto incompleto (singular).
2. Exercício 2: Quais são as propriedades da matriz de variâncias-covariâncias dos erros sob as suposições de homocedasticidade e não-correlação?
  * Gabarito Comentado: A matriz de covariâncias é $`Var(\mathbf{\epsilon}) = \sigma^2 \mathbf{I}_n`$. A diagonal principal é composta pela variância constante $`\sigma^2`$ (homocedasticidade) e os elementos fora da diagonal são nulos (erros não correlacionados).
3. Exercício 3: Se um modelo possui n=50 observações e p=5 preditoras (além do intercepto), determine a dimensão do vetor $`\mathbf{\beta}`$ e da matriz $`\mathbf{X}^T \mathbf{X}`$.
  * Gabarito Comentado: O número total de parâmetros é p+1 = 5+1 = 6.
    * Vetor de parâmetros $`\mathbf{\beta}`$: dimensão $`6 \times 1`$.
    * Matriz $`\mathbf{X}^T \mathbf{X}`$: dimensão $`6 \times 6`$.

#### Referência Audiovisual

* EST5507 Modelos de Regressão - Modelos lineares com matriz de planejamento de posto completo Parte 1 & Modelos Lineares - Modelo linear geral com matriz de planejamento de posto completo (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Garantida a condição de posto completo, podemos derivar analiticamente os estimadores por Mínimos Quadrados e por Máxima Verossimilhança na Aula 8.

### Aula 8: Estimação por Mínimos Quadrados e Máxima Verossimilhança Matricial (Equações Normais, Derivação Matricial, Estimadores de $`\mathbf{\beta}`$ e $`\sigma^2`$)

#### Objetivo Didático

Derivar as Equações Normais no formato matricial, obter o estimador de $`MQO (\hat{\mathbf{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y})`$ e demonstrar sua equivalência ao Estimador de Máxima Verossimilhança (EMV) sob normalidade, diferenciando as estimativas de $`\sigma^2`$.

#### Intuição e Analogias

Pense na função de verossimilhança como o botão de sintonia analógico de um rádio. Desejamos girar os botões dos parâmetros ($`\mathbf{\beta}`$ e $`\sigma^2`$) até sintonizar a frequência exata na qual o sinal captado (a amostra observada) soe com a máxima probabilidade possível. Sob erros normais, a frequência de máxima verossimilhança coincide exatamente com a solução geométrica de Mínimos Quadrados.

#### Exemplo Numérico Passo a Passo

Considere as seguintes matrizes obtidas em um estudo com n=10 observações e p=1 preditora: $`\mathbf{X}^T \mathbf{X} = \begin{bmatrix} 4 & 0 \\ 0 & 10 \end{bmatrix}, \quad \mathbf{X}^T \mathbf{y} = \begin{bmatrix} 20 \\ 30 \end{bmatrix}`$

1. Inversão de $`\mathbf{X}^T \mathbf{X}: (\mathbf{X}^T \mathbf{X})^{-1} = \begin{bmatrix} 1/4 & 0 \\ 0 & 1/10 \end{bmatrix} = \begin{bmatrix} 0{,}25 & 0 \\ 0 & 0{,}10 \end{bmatrix}`$

2. Cálculo do Estimador $`\hat{\mathbf{\beta}}: \hat{\mathbf{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y} = \begin{bmatrix} 0{,}25 & 0 \\ 0 & 0{,}10 \end{bmatrix} \begin{bmatrix} 20 \\ 30 \end{bmatrix} = \begin{bmatrix} 0{,}25 \cdot 20 \\ 0{,}10 \cdot 30 \end{bmatrix} = \begin{bmatrix} 5 \\ 3 \end{bmatrix}`$ Assim, $`\hat{\beta}_0 = 5`$ e $`\hat{\beta}_1 = 3`$.

## 3. Estimadores da Variância $`(\sigma^2)`$: Se a Soma de Quadrados dos Resíduos for SQR = 32:

* Estimador de MQO (Não Viciado, $`s^2): s^2 = \frac{SQR}{n - p - 1} = \frac{32}{10 - 1 - 1} = \frac{32}{8} = 4{,}0`$
* Estimador de Máxima Verossimilhança (Viciado, $`\hat{\sigma}_{MV}^2): \hat{\sigma}_{MV}^2 = \frac{SQR}{n} = \frac{32}{10} = 3{,}2`$

#### Erros Comuns de Iniciantes

* Confundir Denominadores de $`\sigma^2`$: O estimador de MV divide a SQR por n (sendo viciado para amostras pequenas), enquanto o estimador de MQO divide por n - p - 1 para corrigir os graus de liberdade perdidos.
* Inverter a Ordem do Produto Matricial: Tentar efetuar $`(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{y} \mathbf{X}^T`$ em vez da sequência correta $`(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}`$.

#### Resumo Conceitual

1. As Equações Normais são dadas em notação matricial por $`(\mathbf{X}^T \mathbf{X}) \mathbf{\beta} = \mathbf{X}^T \mathbf{y}`$.
2. Sendo $`\mathbf{X}`$ de posto completo, o estimador único de MQO é $`\hat{\mathbf{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}`$.
3. Sob erros normais, o estimador pontual de MQO de $`\mathbf{\beta}`$ é idêntico ao Estimador de Máxima Verossimilhança.

#### Exercícios Resolvidos

1. Exercício 1: Obtenha $`\hat{\mathbf{\beta}}`$ para $`\mathbf{X}^T \mathbf{X} = \begin{bmatrix} 2 & 0 \\ 0 & 5 \end{bmatrix}`$ e $`\mathbf{X}^T \mathbf{y} = \begin{bmatrix} 10 \\ 15 \end{bmatrix}`$.
  * Gabarito Comentado: $`(\mathbf{X}^T \mathbf{X})^{-1} = \begin{bmatrix} 0{,}5 & 0 \\ 0 & 0{,}2 \end{bmatrix} \hat{\mathbf{\beta}} = \begin{bmatrix} 0{,}5 & 0 \\ 0 & 0{,}2 \end{bmatrix} \begin{bmatrix} 10 \\ 15 \end{bmatrix} = \begin{bmatrix} 5 \\ 3 \end{bmatrix}`$
2. Exercício 2: Mostre a relação algébrica entre o estimador de $`MV \hat{\sigma}_{MV}^2`$ e o estimador de $`MQO s^2`$.
  * Gabarito Comentado: Como $`\hat{\sigma}_{MV}^2 = \frac{SQR}{n}`$ e $`s^2 = \frac{SQR}{n - p - 1}`$, temos $`SQR = n \cdot \hat{\sigma}_{MV}^2`$. Substituindo em $`s^2: s^2 = \left( \frac{n}{n - p - 1} \right) \hat{\sigma}_{MV}^2`$
3. Exercício 3: Qual é a distribuição de amostragem de $`\hat{\mathbf{\beta}}`$ sob erros $`\mathbf{\epsilon} \sim N_n(\mathbf{0}, \sigma^2 \mathbf{I}_n)?`$
  * Gabarito Comentado: Sendo $`\hat{\mathbf{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}`$ uma transformação linear de $`\mathbf{y}`$, ele segue distribuição Normal Multivariada: $`\hat{\mathbf{\beta}} \sim N_{p+1}\left( \mathbf{\beta}, \sigma^2 (\mathbf{X}^T \mathbf{X})^{-1} \right)`$

#### Referência Audiovisual

* EST5507 Modelos de Regressão Modelo matriz de planej posto completo estimador máxima verossimilhança & Modelos de regressão- EQM Estimador de mínimos quadrados de B (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Para compreender como o modelo projeta os valores observados e gera os ajustes $`(\hat{\mathbf{y}})`$, estudaremos a geometria dos Mínimos Quadrados e a Matriz Chapéu na Aula 9.

### Aula 9: Propriedades dos Estimadores e Matriz Chapéu (Hat Matrix $`\mathbf{H}`$, Projeção Ortogonal e Resíduos)

#### Objetivo Didático

Explorar a geometria dos Mínimos Quadrados, a estrutura da Matriz Chapéu $`(\mathbf{H} = \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1}\mathbf{X}^T)`$, suas propriedades de projeção e a matriz geradora de resíduos $`(\mathbf{I} - \mathbf{H})`$.

#### Intuição e Analogias

Pense na Matriz Chapéu $`(\mathbf{H})`$ como um projetor de luz 3D refletindo a imagem de um objeto sobre uma parede 2D. O vetor de respostas observadas $`\mathbf{y}`$ é um vetor livre flutuando no espaço n-dimensional. A parede é o subespaço gerado pelas colunas da matriz $`\mathbf{X}`$. A sombra projetada ortogonalmente sobre a parede é o vetor de valores ajustados $`\hat{\mathbf{y}} = \mathbf{H}\mathbf{y}`$ (colocando um "chapéu" em $`\mathbf{y}`$). O vetor de resíduos $`\mathbf{e} = (\mathbf{I} - \mathbf{H})\mathbf{y}`$ é o segmento perpendicular invisível que une a ponta da sombra à ponta do vetor real.

```text
                  Vetor y (Observado)
                     ^
                    /|
                   / |
                  /  | Vetor de Resíduos (e = y - ŷ)
                 /   | (Ortogonal à parede)
                /    v
               +------> Vetor ŷ = Hy (A sombra na parede)
              Subespaço de X (A parede 2D)
```

#### Exemplo Numérico Passo a Passo

Dada a matriz $`\mathbf{X}_{3 \times 2} = \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix}`$, sabemos da Aula 3 que $`(\mathbf{X}^T \mathbf{X})^{-1} = \begin{bmatrix} 7/3 & -1 \\ -1 & 0{,}5 \end{bmatrix}`$.

Cálculo da Matriz Chapéu $`(\mathbf{H} = \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1}\mathbf{X}^T): \mathbf{H} = \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix} \begin{bmatrix} 7/3 & -1 \\ -1 & 0{,}5 \end{bmatrix} \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix} = \begin{bmatrix} 5/6 & 1/3 & -1/6 \\ 1/3 & 1/3 & 1/3 \\ -1/6 & 1/3 & 5/6 \end{bmatrix}`$

#### Verificação de Propriedades:

1. Simetria $`(\mathbf{H}^T = \mathbf{H})`$: A matriz é igual à sua transposta.
2. Idempotência $`(\mathbf{H}^2 = \mathbf{H})`$: A multiplicação $`\mathbf{H} \cdot \mathbf{H}`$ reproduz a matriz $`\mathbf{H}`$.
3. Traço: $`\text{tr}(\mathbf{H}) = \frac{5}{6} + \frac{1}{3} + \frac{5}{6} = \frac{12}{6} = 2`$ Confirmando que $`\text{tr}(\mathbf{H}) = p+1 = 2`$.

#### Erros Comuns de Iniciantes

* Confundir os Espaços de Projeção: Esquecer que $`\mathbf{H}`$ projeta no espaço dos valores ajustados $`(\hat{\mathbf{y}} = \mathbf{H}\mathbf{y})`$, enquanto $`(\mathbf{I} - \mathbf{H})`$ projeta no espaço ortogonal dos resíduos $`(\mathbf{e} = (\mathbf{I} - \mathbf{H})\mathbf{y})`$.
* Negligenciar a Alavancagem (Leverage): Ignorar que os elementos da diagonal principal $`h_{ii}`$ medem o grau de afastamento da observação i em relação à média das preditoras, identificando pontos influentes.

#### Resumo Conceitual

1. A Matriz Chapéu $`\mathbf{H} = \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1}\mathbf{X}^T`$ é simétrica e idempotente, operando a projeção ortogonal de $`\mathbf{y}`$ no subespaço de colunas de $`\mathbf{X}`$.
2. A matriz geradora de resíduos $`(\mathbf{I} - \mathbf{H})`$ é simétrica e idempotente, com traço $`\text{tr}(\mathbf{I} - \mathbf{H}) = n - p - 1`$.
3. Pelo Teorema de Gauss-Markov, o estimador de $`MQO \hat{\mathbf{\beta}}`$ é o Estimador Linear Não Viciado de Mínima Variância (BLUE).

#### Exercícios Resolvidos

1. Exercício 1: Demonstre algebricamente que a matriz de resíduos $`\mathbf{M} = (\mathbf{I} - \mathbf{H})`$ é idempotente.
  * Gabarito Comentado: $`\mathbf{M}^2 = (\mathbf{I} - \mathbf{H})(\mathbf{I} - \mathbf{H}) = \mathbf{I} \cdot \mathbf{I} - \mathbf{I} \mathbf{H} - \mathbf{H} \mathbf{I} + \mathbf{H}^2`$ Pela idempotência de $`\mathbf{H} (\mathbf{H}^2 = \mathbf{H}): \mathbf{M}^2 = \mathbf{I} - \mathbf{H} - \mathbf{H} + \mathbf{H} = \mathbf{I} - \mathbf{H} = \mathbf{M}`$
2. Exercício 2: Prove que a covariância entre os valores ajustados $`\hat{\mathbf{y}}`$ e os resíduos $`\mathbf{e}`$ é nula.
  * Gabarito Comentado: $`Cov(\hat{\mathbf{y}}, \mathbf{e}) = Cov(\mathbf{H}\mathbf{y}, (\mathbf{I} - \mathbf{H})\mathbf{y}) = \mathbf{H} Var(\mathbf{y}) (\mathbf{I} - \mathbf{H})^T`$. Como $`Var(\mathbf{y}) = \sigma^2 \mathbf{I}_n`$ e $`(\mathbf{I} - \mathbf{H})`$ é simétrica: $`Cov(\hat{\mathbf{y}}, \mathbf{e}) = \sigma^2 \mathbf{H} (\mathbf{I} - \mathbf{H}) = \sigma^2 (\mathbf{H} - \mathbf{H}^2)`$ Sendo $`\mathbf{H}^2 = \mathbf{H}, \mathbf{H} - \mathbf{H} = \mathbf{0}`$. Logo, $`Cov(\hat{\mathbf{y}}, \mathbf{e}) = \mathbf{0}`$.
3. Exercício 3: Se um conjunto de dados possui n=20 observações e p=3 preditoras, determine a soma dos elementos da diagonal principal de $`(\mathbf{I} - \mathbf{H})`$.
  * Gabarito Comentado: A soma dos elementos da diagonal principal é dada pelo traço: $`\text{tr}(\mathbf{I} - \mathbf{H}) = \text{tr}(\mathbf{I}_n) - \text{tr}(\mathbf{H}) = n - (p + 1) = 20 - (3 + 1) = 16`$

#### Referência Audiovisual

* Modelos Lineares - Aplicação mod linear e Estimador de máxima verossimilhança & Notação matricial derivadas vetoriais (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Estabelecida a estrutura de projeção e resíduos, iniciamos o Módulo 4 focando na decomposição da variância total (ANOVA) e em testes de hipóteses lineares.

## Módulo 4: Inferência, ANOVA e Diagnóstico em Modelos Lineares

### Aula 10: Análise de Variância (ANOVA) e Decomposição de Somas Quadráticas (SST = SSR + SSE, Teste F Global)

#### Objetivo Didático

Efetuar a partição ortogonal da Soma de Quadrados Total (SST) em Soma de Quadrados da Regressão (SSR) e Soma de Quadrados dos Erros (SSE), construindo a Tabela de ANOVA e aplicando o Teste F de significância global.

#### Intuição e Analogias

#### Pense na Análise de Variância (ANOVA) como um "orçamento familiar de incerteza":

* Soma de Quadrados Total (SST): É a variação total observada na resposta (a quantidade total de incerteza da amostra).
* Soma de Quadrados da Regressão (SSR): É a parcela da variação explicada pelas variáveis do modelo (despesas explicadas).
* Soma de Quadrados dos Erros (SSE): É a variação residual não explicada que permanece no ruído (desperdício).

O Teste F é a razão entre a taxa média de incerteza explicada pela regressão e a taxa média do ruído residual.

#### Exemplo Numérico Passo a Passo

Considere um modelo com n=20 observações e p=3 preditoras (com intercepto), apresentando SST = 200 e SSR = 150.

## 1. Cálculo de SSE: SSE = SST - SSR = 200 - 150 = 50

2. Coeficiente de Determinação ($`R^2): R^2 = \frac{SSR}{SST} = \frac{150}{200} = 0{,}75 \quad (75\% \text{ da variância de }`$ $`Y \text{ é explicada pelo modelo}`$)

## 3. Tabela de ANOVA:

| Fonte de Variação | Graus de Liberdade (df) | Soma de Quadrados (SS) | Quadrado Médio (MS = SS/df) | Estatística $`F_{calc}`$ |
|---|---|---|---|---|
| Regressão | p = 3 | SSR = 150 | $`MSR = 150 / 3 = 50{,}0`$ | $`F = \frac{MSR}{MSE} = \frac{50{,}0}{3{,}125} = 16{,}0`$ |
| Resíduo (Erro) | n - p - 1 = 16 | SSE = 50 | $`MSE = 50 / 16 = 3{,}125`$ | — |
| Total Corrigido | n - 1 = 19 | SST = 200 | — | — |

Decisão: Se $`F_{critico}(3, 16) = 3{,}24`$ ao nível de $`5\%`$, como $`F_{calc} = 16{,}0 > 3{,}24`$, rejeita-se a hipótese nula $`H_0: \beta_1 = \beta_2 = \beta_3 = 0`$. O modelo é estatisticamente significante.

#### Erros Comuns de Iniciantes

* Equiparar $`R^2`$ Alto à Validade do Modelo: Acreditar que um $`R^2`$ elevado garante especificação correta, ignorando padrões não lineares não modelados ou violações de premissas residuais.
* Errar Graus de Liberdade de SSE: Usar n-p em vez do correto n - p - 1 quando o modelo inclui o termo de intercepto.

#### Resumo Conceitual

1. A variabilidade total decompoe-se ortogonalmente em SST = SSR + SSE.
2. O Coeficiente de Determinação $`R^2 = SSR / SST`$ mede a proporção de variância explicada.
3. O Teste F global avalia $`H_0: \beta_1 = \beta_2 = \dots = \beta_p = 0`$ contra $`H_1`$: pelo menos um $`\beta_j \neq 0`$.

#### Exercícios Resolvidos

1. Exercício 1: Preencha os valores ausentes na tabela ANOVA: SST = 500, SSE = 100, com n=31 observações e p=5 preditoras.
  * Gabarito Comentado:
    * SSR = SST - SSE = 500 - 100 = 400.
    * $`df_{Regressao} = p = 5`$.
    * $`df_{Erro} = n - p - 1 = 31 - 5 - 1 = 25`$.
    * $`MSR = 400 / 5 = 80{,}0`$.
    * $`MSE = 100 / 25 = 4{,}0`$.
    * $`F_{calc} = 80{,}0 / 4{,}0 = 20{,}0`$.
2. Exercício 2: Calcule o $`R^2`$ Ajustado $`(R_{adj}^2)`$ para os dados do Exercício 1.
  * Gabarito Comentado: $`R^2 = \frac{400}{500} = 0{,}80 R_{adj}^2 = 1 - \left[ (1 - R^2) \frac{n - 1}{n - p - 1} \right] = 1 - \left[ (1 - 0{,}80) \frac{30}{25} \right] = 1 - [0{,}20 \cdot 1{,}2] = 0{,}76`$
3. Exercício 3: O que ocorre com o $`R^2`$ e com o $`R_{adj}^2`$ ao adicionar uma preditora irrelevante ao modelo?
  * Gabarito Comentado: O $`R^2`$ nunca diminui com a inclusão de novas preditoras. O $`R_{adj}^2`$ penaliza a inclusão de parâmetros sem ganho explicativo real, sofrendo redução em seu valor.

#### Referência Audiovisual

* EST5507 Modelos de Regressão ANOVA no modelo c matriz de planejamento de posto completo c intercepto & Modelos Lineares - ANOVA Distribuição de somas quadráticas (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: A ANOVA testa a significância global. Para testar restrições lineares customizadas sobre subconjuntos de parâmetros, empregamos o Teste de Hipótese Linear Geral na Aula 11.

### Aula 11: Teste de Hipótese Linear Geral e Razão de Verossimilhanças ($`\mathbf{C}\mathbf{\beta} = \mathbf{m}`$, Matriz $`\mathbf{C}`$ e o Teste Likelihood Ratio)

#### Objetivo Didático

Formular hipóteses lineares simultâneas no formato matricial $`H_0: \mathbf{C}\mathbf{\beta} = \mathbf{m}`$ e aplicar a estatística do Teste F Geral e do Teste da Razão de Verossimilhanças Generalizada (LRT).

#### Intuição e Analogias

Pense no Teste da Razão de Verossimilhanças como um "tribunal de arbitragem de custo-benefício":

* Modelo Irrestrito $`(H_1)`$: O modelo completo com todos os parâmetros livres.
* Modelo Restrito $`(H_0)`$: Uma versão simplificada sob travas/restrições nos parâmetros.

O tribunal avalia: qual foi a perda de qualidade de ajuste ao simplificar o modelo impondo $`\mathbf{C}\mathbf{\beta} = \mathbf{m}?`$ Se a perda for estatisticamente irrelevante, adota-se o modelo restrito por parcimônia.

#### Exemplo Numérico Passo a Passo

Considere o vetor $`\mathbf{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \\ \beta_2 \end{bmatrix}_{3 \times 1}`$ de um modelo com duas preditoras. Desejamos testar a igualdade entre os efeitos das duas preditoras: $`H_0: \beta_1 = \beta_2 \implies H_0: \beta_1 - \beta_2 = 0`$.

#### Especificação da Matriz $`\mathbf{C}`$ e do Vetor $`\mathbf{m}`$:

* Número de restrições (q = 1).
* Matriz $`\mathbf{C}`$ de dimensão $`1 \times 3: \mathbf{C} = \begin{bmatrix} 0 & 1 & -1 \end{bmatrix}`$
* Vetor $`\mathbf{m}`$ de dimensão $`1 \times 1: \mathbf{m} = \begin{bmatrix} 0 \end{bmatrix}`$

Multiplicação Matricial $`\mathbf{C}\mathbf{\beta}: \mathbf{C}\mathbf{\beta} = \begin{bmatrix} 0 & 1 & -1 \end{bmatrix} \begin{bmatrix} \beta_0 \\ \beta_1 \\ \beta_2 \end{bmatrix} = \beta_1 - \beta_2`$ A igualdade $`\mathbf{C}\mathbf{\beta} = \mathbf{m}`$ traduz perfeitamente a restrição $`\beta_1 - \beta_2 = 0`$.

Estatística F do Teste Geral: $`F_{calc} = \frac{(\mathbf{C}\hat{\mathbf{\beta}} - \mathbf{m})^T \left[ \mathbf{C}(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{C}^T \right]^{-1} (\mathbf{C}\hat{\mathbf{\beta}} - \mathbf{m})}{q \cdot s^2} \sim F(q`$, n - p - 1)

#### Erros Comuns de Iniciantes

* Incompatibilidade de Dimensões na Matriz $`\mathbf{C}`$: Criar a matriz de restrições com número de colunas diferente do tamanho do vetor $`\mathbf{\beta} (p+1)`$.
* Confundir Graus de Liberdade: Esquecer que o número de linhas linearmente independentes de $`\mathbf{C} (q)`$ determina os graus de liberdade do numerador do teste F.

#### Resumo Conceitual

1. Hipóteses lineares simultâneas são expressas na forma $`H_0: \mathbf{C}\mathbf{\beta} = \mathbf{m}`$, onde $`\mathbf{C}`$ tem dimensão $`q \times (p+1)`$ com posto q.
2. O teste compara o incremento na soma de quadrados residuais entre o modelo restrito (SSE_R) e irrestrito (SSE_I).
3. Sob $`H_1`$, a estatística do teste segue uma distribuição F Não-Central.

#### Exercícios Resolvidos

1. Exercício 1: Monte a matriz $`\mathbf{C}`$ e o vetor $`\mathbf{m}`$ para testar $`H_0: \beta_1 = 0 \text{ e } \beta_2 = 0`$ em um modelo com $`\mathbf{\beta} = [\beta_0, \beta_1, \beta_2, \beta_3]^T`$.
  * Gabarito Comentado: Com q=2 restrições e p+1=4: $`\mathbf{C} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \end{bmatrix}_{2 \times 4}, \quad \mathbf{m} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}_{2 \times 1}`$
2. Exercício 2: Especifique a restrição $`H_0: \beta_1 + 2\beta_2 = 1`$ para o vetor $`\mathbf{\beta} = [\beta_0, \beta_1, \beta_2]^T`$.
  * Gabarito Comentado: Com q=1 restrição: $`\mathbf{C} = \begin{bmatrix} 0 & 1 & 2 \end{bmatrix}_{1 \times 3}, \quad \mathbf{m} = \begin{bmatrix} 1 \end{bmatrix}_{1 \times 1}`$
3. Exercício 3: Qual é a distribuição assintótica da estatística do teste da Razão de Verossimilhanças $`-2 \ln(\Lambda)`$ sob $`H_0?`$
  * Gabarito Comentado: Assintoticamente, a estatística $`-2 \ln(\Lambda)`$ do teste LRT segue distribuição Chi-quadrado central com graus de liberdade iguais ao número de restrições q: $`-2 \ln(\Lambda) \stackrel{a}{\sim} \chi^2(q)`$.

#### Referência Audiovisual

* EST5507 Modelos de Regressão Teste Razão de verossimilhanças generalizado para hipótese linear geral (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Quando os erros violam a homocedasticidade ou apresentam dependência, empregamos os Mínimos Quadrados Generalizados na Aula 12.

### Aula 12: Erros Correlacionados e Mínimos Quadrados Generalizados (GLS, Matriz $`\mathbf{V}`$, Estruturas de Covariância WLS e AR(1))

#### Objetivo Didático

Resolver o Modelo Linear quando a matriz de variâncias-covariâncias dos erros é da forma $`Var(\mathbf{\epsilon}) = \sigma^2 \mathbf{V} (\mathbf{V}`$ conhecida e positiva definida), aplicando a decomposição $`\mathbf{V} = \mathbf{P} \mathbf{P}^T`$ para transformar o modelo original em erros esféricos.

#### Intuição e Analogias

Imagine tentar fotografar uma paisagem através de um vidro ondulado e embaçado (erros heterocedásticos ou correlacionados). As fotos sairão distorcidas. O método de Mínimos Quadrados Generalizados (GLS) equivale a colocar óculos corretivos (a matriz $`\mathbf{P}^{-1}`$). A lente desentorta os dados, fazendo com que o modelo transformado volte a ter erros esféricos i.i.d.

#### Estruturas Canônicas da Matriz $`\mathbf{V}`$

1. Mínimos Quadrados Ponderados (WLS - Heterocedasticidade): $`\mathbf{V} = \text{diag}(v_1, v_2, \dots, v_n) = \begin{bmatrix} v_1 & 0 & \dots & 0 \\ 0 & v_2 & \dots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \dots & v_n \end{bmatrix}`$ A matriz inversa $`\mathbf{V}^{-1} = \text{diag}(1/v_1, 1/v_2, \dots, 1/v_n)`$ atribui pesos $`w_i = 1/v_i`$ inversamente proporcionais às variâncias de cada observação.
2. Processo Autorregressivo de Primeira Ordem (AR(1) - Autocorrelação Temporal): $`\mathbf{V} = \begin{bmatrix} 1 & \rho & \rho^2 & \dots & \rho^{n-1} \\ \rho & 1 & \rho & \dots & \rho^{n-2} \\ \rho^2 & \rho & 1 & \dots & \rho^{n-3} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ \rho^{n-1} & \rho^{n-2} & \rho^{n-3} & \dots & 1 \end{bmatrix}`$ Onde $`V_{ij} = \rho^{\vert i-j\vert }`$ modela a atenuação da correlação à medida que a distância temporal |i-j| aumenta $`(\vert \rho\vert  < 1)`$.

#### Exemplo Numérico Passo a Passo (WLS)

Considere n=3 observações com variâncias heterocedásticas conhecidas $`\mathbf{V} = \text{diag}(1, 2`$, 4).

1. Inversão da Matriz $`\mathbf{V}: \mathbf{V}^{-1} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1/2 & 0 \\ 0 & 0 & 1/4 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0{,}5 & 0 \\ 0 & 0 & 0{,}25 \end{bmatrix}`$

2. Formulação do Estimador $`GLS: \hat{\mathbf{\beta}}_{GLS} = (\mathbf{X}^T \mathbf{V}^{-1} \mathbf{X})^{-1} \mathbf{X}^T \mathbf{V}^{-1} \mathbf{y}`$

#### A matriz $`\mathbf{V}^{-1}`$ penaliza observações de alta variabilidade:

* Observação $`1 (v_1 = 1)`$: recebe peso $`w_1 = 1{,}0`$ (máxima precisão).
* Observação $`3 (v_3 = 4)`$: recebe peso $`w_3 = 0{,}25`$ (menor peso relativo).

#### Erros Comuns de Iniciantes

* Usar MQO sob Erros Correlacionados: Ajustar por MQO simples na presença de autocorrelação ou heterocedasticidade. Embora o estimador permaneça não viciado, ele perde eficiência (deixa de ser BLUE) e seus erros padrões ficam incorretos, invalidando p-valores.
* Matriz $`\mathbf{V}`$ Não Positiva Definida: Tentar operar o GLS com matrizes $`\mathbf{V}`$ que possuem determinantes nulos ou autovalores negativos.

#### Resumo Conceitual

1. Sob $`Var(\mathbf{\epsilon}) = \sigma^2 \mathbf{V}`$, o estimador BLUE é o $`GLS: \hat{\mathbf{\beta}}_{GLS} = (\mathbf{X}^T \mathbf{V}^{-1} \mathbf{X})^{-1} \mathbf{X}^T \mathbf{V}^{-1} \mathbf{y}`$.
2. Decompondo $`\mathbf{V} = \mathbf{P} \mathbf{P}^T`$, a transformação $`\mathbf{z} = \mathbf{P}^{-1}\mathbf{y}`$ e $`\mathbf{Q} = \mathbf{P}^{-1}\mathbf{X}`$ produz erros esféricos $`\mathbf{\eta} = \mathbf{P}^{-1}\mathbf{\epsilon}`$.
3. A matriz de covariâncias do estimador GLS é $`Var(\hat{\mathbf{\beta}}_{GLS}) = \sigma^2 (\mathbf{X}^T \mathbf{V}^{-1} \mathbf{X})^{-1}`$.

#### Exercícios Resolvidos

1. Exercício 1: Dada a matriz heterocedástica $`\mathbf{V} = \begin{bmatrix} 0{,}5 & 0 \\ 0 & 2 \end{bmatrix}`$, determine $`\mathbf{V}^{-1}`$ e os pesos de amostragem no WLS.
  * Gabarito Comentado: $`\mathbf{V}^{-1} = \begin{bmatrix} 1/0{,}5 & 0 \\ 0 & 1/2 \end{bmatrix} = \begin{bmatrix} 2 & 0 \\ 0 & 0{,}5 \end{bmatrix}`$ A primeira observação recebe peso $`w_1 = 2{,}0`$ e a segunda recebe peso $`w_2 = 0{,}5`$.
2. Exercício 2: Mostre que se $`\mathbf{V} = \mathbf{I}_n`$, o estimador $`\hat{\mathbf{\beta}}_{GLS}`$ reduz-se ao estimador de MQO clássico.
  * Gabarito Comentado: Substituindo $`\mathbf{V} = \mathbf{I}_n: \hat{\mathbf{\beta}}_{GLS} = (\mathbf{X}^T \mathbf{I}_n^{-1} \mathbf{X})^{-1} \mathbf{X}^T \mathbf{I}_n^{-1} \mathbf{y} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y} = \hat{\mathbf{\beta}}_{MQO}`$
3. Exercício 3: Prove que o vetor de erros transformado $`\mathbf{\eta} = \mathbf{P}^{-1}\mathbf{\epsilon}`$ possui covariância esférica $`Var(\mathbf{\eta}) = \sigma^2 \mathbf{I}_n`$.
  * Gabarito Comentado: $`Var(\mathbf{\eta}) = Var(\mathbf{P}^{-1}\mathbf{\epsilon}) = \mathbf{P}^{-1} Var(\mathbf{\epsilon}) (\mathbf{P}^{-1})^T = \mathbf{P}^{-1} (\sigma^2 \mathbf{V}) (\mathbf{P}^T)^{-1}`$ Como $`\mathbf{V} = \mathbf{P} \mathbf{P}^T: Var(\mathbf{\eta}) = \sigma^2 \mathbf{P}^{-1} (\mathbf{P} \mathbf{P}^T) (\mathbf{P}^T)^{-1} = \sigma^2 \mathbf{I}_n`$

#### Referência Audiovisual

* EST5507 Modelos de Regressão - Modelo linear com erros correlacionados - Caso 2 (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Encerrada a teoria clássica gaussianas e heterocedástica, ingressamos no Módulo 5, expandindo a modelagem para a Família Exponencial de distribuições (MLGs).

## Módulo 5: Modelos Lineares Generalizados (MLG) e Regressão Logística

### Aula 13: Introdução aos MLGs e Aprendizado de Máquina (Família Exponencial, Preditor Linear e Função de Ligação)

#### Objetivo Didático

Definir a estrutura conceitual dos Modelos Lineares Generalizados (MLGs), mapear as distribuições da Família Exponencial, conectar a média ao Preditor Linear via Função de Ligação $`g(\mu) = \eta = \mathbf{X}\mathbf{\beta}`$ e contrastar a abordagem estatística inferencial com a abordagem preditiva do Aprendizado de Máquina.

#### Intuição e Analogias

#### Pense no MLG como um "tradutor universal de idiomas":

* Preditor Linear $`(\eta = \mathbf{X}\mathbf{\beta})`$: Opera no idioma numérico contínuo, gerando combinações que variam no intervalo $`(-\infty, +\infty)`$.
* Função de Ligação $`(g(\mu))`$: É o tradutor que conecta a combinação contínua à escala física real da resposta (ex: mapeando o intervalo infinito para probabilidades em (0,1) ou contagens não negativas em [$`0, \infty`$)).

Enquanto o Aprendizado de Máquina foca na acurácia preditiva pontual ("caixa-preta"), os MLGs priorizam a inferência, significância estatística e a interpretação causal dos coeficientes.

#### Exemplo Numérico Passo a Passo

A distribuição de Poisson é dada por f(y; $`\lambda) = \frac{e^{-\lambda} \lambda^y}{y!}`$. Reescrevendo a densidade na forma canônica da Família Exponencial: f(y; $`\theta, \phi) = \exp\left\{ \frac{y\theta - b(\theta)}{a(\phi)} + c(y, \phi) \right\}`$

#### Passos algébricos:

1. Aplicando a transformação exponencial e o logaritmo natural: f(y; $`\lambda) = \exp\left\{ \ln\left( \frac{e^{-\lambda} \lambda^y}{y!} \right) \right\} = \exp\left\{ y \ln(\lambda) - \lambda - \ln(y!) \right\}`$
2. Mapeando os termos na estrutura canônica:
  * Parâmetro Natural $`(\theta): \theta = \ln(\lambda) \implies \lambda = e^\theta`$.
  * Função $`b(\theta): b(\theta) = e^\theta`$.
  * Parâmetro de Dispersão $`a(\phi): a(\phi) = 1 (\phi = 1)`$.
  * Termo Residual $`c(y, \phi): c(y, \phi) = -\ln(y!)`$.

Obtenção da Média E(Y): Pelas propriedades da Família Exponencial, $`E(Y) = b'(\theta) = \frac{d}{d\theta}(e^\theta) = e^\theta = \lambda = \mu`$. A função de ligação canônica satisfaz $`\eta = \theta`$, resultando em $`g(\mu) = \ln(\mu)`$ (Ligação Logarítmica).

#### Erros Comuns de Iniciantes

* Confundir g(E(Y)) com E(g(Y)): Aplicar a transformação não linear diretamente sobre os dados observados da resposta em vez de transformar a função de média populacional.
* Exigir Normalidade Residual em MLG: Esquecer que os MLGs foram desenvolvidos especificamente para acomodar respostas não gaussianas (Bernoulli, Binomial, Poisson, Gama, Gaussiana Inversa).

#### Resumo Conceitual

1. Um MLG é constituído por três componentes: Componente Aleatório (Família Exponencial), Componente Sistemático (Preditor Linear $`\eta = \mathbf{X}\mathbf{\beta}`$) e Função de Ligação $`(g(\mu) = \eta)`$.
2. A variância da resposta é decomposta como $`Var(Y) = a(\phi) V(\mu)`$, onde $`V(\mu)`$ é a função de variância.
3. Os MLGs preservam a inferência intervalar e o teste de hipóteses, ao passo que o Aprendizado de Máquina foca na minimização do erro de predição fora da amostra.

#### Exercícios Resolvidos

1. Exercício 1: Escreva a distribuição Bernoulli f(y; $`p) = p^y (1-p)^{1-y}`$ na forma da Família Exponencial e determine a ligação canônica.
  * Gabarito Comentado: f(y; $`p) = \exp\left\{ \ln\left( p^y (1-p)^{1-y} \right) \right\} = \exp\left\{ y \ln\left(\frac{p}{1-p}\right) + \ln(1-p) \right\}`$ Parâmetro natural $`\theta = \ln\left(\frac{p}{1-p}\right)`$. A ligação canônica satisfaz $`\eta = \theta = \ln\left(\frac{\mu}{1-\mu}\right)`$ (Função Logito).
2. Exercício 2: Qual é a função de variância $`V(\mu)`$ para a distribuição de Poisson e para a distribuição Gama?
  * Gabarito Comentado:
    * Poisson: $`V(\mu) = \mu`$ (a variância é igual à média).
    * Gama: $`V(\mu) = \mu^2`$ (o desvio padrão cresce linearmente com a média).
3. Exercício 3: Por que não se deve aplicar MQO clássico em dados de contagem discreta com presença substancial de zeros?
  * Gabarito Comentado: Dados de contagem violam a premissa de continuidade e normalidade, apresentando heterocedasticidade intrínseca e gerando previsões negativas incoerentes sob regressão linear simples.

#### Referência Audiovisual

* Modelos de Regressão e Aprendizado Supervisionado II - Aprendizado de Máquina & Estimação de máxima verossimilhança em MLGs (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Para avaliar a qualidade do ajuste em MLGs sem utilizar a soma de quadrados residual simples, empregamos a Função Desvio (Deviance), abordada na Aula 14.

### Aula 14: Diagnóstico em MLGs e Função Desvio (Deviance e Resíduos em MLGs)

#### Objetivo Didático

Avaliar a qualidade de ajuste de um MLG empregando a Função Desvio $`(D(\mathbf{y}; \hat{\mathbf{\mu}}))`$, comparando o modelo proposto ao Modelo Saturado e efetuando o diagnóstico via Resíduos de Pearson e do Desvio.

#### Intuição e Analogias

Pense no Modelo Saturado como um traje feito sob medida por um alfaiate: ele atribui um parâmetro individual para cada ponto amostral, obtendo um ajuste perfeito com erro nulo $`(L(\mathbf{y}; \mathbf{y}))`$, mas sem capacidade de generalização. A Deviance mede a "distância" entre o nosso modelo simplificado proposto (o traje de tamanho padrão) e o modelo saturado ideal. Quanto menor a deviance, mais próximo nosso modelo está do ajuste perfeito sob medida.

```text
       [ Modelo Saturado ]  --> Ajuste Perfeito (n parâmetros, 0 df)
               |
               |  <--- DISTÂNCIA MEDIDA PELA DEVIANCE D(y; µ̂)
               v
       [ Modelo Proposto ]  --> Modelo Simplificado (p+1 parâmetros, n-p-1 df)
```

#### Exemplo Numérico Passo a Passo

A fórmula da Deviance para o modelo Poisson é dada por: $`D(\mathbf{y}; \hat{\mathbf{\mu}}) = 2 \sum_{i=1}^{n} \left[ y_i \ln\left( \frac{y_i}{\hat{\mu}_i} \right) - (y_i - \hat{\mu}_i) \right]`$

(Convenção: se $`y_i = 0`$, o termo $`y_i \ln(y_i / \hat{\mu}_i)`$ é igual a 0).

#### Considere n=3 observações de contagem com os dados observados e preditos pelo modelo:

| Indivíduo (i) | Observado $`(y_i)`$ | Predito $`(\hat{\mu}_i)`$ | $`y_i \ln(y_i / \hat{\mu}_i)`$ | $`(y_i - \hat{\mu}_i)`$ | Termo Interno |
|---|---|---|---|---|---|
| 1 | 2 | 2,5 | $`2 \cdot \ln(2 / 2{,}5) = -0{,}4463`$ | $`-0{,}5`$ | $`-0{,}4463 - (-0{,}5) = 0{,}0537`$ |
| 2 | 5 | 4,0 | $`5 \cdot \ln(5 / 4{,}0) = 1{,}1157`$ | $`1{,}0`$ | $`1{,}1157 - 1{,}0 = 0{,}1157`$ |
| 3 | 1 | 1,2 | $`1 \cdot \ln(1 / 1{,}2) = -0{,}1823`$ | $`-0{,}2`$ | $`-0{,}1823 - (-0{,}2) = 0{,}0177`$ |
| Soma | 8 | 7,7 | — | — | 0,1871 |

Cálculo da Deviance Total: $`D(\mathbf{y}; \hat{\mathbf{\mu}}) = 2 \times (0{,}1871) = 0{,}3742`$

A deviance calculada de $`0{,}3742`$ é comparada aos graus de liberdade residuais (n - p - 1) através da distribuição $`\chi^2`$.

#### Erros Comuns de Iniciantes

* Analisar Resíduos Brutos em MLG: Utilizar resíduos brutos $`(y_i - \hat{\mu}_i)`$ no diagnóstico de MLGs, ignorando a dependência entre variância e média. Deve-se empregar os Resíduos de Pearson $`(r_i^P = \frac{y_i - \hat{\mu}_i}{\sqrt{V(\hat{\mu}_i)}})`$ ou Resíduos Componente do Desvio $`(r_i^D)`$.
* Avaliar a Deviance sem Graus de Liberdade: Julgar o valor absoluto da Deviance sem considerar os graus de liberdade residuais do modelo.

#### Resumo Conceitual

1. O Modelo Saturado destina um parâmetro para cada observação, obtendo a verossimilhança máxima $`L(\mathbf{y}; \mathbf{y})`$.
2. A Deviance $`D(\mathbf{y}; \hat{\mathbf{\mu}}) = 2 \left[ \ln L(\mathbf{y}; \mathbf{y}) - \ln L(\hat{\mathbf{\mu}}; \mathbf{y}) \right]`$ mede o desvio de ajuste em relação ao modelo ideal.
3. A diferença de Deviances entre dois modelos aninhados atua como o Teste da Razão de Verossimilhanças $`(\Delta D \sim \chi^2(\Delta p))`$.

#### Exercícios Resolvidos

1. Exercício 1: Determine o Resíduo de Pearson para uma observação $`y_i = 6`$ cujo valor predito em um modelo Poisson é $`\hat{\mu}_i = 4{,}0`$.
  * Gabarito Comentado: Para a Poisson, $`V(\mu) = \mu. r_i^P = \frac{y_i - \hat{\mu}_i}{\sqrt{V(\hat{\mu}_i)}} = \frac{6 - 4{,}0}{\sqrt{4{,}0}} = \frac{2}{2} = 1{,}0`$
2. Exercício 2: Um modelo reduzido apresentou $`D_0 = 45{,}2 (df = 20)`$ e o modelo completo apresentou $`D_1 = 32{,}1 (df = 17)`$. Teste se a inclusão das 3 novas preditoras é significante ao nível de $`5\% (\chi^2_{critico}(3) = 7{,}81)`$.
  * Gabarito Comentado: $`\Delta D = D_0 - D_1 = 45{,}2 - 32{,}1 = 13{,}1 \Delta`$ df = 20 - 17 = 3 Como $`\Delta D = 13{,}1 > 7{,}81`$, rejeita-se $`H_0`$. A adição dos parâmetros melhora significantemente o ajuste.
3. Exercício 3: Qual é a diferença entre a Deviance Escalada e Não Escalada?
  * Gabarito Comentado: A Deviance Escalada divide a Deviance pelo parâmetro de dispersão $`\phi: D^*(\mathbf{y}; \hat{\mathbf{\mu}}) = \frac{D(\mathbf{y}; \hat{\mathbf{\mu}})}{\phi}`$. Na Poisson e Binomial $`(\phi=1)`$, a deviance escalada coincide com a não escalada.

#### Referência Audiovisual

* Modelos de Regressão e Aprendizado Supervisionado II - Diagnóstico em MLGs - a função desvio (Profa. Cibele Russo).

> [!TIP]
> Próximos Passos & Transição: Concluímos nossa trilha aplicando a estrutura dos MLGs ao caso mais proeminente da modelagem categórica: a Regressão Logística Binária na Aula 15.

### Aula 15: Regressão Logística e Resposta Binária (Razão de Chances / Odds Ratio, Função Logito e Prática Computacional)

#### Objetivo Didático

Formular o Modelo de Regressão Logística para variáveis respostas binárias $`(Y_i \sim \text{Bernoulli}(p_i))`$, aplicar a função de ligação logito $`(\ln\left(\frac{p}{1-p}\right) = \mathbf{x}_i^T \mathbf{\beta})`$ e interpretar a Razão de Chances (Odds Ratio - $`OR = e^{\beta_1}`$).

#### Intuição e Analogias

#### Pense na Chance (Odds) como em uma aposta de corrida de cavalos:

* Se a probabilidade de vitória é $`80\% (p = 0{,}80)`$, a probabilidade de derrota é $`20\% (1-p = 0{,}20)`$.
* A Chance (Odds) de vitória é $`0{,}80 / 0{,}20 = 4{,}0`$ (ou seja, 4 para 1).

A Razão de Chances (Odds Ratio - OR) compara como essa aposta muda ao alterarmos um fator explicativo. Se o $`OR = 2{,}22`$, a chance de vitória do cavalo aumenta em $`122{,}55\%`$ sob o novo fator.

#### Exemplo Numérico Passo a Passo

Considere um modelo logístico ajustado para prever a aprovação de um crédito (Y=1), onde X indica a presença de garantia bancária (X=1) ou ausência (X=0): $`\ln\left( \frac{\hat{p}}{1 - \hat{p}} \right) = -2{,}0 + 0{,}8 \cdot X`$

1. Probabilidade para X=0 (Sem Garantia): $`\eta_0 = -2{,}0 + 0{,}8(0) = -2{,}0 \hat{p}_0 = \frac{e^{\eta_0}}{1 + e^{\eta_0}} = \frac{e^{-2{,}0}}{1 + e^{-2{,}0}} = \frac{0{,}1353}{1 + 0{,}1353} \approx 0{,}1192 \quad (11{,}92\%)`$

2. Probabilidade para X=1 (Com Garantia): $`\eta_1 = -2{,}0 + 0{,}8(1) = -1{,}2 \hat{p}_1 = \frac{e^{-1{,}2}}{1 + e^{-1{,}2}} = \frac{0{,}3012}{1 + 0{,}3012} \approx 0{,}2315 \quad (23{,}15\%)`$

3. Cálculo e Interpretação da Razão de Chances (Odds Ratio): $`OR = e^{\hat{\beta}_1} = e^{0{,}8} \approx 2{,}2255`$

Interpretação: Possuir garantia bancária (X=1) aumenta a chance de aprovação do crédito em $`122{,}55\% (2{,}2255 - 1)`$ em relação à ausência de garantia. A relação no logito é linear $`(+0{,}8)`$, enquanto a relação com a probabilidade direta é descrita por uma curva sigmoide não linear.

```text
     Probabilidade (p)
     1.0 |                                    .---'
         |                                  .'
     0.5 |                              _.-'
         |                          _.-'
     0.0 |                  _.----''
         +----------------------------------------- Preditor Linear (η)
                          Curva Sigmoide Logística
```

#### Erros Comuns de Iniciantes

* Confundir Probabilidade com Chance (Odds): Afirmar que $`OR = 2{,}0`$ dobra a probabilidade. O OR indica que a chance (p/(1-p)) foi dobrada. A variação na probabilidade depende do ponto inicial sobre a curva sigmoide.
* Interpretar $`\hat{\beta}_1`$ como Mudança Linear Direta em Y: Esquecer que $`\hat{\beta}_1`$ mede a variação constante no logito, e não uma alteração linear constante na probabilidade $`\hat{p}`$.

#### Resumo Conceitual

1. A Regressão Logística adota a ligação logito $`\ln\left(\frac{p}{1-p}\right) = \mathbf{X}\mathbf{\beta}`$, garantindo probabilidades no intervalo (0,1).
2. O exponencial $`e^{\hat{\beta}_j}`$ expressa a Razão de Chances (Odds Ratio) para incrementos unitários na preditora $`X_j`$.
3. Ausente uma solução analítica fechada, a estimação de MV é conduzida numericamente via algoritmos iterativos (IWLS / Newton-Raphson).

#### Exercícios Resolvidos

1. Exercício 1: Se a probabilidade de sucesso é $`p = 0{,}75`$, determine a chance (odds) e o logito.
  * Gabarito Comentado:
    * Chance (Odds): Odds = $`\frac{0{,}75}{1 - 0{,}75} = \frac{0{,}75}{0{,}25} = 3{,}0 (3`$ para 1).
    * Logito: Logit = $`\ln(3{,}0) \approx 1{,}0986`$.
2. Exercício 2: Em um modelo logístico, o coeficiente para o hábito de fumar foi $`\hat{\beta}_1 = 0{,}5`$. Calcule e interprete o Odds Ratio.
  * Gabarito Comentado: $`OR = e^{\hat{\beta}_1} = e^{0{,}5} \approx 1{,}6487`$ A chance de desenvolver a enfermidade entre fumantes é $`64{,}87\%`$ superior à chance entre não fumantes.
3. Exercício 3: Se um indivíduo apresenta um preditor linear $`\eta = 0`$, qual é sua probabilidade estimada de sucesso $`\hat{p}?`$
  * Gabarito Comentado: $`\hat{p} = \frac{e^0}{1 + e^0} = \frac{1}{1 + 1} = 0{,}50 \quad (50\%)`$

#### Referência Audiovisual

* Modelos de Regressão - Modelo de regressão logística e MLGs & Prática - Modelo de regressão logística e outros modelos para resposta binária (Profa. Cibele Russo).

#### Conclusão da Trilha Didática

Parabéns por concluir esta jornada avançada sobre Modelos de Regressão! Você percorreu uma trilha rigorosa que se iniciou na intuição dos Mínimos Quadrados Simples, avançou pelos fundamentos da Álgebra Matricial e Distribuições Multivariadas, formalizou o Modelo Linear Geral e consolidou a inferência em Modelos Lineares Generalizados.

Com este arcabouço conceitual e matemático, você está preparado para formular, estimar, diagnosticar e interpretar modelos estatísticos avançados, unindo o rigor analítico à tomada de decisão fundamentada em dados.

## Atualização nov/2026

O [guia de estudo](guia-de-estudo.md) desta atualização inclui as aulas novas de MLG para dados de contagem (Poisson e binomial negativa) e de regressão logística. O guia não menciona explicitamente os dados eleitorais da aula-fonte; o trecho abaixo reproduz só o que o guia traz sobre esses temas. O guia completo está em [guia-de-estudo.md](guia-de-estudo.md).

### Questionário (trechos)

8. Por que a acurácia se mostra uma métrica inadequada para diagnosticar a qualidade de modelos de regressão logística em cenários de dados desbalanceados e quais métricas alternativas são recomendadas?

9. Como se diagnostica o fenômeno da sobredispersão em modelos de regressão Poisson e qual distribuição é indicada para contornar essa limitação?

10. Qual é a interpretação prática do coeficiente exponenciado $`\exp(\hat{\beta}_j)`$ em um modelo Poisson com ligação logarítmica comparada à sua interpretação em um modelo de regressão logística?

### Gabarito (trechos)

8. Em bases desbalanceadas, onde a classe majoritária domina a amostra, um classificador simplório que atribua todas as observações à classe majoritária obterá uma acurácia elevada, mas falhará em identificar os eventos da classe minoritária de interesse. Essa métrica gera uma falsa sensação de adequação do ajuste estatístico em problemas como análise de fraude ou inadimplência. Para contornar essa limitação, recomendam-se métricas baseadas na matriz de confusão como o Coeficiente de Correlação de Matthews (MCC), a Média Geométrica (G-Mean) e o índice Kappa de Cohen.

9. A sobredispersão em modelos Poisson ocorre quando a variância empírica dos dados excede a média esperada, sendo identificada por resíduos desalinhados fora das bandas de confiança do gráfico de envelope ou por razões elevadas entre o desvio e os graus de liberdade. Esse desajuste indica que a suposição de igualdade entre média e variância ($`Var(Y) = \mu`$) foi violada. Para corrigi-lo, adota-se o modelo Binomial Negativo, que introduz um parâmetro adicional de dispersão $`\phi`$, expandindo a variância para $`\operatorname{Var}(Y) = \mu + \mu^2 \phi`$.

10. No modelo Poisson com ligação logarítmica, a exponenciação $`\exp(\hat{\beta}_j)`$ representa a Razão de Taxas, indicando que o incremento de uma unidade na covariável $`X_j`$ multiplica a resposta média esperada $`\mu`$ pelo fator $`\exp(\hat{\beta}_j)`$. No modelo de regressão logística, $`\exp(\hat{\beta}_j)`$ corresponde à Razão de Chances (odds ratio), representando o fator multiplicativo sobre a chance de sucesso $`\mu`$/(1-$`\mu`$). Dessa forma, enquanto na Poisson o efeito é multiplicativo direto sobre a taxa esperada, na regressão logística ele atua sobre a razão entre probabilidades de sucesso e fracasso.

### Questões para desenvolvimento (trechos)

4. Analise o problema da sobredispersão em modelos para dados de contagem. Explique as limitações teóricas do modelo Poisson diante da sobredispersão, como a distribuição Binomial Negativa estrutura a sua função de variância para contornar essa deficiência e quais são as consequências inferenciais de se ignorar a sobredispersão em uma análise estatística.

5. Em contextos de classificação binária com dados desbalanceados (como na modelagem de risco de crédito ou detecção de fraudes), discuta por que métricas tradicionais baseadas na acurácia falham. Sintetize as métricas alternativas de desempenho (MCC, G-Mean, Kappa) e discuta os impactos da escolha de funções de ligação alternativas (como probito ou complemento log-log) em relação ao modelo logito convencional.

### Termos do glossário (trechos)

| Termo | Definição |
|---|---|
| Acurácia | Proporção total de acertos de um modelo de classificação em relação ao total de observações. Revela-se uma métrica enganosa em conjuntos de dados desbalanceados. |
| Desvio Nulo (Null Deviance) | Valor da função desvio obtido ao se ajustar o modelo nulo (contendo apenas o termo de intercepto), servindo como base de comparação para medir o ganho de informação trazido pelas covariáveis no modelo proposto. |
| Função de Ligação ($`g(\mu)`$) | Função monótona e diferenciável que relaciona a média esperada da variável resposta $`\mu_i`$ ao preditor linear $`\eta_i`$, permitindo acomodar restrições no suporte da variável resposta. |
| Função de Ligação Canônica | Função de ligação para a qual o preditor linear equivale diretamente ao parâmetro natural da família exponencial ($`\eta_i = \theta_i`$). Garante boas propriedades algébricas e a unicidade das estimativas de máxima verossimilhança. |
| Função Desvio (Deviance) | Estatística de qualidade do ajuste definida por $`2[L(y; y) - L(\hat{\mu}; y)]`$, que mede o quão distante a log-verossimilhança do modelo proposto está da log-verossimilhança do modelo saturado. |
| Modelo Binomial Negativo | Modelo para dados de contagem que estende a distribuição Poisson ao incorporar um parâmetro extra de dispersão, tornando a variância uma função quadrática da média ($`\operatorname{Var}(Y) = \mu + \mu^2 \phi`$) e corrigindo a sobredispersão. |
| Modelo Poisson | MLG para dados de contagem que assume a distribuição Poisson para a resposta e utiliza a função de ligação logarítmica, impondo a restrição de equidispersão (Var(Y) = E[Y] = $`\mu`$). |
| Razão de Chances (Odds Ratio) | Medida de efeito obtida por $`\exp(\hat{\beta}_j)`$ na regressão logística, que expressa como a chance de ocorrência de um evento ($`\mu/(1-\mu)`$) é multiplicada ao se incrementar uma unidade na covariável $`X_j`$. |
| Razão de Taxas | Medida de efeito obtida por $`\exp(\hat{\beta}_j)`$ no modelo Poisson com ligação logarítmica, indicando o fator multiplicativo sobre a resposta média esperada $`\mu`$ por unidade de incremento na covariável $`X_j`$. |
| Sobredispersão (Superdispersão) | Fenômeno verificado quando a variância empírica dos dados observados excede a variância teórica prevista pelo modelo probabilístico assumido (frequente em modelos Poisson). |
