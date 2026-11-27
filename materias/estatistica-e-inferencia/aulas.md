> Material derivado das aulas da Profa. Cibele Russo (ICMC/USP), organizado com auxílio do NotebookLM. [↩ Voltar à trilha](README.md)

# Guia de Estudo: Inferência Estatística e Análise Exploratória de Dados

## 1. Questionário de Avaliação (Quiz)

1. Qual é a principal diferença conceitual e de estrutura de dados entre o Teste t pareado (Caso 1) e o Teste t para amostras independentes?
2. Como se calcula a variância combinada $`(S_c^2)`$ no Caso 3A de comparação de médias e qual a razão teórica para utilizar $`n_1 + n_2 - 2`$ graus de liberdade no denominador?
3. No contexto de estimação pontual via Modelo Normal $`N(\mu, \sigma^2)`$, qual é a propriedade do estimador de máxima verossimilhança da média $`(\hat{\mu}_{MV})`$ e da variância $`(\hat{\sigma}^2_{MV})`$ em relação ao viés?
4. Quais são as três premissas fundamentais (suposições) necessárias para a aplicação da Análise de Variância (ANOVA) de um fator?
5. Como são definidas as hipóteses nula $`(H_0)`$ e alternativa $`(H_1)`$ em uma ANOVA de um fator e qual é a limitação do resultado do teste quando $`H_0`$ é rejeitada?
6. Qual é o intervalo de variação do coeficiente de correlação linear de Pearson (r) e como se classificam as forças de associação em termos numéricos absolutos?
7. Por que a análise gráfica (como os gráficos de dispersão ou o Quarteto de Anscombe) é indispensável antes de se tirar conclusões baseadas exclusivamente no coeficiente de correlação de Pearson?
8. Qual é o impacto da presença de dados altamente inflacionados de zeros (como saldos nulos de poupança ou investimentos) em análises de regressão ou correlação, e qual a conduta analítica recomendada?
9. Descreva a anatomia estatística do Boxplot e explique o critério padronizado para a identificação de pontos atípicos (outliers).
10. Em quais circunstâncias é apropriado aplicar o Teorema do Limite Central (TLC) para realizar o teste de comparação de médias com a estatística Z, em detrimento de um teste de normalidade estrito?

## 2. Gabarito do Questionário

1. O Teste t pareado é aplicado a amostras dependentes, nas quais duas medidas são tomadas na mesma unidade amostral ou em pares correlacionados (como medições de pressão antes e depois do tratamento). Em contrapartida, o teste para amostras independentes compara dois grupos distintos sem correlação probabilística entre as observações de cada população. Assim, no caso pareado calcula-se a diferença individual $`D_i = X_i - Y_i`$ antes da análise, enquanto no independente analisam-se as médias amostrais dos dois grupos separadamente.

2. A variância combinada é calculada pela fórmula $`S_c^2 = \frac{(n_1 - 1)S_x^2 + (n_2 - 1)S_y^2}{n_1 + n_2 - 2}`$, ponderando as variâncias amostrais pelas suas respectivas contribuições. Utiliza-se $`n_1 + n_2 - 2`$ graus de liberdade porque do total de observações $`(n_1 + n_2)`$ foram calculadas duas estatísticas ($`\bar{X}`$ e $`\bar{Y}`$) para resumir os dados dos dois grupos. Essa correção ajusta o denominador para garantir propriedades estatísticas adequadas ao estimador da variância populacional comum.

3. O estimador de máxima verossimilhança para a média populacional é a própria média amostral $`(\hat{\mu}_{MV} = \bar{X})`$, o qual é um estimador não viesado. Em contrapartida, o estimador de máxima verossimilhança para a variância $`(\hat{\sigma}^2_{MV} = \frac{1}{n}\sum(X_i - \bar{X})^2)`$ é viesado, apresentando esperança igual a $`\frac{n-1}{n}\sigma^2`$. Para eliminar esse viés, multiplica-se a expressão por $`\frac{n}{n-1}`$, obtendo-se a variância amostral tradicional dividida por n-1.

4. A realização da ANOVA de um fator exige que as amostras dos grupos sejam independentes entre si. Além disso, pressupõe-se que as variáveis de cada grupo sigam distribuição normal e apresentem a mesma variância populacional, propriedade denominada homocedasticidade. Caso a premissa de homocedasticidade ou de normalidade seja violada, deve-se considerar a padronização dos dados ou o uso de testes não paramétricos, como o teste de Kruskal-Wallis.

5. Na ANOVA de um fator, a hipótese nula estabelece que todas as médias populacionais dos grupos são iguais $`(H_0: \mu_1 = \mu_2 = \dots = \mu_k)`$, enquanto a hipótese alternativa afirma que pelo menos uma das médias é diferente das demais. A limitação do teste reside no fato de que a rejeição de $`H_0`$ confirma a existência de diferença, mas não identifica especificamente qual grupo difere dos outros. Para determinar quais pares de grupos possuem médias distintas, faz-se necessária a aplicação posterior de testes de comparações múltiplas.

6. O coeficiente de correlação linear de Pearson varia estritamente no intervalo entre -1 e 1. Valores iguais a 1 ou -1 indicam correlação perfeita positiva ou negativa, respectivamente, enquanto o valor 0 indica ausência de associação linear. Em termos de força absoluta, correlações entre 0,70 e 1,00 são classificadas como fortes, entre 0,50 e 0,69 como moderadas, e valores abaixo de 0,49 como correlações fracas.

7. A análise gráfica é indispensável porque o coeficiente de Pearson mede exclusivamente a força de associações de natureza linear. Conjuntos de dados com estruturas e distribuições totalmente distintas, como demonstrado pelo Quarteto de Anscombe, podem apresentar exatamente o mesmo valor numérico de correlação. Ademais, gráficos de dispersão revelam relações não lineares, agrupamentos ocultos e a presença de outliers que distorcem o cálculo do coeficiente.

8. A presença de uma massa elevada de zeros em variáveis contínuas (como saldos de poupança ou investimentos de clientes que não possuem o produto) distorce a inclinação da reta de regressão e reduz o coeficiente de correlação, mascarando a real relação entre as variáveis. A conduta recomendada é realizar uma análise estratificada, separando os dados entre o grupo de clientes sem o produto (valor zero) e o grupo com o produto ativo (valores estritamente positivos). Essa separação evita que a concentração de zeros contamine a avaliação da associação quantitativa nos clientes ativos.

9. O Boxplot exibe o menor valor não atípico, o primeiro quartil (Q1), a mediana (Q2), o terceiro quartil (Q3), o maior valor não atípico e a amplitude interquartil (AIQ = Q3 - Q1). O critério padronizado para identificação de outliers considera atípico qualquer ponto situado a uma distância superior a $`1,5 \times AIQ`$ abaixo de Q1 ou acima de Q3. Além das medidas de posição e dispersão, a assimetria da distribuição é evidenciada pelo deslocamento da linha da mediana dentro da caixa.

10. A aplicação do Teorema do Limite Central (TLC) é adequada quando o tamanho das amostras é suficientemente grande (geralmente $`n \ge 30`$). Nessas condições, a distribuição da média amostral aproxima-se de uma distribuição normal, independentemente da forma da distribuição populacional subjacente. Isso permite a utilização da estatística Z e da tabela normal padrão para efeituar testes de hipóteses sobre as médias populacionais com segurança analítica.

## 3. Questões para Desenvolvimento (Ensaio)

1. Discuta os procedimentos matemáticos para a obtenção dos estimadores de máxima verossimilhança dos parâmetros $`\mu`$ e $`\sigma^2`$ sob o modelo Normal $`N(\mu, \sigma^2)`$. Em sua resposta, detalhe a construção da função de densidade de probabilidade conjunta, a aplicação do logaritmo da verossimilhança, a resolução do sistema de derivadas parciais e os critérios para comprovação de que os pontos críticos obtidos minimizam ou maximizam a função (utilizando a matriz Hessiana).
2. Explique a fundamentação teórica da Análise de Variância (ANOVA) de um fator, demonstrando como a variabilidade total das observações é decomposta nas somas de quadrados (total, entre grupos e dentro dos grupos). Detalhe a construção dos quadrados médios, o papel dos graus de liberdade e a dedução da estatística F, explicando o racional de comparar a variação explicada pelo modelo com o ruído residual.
3. Elabore um quadro comparativo detalhado entre os quatro casos de testes paramétricos para comparação de duas médias apresentados na literatura do curso:
  * Amostras Pareadas (Caso 1);
  * Amostras Independentes com variâncias conhecidas (Caso 2);
  * Amostras Independentes com variâncias desconhecidas e iguais (Caso 3A);
  * Amostras Independentes com variâncias desconhecidas e diferentes (Caso 3B).
Para cada caso, especifique o tipo de dependência, a premissa sobre as variâncias, a estatística de teste utilizada e a sua respectiva distribuição amostral sob $`H_0`$.
4. Analise a importância da identificação da natureza dos dados (qualitativos nominais/ordinais e quantitativos discretos/contínuos) na escolha de técnicas adequadas de Análise Exploratória e Visualização de Dados. Discuta como combinações de diferentes tipos de variáveis (qualitativa vs. qualitativa, qualitativa vs. quantitativa, quantitativa vs. quantitativa) condicionam a escolha de representações gráficas específicas (como gráficos de mosaico, boxplots emparelhados e gráficos de dispersão) e a seleção de medidas descritivas.
5. Discuta criticamente a distinção conceitual entre Associação, Correlação e Causalidade na prática da estatística e ciência de dados. Utilize exemplos práticos (como dados socioeconômicos ou de comportamento de consumo) para demonstrar como correlações espúrias e variáveis de confusão podem conduzir a conclusões errôneas, e explique quais cuidados metodológicos devem ser tomados na interpretação de coeficientes de correlação linear e não linear.

## 4. Glossário Abrangente de Termos-Chave

| Termo / Conceito | Definição Baseada na Fonte Contextual |
|---|---|
| Amostras Dependentes (Pareadas) | Estrutura de dados em que as observações de duas amostras estão correlacionadas por serem tomadas na mesma unidade amostral (ex.: medições antes e depois de uma intervenção) ou em unidades emparelhadas (ex.: irmãos gêmeos, braço esquerdo/direito). |
| Amostras Independentes | Configuração em que os elementos selecionados de uma população não possuem qualquer relação probabilística ou correlação com os elementos selecionados da outra população. |
| Amplitude Interquartil (AIQ / IQR) | Medida de dispersão resistente calculada pela diferença entre o terceiro quartil e o primeiro quartil (Q3 - Q1), representando a extensão dos $`50\%`$ centrais dos dados. |
| Análise de Variância (ANOVA 1 Fator) | Teste estatístico paramétrico utilizado para comparar as médias de três ou mais populações independentes com base em uma variável resposta quantitativa contínua e um fator categórico. |
| Assimetria (Skewness) | Propriedade que descreve o desvio da distribuição dos dados em relação à simetria. Pode ser simétrica $`(\text{Média} \approx \text{Mediana} \approx \text{Moda})`$, assimétrica à direita/positiva $`(\text{Moda} < \text{Mediana} < \text{Média})`$ ou assimétrica à esquerda/negativa $`(\text{Média} < \text{Mediana} < \text{Moda})`$. |
| Boxplot (Gráfico de Caixa) | Representação gráfica visual da distribuição de variáveis quantitativas contínuas que exibe o mínimo não atípico, Q1, Q2 (mediana), Q3, máximo não atípico e os pontos considerados outliers. |
| Coeficiente de Correlação de Pearson (r) | Estatística padronizada que mensura o grau e a direção da associação estritamente linear entre duas variáveis quantitativas contínuas, variando no intervalo de -1 a 1. |
| Coeficiente de Variação (CV) | Medida de dispersão relativa e adimensional dada pela razão entre o desvio padrão e a média amostral $`(S / \bar{X})`$, utilizada para comparar a variabilidade de variáveis com unidades de medida distintas. |
| Curtose (Kurtosis) | Medida que caracteriza o grau de achatamento da curva de distribuição de frequência dos dados em comparação com a distribuição normal; classifica-se em leptocúrtica (mais afunilada/pontiaguda), mesocúrtica (similar à normal) ou platicúrtica (mais achatada). |
| Dados Inflacionados de Zeros | Conjunto de dados quantitativos contínuos que apresenta uma frequência desproporcional do valor zero (ex.: ausência de contratação de um produto financeiro), exigindo a estratificação dos dados para evitar contaminação nas análises de associação. |
| Desvio Padrão Amostral (S) | Raiz quadrada positiva da variância amostral, que expressa a variabilidade das observações na mesma unidade de medida da variável original. |
| Estimador de Máxima Verossimilhança (EMV) | Método de estimação pontual que busca identificar os valores dos parâmetros populacionais que maximizam a função de densidade de probabilidade conjunta (ou log-verossimilhança) para uma amostra observada. |
| Graus de Liberdade (df) | Número de observações independentes ou quantidades numéricas livres para variar no cálculo de uma estatística, após descontar o número de parâmetros intermediários estimados a partir dos dados. |
| Homocedasticidade | Premissa de igualdade de variâncias populacionais $`(\sigma_1^2 = \sigma_2^2 = \dots = \sigma_k^2)`$ entre diferentes grupos ou populações submetidos a comparações estatísticas. |
| Matriz Hessiana | Matriz quadrada das derivadas parciais de segunda ordem de uma função escalar (como a log-verossimilhança) utilizada para confirmar se um ponto crítico derivado corresponde a um máximo local (quando a matriz é definida negativa). |
| Mediana (Q2) | Medida de posição ou tendência central que divide um conjunto de dados previamente ordenados exatamente ao meio ($`50\%`$ das observações abaixo e $`50\%`$ acima), sendo resistente a valores extremos. |
| Outlier (Ponto Atípico) | Valor discrepante observado na amostra que se afasta excessivamente dos demais dados. No critério do Boxplot, é qualquer ponto situado abaixo de Q1 - $`1,5 \times AIQ`$ ou acima de Q3 + $`1,5 \times AIQ`$. |
| Quarteto de Anscombe | Conjunto de quatro bases de dados artificiais que possuem médias, variâncias e coeficientes de correlação de Pearson idênticos, mas que apresentam distribuições e formatos gráficos completamente distintos, evidenciando a importância da análise visual. |
| Tabela ANOVA | Tabela auxiliar estruturada que sintetiza as fontes de variação (entre grupos, dentro dos grupos e total), os graus de liberdade, as somas de quadrados, os quadrados médios e o valor calculado da estatística de teste F. |
| Teorema do Limite Central (TLC) | Teorema fundamental da inferência estatística que garante que a distribuição da média amostral aproxima-se de uma distribuição normal à medida que o tamanho da amostra cresce $`(n \ge 30)`$, independentemente da distribuição original da variável. |
| Variância Combinada $`(S_c^2)`$ | Estimador da variância populacional comum utilizado no Teste t para duas amostras independentes com variâncias desconhecidas e iguais (Caso 3A), combinando as variâncias amostrais individuais ponderadas pelos seus graus de liberdade. |
| Variável de Escala | Variável quantitativa cujos valores são mensurados em uma régua contínua, mas na qual razões diretas não possuem significado físico absoluto (ex.: temperatura em Celsius, latitude e longitude). |
| Viés de um Estimador | Diferença entre o valor esperado (esperança matemática) de um estimador amostral e o verdadeiro valor do parâmetro populacional que ele busca estimar. Se essa diferença for zero, o estimador é dito não viesado. |

## Atualização nov/2026

O [guia de estudo](guia-de-estudo.md) desta atualização inclui as aulas novas de testes de hipóteses, teste t, Qui-Quadrado e revisão. Seguem os trechos correspondentes. O guia completo está em [guia-de-estudo.md](guia-de-estudo.md).

### Questionário (trechos)

7. Em testes de hipóteses estatísticas, qual é a definição de Erro do Tipo I e de Erro do Tipo II, e qual é a estratégia clássica empregada para gerenciar as probabilidades de ocorrência desses erros?

8. Em que cenário de avaliação experimental deve-se optar pela utilização do teste t pareado em detrimento do teste t para duas amostras independentes?

9. Como é calculada a quantidade de graus de liberdade e qual é o critério de tomada de decisão para a rejeição da hipótese nula em um Teste Qui-Quadrado de Associação para tabelas de contingência $`R \times S`$?

### Gabarito (trechos)

7. O Erro do Tipo I ($`\alpha`$) consiste em rejeitar a hipótese nula $`H_0`$ quando ela é verdadeira, enquanto o Erro do Tipo II ($`\beta`$) é o erro de não rejeitar $`H_0`$ quando ela é falsa. Como não é possível minimizar ambos os erros simultaneamente para um n fixo, a estratégia padrão é fixar a probabilidade do Erro do Tipo I em um nível de significância $`\alpha`$ (geralmente 5%) e maximizar o poder do teste (1 - $`\beta`$).

8. O teste t pareado é indicado quando as observações dos dois grupos são dependentes ou correlacionadas, como em medições efetuadas nos mesmos indivíduos antes e depois de uma intervenção. Nessas condições, o teste é aplicado sobre as diferenças individuais de cada par, avaliando se a média populacional dessas diferenças pode ser considerada nula sob a hipótese nula.

9. No Teste Qui-Quadrado de Associação, a estatística observada é comparada com o valor crítico da distribuição Qui-Quadrado com graus de liberdade $`GL = (R - 1) \times (S - 1)`$. Se a estatística observada superar o valor crítico tabular para o nível de significância $`\alpha`$ escolhido (ou se o p-valor for menor ou igual a $`\alpha`$), a hipótese nula de independência entre as variáveis é rejeitada.

### Questões para desenvolvimento (trechos)

5. Analise o processo de tomada de decisão em um Teste de Hipóteses, explicitando a inter-relação entre o nível de significância ($`\alpha`$), a região crítica e o p-valor (valor-p). Justifique formalmente a razão metodológica pela qual a conclusão de um teste deve ser expressa em termos de "não rejeitar $`H_0`$" ou "rejeitar $`H_0`$", rejeitando o uso da expressão "aceitar $`H_0`$".

### Termos do glossário (trechos)

| Termo | Definição |
|---|---|
| Nível de Significância ($`\alpha`$) | Threshold probabilístico pré-determinado pelo pesquisador que define o limite máximo aceitável para a ocorrência do Erro do Tipo I em um teste de hipóteses. |
| Teste Qui-Quadrado de Associação | Teste não paramétrico utilizado para avaliar a existência de dependência ou associação estatística entre duas variáveis qualitativas dispostas em uma tabela de contingência. |
| Teste t Pareado | Teste de hipóteses paramétrico empregado para comparar as médias de duas populações dependentes, analisando a distribuição das diferenças pareadas de observações correlacionadas. |
