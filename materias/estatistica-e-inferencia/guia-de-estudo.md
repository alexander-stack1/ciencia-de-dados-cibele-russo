> Material derivado das aulas da Profa. Cibele Russo (ICMC/USP), organizado com auxílio do NotebookLM. [↩ Voltar à trilha](README.md)

# Guia de Estudos: Inferência Estatística e Análise de Dados

## 1. Questionário de Avaliação (Quiz)

1. O que caracteriza a propriedade de homocedasticidade em um modelo de Análise de Variância (ANOVA) com K grupos e qual é o termo empregado para descrever a situação em que essa propriedade é violada?
2. Qual é a diferença conceitual e prática entre dados transversais (cross-sectional) e dados longitudinais no contexto de análise exploratória de dados bancários?
3. No âmbito da estimação pontual, como se define um estimador não viesado e de que maneira a propriedade de consistência descreve o seu comportamento com o aumento do tamanho amostral (n)?
4. Qual é o procedimento básico adotado pelo Método dos Momentos para estimar parâmetros populacionais e como esse método é aplicado para determinar o estimador do parâmetro $`\lambda`$ de uma distribuição Exponencial?
5. De acordo com o Teorema do Limite Central (TLC), qual é a distribuição assintótica da média amostral $`\bar{X}`$ e como o seu erro padrão se altera em função do tamanho da amostra (n)?
6. Quais são as etapas para a construção da função de log-verossimilhança no Método de Máxima Verossimilhança (EMV) e qual a razão de se preferir maximizar o logaritmo da verossimilhança ($`\ln L`$) em vez da função de verossimilhança original (L)?
7. Em testes de hipóteses estatísticas, qual é a definição de Erro do Tipo I e de Erro do Tipo II, e qual é a estratégia clássica empregada para gerenciar as probabilidades de ocorrência desses erros?
8. Em que cenário de avaliação experimental deve-se optar pela utilização do teste t pareado em detrimento do teste t para duas amostras independentes?
9. Como é calculada a quantidade de graus de liberdade e qual é o critério de tomada de decisão para a rejeição da hipótese nula em um Teste Qui-Quadrado de Associação para tabelas de contingência $`R \times S`$?
10. De que forma a Inferência Bayesiana combina a distribuição a priori do parâmetro $`\theta`$ e a função de verossimilhança dos dados para obter a distribuição a posteriori?

## 2. Gabarito do Questionário

1. A homocedasticidade ocorre quando todas as K populações ou grupos analisados possuem a mesma variância populacional $`\sigma^2`$. Caso as variâncias entre os grupos sejam distintas, a condição é denominada heterocedasticidade. No modelo de ANOVA de 1 fator, a homocedasticidade é uma suposição fundamental ao lado da normalidade e da independência das observações.

2. Dados transversais correspondem a um registro estático ou "fotografia" capturada em um momento específico do tempo, agrupando diversas unidades amostrais sob variáveis distintas. Já os dados longitudinais funcionam como um "filme", acompanhando a evolução temporal contínua das mesmas unidades observacionais ao longo de múltiplas datas.

3. Um estimador $`\hat{\theta}`$ é considerado não viesado quando o seu valor esperado é exatamente igual ao parâmetro populacional desconhecido, isto é, $`E[\hat{\theta}] = \theta`$. A consistência estabelece que, à medida que o tamanho da amostra n tende ao infinito, tanto o viés quanto a variância do estimador convergem para zero, fazendo a estimativa aproximar-se do parâmetro real.

4. O Método dos Momentos obtém as estimativas igualando os momentos populacionais teóricos (como a esperança $`E[X]`$) aos respectivos momentos amostrais (como a média $`\bar{X}`$). Para a distribuição Exponencial, cuja esperança é $`E[X] = 1/\lambda`$, o método iguala $`1/\lambda = \bar{X}`$, resultando no estimador $`\hat{\lambda} = 1/\bar{X}`$.

5. O Teorema do Limite Central estabelece que a distribuição da média amostral $`\bar{X}`$ aproxima-se de uma distribuição Normal com média $`\mu`$ e variância $`\sigma^2/n`$ conforme o tamanho amostral n cresce. O erro padrão da média amostral é dado por $`\sigma/\sqrt{n}`$, o que demonstra que a variabilidade da média diminui proporcionalmente à raiz quadrada do aumento de n.

6. A função de log-verossimilhança é obtida aplicando-se o logaritmo natural sobre o produto das densidades ou probabilidades individuais da amostra aleatória. Prefere-se maximizar $`\ln L`$($`\theta`$) porque o logaritmo é uma transformação monótona crescente que simplifica produtos em somas, facilitando a diferenciação para localizar os pontos críticos de máximo.

7. O Erro do Tipo I ($`\alpha`$) consiste em rejeitar a hipótese nula $`H_0`$ quando ela é verdadeira, enquanto o Erro do Tipo II ($`\beta`$) é o erro de não rejeitar $`H_0`$ quando ela é falsa. Como não é possível minimizar ambos os erros simultaneamente para um n fixo, a estratégia padrão é fixar a probabilidade do Erro do Tipo I em um nível de significância $`\alpha`$ (geralmente 5%) e maximizar o poder do teste (1 - $`\beta`$).

8. O teste t pareado é indicado quando as observações dos dois grupos são dependentes ou correlacionadas, como em medições efetuadas nos mesmos indivíduos antes e depois de uma intervenção. Nessas condições, o teste é aplicado sobre as diferenças individuais de cada par, avaliando se a média populacional dessas diferenças pode ser considerada nula sob a hipótese nula.

9. No Teste Qui-Quadrado de Associação, a estatística observada é comparada com o valor crítico da distribuição Qui-Quadrado com graus de liberdade $`GL = (R - 1) \times (S - 1)`$. Se a estatística observada superar o valor crítico tabular para o nível de significância $`\alpha`$ escolhido (ou se o p-valor for menor ou igual a $`\alpha`$), a hipótese nula de independência entre as variáveis é rejeitada.

10. A Inferência Bayesiana utiliza o Teorema de Bayes para multiplicar a distribuição a priori de $`\theta`$, que reflete o conhecimento ou a incerteza prévia, pela função de verossimilhança associada aos dados coletados. A distribuição a posteriori obtida é proporcional a esse produto $`\text{Posteriori} \propto \text{Verossimilhança} \times \text{Priori}`$ e serve como base para a derivação de estimadores de média, mediana ou moda a posteriori.

## 3. Questões para Desenvolvimento (Ensaio)

1. Discuta a importância do Teorema do Limite Central (TLC) como elo de ligação entre a Teoria das Probabilidades e a Inferência Estatística Paramétrica. Explique como a convergência da distribuição da média amostral para a distribuição Normal autoriza a construção de intervalos de confiança e testes de hipóteses mesmo quando a população de origem possui distribuição desconhecida.

2. Compare a abordagem da Inferência Frequentista com o paradigma da Inferência Bayesiana no que tange ao tratamento de parâmetros desconhecidos e ao uso de informações prévias. Aborde como a verossimilhança é combinada com a distribuição a priori na perspectiva Bayesiana em contraposição à estimação pontual via Máxima Verossimilhança na perspectiva frequentista.

3. Avalie o papel da Análise Exploratória de Dados (EDA) na validação de premissas estatísticas antes da condução de testes inferenciais confirmatórios. Em sua resposta, exemplifique como gráficos de dispersão, boxplots e matrizes de correlação identificam outliers, não linearidades e o comportamento assimétrico dos dados (citando os cuidados necessários demonstrados pelo conjunto de dados de Anscombe).

4. Explique a estrutura matemática e a lógica operacional da Análise de Variância (ANOVA) de 1 fator para a comparação de K médias populacionais. Detalhe o motivo pelo qual o procedimento recebe a denominação "análise de variância" apesar de ser destinado a testar a igualdade de médias, esclarecendo a decomposição da variabilidade total em somas de quadrados intra-grupos e entre-grupos.

5. Analise o processo de tomada de decisão em um Teste de Hipóteses, explicitando a inter-relação entre o nível de significância ($`\alpha`$), a região crítica e o p-valor (valor-p). Justifique formalmente a razão metodológica pela qual a conclusão de um teste deve ser expressa em termos de "não rejeitar $`H_0`$" ou "rejeitar $`H_0`$", rejeitando o uso da expressão "aceitar $`H_0`$".

## 4. Glossário Abrangente de Termos-Chave

| Termo | Definição |
|---|---|
| ANOVA (Análise de Variância) | Técnica estatística inferencial empregada para comparar as médias de três ou mais populações independentes por meio da decomposição da variância total dos dados em componentes atribuíveis aos fatores de grupo e aos erros aleatórios. |
| Assimetria | Medida de forma de uma distribuição de probabilidades ou conjunto de dados que quantifica o desvio em relação à simetria centrada na média, classificando-se como assimetria à direita (cauda longa à direita) ou à esquerda (cauda longa à esquerda). |
| Boxplot (Diagrama de Caixas) | Gráfico de análise exploratória que resume a distribuição de uma variável quantitativa apresentando o primeiro quartil, a mediana (segundo quartil), o terceiro quartil, o intervalo interquartil e a sinalização visual de observações atípicas (outliers). |
| Coeficiente de Correlação de Pearson (r) | Medida estatística adimensional de associação linear entre duas variáveis quantitativas contínuas, cujos valores variam estritamente no intervalo entre -1 e +1. |
| Coeficiente de Variação | Medida de dispersão relativa adimensional calculada pela razão entre o desvio padrão e a média amostral ($`\sigma / \mu`$ ou $`S / \bar{X}`$), utilizada para comparar a variabilidade de conjuntos de dados com unidades de medida distintas. |
| Consistência | Propriedade assintótica de um estimador estatístico que garante que a estimativa convirja em probabilidade para o verdadeiro valor do parâmetro populacional à medida que o tamanho da amostra tende ao infinito. |
| Dados Transversais (Cross-Sectional) | Estrutura de dados amostrais em que as variáveis de interesse são observadas em múltiplas unidades amostrais em um único instante temporal estático. |
| Desvio Padrão | Medida de dispersão absoluta expressa na mesma unidade da variável original, correspondente à raiz quadrada positiva da variância populacional ($`\sigma`$) ou amostral ($`S`$). |
| Distribuição a Posteriori | Na Inferência Bayesiana, é a distribuição de probabilidade atualizada de um parâmetro de interesse obtida após a atualização do conhecimento prévio (priori) com os dados observados da amostragem através da verossimilhança. |
| Distribuição a Priori | Na Inferência Bayesiana, é a distribuição de probabilidade que codifica o conhecimento, crença ou incerteza prévia sobre um parâmetro populacional antes da inclusão dos dados da amostra. |
| Distribuição Amostral | Distribuição de probabilidades de uma estatística (como a média amostral $`\bar{X}`$ ou a proporção $`\hat{p}`$) decorrente de todas as amostras possíveis de tamanho n retiradas de uma mesma população. |
| Erro Padrão | O desvio padrão da distribuição amostral de um estimador, quantificando a variabilidade associada ao processo de estimação pontual. |
| Erro Tipo I ($`\alpha`$) | Erro cometido ao rejeitar uma hipótese nula $`H_0`$ quando esta é efetivamente verdadeira. A probabilidade máxima de ocorrência deste erro é denominada nível de significância. |
| Erro Tipo II ($`\beta`$) | Erro cometido ao não rejeitar uma hipótese nula $`H_0`$ quando esta é falsa. A probabilidade complementar (1 - $`\beta`$) representa o poder do teste. |
| Estimador | Função matemática de uma amostra de variáveis aleatórias utilizada para inferir o valor de um parâmetro populacional desconhecido, sendo ele próprio uma variável aleatória antes da amostragem. |
| Estimativa | O valor numérico pontual concreto obtido quando a função do estimador é calculada utilizando os dados observados de uma amostra específica. |
| Heterocedasticidade | Condição estatística em que duas ou mais populações ou grupos sob comparação exibem variâncias populacionais desiguais. |
| Homocedasticidade | Premissa em modelos inferenciais (como ANOVA) que assume a igualdade das variâncias populacionais entre os diferentes grupos ou categorias analisados. |
| Intervalo de Confiança (IC) | Intervalo numérico construído a partir de estatísticas amostrais que possui uma probabilidade fixada (1 - $`\alpha`$) de conter o verdadeiro parâmetro populacional desconhecido. |
| Método de Máxima Verossimilhança (EMV) | Procedimento de estimação pontual que busca determinar os valores dos parâmetros populacionais que maximizam a função de verossimilhança, tornando os dados observados o mais prováveis possível. |
| Método dos Mínimos Quadrados Ordinários (MQO) | Técnica estatística para estimação de parâmetros funcionais que busca minimizar a soma dos quadrados dos erros ou resíduos observados no modelo. |
| Método dos Momentos | Método clássico de estimação pontual que consiste em igualar os momentos teóricos populacionais aos momentos empíricos amostrais para resolver o sistema de equações dos parâmetros. |
| Nível de Significância ($`\alpha`$) | Threshold probabilístico pré-determinado pelo pesquisador que define o limite máximo aceitável para a ocorrência do Erro do Tipo I em um teste de hipóteses. |
| Outlier | Observação atípica ou valor extremo dentro de um conjunto de dados que se distncia consideravelmente do padrão geral de variabilidade dos demais dados. |
| P-Valor (Valor-p) | Menor nível de significância com o qual se rejeitaria a hipótese nula; representa a probabilidade de obter uma estatística de teste tão ou mais extrema que a observada, assumindo $`H_0`$ verdadeira. |
| Poder do Teste (1 - $`\beta`$) | Probabilidade de rejeitar corretamente a hipótese nula $`H_0`$ quando a hipótese alternativa $`H_1`$ é a condição verdadeira. |
| Região Crítica | Conjunto de valores numéricos da estatística de teste que leva à rejeição da hipótese nula $`H_0`$ ao nível de significância fixado. |
| Teorema do Limite Central (TLC) | Resultado fundamental da probabilidade que garante que a média amostral $`\bar{X}`$ de observações independentes e identicamente distribuídas converge em distribuição para uma Normal à medida que o tamanho da amostra n cresce. |
| Teste Qui-Quadrado de Associação | Teste não paramétrico utilizado para avaliar a existência de dependência ou associação estatística entre duas variáveis qualitativas dispostas em uma tabela de contingência. |
| Teste t Pareado | Teste de hipóteses paramétrico empregado para comparar as médias de duas populações dependentes, analisando a distribuição das diferenças pareadas de observações correlacionadas. |
| Variável Aleatória | Função matemática que mapeia cada elemento do espaço amostral de um experimento aleatório a um valor numérico pertencente ao conjunto dos números reais. |
| Viés (ou Vício) | Medida de desvio de um estimador, definida como a diferença entre a esperança matemática do estimador e o verdadeiro valor do parâmetro populacional ($`E[\hat{\theta}] - \theta`$). |
