---
layout: post
title: "Análise dos Dados do Airbnb - Buenos Aires"
date: 2020-06-11 12:00:00 -0300
description: Análise Explorat´rio dos Dados Públicos do Airbnb de Buenos Aires. # Add post description (optional)
img: # Add image post (optional)
---

## A viagem

Sempre que pensamos em viajar, vem a duvida em nossa cabeça: "Vou ficar em um Hotel? Uma pousada? Talvez um Hostel? Ou vou instigar meu lado aventureiro e irei acampar?"

Claro que tudo isso depende de diversos fatores, é complicado querer acampar se você vai viajar com sua família e tem um filho pequeno. Da mesma forma, querer ficar em um hotel 5 estrelas quando se tem a diária de 8 pessoas para pagar, pode ficar um pouco salgado para sua carteira - dependendo de quanto você ganha. E, portanto, escolher o local no qual você irá passar suas noites de descanso se tornam uma tarefa crucial no planejamento da sua viagem.

Tendo isso em vista, o Airbnb chegou com mais uma opção para você levar em conta quando for escolher o local em que irá ficar. Essa plataforma permite que pessoas que possuem um quarto ou uma casa vaga, possam alugar esses locais para pessoas como você, que estão decidindo o melhor lugar para repousar.

Seguindo uma lógica semelhante a de empresas com nomes já conhecidos e gravados em nossos corações - quem nunca pensou no iFood quando bateu aquela fome? Ou disse a famosa frase "Quanto fica o Uber pra sua casa?" para a pessoa que te tira sorrisos bobos? - o Airbnb presta serviços hoteleiros sem nem mesmo ter um hotel com seu nome para contar história.

Mas, desde que nos facilite e poupe nosso tempo - e nossa carteira - quem somos nós para julgá-los, não é mesmo?

>Ainda mais devido a uma das iniciativas super bacanas que eles têm: DISPONIBILIZAR DADOS!

>Qual empresa não ganha o amor de um cientista de dados quando ela disponibiliza seus dados para que esse cientista possa analisar, me diz?

Através do portal Inside Airbnb, nós podemos ter acesso a dados de grandes cidades turísticas ao redor do mundo!

E é aqui que começa nossa jornada rumo a análise desses dados lindos, porém, nem tanto. (spoiler alert: Poxa Airbnb, mínimo de mil noites para alugar uma casa? Meu cartão de crédito não aguenta isso não - meu chefe também não iria me dar umas férias desse tamanho)

A partir daqui, eu irei fazer o possível para ajudá-lo a entender cada passo para a análise desses dados. Vem comigo!

![Buenos-Arires]({{site.baseurl}}/assets/img/we-in-rest.jpg)


## Obtenção dos Dados

### Importação das Bibliotecas

Primeiramente, temos que importar nossas bibliotecas

Para as análises que faremos, vamos utilizar 5 em específico:

* Pandas - É a nossa queridinha dos dados, faz de tudo que precisamos e ainda não nos cobra nada por isso!

* Matplotlib - Se você quer fazer gráficos de forma fácil e rápida, chama ela!

* Seaborn - Gráficos não são tudo na vida, beleza também é essencial! Essa aqui nos ajuda a visualizar de forma mais bonitinha.

* Folium - Também queremos mapas, eles são úteis! É sério!

Então, para importar as bibliotecas, utilizaremos o comando import

Como nós somos preguiçosos e queremos facilitar nossa vida, nós vamos apelidar essas bibliotecas com um nome menor, que vai ser mais fácil pra digitar (É como chamar a Renata de "Re", mas só para os íntimos)

Esses 'apelidos' já são amplamente adotados pela comunidade de Data Science, não vamos mudar as regras do jogo né?

O Pandas, vamos chamar de pd

O Matplotlib, vamos chamar de plt

O Seaborn, vamos chamar de sns

O Folium, não vamos dar nenhum apelido não (Tadinho!)

O comando %matplotlib inline é para que os gráficos que vamos gerar possam ser plotados (ou desenhados) aqui no Colab, logo que a gente executar a célula!

{% highlight python %}
# importar os pacotes necessarios

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import folium

%matplotlib inline
{% endhighlight %}

### Importação dos dados para análise

O arquivo que nós queremos utilizar (Dados do Airbnb de Buenos Aires, caso não se lembre) está com um pequeno defeito - Poxa Airbnb (ou Poxa Pandas?).

Se nós olharmos bem para o link dele:

" http://data.insideairbnb.com/argentina/ciudad-autónoma-de-buenos-aires/buenos-aires/2020-04-26/visualisations/listings.csv "

Essa parte aqui "autónoma" não é reconhecida pelo Pandas para que ele possa acessar esse link (Para o Pandas, no lugar desse " ó " tá escrito "%C3%B3", acredite!).

Portanto a forma mais fácil de trazer esses dados pra cá é baixar ele no nosso PC e jogar o arquivo ali no canto esquerdo do seu Colab

<--

Na pasta "Files"

Depois disso é só clicar com o botão direito do mouse em cima do arquivo e clicar em "Copy path"

Feito isso é só seguir com as análises!

O comando para importar o arquivo .csv para dentro de um DataFrame (O nome é difícil, mas é como se fosse uma tabela do Excel, você vai ver) é o "read_csv"

Como ele faz parte do Pandas (E nós apelidamos carinhosamente nosso Pandas de pd quando importamos a biblioteca) nós utilizaremos o comando pd.read_csv("caminho do seu arquivo a ser lido") e vamos armazenar esses dados em uma variável chamada b_aires

Normalmente, você irá ver as pessoas colocando como padrão o nome da variável de df, a abrevição de DataFrame, mas não faz isso não, por favor!

Usa um nome que reflete o que a variável está armazenando, isso vai facilitar bastante seu trabalho, confia em mim

Usaremos b_aires pois a variável armazena os dados de - adivinha - Buenos Aires! Super intuitivo e muito mais prático!


{% highlight python %}
# importar o arquivo listings.csv para um DataFrame

b_aires = pd.read_csv("/content/listings.csv")
{% endhighlight %}


### Análise dos Dados

#### Dicionário das colunas

Bom, para começar nossas análises, nós precisamos entender primeiramente quais são os dados que nós estamos trabalhando, certo?!

Então abaixo está um dicionário dos nossos dados, ou seja, o nome das colunas que nós temos em nossa tabela e o que cada uma dessas colunas representa

É super útil!

Então se um dia for você criando uma tabela para outras pessoas, FAÇA UM DICIONÁRIO! Obrigado

#### Dicionário das variáveis

* id - ID que identifica o imóvel

* name - Nome dado ao imóvel

* host_id - ID que identifica o proprietário (anfitrião)

* host_name - Nome do proprietário

* neighbourhood_group - Distritos ou regiões que englobam vários bairros (nem todas as cidades terão essa coluna válida)

* neighbourhood - Nome do bairro

* latitude - Latitude do imóvel

* longitude - Longitude do imóvel

* room_type - Tipo de quarto oferecido

* price - O preço da diária de locação

* minimum_nights - Quantidade mínima de noites que deve ser alugada

* number_of_reviews - Número de críticas/avaliações recebidas

* last_review - Data da última crítica/avaliação recebida

* reviews_per_month - Quantidades de críticas/avaliações recebidas por mês

* calculated_host_listings_count - Número de imóveis alugados pelo mesmo anfitrião

* availability_365 - Numero de dias em que o imóvel fica disponivel para aluguel por ano

### Comando Shape

Certo, agora nós já temos uma ideia de quais dados temos em mãos (Airbnb de Buenos Aires)

E, agora, também sabemos quais colunas nós temos para trabalhar e tirar nossas conclusões

Se você ficou com preguiça de contar, eu te digo que nós temos 16 colunas na nossa tabela

Mas, como eu sei que você é preguiçoso e nao quer voltar lá pra contar e ter certeza que eu te disse a verdade

Nós precisamos ter uma maneira mais fácil de verificar isso

Além do mais, nós também não sabemos quantas linhas essa nossa tabela tem

Então, primeiramente vamos verificar a forma da nossa tabela, como seria o "corpo" dela

Ou, em inglês, o shape do nosso DataFrame

Então vamos lá, nós queremos ver o shape de qual dado?

Se você esta prestando atenção, sabe que é da nossa variável b_aires, que contém a tabela dos dados de Buenos Aires

Então é simples, vamos pegar a nossa tabela e chamar esse tal shape

No Pandas, nós fazemos essa "chamada" utilizando um ponto - é, isso mesmo, esse ponto aqui "." que usa no fim de uma frase

Portanto, faremos: b_aires.shape()

{% highlight pyython %}
# Como verificar a quantidade de linhas e colunas no DataFrame

b_aires.shape()
{% endhighlight %}

{% highlight python %}
(23729, 16)
{% endhighlight %}

Bom, como nós ja sabemos que a nossa tabela tem 16 colunas

É intuitivo entender que o outro valor é a quantidade de linhas da nossa tabela

Mas, para deixar claro algumas coisas, vamos entender melhor o que o shape nos retorna:

A princípio nos é fornecido dois valores, separados por uma vírgula

Vimos que o primeiro valor se refere as linhas, já o segundo as colunas

Como em programação, as coisas sempre começam com zero [0], nosso querido Pandas segue a mesma lógica

Portanto os valores que foram retornados pelo shape é:

shape[0]: 23729 linhas

shape[1]: 16 colunas

Lembre-se disso, pois, a partir de agora, todas as vezes que você quiser referenciar a coluna, você utilizará o valor (ou índice) [1]

E, quando quiser referenciar as linhas, voce utilizará o valor (ou índice) [0]

Veremos mais para frente, comandos em que nós precisamos passar o "eixo" que nós queremos modificar, dessa maneira, há duas opções:

Se passarmos o "eixo" [0], modificaremos as linhas

Se passarmos o "eixo" [1], modificaremos as colunas

Guarde isso no coração, com carinho!

### Comando Head

Com esses conceitos entendidos, vamos prosseguir com nossa análise

Até o momento, nós sabemos com quais dados estamos trabalhando (Airbnb Buenos Aires)

Sabemos o tamanho dessa nossa tabela (23729 Linhas e 16 Colunas)

Maaaas, até agora nós não demos uma olhada em como essa nossa tabela é!

Não vimos a carinha dela até o presente momento

Vamos ter que dar um jeito nisso!

Como nós sabemos, essa tabela tem mais de 20 mil linhas, isso quer dizer que se nós quiséssemos que essa tabela fosse desnhada aqui, ficaria muito grande! (E consequentemente muito feio, vamos manter as coisas bonitas por aqui, certo?)

Então nós precisamos de uma maneira de ver só uma parte desses dados

Para termos uma ideia de como é a carinha dessa tabela

Existe um meio de fazer isso!

Esse meio se chama head() - isso mesmo, de "cabeça" - e ele nos mostra as primeiras 5 linhas da nossa tabela, intuitivo não?

Ver 5 linhas é bem melhor do que 20 mil, né?

Então novamente, nós pegamos a nossa tabela b_aires e chamamos esse tal de head()

Por algumas questões do Pandas, nós precisamos colocar o () no final dessa chamada

Não se preocupe, com o tempo nós vamos decorando quando se deve usar o () ou não! Pode ficar em paz (Além de tudo, temos o StackOverflow do nosso lado da força)

Vale ressaltar que: se colocarmos algum valor dentro dos (), como por exemplo head(15), o Pandas vai nos mostrar as primeiras 15 linhas, em vez de apenas 5

Não utilizaremos essa função por enquanto, mas fica a informação aqui

Caso queira brincar e ver mais linhas da sua tabela, fique a vontade

{% highlight python %}
# mostrar as 5 primeiras entradas

b_aires.head()
{% endhighlight %}

### Comando isnull

Agoooora sim!

Agora nós já sabemos todas as informações da nossa tabela que precisamos para conseguir avançar nas nossas inferências

E, por falar nisso, já podemos verificar algumas coisas olhando para essas 5 primeiras linhas da nossa tabela

Lembra que, lá no dicionário, eu te disse que a coluna neighbourhood_group não seria válida para todas as cidades? Pois é...

Buenos Aires não tem essa informação

Como podemos ver, essa coluna está preenchida com NaN

Algumas outras colunas, como as last_review e reviews_per_month, estão com algumas linhas preenchidas e outras com NaN

Curiosidade: NaN significa "Not a Number", o que quer dizer que os valores dessas células não são um número

Normalmente essa notação (NaN) é utilizada para quando os dados foram deixados em branco

Podemos encontrar outras notações também, como a NA, null, entre outros

Então vamos continuar as nossas análises

Já vimos que nossa tabela possui alguns valores ausentes

Então agora nós precisamos saber algumas coisas:

* Quantas colunas possuem valores ausentes?

* Qual a porcentagem desses valores ausentes em relação a nossa tabela completa?

Para isso, nós iremos chamar outra função que nos retorna os valores ausentes da tabela

Essa função é a isnull() - isso mesmo, como se uma pessoa estivesse perguntando "É ausente?" - intuitivo!

Novamente faremos os passos anteriores, pegaremos nosso querido b_aires e vamos perguntar isnull()?

Como nós somos preguiçosos, e queremos deixar tudo bonito, se só colocarmos: b_aires.isnull(), nós teremos como retorno uma tabela gigante (mais de 20 mil linhas) com vários True/False

Onde os valores que estão ausentes recebem True e os valores não ausentes (ou seja, o que está preenchido) recebem False

Vai ser meio dificil contar todos esses valores na unha, um por um, não quero isso não

Então, para facilitar nossa vida, vamos chamar outra função para nos ajudar, a função sum() - isso mesmo, de somar - vou parar de falar que o Pandas é intuitivo, acho que você já entendeu isso

Portanto, nosso código final será: b_aires.isnull().sum()

O que nos retornará uma lista bonitinha e fácil de visualizar dos valores ausentes

Vamos lá!


### Análise geral dos dados

{% highlight python %}
# Vendo as variáveis ausentes

b_aires.isnull().sum()
{% endhighlight %}

{% highlight python %}
id                                    0
name                                 10
host_id                               0
host_name                             3
neighbourhood_group               23729
neighbourhood                         0
latitude                              0
longitude                             0
room_type                             0
price                                 0
minimum_nights                        0
number_of_reviews                     0
last_review                        6507
reviews_per_month                  6507
calculated_host_listings_count        0
availability_365                      0
{% endhighlight %}

Certo! Bom trabalho para nós! (Se a gente não se valorizar, quem vai né?)

Podemos ver que existem algumas colunas com valores ausentes

Algumas coulunas com mais valores ausentes do que outras

Mas fica dificil mensurar isso, dizer que 6 mil valores estão ausentes a principio parece muito

Porém, em um universo paralelo, se nossa tabela possuisse 100 mil linhas, um valor de 6 mil linhas ausentes já não seria tanto...

Pois bem, voltando ao nosso universo, o que eu quero dizer é: Há alguma maneira mais fácil de conseguirmos compreender se esses valores ausentes são realmente uma grande parte dos nossos dados?

A resposta é Sim!

Mas como?

Jovem gafanhoto, é só usarmos a porcentagem!

Pois bem, como usaremos a porcentagem aqui?

Nós ja temos a soma das linhas com valores ausentes certo? (Dica: b_aires.isnull().sum())

Se lembrarmos da época de escola, recordamos que a porcentagem é a soma dos valores que queremos, dividido pela quantidade total de valores Depois pegamos isso e multiplicamos por 100

Mas, como nós pegamos a quantidade de linhas da nossa tabela? Você se lembra? (Dica: b_aires.shape[0] (Eu disse que iriamos usar aquela informação não é?))

Pois bem, já temos a faca e o quejo na mão!

Só precisamos dividir um pelo outro

Por fim, teremos algo assim: (b_aires.isnull().sum() / b_aires.shape[0])

E, então, multiplicamos por 100: ((b_aires.isnull().sum() / b_aires.shape[0]) * 100)


{% highlight python %}
# Verificando a porcentagem de valores ausentes

((b_aires.isnull().sum() / b_aires.shape[0]) * 100
{% endhighlight %}

{% highlight python %}
id                                  0.000000
name                                0.042143
host_id                             0.000000
host_name                           0.012643
neighbourhood_group               100.000000
neighbourhood                       0.000000
latitude                            0.000000
longitude                           0.000000
room_type                           0.000000
price                               0.000000
minimum_nights                      0.000000
number_of_reviews                   0.000000
last_review                        27.422142
reviews_per_month                  27.422142
calculated_host_listings_count      0.000000
availability_365                    0.000000
{% endhighlight %}


Caso nós queiramos esses dados ordenados, é só adicionar mais um comando no final

E qual comando seria esse?

Vou te dar uma chance de adivinhar... Eu já disse que o Pandas é intuitivo

O comando sort_values() - isso mesmo, de "ordenar valores" - intuitivo demais!

Pois bem, só devemos nos atentar para uma coisa, nós queremos que os maiores dados fiquem em cima, correto?

Então queremos uma ordenação do maior para o menor, de forma descendente

O sort_values possui uma informação que se chama ascending, que basicamente diz que os dados vão ser ordenador de forma "Ascendente"

Por tanto, como queremos o contrário, precisamos passar esse parametro como False

Ao final, teremos isso: ((b_aires.isnull().sum() / b_aires.shape[0]) * 100).sort_values(ascending=False)

{% highlight python %}
# Ordenando os valores da porcentagem

((b_aires.isnull().sum() / b_aires.shape[0]) * 100).sort_values(ascending=False)
{% endhighlight %}

{% highlight python %}
neighbourhood_group               100.000000
reviews_per_month                  27.422142
last_review                        27.422142
name                                0.042143
host_name                           0.012643
availability_365                    0.000000
calculated_host_listings_count      0.000000
number_of_reviews                   0.000000
minimum_nights                      0.000000
price                               0.000000
room_type                           0.000000
longitude                           0.000000
latitude                            0.000000
neighbourhood                       0.000000
host_id                             0.000000
id                                  0.000000
{% endhighlight %}


Dessa maneira, temos que:

* A coluna neighbourhood_group possui todos os seus valores nulos, 100% deles, tudinho

* As colunas reviews_per_month e last_review possuem a mesma quantidade de valores ausentes, o que é de fácil compreensão: Quando não se tem nenhuma avaliação por mês, consequentemente não há uma "ultima avaliação"; São colunas que se completam (De certa forma)

* As colunas name e host_name possuem poquissimos valores ausentes

Para nossa sorte, nenhuma dessas colunas que possuem os valores ausentes são de grande importancia para nós (Aí sim! Pelo menos alguma sorte!)

Mas você pode estar se perguntando: "A coluna number_of_reviews também não deveria estar com valores ausentes? Afinal, se não há nenhuma avaliação por mês, nem uma "última avaliação"

Se nós olharmos para nosso b_aires.head(), podemos perceber que alguns valores dessa coluna estão preenchidos com zero [0], esses zeros correspondem ao NaN das outras duas colunas

Mas será que a quantidade de zeros nessa coluna, também é a mesma quantidade de valores ausentes nas outras duas colunas?

Vamos checar isso!

Para tal, precisamos verificar na coluna number_of_reviews, quantas linhas possuem o valor zero

E como faremos?

Bom... precisamos pegar a coluna number_of_reviews == 0 (Lembre-se que "igual" em programação é "==")

Como essa coluna esta no nosso querido b_aires, passamos: b_aires.number_of_reviews == 0

>Acho que eu devo ter esquecido de te mencionar que, quando queremos acessar uma coluna especifica em uma tabela, nós colocamos o nome da tabela, um ponto e colocamos o nome da nossa coluna

Com mais essa dica, vamos prosseguir!

Essa expressão vai nos retornar uma tabela com um monte de True e False, e nós somos preguiçosos né? Queremos tudo contadinho para a gente, sem esforço

Então é só adicionarmos o .sum() ao final

Como iremos chamar o .sum(), precisamos colocar nossa expressão b_aires.number_of_reviews == 0 entre (), para dar tudo certo

Vamos lá?


{% highlight python %}
# Verificar a quantidade de linhas com valor 0 em 'number_of_reviews'

(b_aires.number_of_reviews == 0).sum()
{% endhighlight %}

{% highlight python %}
6507
{% endhighlight %}

Como podemos ver, existem exatos 6507 linhas com valores zero [0]

Exatamente o mesmo tanto que as outras duas colunas reviews_per_month e last_review

Esses valores estão nulos ou zerados pois, possivelmente, esses imóveis nunca foram alugados

Então, podemos tirar nosso primeiro insight dos dados:

* Existem 6507 imóveis em Buenos Aires que, possivelmente, nunca foram alugados

Acho que está na hora de vermos nossos dados de uma maneira mais bonitinha, não?

Que tal, gráficos?

Vamos ver como nossas colunas estão distribuidas em histogramas

Isso é bem simples, basta nós colocarmos o nome da nossa tabela (b_aires), chamar o hist (De histograma, uau!) e passar alguns parâmetros para nosso gráfico ficar bonito

Os parâmetros que vamos passar são dois:

bins - A quantidade de "barrinhas" que nosso gráfico vai ter

figsize - O tamanho dos nossos gráficos

E os valores respectivos são de sua escolha!

Eu achei bem bonito os valores de bins = 30 e figsize(20, 15), então usaremos esses

Se você prestar bastante atenção, ao final do código eu coloco um ";" ele serve para que não seja escrito os parametros do gráfico antes de desenhar o mesmo

Tire o ";" e veja a diferença, muito mais bonito com ele não é?

{% highlight python %}
# Plotar os histogramas dos nossos dados

b_aires.hist(bins = 30, figsize = (20, 15));
{% endhighlight %}

Quando usamos o b_aires.hist(), são desenhados os gráficos de todas as colunas numéricas que nós temos

Algumas não nos dizem nada, como as colunas host_id, id e neighbourhood_group

Outras podem até nos dizer algo, como as colunas:

* calculated_host_listings_count que nos diz a quantidade de imóveis para aluguel que cada anfitrião tem (Alguns tem mais de 80 imóveis, eita!)

* latitude que nos mostra em qual latitude estão concentradas a maior parte do imóveis

* longitute que segue a mesma lógica da latitude

Mas, estas colunas não são nosso foco

Para continuar nossas análises, vamos focar nas colunas price, minimun_nights e availability_365

Olhando nossos gráficos, podemos perceber que:

* price - Existem valores de diárias que vão até 600.000 Pesos Argentinos, eu não sei quanto vale um Peso Argentino, mas esse valor não parece agradável ao meu bolso...

* minimum_nights - Existem imóveis que pedem que você fique no mínimo mil noites (Isso da quase 3 anos, meu chefe não ia gostar de me dar umas férias desse tanto não...)

* availability_365 - Há imóveis que não estão disponiveis nenhum dia do ano!? Como o anfitrião aluga eles?


Esses valores exorbitantes, nós chamamos de outliers

São pontos fora da curva que não representam a maioria dos dados (Tipo aquele seu primo que passou em 5 faculdades de medicina diferentes em primeiro lugar)

E, portanto, precisamos retirar esses valores dos nossos dados, para deixá-los mais condizentes com a realidade

Antes de tudo, vamos verificar a descrição estatística desses dados

Para isso, vamos utilizar a função describe()

Como as colunas que nos interessam são as: price, minimun_nights e availability_365, vamos passar somente essas duas colunas para a função

Entao faremos: b_aires[['price', 'minimum_nights', 'availability_365']].describe()

Isso mesmo, entre dois "[[" pois o primeiro colchete é para selecionarmos a coluna, mas como nós queremos mais de uma coluna, precisamos passar todas as colunas que queremos dentro de uma lista

E, listas em Python sao definidas entre "[ ]", portanto há a necessidade de se colocar dois colchetes

Vamos lá!

{% highlight python %}
# Verificar a descrição estatistica dos dados

b_aires[['price', 'minimum_nights', 'availability_365']].describe()
{% endhighlight %}

{% highlight python %}
	price	minimum_nights	availability_365
count	23729.000000	23729.000000	23729.000000
mean	4014.875595	6.027519	209.383750
std	16075.326378	25.635455	137.991373
min	0.000000	1.000000	0.000000
25%	1394.000000	1.000000	89.000000
50%	2124.000000	3.000000	180.000000
75%	3319.000000	4.000000	363.000000
max	663732.000000	1125.000000	365.000000
{% endhighlight %}

Quando executamos o describe(), várias informações são passadas

Nós focaremos somente em algumas:

* count - A quantidade de imóveis da nossa tabela

* mean - A média dos nossos dados, ela é MUITO influenciada pelos nossos indesejados outliers

* min - O valor minimo presente nos nossos dados (Existem imóveis com preço zero!? Tipo... eu posso ir morar lá de graça? Eita")

* 75% - O valor em que 75% dos nossos dados são menores ou iguais a ele (75% dos anfitriões exigem um minimo de 4 noites ou menos para alugar seu imóvel) - a mesma lógica vale para os valores 25% e 50%

* max - O valor máximo presente nos dados (Eu queria muito umas férias de 1125 dias)

Dessa maneira, podemos perceber que:

* 75% dos imóveis tem uma diária de 3319 Pesos Argentinos ou menos

* 75% dos anfitriões exigem um minimo de 4 noites ou menos

* 75% dos imóveis estão disponiveis praticamente o ano todo - apesar de existirem imóveis que não estão disponiveis nenhum dia do ano (Pelo menos a disponibilidade máxima está como 365, Ufa!)


### Limpeza dos dados

Tendo em vista esses novos conhecimentos a cerca de nossos dados

Precisamos retirar esses dados que estão fora do comum

Para esclarecer e confirmar que você está pensando junto comigo, vamos detalhar os dados que vamos retirar e o motivo:

* Retirar os dados com valores na coluna price muito baixos ou muito altos

* Retirar os valores na coluna minimum_nights com valores acima de 30 dias, pois (para questões das nossas análises) ficar mais de 30 dias passeando fica fora do orçamento e não há dados com minimum_nights com valores de zero

* Retirar os valores na coluna availability_365 iguais a zero, afinal se o minimo de noites que você tem que ficar é 1, o imóvel tem que estar disponivel pelo menos 1 dia por ano, né?

Bom, quase tudo certo até aqui, exceto por uma coisa:

* Como vamos definir quais valores são "muito baixos" e "muito altos" para retirar da nossa coluna price?

Para definir isso, eu irei adotar duas condições que são as seguintes:

* Os valores abaixo do percentil 1% são considerados muito baixo

* Os valores acima de 6640 Pesos Argentinos (2x o percentil 75%) são considerados muito altos

Lembre-se: Estes valores foram definidos por mim, pois foi o que considerei mais válido para continuar as análises. Você, meu pequeno gafanhoto, pode achar melhor considerar outros valores e está tudo bem!

Então... Os valores "muito altos" estão fáceis, mas como eu vou saber os valores abaixo do percentil 1%?

Para isso, vamos retornar a nossa função describe() e passar alguns novos parâmetros para ela, que será o percentiles

Dessa maneira, podemos escolher os valores dos percentis que nós queremos e passamos para o describre() em forma de uma lista

Como queremos o percentil 1% e os demais valores queremos que continue, vamos passar: [.01, .25, .5, .75]

Então, no final, teremos:

b_aires[['price', 'minimum_nights', 'availability_365']].describe(percentiles=[.01, .25, .5, .75])

{% highlight python %}
# Verificar a descrição estatistica dos dados, mas adicionando o percentil 1%

b_aires[['price', 'minimum_nights', 'availability_365']].describe(percentiles=[.01, .25, .5, .75])
{% endhighlight %}

{% highlight python %}
	price		minimum_nights		availability_365
count	23729.000000	23729.000000	23729.000000
mean	4014.875595	6.027519	209.383750
std	16075.326378	25.635455	137.991373
min	0.000000	1.000000	0.000000
1%	398.000000	1.000000	0.000000
25%	1394.000000	1.000000	89.000000
50%	2124.000000	3.000000	180.000000
75%	3319.000000	4.000000	363.000000
max	663732.000000	1125.000000	365.000000
{% endhighlight %}


Então agora temos tudo em nossas mãos novamente

Os valores que iremos usar são:

* "Muito alto" = 6640 Pesos Argentinos

* "Muito baixo' = 400 Pesos Argentinos

Então chegou o grande momento, vamos retirar esses dados indesejados das nossas análises

Mas espera... Como vamos fazer isso?

Para retirar os dados vamos primeiramente fazer uma cópia da nossa tabela b_aires em uma outra variável chamda b_aires_clean

Faremos essa cópia chamando o comando copy()

Vamos fazer essa cópia para que, caso seja necessário a gente voltar para a tabela original, tudo vai estar certinho lá ainda né? É só chamar nossa querida b_aires

então, teremos algo assim:

b_aires_clean = b_aires.copy()

{% highlight python %}
# Fazendo a cópia da nossa tabela para uma nova variável

b_aires_clean = b_aires.copy()
{% endhighlight %}

A partir de agora, vamos retirar tudo que nós quisermos dessa segunda tabela: b_aires_clean

Mas como retiramos algum dado?

Para isso, vamos utilizar o comando drop()

Dessa maneira, chamamos o drop() e passamos os parâmetros que nós queremos retirar

Não entendeu?

Vamos por partes...

A principio, nosso comando vai ficar assim:

* b_aires_clean.drop(b_aires_clean[b_aires_clean.price > 6640].index, axis = 0, inplace = True)

Agora vamos entender cada parte dele:

* b_aires_clean.drop(...) - Isso é o que estamos chamando, nossos parâmetros vão ficar dentro dos () do comando drop()

* (b_aires_clean[b_aires_clean.price > 6640].index) - A primeira coisa que temos que informar ao comando drop é de qual tabela ele vai retirar os dados, fazemos isso colocando "b_aires_clean"

* Após isso informamos quais dados vão ser retirados de dentro dessa nossa tabela, no nosso caso será: [b_aires_clean.price > 6640], ou seja, os dados com um valor maior que 6640 na coluna 'price'

* Ao final, vem um ".index", para informar ao drop() que o 'price' é uma coluna e queremos retirar as linhas que atendam a esse requisito (Caso essa informação não seja passada, o drop() vai procurar linhas com o nome 'price' e vai nos retornar um erro... Nós não gostamos de erros né?)

* axis = 0 - Agora, vamos especificar que nós estamos nos referindo as linhas, e lembra que eu te pedi para decorar lá em cima que linhas são 0 e colunas são 1? Pois bem, vamos usar isso de novo

* inplace = True - Por fim, vamos informar ao drop() que nós queremos que ele salve essas informações dentro da nossa própria variável b_aires_clean, caso não coloquemos esse parâmetro, seria necessário criar uma nova variável para salvar essas modificações que acabamos de fazer (Não é necessário né? Já criamos a b_aires_clean para isso)

Entãaaao, como nós já entendemos cada parte desse comando

Para retirar os outros dados que nós queremos, vamos seguir a mesma lógica

Teremos:

* Retirar os valores "muito alto" e "muito baixo" da coluna 'price' b_aires_clean.drop(b_aires_clean[b_aires_clean.price > 6640].index, axis = 0, inplace = True) b_aires_clean.drop(b_aires_clean[b_aires_clean.price < 400].index, axis = 0, inplace = True)

* Retirar os valores maiores que 30 da coluna 'minimum_nights' b_aires_clean.drop(b_aires_clean[b_aires_clean.minimum_nights > 30].index, axis = 0, inplace = True)

* Retirar os valores iguais a zero da coluna 'availability_365' b_aires_clean.drop(b_aires_clean[b_aires_clean.availability_365 == 0].index, axis = 0, inplace = True)

Dessa forma, retiraremos todos os dados indesejados que encontramos anteriormente

Mas, voce lembra que nós temos uma coluna que não contém valor nenhum?

Ela está toda em branco... Então vamos retirar ela também

Seguiremos a mesma lógica:

* b_aires_clean.drop('neighbourhood_group', axis = 1, inplace = True)

As mudanças são:

* Como nós queremos retirar a coluna toda, é só colocarmos o nome da coluna primeiramente 'neighbourhood_group'

* Não precisamos do '.index' pois iremos retirar a coluna

* Especificamos que queremos retirar a coluna com o 'axis = 1'


{% highlight python %}
# Retirar os valores "muito alto" e "muito baixo" da coluna 'price'
b_aires_clean.drop(b_aires_clean[b_aires_clean.price > 6640].index, axis = 0, inplace = True)
b_aires_clean.drop(b_aires_clean[b_aires_clean.price < 400].index, axis = 0, inplace = True)

# Retirar os valores maiores que 30 da coluna 'minimum_nights'
b_aires_clean.drop(b_aires_clean[b_aires_clean.minimum_nights > 30].index, axis = 0, inplace = True)

# Retirar os valores iguais a zero da coluna 'availability_365'
b_aires_clean.drop(b_aires_clean[b_aires_clean.availability_365 == 0].index, axis = 0, inplace = True)

# Retirar a coluna 'neighbourhood_group' da nossa tabela
b_aires_clean.drop('neighbourhood_group', axis = 1, inplace = True)
{% endhighlight %}

Por fim, temos nosso b_aires_clean realmente Clean!

Com todos os dados indesejados retirados e prontos para realizar novas análises

A primeira coisa que vamos descobrir é:

* Quantos dados nós retiramos?

Já sabemos ver isso, vamos usar o shape para descobrir o tamanho da nossa tabela


{% highlight python %}
# Verificar o tamanho da nossa tabela
b_aires_clean.shape
{% endhighlight %}

{% highlight python %}
(18671, 15)
{% endhighlight %}


Uau, temos 18671 linhas em nossa tabela

Mas... Em relação a tabela original b_aires, nossa tabela Clean perdeu muitos dados?

Vamos verificar isso com a porcentagem, igual fizemos lá em cima

Vamos utilizar o shape[0] das duas tabelas, pois é a quantidade de linhas das tabelas

Teremos:

* ((b_aires_clean.shape[0] / b_aires.shape[0]) * 100)

{% highlight python %}
# Verificar a porcentagem entre nossas duas tabelas

((b_aires_clean.shape[0] / b_aires.shape[0]) * 100)
{% endhighlight %}

{% highlight python %}
78.68431033756164
{% endhighlight %}

Podemos ver que ainda temos aproximadamente 78,7% dos nossos dados aqui

Isso quer dizer entao que 21,3% dos dados eram outliers, segundo nossos parametros

Agora me surgiu uma dúvida...

Como será a distribuição desses dados de forma gráfica?

Será que fica parecido com os gráficos que fizemos lá em cima?

Vamos ver!

{% highlight python %}
# Plotar os histogramas dos nossos dados Clean

b_aires_clean.hist(bins = 30, figsize = (20, 15));
{% endhighlight %}



Agora sim!

As colunas price e minimum_nights ficaram muito mais faceis de se visualizar

Vamos prosseguir com nossas análises

### Correlação dos dados

Uma coisa importante para saber, é se nossos dados tem alguma correlação entre as colunas

Ter correlação significa que o valor de uma coluna influencia diretamente o valor de outra, tanto de forma positiva quanto de forma negativa (Por exemplo, o tempo que você deixa as luzes da sua casa acesa, influencia diretamente no valor da sua conta de luz - Isso é triste, mas é real)

Mas como nós verificamos a correlação entre nossas colunas?

Para essa tarefa, vamos chamar a função corr()

Então teremos:

* b_aires_clean.corr()

Simples, né?

Vamos gravar esse resultado em uma variável chamada corr_ba (O nome aqui não importa, mas significa "Correlação Buenos Aires", você pode dar o nome que quiser)

{% highlight python %}
# Realizar a matriz de correlação dos nossos dados
corr_ba = b_aires_clean.corr()

# Mostrar a matriz
corr_ba
{% endhighlight %}

{% highlight python %}
	id	host_id	latitude	longitude	price	minimum_nights	number_of_reviews	reviews_per_month	calculated_host_listings_count	availability_365
id	1.000000	0.514781	0.004495	-0.036254	-0.119784	-0.049369	-0.379507	0.180580	-0.015103	-0.192594
host_id	0.514781	1.000000	-0.068087	-0.008644	-0.128083	-0.088182	-0.190039	0.119717	-0.175336	-0.131867
latitude	0.004495	-0.068087	1.000000	-0.542824	0.138711	0.032101	0.016903	0.028815	0.024025	-0.011137
longitude	-0.036254	-0.008644	-0.542824	1.000000	0.062490	-0.023134	0.064427	0.053488	0.043281	0.035953
price	-0.119784	-0.128083	0.138711	0.062490	1.000000	-0.030622	0.031533	-0.010926	0.152592	0.093237
minimum_nights	-0.049369	-0.088182	0.032101	-0.023134	-0.030622	1.000000	-0.103908	-0.158128	0.092688	0.023279
number_of_reviews	-0.379507	-0.190039	0.016903	0.064427	0.031533	-0.103908	1.000000	0.617626	-0.067463	0.020986
reviews_per_month	0.180580	0.119717	0.028815	0.053488	-0.010926	-0.158128	0.617626	1.000000	-0.101441	-0.114440
calculated_host_listings_count	-0.015103	-0.175336	0.024025	0.043281	0.152592	0.092688	-0.067463	-0.101441	1.000000	0.054733
availability_365	-0.192594	-0.131867	-0.011137	0.035953	0.093237	0.023279	0.020986	-0.114440	0.054733	1.000000
{% endhighlight %}

Bom, vemos que o resultado é uma tabela bem feia e estranha...

Vamos deixar isso mais bonito?

Faremos um heatmap, que é basicamente um gráfico que exemplifica uma matriz de correlação de forma bem mais bonita (Colorido!)

Para conseguirmos realizar essa etapa, vamos utilizar nosso querido Seaborn

Vamos passar os seguintes dados:

* sns.heatmap(corr_ba, cmap='RdBu', fmt='.2f', annot=True);

Em que:

* corr_ba - É a nossa matriz de correlação, que criamos na etapa anterior

* cmap - É o mapa de cores que vamos utilizar, esse 'RdBu' é bem bonitinho, mas você pode pesquisar na internet os diferentes mapas de cores que existem e testar

* fmt - É a formatação que iremos utilizar, como você viu na nossa matriz, os numero decimais estão com 6 casas depois da vírgula, vamos arrendondar isso para apenas 2 casas com o comando '.2f' (Beleza é tudo, né?)

* annot - É para decidir se queremos que os valores sejam escritos no meio dos quadradinhos, isso pode deixar bonito! Ou não...

Vamos lá!


{% highlight python %}
# Plotar nosso heatmap

sns.heatmap(corr_ba, cmap= 'RdBu', fmt='.2f', annot=True);
{% endhighlight %}

Como podemos ver, não há nenhuma correlação clara e informativa entre nossas colunas

Não conseguimos extrair nenhuma informação válida dos nossos dados, a partir dessa correlação...

Maaas, serviu de aprendizado!

### Perguntas a serem respondidas

Agora, vamos tentar responder algumas perguntas sobre nossos dados, o que acha?

Eu ouvi dizer que os anfitriões do Airbnb disponibilizam diferentes tipos de quartos, podendo ser até quartos de hotel (Uau!)

Entao, vamos fazer algumas perguntas referentes a isso:

* Quais os tipos de imóveis disponiveis em Buenos Aires e Quantos imóveis de cada tipo estão disponiveis para locação?

* Quais os valores médios desses imóveis?

* Há diferenças na quantidades de noites mínimas para cada tipo de quarto?

* Quais são os bairros mais caros de Buenos Aires?

* Quantos imóveis estão disponiveis para alugar em cada Bairro?

* Poderiamos fazer mais mil perguntas em cima desses dados, mas vamos focar somente nessas por enquanto

Vamos lá! Uma por uma

### Quais os tipos de imóveis disponiveis em Buenos Aires e Quantos imóveis de cada tipo estão disponiveis para locação?

Para responder essa pergunta, vamos contar a quantidade de quartos diferentes através da nossa coluna 'room_type'

Vamos matar dois coelhos com uma cajadada só! (Mentira gente, não vamos matar nada não, é apenas uma expressão popular! Obs: Coelhos são fofos)

Contando a quantidade de quartos diferentes, já saberemos quais são esses dados diferentes e quais as quantidades de cada um (Meio óbvio, mas ok... Se vamos contar, teremos as quantidades)

Vamos fazer essa contagem através da função value_counts()

Essa é uma função que soma quantas linhas de um determinado tipo nós temos

Então, vamos chamar nossa tabela, a coluna que queremos e a função

Teremos:

* b_aires_clean.room_type.value_counts()

{% highlight python %}
#Verificando a quantidade de quartos e quais são eles

b_aires_clean.room_type.value_counts()
{% endhighlight %}

{% highlight python %}
Entire home/apt    14579
Private room        3473
Shared room          397
Hotel room           222
{% endhighlight %}


Então, através disso podemos perceber que:

* Há 4 tipos diferentes de imóveis:

	* Casa/apartamento inteiro (Isso mesmo, uma casa inteira só para você!)
	
	* Quarto privado (Você vai ficar na casa de alguém, mas o quarto é todinho seu)
	
	* Quarto compartilhado (Você vai ficar na casa de alguém, e vai dividir seu quarto provavelmente com um desconhecido... É... Pode até ser uma experiência bacana)
	
	* Quarto de hotel (Você fica em um hotel, pagando o preço do Airbnb, uau! - Só não sendo aquele quarto de 600.000 Pesos Argentinos, ta ótimo...)

* A maioria dos imóveis disponiveis para locação são Apartamentos ou Casas inteiras

* Quartos de hotel e quartos compartilhados não são muito populares em Buenos Aires...


### Quais os valores médios desses imóveis?

Para prosseguir com a resposta dessa pergunta, vamos ter que aprender uma função nova, a groupby()

Como o nome sugere, ela agrupa nossos dados de acordo com uma coluna específica e depois nós podemos separar esses dados passando outros critérios e filtros

Então vamos lá, parte por parte

Nossa expressão ficará assim:

* b_aires_clean.groupby(['room_type']).price.mean().sort_values(ascending = False)

Agora vamos entende-la:

* Primeiramente, chamamos nossa tabela b_aires_clean

* Depois nós chamamos a função que vai agrupar nossos dados de acordo com alguma coluna específica .groupby()

* Passamos a coluna que nós queremos que ele agrupe os dados, como queremos saber a média dos preços dos quartos, vamos agrupar pelos quartos e depois pegar a média de cada tipo

	* Então passamos dentro do ".groupby()" nossa coluna [room_type]

* Agora precisamos da média dos valores desses quartos, que está na coluna price

	* Então passamos a expressão que calcula a média desses valores .price.mean()

Como nós já agrupamos os dados por cada tipo de quarto, será calculada a média separadamente para cada tipo

Muito fácil!

Eu já disse que o Pandas é incrível? Pois ele é!

Por fim, caso nós sejamos tentados a deixar tudo bonito (E nós vamos ser), passamos o .sort_values(ascending = False) no final, assim nossos dados serão mostrados do maior para o menor

Vamos lá!

{% highlight python %}
# Agrupar nossos dados pelos tipos de quarto e calcular a média dos mesmos

b_aires_clean.groupby(['room_type']).price.mean().sort_values(ascending = False)
{% endhighlight %}


{% highlight python %}
room_type
Hotel room         2786.414414
Entire home/apt    2723.801907
Private room       1445.029945
Shared room        1072.133501
{% endhighlight %}


O que podemos observar?

* Apesar dos quartos de hotel serem os menos populares em Buenos Aires, eles são os mais caros!

* Os quartos privados, que ficam com o segundo lugar no quesito "Quantidade", são quase a metade do preço dos quartos de hotel e casas/apartamentos inteiros

* Quartos compartilhados são uma opção muito barata caso você esteja com a carteira apertada

### Há diferenças na quantidades de noites mínimas para cada tipo de quarto?

Seguindo a mesma lógica da pergunta anterior, agora nós queremos saber: para cada tipo de quarto, qual a quantidade média de noites minimas?

Então, vamos agrupar novamente nossos dados pela coluna room_type

E vamos passar a média da coluna minimum_nights para separar cada tipo de quarto

Igualzinho no exemplo anterior!

Teremos:

* b_aires_clean.groupby(['room_type']).minimum_nights.mean().sort_values(ascending = False)

Vamos lá!

{% highlight python %}
# Agrupar os dados pelos tipos de quarto e verificar a média de noites mínimas

b_aires_clean.groupby(['room_type']).minimum_nights.mean().sort_values(ascending = False)
{% endhighlight %}

{% highlight python %}
room_type
Entire home/apt    4.333013
Private room       3.898071
Shared room        2.675063
Hotel room         2.612613
{% endhighlight %}

Primeiramente, precisamos observar que não existem noites "picadas", ou seja, não existe "2.67" noites... Ou são 2 noites ou são 3 noites

Tendo isso em vista, com um arredondameto simples, temos que:

* Casa/Apartamento inteiro: 4 noites

* Quarto privado: 4 noites

* Quarto compartilhado: 3 noites

* Quarto de hotel: 3 noites

Então, podemos ver que, em média:

* Não há uma diferença grande de noites minimas exigidas entre os diferentes tipos de quarto

* As casas/apartamentos completos e os quartos privados, exigem que você fique um pouco mais tempo (Isso é, uma diária a mais, deixa mais caro né?)

* A principal diferença entre o quarto compartilhado e o quarto de hotel é o preço (E muito...)


### Quais são os bairros mais caros de Buenos Aires?

Como o groupby é uma "mão na roda" e extremamente poderoso, nós podemos (E vamos) usar ele em praticamente toda análise

Para responder essa pergunta, vamos seguir a mesma lógica das outras duas perguntas feitas:

Agrupar nossos dados a partir de uma coluna específica (Nesse caso, os bairros, ou neighbourhood)

Dividir esses dados pela média dos preços desse local (Nesse caso .price.mean(), igual o primeiro!)

Teremos:

* b_aires_clean.groupby(['neighbourhood']).price.mean().sort_values(ascending = False)

{% highlight python %}
# Verificar quais são os bairros mais caros de Buenos Aires

b_aires_clean.groupby(['neighbourhood']).price.mean().sort_values(ascending = False)
{% endhighlight %}

{% highlight python %}
neighbourhood
Puerto Madero        3966.006993
Villa Soldati        3761.666667
Palermo              2821.959108
Retiro               2670.495992
Recoleta             2668.466879
Versalles            2588.571429
Belgrano             2380.204465
San Nicolas          2372.478112
San Telmo            2363.080342
Nuñez                2346.164420
Villa Devoto         2336.454545
Coghlan              2269.791045
Monserrat            2251.345209
Colegiales           2229.218289
Chacarita            2159.637771
Barracas             2145.782946
Villa Real           2124.000000
Liniers              1973.666667
Floresta             1964.088235
Villa Urquiza        1925.748571
Villa Ortuzar        1925.015625
Velez Sarsfield      1905.705882
Almagro              1894.844595
Villa Crespo         1882.477346
Parque Chas          1858.684211
Agronomia            1858.636364
Balvanera            1845.246502
Constitucion         1816.410853
Monte Castro         1806.428571
San Cristobal        1791.675214
Caballito            1787.279762
Flores               1642.680000
Villa Pueyrredon     1640.718750
Boca                 1627.214953
Saavedra             1604.773196
Parque Chacabuco     1580.121951
Villa Luro           1551.562500
Boedo                1549.879518
Nueva Pompeya        1540.100000
Villa Del Parque     1537.511628
Villa Gral. Mitre    1377.350000
Parque Patricios     1354.145455
Parque Avellaneda    1342.444444
Villa Santa Rita     1330.809524
Villa Riachuelo      1294.500000
Paternal             1292.857143
Mataderos            1269.500000
Villa Lugano         1062.000000
{% endhighlight %}


Bom, são muitos bairros...

Mas como nossa pergunta é: "Quais são os bairros mais caros?"

Vamos focar nos 5 primeiros, que são:

* Puerto Madero - 3966,00 Pesos Argentinos

* Villa Soldati - 3761,70 Pesos Argentinos

* Palermo - 2821,96 Pesos Argentinos

* Retiro - 2670,50 Pesos Argentinos

* Recoleta - 2668,47 Pesos Argentinos

Mas... Eu nunca fui para Buenos Aires

Será que esses bairros realmente são os mais caros?

Com uma pesquisa rápida na internet, chegamos a conclusão que: SIM!

Exceto pela "Villa Soldati", no qual não ouvi falar muito

Esses são realmente os bairros mais caros de Buenos Aires (Da uma olhada: https://aguiarbuenosaires.com/bairros-de-buenos-aires/ e https://olaargentina.com/alugar-10000-pesos-em-buenos-aires/)

E, aparentemente, além de caros eles são extremamente bonitos e turistáveis

Creio que você não vai se arrepender de ficar nesses bairros durante sua viagem! (Sua carteira talvez se arrependa depois, mas o dinheiro tá ai para ser gasto né? Brincadeira, economiza)


### Quantos imóveis estão disponiveis para alugar em cada Bairro?

Por fim, para responder nossa última pergunta

Vamos utilizar a função - adivinha - se você disse: groupby(), desculpe, mas você errou!!

Nós não iremos precisar dela agora

Vamos ultilizar a função value_counts, pois queremos contar a quantidade de linhas que cada bairro tem

Mas, vamos seguir novamente a mesma lógica:

Queremos saber:

* A quantidade de imóveis por bairro

Então:

* Temos que escolher a coluna que estão os bairros em nossa tabela

* Realizar a contagem de imóveis disponiveis

Então nós teremos:

* b_aires_clean.neighbourhood.value_counts()


{% highlight python %}
# Contar a quantidade de imóveis em cada bairro

b_aires_clean.neighbourhood.value_counts()
{% endhighlight %}


{% highlight python %}
Palermo              5649
Recoleta             3140
San Nicolas          1165
Retiro                998
Balvanera             929
Belgrano              851
Monserrat             814
Almagro               740
Villa Crespo          618
San Telmo             585
Nuñez                 371
Colegiales            339
Caballito             336
Chacarita             323
Constitucion          258
Villa Urquiza         175
Puerto Madero         143
Barracas              129
San Cristobal         117
Boca                  107
Saavedra               97
Boedo                  83
Flores                 75
Coghlan                67
Villa Ortuzar          64
Villa Devoto           55
Parque Patricios       55
Villa Del Parque       43
Parque Chacabuco       41
Parque Chas            38
Floresta               34
Villa Pueyrredon       32
Agronomia              22
Villa Santa Rita       21
Paternal               21
Villa Gral. Mitre      20
Velez Sarsfield        17
Villa Luro             16
Liniers                15
Monte Castro           14
Versalles              14
Nueva Pompeya          10
Parque Avellaneda       9
Mataderos               8
Villa Real              7
Villa Soldati           3
Villa Riachuelo         2
Villa Lugano            1
{% endhighlight %}


Logo de cara, podemos perceber que:

* 3 dos bairros mais caros de Buenos Aires estão presentes, também, como os bairros que tem mais imóveis disponiveis para alugar

Mas, olhando um pouco mais, vemos que:

* Puerto Madero, o bairro mais caro e luxuoso, possui apenas 143 imóveis para aluguel

* Villa Soldati, o segundo bairro mais caro (segundo nossas análises), possui apenas 3 imóveis para alugar (São poucos, mas são bem caros... Que tal dar uma olhadinha nesses imóveis?)

### Dando uma olhada em Villa Soldati

Eu fiquei curioso para dar uma olhada nesses imóveis da Villa Soldati

Vamos conferir?

Para isso, vamos utilizar a expressão:

* b_aires_clean[b_aires_clean.neighbourhood == 'Villa Soldati']

Que, de forma fácil, quer dizer:

* Da minha tabela b_aires_clean, pega todas as linhas que na coluna neighbourhood tem 'Villa Soldati' escrito: [b_aires_clean.neighbourhood == 'Villa Soldati']

Vamos lá!

{% highlight python %}
# Olhando os imóveis de Villa Soldati

b_aires_clean[b_aires_clean.neighbourhood == 'Villa Soldati']
{% endhighlight %}

{% highlight python %}
	id	name	host_id	host_name	neighbourhood	latitude	longitude	room_type	price	minimum_nights	number_of_reviews	last_review	reviews_per_month	calculated_host_listings_count	availability_365
14026	32548725	Departamento amplio y cómodo	226116702	Aneth	Villa Soldati	-34.66094	-58.43645	Entire home/apt	2655	1	0	NaN	NaN	2	179
15710	34492123	Muy buen hambiente..tranquilidad y espacio	260348454	Nilsa	Villa Soldati	-34.65855	-58.44305	Private room	3983	1	0	NaN	NaN	1	364
16022	34811527	Departamento capital federal 4 ambientes grandes	262448759	Fer	Villa Soldati	-34.66639	-58.44716	Private room	4647	1	0	NaN	NaN	1	365
{% endhighlight %}


Podemos ver que:

* Há dois quartos privados e uma casa/apartamento inteiro para alguel

* Esses imóveis não tem nenhuma avaliação, o que, possivelmente, quer dizer que eles nunca foram alugados

* Os preços dos imóveis do tipo "Quarto privado" são bastante fora do padrão para os tipos de quartos em questão, visto que:

	* A média do preço de 'Quartos Privados' é 1445,00 Pesos Argentinos

Não conseguimos entender o motivo de o(a) dono(a) desses imóveis colocar um valor tão alto...

Mas talvez conseguimos entender o motivo de nunca terem sido alugados, né? (Muito caro! Poxa...)


###Fazendo um ultimo Gráfico bem bonito!

Se nós lembrarmos dos dados, vemos que há duas colunas com valores de latitude e longitude

Se olharmos um mapa Mundi 2D, percebemos que os valores de latitude se encaixam perfeitamente no eixo Y

E os valores de longitute no eixo X

Vamos fazer isso?

Então, para essa tarefa, nós utilizaremos um tipo de gráfico chamado de scatter plot, ou gráfico de dispersão

Ele basicamente adiciona pontinhos em um gráfico seguindo os valores dos eixos X e Y, que no nosso caso, são a longitude e a latitude respectivamente

Teremos:

* b_aires_clean.plot(kind="scatter", x='longitude', y='latitude', s=5, c = b_aires_clean['price'], cmap='cool', figsize=(15,10));

Onde:

* kind - É o tipo de mapa que queremos desenhar, nesse caso será o scatter

* x - Quais serão os valores do eixo X, no nosso caso sõa os valores da coluna 'longitute'

* y - Quais serão os valores do eixo Y, no nosso caso sõa os valores da coluna 'latitude'

* s - É o tamanho de cada ponto (Ou bolinha) do mapa, coloquei 5, mas pode mudar para qualquer valor que você gostar

* c - Se quisermos que nossas bolinhas mudem de cor devido a algum outro dado, colocamos esse dado aqui

	* No nosso caso, queremos que a cor da bolinha seja devido ao preço do imóvel em questão, então passamos b_aires_clean['price']

* cmap - É o mapa de cores que queremos utilizar em nossas bolinhas, acho esse "cool" bem legal (Ahá, pegou o trocadilho?)

* figsize - É o tamanho que queremos desenhar nosso mapa, você também pode mudar esse valor e ver qual fica melhor

Vamos lá?!

{% highlight python %}
# Plotar um gráfico de dispersão com nossos dados de latitude e longitude

b_aires_clean.plot(kind="scatter", x='longitude', y='latitude', s=5, c = b_aires_clean['price'], cmap='cool', figsize=(15,10));
{% endhighlight %}

Lembra que eu disse que mapas são importantes?

Pois é, eles são!

E agora nós vamos ver um mapa da cidade de Buenos Aires

Para isso vamos utilizar nossa biblioteca "Folium" (Tadinha, estava ignorada até agora)

Mas como é a expressão mesmo? "Por último, mas não menos importante"

Vamos desenhar nosso gráfico então

Vamos utilizar apenas uma linha para desenhar esse gráfico, olha que fácil!

Vamos escrever:

* mapa_ba = folium.Map(location=[-34.6083, -58.3712], min_zoom=12, width = '75%', height = '75%')

Agora, passo a passo:

* Primeiramente precisamos chamar a função Map() da biblioteca Folium, para isso, precisamos chamar a biblioteca e depois a função e salvar isso em uma variável que eu vou chamar de "mapa_ba"

	* Fica assim: mapa_ba = folium.Map()

Agora precisamos passar alguns parâmetros dentro dos ():

* location - Aqui passamos as coordenadas de onde nós queremos desenhar o gráfico, colocamos primeiro a latitude e depois a longitute, cuidado não erre a ordem

* Com uma pesquisa rápida na internet, encontramos que as coordenadas de Buenos Aires são: -34.6083 (latitude) e -58.3712 (longitude)

* min_zoom - É o zoom mínimo que nosso mapa pode ter, colocamos em 12 pois assim mostra certinho a cidade e os Bairros de Buenos Aires

* width e height - Aqui mudamos o tamanho do nosso mapa, em porcentagem, deixei em 75% pois fica um tamanho muito bom (Beleza é tudo, né?)

Agora só precisamos chamar a variável "mapa_ba" e ver nosso gráfico gostosinho desenhado

{% highlight python %}
mapa_ba = folium.Map(location=[-34.6083, -58.3712], min_zoom=12, width = '75%', height = '75%')

mapa_ba
{% endhighlight %}

Comparando o nosso gráfico de dispersão com o mapa que nós desenhamos, podemos ver que:

* Primeiramente, tudo ficou bem bonito

* Segundo, grande parte dos imóveis para aluguel estão nas margens do "Rio da Prata" (Deve ser um local bem bonito)

* Grande parte dos imóveis mais caros também estão nas margens do Rio da Prata (Bolinhas rosas)

## Conclusões

Bom...

Chegamos ao fim das nossas análises

Espero que eu tenha te ajudado a entender melhor como funciona uma análise dos dados (Apesar de ter sido uma análise superficial, vimos bastante coisa)

E nós chegamos a algumas conclusões:

* Quase 79% dos nossos dados são de "boa qualidade", isto é, estão dentro do padrão que nós esperamos

* Nossos dados não possuem nenhuma correlação entre si

* A maioria dos imóveis disponiveis para locação são Apartamentos ou Casas inteiras

* Os quartos de hotel são os mais caros, com pouca diferença para os apartamentos/casas inteiras

* Não há diferenças entre a quantididade mínima de noites entre os diferentes tipos de quartos

* Puerto Madero é o bairro mais caro (E parece ser o mais bonito!)

Agradeço por ter chegado até aqui!

Me segue nas outras redes sociais:

[Medium] @thallitonsilva
[LinkedIn] thallitonsilva
[Instagram] @thaaaaal
[Github] ThallitonSilva

[Medium]:  https://medium.com/@thallitonsilva
[LinkedIn]:  https://www.linkedin.com/in/thallitonsilva/
[Instagram]:  https://www.instagram.com/thall_bio.py/
[Github]:  https://github.com/ThallitonSilva

