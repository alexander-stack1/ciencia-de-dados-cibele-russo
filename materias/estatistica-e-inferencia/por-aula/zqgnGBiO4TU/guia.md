> **Aula:** [SME0123 Estatística I - Intro Inferência Estatística - Estimação pontual Aula de 16/10/2026](https://www.youtube.com/watch?v=zqgnGBiO4TU)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudo: SME0123 Estatística I – Estimação Pontual, Máxima Verossimilhança e Propriedades dos Estimadores

#### 1. Objetivos da Aula e Visão Geral

Este guia destina-se a alunos de pós-graduação e graduação avançada e sintetiza os conceitos abordados na aula sobre inferência estatística, estimação pontual, método de máxima verossimilhança e propriedades dos estimadores.

##### Objetivos de Aprendizagem

* **Compreender a Transição Probabilidade vs. Inferência:** Diferenciar a abordagem probabilística (parâmetros fixos conhecidos para cálculo de probabilidades) da abordagem de inferência estatística (parâmetros desconhecidos a serem estimados a partir de dados amostrais).
* **Diferenciar População e Amostra:** Distinguir claramente o parâmetro populacional ($`\mu, \sigma^2, \lambda`$) do seu respectivo estimador ($`\hat{\mu}, \hat{\sigma}^2, \hat{\lambda}`$ ou $`\bar{X}`$), identificando onde cada entidade reside.
* **Dominar o Algoritmo de Máxima Verossimilhança (EMV):** Construir a função de verossimilhança $`L(\theta)`$, aplicar a transformação logarítmica $`\ln L(\theta)`$, derivar em relação aos parâmetros de interesse, igualar a zero e verificar as condições de segundo ordenamento (máximo global).
* **Aplicar a Propriedade de Invariância:** Estimar funções de parâmetros $`g(\theta)`$ a partir do estimador de máxima verossimilhança $`\hat{\theta}`$.
* **Analisar Propriedades dos Estimadores:** Avaliar viés (vício), Erro Quadrático Médio (EQM), eficiência relativa entre estimadores não viesados e consistência em grandes amostras.

#### 2. Desenvolvimento Passo a Passo do Conteúdo

##### 2.1. O Problema da Inferência Estatística e Estimação Pontual

Na inferência estatística, considera-se uma variável aleatória $`X`$ cuja distribuição possui parâmetros desconhecidos. Observa-se uma amostra aleatória $`X_1, X_2, \dots, X_n`$.

* **Estimador:** É uma função da amostra aleatória, isto é, uma expressão ou variável aleatória $`T(X) = T(X_1, \dots, X_n)`$ considerada **antes** da observação dos dados. O estimador possui uma distribuição de probabilidade.
* **Estimativa:** É o valor numérico específico assumido pelo estimador após a coleta e observação da amostra concreta $`x_1, \dots, x_n`$.
* **Distinção Fundamental:**
* Parâmetro populacional ($`\mu = EX`$): Valor fixo e desconhecido pertencente à população.
* Estimador amostral ($`\hat{\mu} = \bar{X}`$): Variável aleatória pertencente à amostra.

##### 2.2. O Método de Máxima Verossimilhança (EMV)

O método busca encontrar o valor do parâmetro que maximiza a probabilidade (ou densidade conjunta) de ter observado a amostra atual.

###### *Algoritmo Geral para Obtenção do EMV:*

* **Construção da Função de Verossimilhança:** Para $`X_1, \dots, X_n`$ independentes e identicamente distribuídas (i.i.d.):
  ```math
  L(\theta \mid x_1, \dots, x_n) = \prod_{i=1}^n f(x_i \mid \theta) \quad \text{ou} \quad \prod_{i=1}^n P(X_i = x_i \mid \theta)
  ```
* **Aplicação do Logaritmo (Log-Verossimilhança):** Como o logaritmo natural ($`\ln`$) é uma função monótona crescente, maximizar $`L(\theta)`$ equivale a maximizar $`\ln L(\theta)`$:
  ```math
  \ell(\theta) = \ln L(\theta \mid x_1, \dots, x_n) = \sum_{i=1}^n \ln f(x_i \mid \theta)
  ```
* **Obtenção do Candidato a Estimador:** Deriva-se a log-verossimilhança em relação ao parâmetro (ou sistema de derivadas parciais no caso multiparamétrico) e iguala-se a zero:
  ```math
  \frac{\partial \ell(\theta)}{\partial \theta} = 0 \implies \hat{\theta}
  ```
  *Ao igualar a zero, substitui-se o parâmetro pelo seu candidato a estimador (* *$`\hat{\theta}`$ *) e os valores observados* *$`x_i`$ *por variáveis aleatórias* *$`X_i`$ *.*
* **Verificação do Ponto de Máximo:**
* Caso unidimensional: Verificar se $`\frac{d^2 \ell(\theta)}{d\theta^2} < 0`$ avaliado em $`\hat{\theta}`$.
* Caso multidimensional: Verificar se a Matriz Hessiana de derivadas segundas é **definida negativa** .

##### 2.3. Exemplos Detalhados de Derivação

###### *Exemplo 1: Distribuição Normal $`N(\mu, \sigma^2)`$ com Parâmetros Desconhecidos*

Seja $`X_1, \dots, X_n \sim N(\mu, \sigma^2)`$. Encontrar os estimadores de máxima verossimilhança para $`\mu`$ e $`\sigma^2`$.

* **Função de Verossimilhança:**
  ```math
  L(\mu, \sigma^2) = \prod_{i=1}^n \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( -\frac{(x_i - \mu)^2}{2\sigma^2} \right) = (2\pi\sigma^2)^{-n/2} \exp\left( -\frac{1}{2\sigma^2} \sum_{i=1}^n (x_i - \mu)^2 \right)
  ```
* **Log-Verossimilhança:**
  ```math
  \ell(\mu, \sigma^2) = \ln L(\mu, \sigma^2) = -\frac{n}{2} \ln(2\pi\sigma^2) - \frac{1}{2\sigma^2} \sum_{i=1}^n (x_i - \mu)^2
  ```
* **Sistema de Equações de Verossimilhança (Gradiente nulo):**
* **Derivada em relação a $`\mu`$(aplicando a regra da cadeia):**
  ```math
  \frac{\partial \ell}{\partial \mu} = -\frac{1}{2\sigma^2} \sum_{i=1}^n 2(x_i - \mu)(-1) = \frac{1}{\sigma^2} \sum_{i=1}^n (x_i - \mu) = 0
  ```
  Como $`\sigma^2 > 0`$:
  ```math
  \sum_{i=1}^n (X_i - \hat{\mu}) = 0 \implies \sum_{i=1}^n X_i - n\hat{\mu} = 0 \implies \hat{\mu} = \frac{1}{n} \sum_{i=1}^n X_i = \bar{X}
  ```
* **Derivada em relação a $`\sigma^2`$:**
  ```math
  \frac{\partial \ell}{\partial \sigma^2} = -\frac{n}{2} \left( \frac{1}{2\pi\sigma^2} \cdot 2\pi \right) - \left( -\frac{1}{2(\sigma^2)^2} \right) \sum_{i=1}^n (x_i - \mu)^2 = -\frac{n}{2\sigma^2} + \frac{1}{2(\sigma^2)^2} \sum_{i=1}^n (x_i - \mu)^2 = 0
  ```
  Multiplicando toda a equação por $`2(\sigma^2)^2`$ e substituindo $`\mu`$ por $`\hat{\mu} = \bar{X}`$:
  ```math
  -n\hat{\sigma}^2 + \sum_{i=1}^n (X_i - \bar{X})^2 = 0 \implies \hat{\sigma}^2 = \frac{1}{n} \sum_{i=1}^n (X_i - \bar{X})^2
  ```
* **Ponto de Máximo:** Para comprovação rigorosa, a matriz Hessiana $`H`$ dada por:
  ```math
  H = \begin{bmatrix} \frac{\partial^2 \ell}{\partial \mu^2} & \frac{\partial^2 \ell}{\partial \mu \partial \sigma^2} \\ \frac{\partial^2 \ell}{\partial \sigma^2 \partial \mu} & \frac{\partial^2 \ell}{\partial (\sigma^2)^2} \end{bmatrix}
  ```
  deve ser provada como **definida negativa** (ou ter determinante $`det(H) > 0`$ e $`\frac{\partial^2 \ell}{\partial \mu^2} < 0`$).

###### *Exemplo 2: Distribuição Poisson com Parâmetro $`\lambda > 0`$*

Seja $`X_1, \dots, X_n \sim \text{Poisson}(\lambda)`$, onde $`P(X = x) = \frac{e^{-\lambda} \lambda^x}{x!}`$.

1. **Função de Verossimilhança:**
   ```math
   L(\lambda) = \prod_{i=1}^n \frac{e^{-\lambda} \lambda^{x_i}}{x_i!} = \frac{e^{-n\lambda} \lambda^{\sum_{i=1}^n x_i}}{\prod_{i=1}^n x_i!}
   ```
2. **Log-Verossimilhança:**
   ```math
   \ell(\lambda) = -n\lambda + \left( \sum_{i=1}^n x_i \right) \ln(\lambda) - \ln\left( \prod_{i=1}^n x_i! \right)
   ```
3. **Derivada e Candidato a EMV:**
   ```math
   \frac{d\ell(\lambda)}{d\lambda} = -n + \frac{\sum_{i=1}^n x_i}{\lambda} = 0 \implies n = \frac{\sum_{i=1}^n X_i}{\hat{\lambda}} \implies \hat{\lambda} = \frac{1}{n} \sum_{i=1}^n X_i = \bar{X}
   ```
4. **Confirmação do Máximo:**
   ```math
   \frac{d^2\ell(\lambda)}{d\lambda^2} = -\frac{\sum_{i=1}^n X_i}{\lambda^2}
   ```
   Como $`X_i \ge 0`$ para a distribuição Poisson, a segunda derivada é menor do que zero ($`\frac{d^2\ell}{d\lambda^2} < 0`$), confirmando que $`\hat{\lambda} = \bar{X}`$ é o EMV global.

###### *Tabela Resumo de Exercícios Propostos de EMV*

| Distribuição | Parâmetro de Interesse | Função de Massa / Densidade | EMV Obtido ($`\hat{\theta}`$) |
|---|---|---|---|
| Bernoulli | $`\theta \in (0,1)`$ | $`P(X=x) = \theta^x (1-\theta)^{1-x}`$ | $`\hat{\theta} = \bar{X}`$ |
| Binomial | $`\theta \in (0,1)`$ ($`m`$ conhecido) | $`P(X=x) = \binom{m}{x} \theta^x (1-\theta)^{m-x}`$ | $`\hat{\theta} = \frac{\bar{X}}{m}`$ |
| Normal | $`\mu = \theta`$ ($`\sigma^2 = 1`$ conhecida) | $`f(x) = \frac{1}{\sqrt{2\pi}} e^{-\frac{(x-\theta)^2}{2}}`$ | $`\hat{\theta} = \bar{X}`$ |


##### 2.4. Propriedade de Invariância dos EMVs

Se $`\hat{\theta}`$ é o estimador de máxima verossimilhança de $`\theta`$, então para qualquer função $`g(\theta)`$, o estimador de máxima verossimilhança de $`g(\theta)`$ é dado simplesmente por $`g(\hat{\theta})`$.

* **Exemplo (Poisson):** Estimar a probabilidade de ocorrência de zero eventos, isto é, $`g(\lambda) = P(X = 0) = e^{-\lambda}`$. Como o EMV de $`\lambda`$ é $`\hat{\lambda} = \bar{X}`$, pela propriedade de invariância, o EMV de $`P(X=0)`$ é:
  ```math
  \widehat{P(X=0)} = e^{-\bar{X}}
  ```
* **Exemplo (Bernoulli):** Estimar a variância $`g(p) = p(1-p)`$. O EMV será:
  ```math
  \widehat{g(p)} = \hat{p}(1-\hat{p}) = \bar{X}(1-\bar{X})
  ```

##### 2.5. Propriedades Avançadas dos Estimadores

###### *A. Viés ou Vício ($`B_\theta(T)`$)*

O viés de um estimador $`T(X)`$ para estimar $`\theta`$ é a diferença entre o seu valor esperado e o valor real do parâmetro:
```math
B_\theta(T) = ET(X) - \theta
```

* **Não Viesado (Imparcial/Não Viciado):** Se $`B_\theta(T) = 0 \iff ET(X) = \theta, \forall \theta`$.
* **Assintoticamente Não Viesado:** Se $`\lim_{n \to \infty} B_\theta(T) = 0`$.

###### *B. Erro Quadrático Médio (EQM)*

Mede a distância quadrática média entre o estimador e o verdadeiro parâmetro:
```math
EQM(T(X)) = E\left[ (T(X) - \theta)^2 \right ]
```
**Decomposição Teórica do EQM (Dedução):**
```math
EQM(T) = E\left[ \left( (T - ET) + (ET - \theta) \right)^2 \right]
```
```math
EQM(T) = E\left[ (T - ET)^2 + 2(T - ET)(ET - \theta) + (ET - \theta)^2 \right]
```
Como $`(ET - \theta)`$ é uma constante em relação à esperança:
```math
EQM(T) = E[(T - ET)^2] + 2(ET - \theta) \underbrace{E[T - ET]}_{= 0} + (ET - \theta)^2
```
```math
EQM(T) = \text{Var}(T(X)) + B_\theta(T)^2
```
*Nota de Aplicação:* Esta relação reflete o balanço viés-variância ( *bias-variance tradeoff* ), crucial em modelos de aprendizado de máquina.

###### *C. Eficiência*

Dados dois estimadores **não viesados** $`T_1(X)`$ e $`T_2(X)`$ para $`\theta`$, diz-se que $`T_1(X)`$ é **mais eficiente** que $`T_2(X)`$ se:
```math
\text{Var}(T_1(X)) < \text{Var}(T_2(X))
```

###### *D. Consistência*

Uma sequência de estimadores $`T_n(X)`$ é **consistente** se converge em probabilidade para o parâmetro $`\theta`$ quando o tamanho da amostra tende ao infinito:
```math
\forall \epsilon > 0, \quad \lim_{n \to \infty} P(|T_n(X) - \theta| \ge \epsilon) = 0
```

1. **Desigualdade de Tchebycheff (Limitante):**
   ```math
   P(|T_n(X) - \theta| \ge \epsilon) \le \frac{\text{Var}(T_n(X))}{\epsilon^2}
   ```
2. **Condições Suficientes Práticas para Consistência:**
3. $`\lim_{n \to \infty} ET_n(X) = \theta`$ (Não viesado assintoticamente)
4. $`\lim_{n \to \infty} \text{Var}(T_n(X)) = 0`$

##### 2.6. Análise de Viés e Correção para a Variância Normal (Exemplo 6.8)

Considere a amostra $`X_1, \dots, X_n \sim N(\mu, \sigma^2)`$:

1. **Análise do Estimador da Média ($`\hat{\mu} = \bar{X}`$):**
   ```math
   E\bar{X} = E\left [\frac{1}{n} \sum_{i=1}^n X_i \right ] = \frac{1}{n} \sum_{i=1}^n EX_i = \frac{1}{n} (n\mu) = \mu
   ```
   Portanto, $`B(\hat{\mu}) = \mu - \mu = 0`$. O estimador $`\bar{X}`$ é **não viesado** .
2. **Análise do EMV da Variância ($`\hat{\sigma}^2 = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2`$):** Ao calcular a esperança de $`\hat{\sigma}^2`$, obtém-se:
   ```math
   E\hat{\sigma}^2 = \frac{n-1}{n} \sigma^2
   ```
   O viés do EMV da variância é:
   ```math
   B(\hat{\sigma}^2) = E\hat{\sigma}^2 - \sigma^2 = \frac{n-1}{n}\sigma^2 - \sigma^2 = -\frac{1}{n}\sigma^2 \neq 0
   ```
   Assim, o EMV da variância é **viesado** (subestima a variância populacional para amostras finitas), embora seja **assintoticamente não viesado** pois $`\lim_{n \to \infty} E\hat{\sigma}^2 = \sigma^2`$.
3. **Mecanismo de Correção de Viés:** Para eliminar o vício, multiplica-se $`\hat{\sigma}^2`$ por $`\frac{n}{n-1}`$:
   ```math
   E\left [\frac{n}{n-1} \hat{\sigma}^2 \right ] = \frac{n}{n-1} E\hat{\sigma}^2 = \frac{n}{n-1} \left( \frac{n-1}{n} \sigma^2 \right) = \sigma^2
   ```
   Isso define a variância amostral corrigida $`S^2`$:
   ```math
   S^2 = \frac{n}{n-1} \left( \frac{1}{n} \sum_{i=1}^n (X_i - \bar{X})^2 \right) = \frac{1}{n-1} \sum_{i=1}^n (X_i - \bar{X})^2
   ```
   Portanto, $`S^2`$ é o estimador não viesado da variância populacional $`\sigma^2`$.

#### 3. Questionário de Fixação (Quiz)

1. Qual é a diferença conceitual fundamental entre uma abordagem probabilística e a inferência estatística?
2. Em termos formais, qual é a distinção entre um "estimador" e uma "estimativa"?
3. Por que se utiliza a transformação da log-verossimilhança ($`\ln L`$) em vez de trabalhar diretamente com a função de verossimilhança ($`L`$)?
4. Qual é o estimador de máxima verossimilhança para o parâmetro $`\lambda`$ de uma distribuição Poisson?
5. Qual é a condição matemática necessária para comprovar que uma solução encontrada via derivadas de primeira ordem é um ponto de máximo global em modelos multiparamétricos?
6. O que estabelece a propriedade de invariância dos estimadores de máxima verossimilhança?
7. Como é definido formalmente o viés (ou vício) de um estimador $`T(X)`$ para um parâmetro $`\theta`$ ?
8. Qual é a expressão matemática que relaciona o Erro Quadrático Médio (EQM) com a variância e o viés de um estimador?
9. Quais são as duas condições suficientes utilizadas na prática para demonstrar que uma sequência de estimadores é consistente?
10. Por que o estimador de máxima verossimilhança da variância de uma distribuição normal ($`\hat{\sigma}^2`$) é dividido por $`n`$, e por que os pacotes estatísticos costumam utilizar a divisão por $`n-1`$ ?

#### 4. Gabarito do Questionário

1. **Resposta:** Na probabilidade, os parâmetros da distribuição populacional são assumidos como conhecidos e fixos para calcular a probabilidade de futuros eventos. Na inferência estatística, observa-se a amostra aleatória e pretende-se deduzir ou inferir informações sobre os parâmetros populacionais desconhecidos.
2. **Resposta:** O estimador é uma função matemática das variáveis aleatórias da amostra (uma variável aleatória antes da coleta dos dados). A estimativa é o valor numérico fixo obtido quando os dados amostrais específicos são coletados e aplicados na fórmula do estimador.
3. **Resposta:** A transformação logarítmica converte os produtos das densidades/massas de probabilidade em somas, o que facilita significativamente o processo de derivação. Além disso, elimina os expoentes exponenciais e mantém a localização dos mesmos pontos de máximo por ser uma função monótona crescente.
4. **Resposta:** O estimador de máxima verossimilhança para o parâmetro $`\lambda`$ da distribuição Poisson é a média amostral $`\bar{X}`$. Esse resultado é obtido derivando a log-verossimilhança $`-n\lambda + (\sum X_i)\ln\lambda`$, igualando a zero e verificando que a segunda derivada é estritamente negativa.
5. **Resposta:** Deve-se calcular a Matriz Hessiana contendo todas as derivadas parciais de segunda ordem da log-verossimilhança. Para garantir um ponto de máximo, essa matriz precisa ser provada como definida negativa no ponto dos candidatos a estimadores.
6. **Resposta:** A propriedade garante que se $`\hat{\theta}`$ é o EMV do parâmetro $`\theta`$, então o EMV de qualquer função desse parâmetro, $`g(\theta)`$, será dado simplesmente por $`g(\hat{\theta})`$. Um exemplo é estimar a probabilidade de zero em uma Poisson através de $`e^{-\bar{X}}`$.
7. **Resposta:** O viés $`B_\theta(T)`$ é definido como a diferença entre o valor esperado do estimador e o valor real do parâmetro populacional, ou seja, $`B_\theta(T) = ET(X) - \theta`$. Se esse valor for zero para todo $`\theta`$, o estimador é dito não viesado.
8. **Resposta:** O Erro Quadrático Médio é expresso por $`EQM(T(X)) = \text{Var}(T(X)) + B_\theta(T)^2`$. Essa fórmula demonstra que o erro total do estimador depende tanto de sua dispersão (variância) quanto do seu afastamento sistemático do alvo (viés ao quadrado).
9. **Resposta:** Demonstra-se que o estimador é assintoticamente não viesado, isto é, $`\lim_{n \to \infty} ET_n(X) = \theta`$, e que sua variância converge para zero conforme o tamanho da amostra cresce, ou seja, $`\lim_{n \to \infty} \text{Var}(T_n(X)) = 0`$.
10. **Resposta:** O EMV da variância é dividido por $`n`$ porque surge diretamente da otimização da função de verossimilhança, resultando em um estimador viesado que subestima $`\sigma^2`$ por um fator de $`\frac{n-1}{n}`$. A divisão por $`n-1`$ na variância amostral corrigida $`S^2`$ elimina esse viés para amostras finitas, garantindo que $`ES^2 = \sigma^2`$.

#### 5. Questões Dissertativas para Aprofundamento

1. Demonstre detalhadamente o desenvolvimento do estimador de máxima verossimilhança para o parâmetro $`\theta`$ de uma distribuição de Bernoulli com parâmetro $`\theta \in (0,1)`$. Apresente a função de verossimilhança, a log-verossimilhança e o teste de derivada segunda para confirmar o ponto de máximo.
2. A partir da definição inicial de Erro Quadrático Médio $`EQM(T(X)) = E(T(X) - \theta)^2`$, deduza passo a passo, justificando todas as propriedades operacionais da esperança utilizadas, a igualdade $`EQM(T(X)) = \text{Var}(T(X)) + B_\theta(T)^2`$. Discuta as implicações dessa decomposição no contexto do aprendizado de máquina.
3. Considere uma amostra aleatória $`X_1, \dots, X_n`$ da distribuição Normal $`N(\mu, \sigma^2)`$. Mostre matematicamente por que $`E\hat{\sigma}^2 = \frac{n-1}{n}\sigma^2`$, identificando onde surge a necessidade de ajuste de graus de liberdade e justificando a transição do EMV $`\hat{\sigma}^2`$ para a variância amostral corrigida $`S^2`$.
4. Explique o conceito de consistência de um estimador. Como a Desigualdade de Tchebycheff é empregada no arcabouço teórico para provar a convergência em probabilidade de um estimador à medida que $`n \to \infty`$ ?
5. Enuncie e aplique a propriedade de invariância dos EMVs para obter o estimador de máxima verossimilhança da variância de uma variável aleatória com distribuição de Bernoulli com parâmetro $`p`$. Desenvolva a expressão final em termos da média amostral $`\bar{X}`$.

#### 6. Glossário Completo de Termos Técnicos

* **Amostra Aleatória:** Conjunto de $`n`$ variáveis aleatórias $`X_1, X_2, \dots, X_n`$ independentes e identicamente distribuídas (i.i.d.) retiradas de uma determinada população.
* **Consistência:** Propriedade assintótica de uma sequência de estimadores cujas distribuições se concentram arbitrariamente próximas do verdadeiro valor do parâmetro populacional quando o tamanho amostral $`n`$ tende ao infinito.
* **Correção de Viés:** Procedimento matemático aplicado a um estimador viciado para transformar sua estrutura de modo que sua esperança matemática passe a ser exatamente igual ao parâmetro de interesse.
* **Desigualdade de Tchebycheff:** Teorema que estabelece um limitante superior para a probabilidade de um desvio entre uma variável aleatória e sua esperança ser maior ou igual a uma constante positiva, utilizada para demonstrar a consistência de estimadores.
* **Eficiência:** Critério relativo de comparação entre dois estimadores não viesados que elege como preferível aquele que apresentar a menor variância.
* **Erro Quadrático Médio (EQM):** Métrica de desempenho de um estimador calculada como a esperança do quadrado do erro de estimação; equivale numericamente à soma da variância do estimador com o quadrado do seu viés.
* **Estimador:** Função ou fórmula matemática aplicada aos elementos de uma amostra aleatória, considerada como variável aleatória antes da observação empírica dos dados.
* **Estimador de Máxima Verossimilhança (EMV):** Estimador pontual obtido determinando-se o ponto do espaço paramétrico que maximiza a função de verossimilhança da amostra observada.
* **Estimativa:** Valor numérico específico resultante da aplicação de um estimador a uma amostra de dados observada concretamente.
* **Função de Verossimilhança ($`L(\theta)`$):** Função joint-probabilidade ou densidade conjunta da amostra interpretada como uma função do parâmetro populacional $`\theta`$, para dados observados fixos.
* **Log-Verossimilhança ($`\ell(\theta)`$):** Aplicação do logaritmo natural à função de verossimilhança, usada para transformar produtos de probabilidades em somas e facilitar operações de otimização via derivadas.
* **Matriz Hessiana:** Matriz de derivadas parciais de segunda ordem de uma função escalar, utilizada na inferência multiparamétrica para verificar se um ponto crítico derivado é uma matriz definida negativa (caracterizando um máximo global).
* **Parâmetro Populacional:** Medida numérica fixa e desconhecida que descreve uma característica específica da distribuição de probabilidade de toda a população (ex.: $`\mu, \sigma^2, \lambda`$).
* **Propriedade de Invariância:** Propriedade do EMV que estabelece que o estimador de máxima verossimilhança de uma transformação $`g(\theta)`$ é a própria transformação aplicada ao EMV de $`\theta`$, ou seja, $`\widehat{g(\theta)} = g(\hat{\theta})`$.
* **Viés (ou Vício):** Desvio ou erro sistemático de um estimador, medido pela diferença entre o valor esperado do estimador e o valor real do parâmetro populacional que se deseja estimar ($`ET - \theta`$).
