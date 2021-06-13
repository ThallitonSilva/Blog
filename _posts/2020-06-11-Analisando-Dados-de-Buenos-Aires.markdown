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



{% highlight python %}

{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
