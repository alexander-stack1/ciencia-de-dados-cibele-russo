> **Aula:** [SME0123 Estatística I - Intro Inferência Estatística - Estimação Intervalar Aula de 27/10/2026](https://www.youtube.com/watch?v=CZj1O_mTiVE)  
> Aula da Profa. Dra. Cibele Russo (ICMC/USP), canal [@CibeleRussoUSP](https://www.youtube.com/@CibeleRussoUSP). Guia gerado com o Google NotebookLM a partir da aula e formatado para este repositório. Pode conter imprecisões: na dúvida, vale o vídeo original.  
> [Slides desta aula](slides.pdf) · [Voltar ao índice](../README.md)

### Guia de Estudo: Inferência Estatística e Estimação Intervalar

Este guia de estudo destina-se à revisão aprofundada dos conceitos de inferência estatística, cobrindo as propriedades dos estimadores pontuais, o desenvolvimento formal dos intervalos de confiança para a média populacional (com variância conhecida e desconhecida) e para a proporção populacional, além do dimensionamento de amostras.

#### Parte 1: Conteúdo da Aula — Estimação Intervalar e Propriedades dos Estimadores

##### 1. Revisão das Propriedades dos Estimadores

A inferência estatística busca estimar quantidades desconhecidas de uma população a partir de uma amostra aleatória $`X_1, X_2, \dots, X_n`$. Antes da observação dos dados, as funções da amostra utilizadas para estimar parâmetros são **variáveis aleatórias** , possuindo distribuição de probabilidade, média e variância.
As quatro propriedades fundamentais para avaliar e comparar estimadores são:

###### *A. Viés (ou Vício)*

O viés de um estimador $`\hat{\theta}`$ em relação ao parâmetro $`\theta`$ é a diferença entre o seu valor esperado e o verdadeiro valor do parâmetro: $`V(\hat{\theta}) = E\hat{\theta} - \theta`$

* **Estimador Não Viezado:** Ocorre quando $`E\hat{\theta} = \theta`$, ou seja, $`V(\hat{\theta}) = 0`$. Por exemplo, a média amostral $`\bar{X}`$ é um estimador não viezado para a média populacional $`\mu`$, pois $`E\bar{X} = \mu`$.

###### *B. Erro Quadrático Médio (EQM)*

Avalia a qualidade de um estimador de forma quadrática. É definido como o valor esperado da diferença quadrática entre o estimador e o parâmetro: $`EQM(\hat{\theta}) = E(\hat{\theta} - \theta)^2`$
O EQM pode ser decomposto na soma da variância do estimador com o quadrado do seu viés: $`EQM(\hat{\theta}) = Var(\hat{\theta}) + V(\hat{\theta})^2`$

###### *C. Eficiência*

Para comparar dois ou mais estimadores **não viezados** , avalia-se a variância de cada um. O estimador que apresentar a menor variância é considerado o **mais eficiente** .

###### *D. Consistência*

Um estimador é assintoticamente consistente se atende a duas condições quando o tamanho da amostra $`n`$ tende ao infinito ($`n \to \infty`$):

1. É assintoticamente não viezado: $`\lim_{n \to \infty} E\hat{\theta} = \theta`$
2. Sua variância tende a zero: $`\lim_{n \to \infty} Var(\hat{\theta}) = 0`$

###### *Exemplo Detalhado: Comparação entre Dois Estimadores para a Média (Exemplo 6.10)*

Considere uma amostra aleatória $`X_1, \dots, X_n`$ vinda de uma população $`N(\mu, \sigma^2)`$. Deseja-se avaliar dois estimadores para a média populacional $`\mu`$:

* $`\hat{\mu}_1 = X_1`$ (utiliza apenas a primeira observação da amostra)
* $`\hat{\mu}_2 = \bar{X} = \frac{1}{n} \sum_{i=1}^n X_i`$ (média amostral)

**1. Análise de Viés:**

* $`E\hat{\mu}_1 = EX_1 = \mu \implies V(\hat{\mu}_1) = 0`$
* $`E\hat{\mu}_2 = E\bar{X} = \mu \implies V(\hat{\mu}_2) = 0`$
* *Conclusão:* Ambos são estimadores não viezados.

**2. Análise de Eficiência:**

* $`Var(\hat{\mu}_1) = Var(X_1) = \sigma^2`$
* $`Var(\hat{\mu}_2) = Var(\bar{X}) = \frac{\sigma^2}{n}`$
* Para $`n > 1`$, tem-se $`\frac{\sigma^2}{n} < \sigma^2`$.
* *Conclusão:* $`\hat{\mu}_2 = \bar{X}`$ possui menor variância, sendo mais eficiente que $`\hat{\mu}_1`$.

**3. Análise de Consistência:**

* Ambos satisfazem a condição de valor esperado, pois $`E\hat{\mu}_1 = E\hat{\mu}_2 = \mu`$ para todo $`n`$.
* Para as variâncias quando $`n \to \infty`$:
* $`\lim_{n \to \infty} Var(\hat{\mu}_1) = \sigma^2 \neq 0`$
* $`\lim_{n \to \infty} Var(\hat{\mu}_2) = \lim_{n \to \infty} \frac{\sigma^2}{n} = 0`$
* *Conclusão:* A média amostral $`\bar{X}`$ é um estimador consistente, enquanto $`X_1`$ não é consistente.

##### 2. Introdução à Estimação Intervalar

A estimação pontual fornece um único valor numérico como estimativa do parâmetro, mas não expressa a variabilidade amostral associada. A **estimação intervalar** busca construir intervalos de confiança que associem à estimativa pontual uma medida de precisão e incerteza.

###### *Interpretação do Intervalo de Confiança*

O parâmetro populacional ($`\mu`$) é fixo e desconhecido, não sendo uma variável aleatória. A variabilidade reside no estimador e no intervalo construído a partir de amostras aleatórias.
A interpretação adequada (conforme Magalhães e Lima) estabelece que: se pudéssemos extrair um grande número de amostras independentes de mesmo tamanho $`n`$ da mesma população e, para cada uma, calcular o intervalo de confiança com nível de confiança $`\gamma = 1 - \alpha`$, esperamos que a proporção de intervalos que contêm o verdadeiro parâmetro $`\mu`$ seja igual a $`1 - \alpha`$ (por exemplo, 95%).

###### *Analogia com Pesquisas Eleitorais e Significância Estatística*

Em uma pesquisa eleitoral hipotética onde o Candidato A obtém 51% das intenções de voto e o Candidato B obtém 49%, com uma margem de erro de 3 pontos percentuais para mais ou para menos:

* Intervalo do Candidato A: $`48\%, 54\%`$
* Intervalo do Candidato B: $`46\%, 52\%`$

Como os intervalos de confiança se sobrepõem (possuem intersecção), há um **empate técnico** . Não existe evidência com significância estatística de que a intenção de voto no Candidato A seja superior à do Candidato B.

##### 3. Intervalo de Confiança para a Média ($`\sigma^2`$ Conhecido) e Tamanho Amostral

Seja $`X_1, \dots, X_n`$ uma amostra aleatória de uma população com distribuição normal $`N(\mu, \sigma^2)`$, onde a variância $`\sigma^2`$ é **conhecida** .
A distribuição amostral da média amostral é $`\bar{X} \sim N\left(\mu, \frac{\sigma^2}{n}\right)`$. Padronizando a variável, obtém-se: $`Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \sim N(0, 1)`$
Para um nível de significância $`\alpha`$ e nível de confiança $`\gamma = 1 - \alpha`$, selecionam-se os quantis $`-z_{\alpha/2}`$ e $`z_{\alpha/2}`$ da distribuição normal padrão tais que: $`P\left(-z_{\alpha/2} \le \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \le z_{\alpha/2}\right) = 1 - \alpha`$
Isolando o parâmetro $`\mu`$ no centro da desigualdade: $`P\left(\bar{X} - z_{\alpha/2} \frac{\sigma}{\sqrt{n}} \le \mu \le \bar{X} + z_{\alpha/2} \frac{\sigma}{\sqrt{n}}\right) = 1 - \alpha`$
O **Intervalo de Confiança Numérico** para $`\mu`$ (com nível de confiança $`(1-\alpha) \times 100\%`$) é dado por: $`IC(\mu; 1-\alpha) = \left [\bar{x} - z_{\alpha/2} \frac{\sigma}{\sqrt{n}} \,;\, \bar{x} + z_{\alpha/2} \frac{\sigma}{\sqrt{n}} \right ]`$

###### *Erro Amostral e Tamanho da Amostra*

O erro amostral (ou margem de erro), representado por $`\varepsilon`$, é a metade da largura do intervalo de confiança: $`\varepsilon = z_{\alpha/2} \frac{\sigma}{\sqrt{n}}`$
Para determinar o **tamanho mínimo de amostra** **$`n`$** necessário para garantir um erro máximo admissível $`\varepsilon`$ com nível de confiança $`1 - \alpha`$, isola-se $`n`$ na equação: $`n = \left( \frac{z_{\alpha/2} \cdot \sigma}{\varepsilon} \right)^2`$

* *Relações diretas:* Aumentar o nível de confiança ($`1-\alpha`$) aumenta $`z_{\alpha/2}`$ e amplia a largura do intervalo. Aumentar o tamanho da amostra $`n`$ reduz a margem de erro $`\varepsilon`$ e aumenta a precisão.

##### 4. Intervalo de Confiança para a Média ($`\sigma^2`$ Desconhecido) e a Distribuição $`t`$ de Student

Quando a variância populacional $`\sigma^2`$ é desconhecida, ela deve ser estimada a partir da amostra utilizando a variância amostral não viezada: $`S^2 = \frac{1}{n-1} \sum_{i=1}^n (X_i - \bar{X})^2`$
A substituição de $`\sigma`$ pelo desvio padrão amostral $`S`$ introduz uma nova fonte de variabilidade. A variável padronizada resultante não segue mais uma distribuição normal padrão, mas sim uma **distribuição $`t`$de Student** com $`n - 1`$ graus de liberdade: $`T = \frac{\bar{X} - \mu}{S / \sqrt{n}} \sim t_{(n-1)}`$

###### *Contexto Histórico da Distribuição $`t`$*

A distribuição foi desenvolvida por William Sealy Gosset em 1908, enquanto trabalhava como químico/estatístico na cervejaria Guinness. Devido às restrições de segredo industrial da empresa, Gosset publicou seus resultados sob o pseudônimo de *"Student"* . A distribuição é caracterizada por ter caldas mais pesadas que a normal padrão, acomodando a incerteza adicional de pequenas amostras. Conforme o número de graus de liberdade ($`n-1`$) aumenta, a distribuição $`t`$ converge para a distribuição normal padrão $`N(0, 1)`$.

###### *Fórmula do Intervalo de Confiança*

$`IC(\mu; 1-\alpha) = \left [\bar{x} - t_{\alpha/2, n-1} \frac{S}{\sqrt{n}} \,;\, \bar{x} + t_{\alpha/2, n-1} \frac{S}{\sqrt{n}} \right ]`$
O quantil $`t_{\alpha/2, n-1}`$ é obtido da tabela da distribuição $`t`$ de Student considerando $`n - 1`$ graus de liberdade e uma probabilidade de cauda superior de $`\alpha/2`$.

##### 5. Intervalo de Confiança para a Proporção Populacional $`p`$

Seja $`X_1, \dots, X_n`$ uma amostra aleatória de variáveis de Bernoulli onde $`X_i = 1`$ indica sucesso e $`X_i = 0`$ indica fracasso, com probabilidade populacional de sucesso $`p`$. O estimador pontual de $`p`$ é a proporção amostral $`\hat{p} = \bar{X} = \frac{1}{n}\sum X_i`$.
Pelo Teorema do Limite Central, para amostras suficientemente grandes, a distribuição de $`\hat{p}`$ é aproximadamente normal: $`\hat{p} \approx N\left(p, \frac{p(1-p)}{n}\right)`$
Padronizando: $`Z = \frac{\hat{p} - p}{\sqrt{\frac{p(1-p)}{n}}} \sim N(0, 1)`$
Como o desvio padrão depende do parâmetro desconhecido $`p`$, utilizam-se duas estratégias para contornar o problema:

###### *Estratégia Otimista*

Substitui-se o parâmetro desconhecido $`p`$ na fórmula do erro padrão pelo próprio estimador amostral $`\hat{p}`$: $`IC(p; 1-\alpha) = \left [\hat{p} - z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} \,;\, \hat{p} + z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} \right ]`$

###### *Estratégia Conservadora*

A função $`f(p) = p(1-p)`$ atinge seu ponto máximo em $`p = 0,5`$. O valor máximo do produto é: $`p(1-p) \le 0,5 \times (1 - 0,5) = 0,25 = \frac{1}{4}`$
Substituindo $`p(1-p)`$ por $`0,25`$, obtém-se o erro amostral máximo possível, resultando em um intervalo mais amplo e seguro: $`IC_{cons}(p; 1-\alpha) = \left [\hat{p} - z_{\alpha/2} \frac{1}{2\sqrt{n}} \,;\, \hat{p} + z_{\alpha/2} \frac{1}{2\sqrt{n}} \right ]`$

#### Parte 2: Questionário de Avaliação (Quiz)

1. Qual é a definição formal do viés de um estimador $`\hat{\theta}`$ e o que significa dizer que um estimador é não viezado?
2. Como se calcula o Erro Quadrático Médio (EQM) de um estimador e qual é a sua decomposição em termos de variância e viés?
3. No confronto entre dois estimadores não viezados, qual critério determina qual deles é o mais eficiente?
4. Quais são as duas condições necessárias para que um estimador seja considerado consistente?
5. No Exemplo 6.10, por que o estimador $`\hat{\mu}_2 = \bar{X}`$ é preferível ao estimador $`\hat{\mu}_1 = X_1`$ sob a ótica da consistência?
6. Por que é conceitualmente incorreto afirmar que "existe uma probabilidade de 95% de que o parâmetro $`\mu`$ esteja dentro do intervalo numérico calculado $`1,5; 2,1`$ "?
7. O que acontece com a largura de um intervalo de confiança quando aumentamos o nível de confiança de 90% para 95%, mantendo o tamanho amostral constante?
8. Qual a mudança prática na construção do intervalo de confiança para a média populacional quando a variância populacional $`\sigma^2`$ é desconhecida?
9. Qual foi a motivação histórica para o desenvolvimento da distribuição $`t`$ de Student e quem foi seu criador?
10. Qual é a diferença entre a estratégia otimista e a estratégia conservadora na construção do intervalo de confiança para uma proporção populacional $`p`$ ?

#### Parte 3: Gabarito do Questionário

1. O viés de um estimador é a diferença entre o seu valor esperado e o verdadeiro valor do parâmetro, expressado por $`V(\hat{\theta}) = E\hat{\theta} - \theta`$. Um estimador é não viezado quando seu valor esperado é exatamente igual ao parâmetro que se deseja estimar, resultando em um viés igual a zero.
2. O Erro Quadrático Médio é o valor esperado do quadrado da diferença entre o estimador e o parâmetro, $`EQM(\hat{\theta}) = E(\hat{\theta} - \theta)^2`$. Ele pode ser decomposto algebraicamente na soma da variância do estimador com o quadrado do seu viés: $`EQM(\hat{\theta}) = Var(\hat{\theta}) + V(\hat{\theta})^2`$.
3. Para estimadores que são comprovadamente não viezados, a eficiência é determinada comparando-se as suas variâncias. O estimador que apresentar a menor variância entre os concorrentes é classificado como o mais eficiente.
4. Um estimador é consistente se, ao expandir o tamanho da amostra ao infinito ($`n \to \infty`$), ele se torna assintoticamente não viezado ($`\lim E\hat{\theta} = \theta`$) e a sua variância converge para zero ($`\lim Var(\hat{\theta}) = 0`$).
5. O estimador $`\hat{\mu}_2 = \bar{X}`$ é consistente porque sua variância é $`\frac{\sigma^2}{n}`$, que tende a zero quando $`n \to \infty`$. Já o estimador $`\hat{\mu}_1 = X_1`$ possui variância constante $`\sigma^2`$ independentemente do tamanho da amostra, violando o critério de convergência da variância.
6. A afirmação é incorreta porque o parâmetro $`\mu`$ é uma constante fixa desconhecida, e não uma variável aleatória. Uma vez calculado numericamente a partir de dados observados, o intervalo ou contém o parâmetro ou não contém, sendo a probabilidade uma propriedade do processo aleatório de amostragem prévio.
7. Ao aumentar o nível de confiança mantendo o tamanho amostral constante, o valor crítico (quantil $`z_{\alpha/2}`$ ou $`t_{\alpha/2}`$) aumenta. Consequentemente, a margem de erro se expande, resultando em um intervalo de confiança mais largo.
8. Quando $`\sigma^2`$ é desconhecido, utiliza-se o desvio padrão amostral $`S`$ em substituição a $`\sigma`$. Devido à incerteza adicional introduzida por $`S`$, substitui-se o quantil da distribuição normal padrão $`z_{\alpha/2}`$ pelo quantil da distribuição $`t`$ de Student $`t_{\alpha/2, n-1}`$ com $`n-1`$ graus de liberdade.
9. A distribuição foi criada por William Sealy Gosset em 1908 enquanto trabalhava na cervejaria Guinness, sob o pseudônimo de "Student". A motivação foi resolver o problema de inferência estatística no controle de qualidade com pequenas amostras, onde a variância populacional era desconhecida.
10. A estratégia otimista substitui o parâmetro $`p`$ na fórmula do erro padrão pelo valor estimado na amostra $`\hat{p}`$. A estratégia conservadora assume o caso de variância máxima possível fixando $`p(1-p) = 0,25`$, garantindo um intervalo mais amplo que limita o erro para qualquer valor de $`p`$.

#### Parte 4: Questões Discursivas (Formato Ensaio)

*Nota: Estas questões foram projetadas para aprofundamento analítico e reflexão teórica. Não são fornecidas respostas.*

1. **Análise Crítica da Interpretação de Intervalos de Confiança:** Discuta as polêmicas e os equívocos conceituais comuns em relação à interpretação do Intervalo de Confiança no paradigma frequentista. Explique por que a expressão "grau de confiança" é preferida em relação ao termo "probabilidade" após a observação da amostra numérica.
2. **Dedução Formal e Propriedades da Média Amostral:** Demonstre matematicamente que a média amostral $`\bar{X}`$ é um estimador não viezado e consistente para a média populacional $`\mu`$. Compare o desempenho de $`\bar{X}`$ com estimadores alternativos baseados em um subconjunto fixo da amostra.
3. **O Impacto do Tamanho Amostral e Variabilidade no Dimensionamento de Experimentos:** Derive a fórmula do tamanho amostral $`n`$ a partir do erro máximo admissível $`\varepsilon`$ para a média de uma população normal com variância conhecida. Em seguida, analise como o custo de amostragem versus a precisão desejada influenciam o planejamento estatístico em pesquisas aplicadas.
4. **Convergência Assintótica da Distribuição $`t`$de Student:** Explique o papel dos graus de liberdade na distribuição $`t`$ de Student e detalhe o comportamento de suas caudas em comparação com a distribuição normal padrão. O que ocorre com os quantis críticos de $`t`$ conforme o tamanho da amostra $`n`$ cresce indefinidamente?
5. **Incerteza na Estimação de Proporções Amostrais:** Compare detalhadamente as estratégias otimista e conservadora para estimar a proporção de um evento binário. Em que cenários práticos a adoção da abordagem conservadora é estritamente recomendada e como essa escolha afeta a margem de erro estimada?

#### Parte 5: Glossário de Termos-Chave

Termo,Definição Baseada no Texto
Amostra Aleatória,"Conjunto de variáveis aleatórias $`X_1, X_2, \dots, X_n`$ independentes e identicamente distribuídas (i.i.d.)."
Consistência,Propriedade assintótica de um estimador cujo valor esperado converge para o verdadeiro parâmetro e cuja variância tende a zero quando $`n \to \infty`$.
Distribuição $`t`$ de Student,"Distribuição de probabilidade simétrica com caudas mais pesadas que a normal padrão, utilizada quando a variância populacional é desconhecida e estimada por $`S^2`$. É parametrizada pelos graus de liberdade ($`n-1`$)."
Eficiência,Critério de comparação entre estimadores não viezados que elege aquele com a menor variância como o estimador preferível.
Erro Amostral ($`\varepsilon`$),"A margem de erro máxima esperada entre a estimativa pontual e o verdadeiro valor do parâmetro, correspondente à metade da largura do intervalo de confiança."
Erro Quadrático Médio (EQM),"Métrica de qualidade de um estimador dada por $`EQM(\hat{\theta}) = E(\hat{\theta} - \theta)^2`$, equivalente à soma da variância com o viés ao quadrado."
Estimador Pontual,Função da amostra (variável aleatória) utilizada para estimar um valor numérico único para um parâmetro populacional.
Estratégia Conservadora,"Método de construção de IC para proporções que assume a variância máxima possível ($`p(1-p) = 0,25`$), resultando em uma margem de erro superior e segura."
Estratégia Otimista,Método de construção de IC para proporções que substitui o parâmetro $`p`$ no erro padrão diretamente pelo valor estimado da amostra $`\hat{p}`$.
Graus de Liberdade,Parâmetro da distribuição $`t`$ de Student (dado por $`n-1`$ no caso de uma média) que reflete o número de informações independentes disponíveis para estimar a variância.
Intervalo de Confiança Aleatório,"Intervalo expresso em termos de variáveis aleatórias (antes da observação dos dados), o qual possui probabilidade $`1-\alpha`$ de conter o parâmetro."
Intervalo de Confiança Numérico,"Intervalo calculado com os valores observados da amostra, interpretado sob a ótica de confiança na metodologia empregada."
Nível de Confiança ($`1 - \alpha`$ ou $`\gamma`$),Proporção esperada de intervalos de confiança que conterão o verdadeiro parâmetro populacional caso o processo de amostragem seja repetido infinitas vezes.
Nível de Significância ($`\alpha`$),"Probabilidade de erro fixada na inferência, representando a área total nas caudas da distribuição de referência (geralmente 5%)."
Viés (ou Vício),"Medida de desvio linear de um estimador, dada pela diferença entre o seu valor esperado e o parâmetro real: $`E\hat{\theta} - \theta`$."
