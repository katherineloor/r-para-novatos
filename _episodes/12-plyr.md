---
title: Split-Apply-Combine
teaching: 30
exercises: 30
questions:
- "Como posso fazer diferentes cálculos em diferentes conjuntos de dados?"
objectives:
- "Poder usar a estratégia de split-apply-combine para análise de dados."
keypoints:
- "Use o pacote `plyr` para dividir dados, aplicar funções aos subconjuntos e combinar os resultados."
---




Anteriormente analisamos como usar funções para simplificar o seu código.
Nós definimos a função `calcGDP`, que pega o conjunto de dados gapminder,
e multiplica a população com a coluna do PIB per capita. Também definimos
argumentos adicionais para que possamos filtrar por `year` e `country`:


~~~
# Takes a dataset and multiplies the population column
# with the GDP per capita column.
calcGDP <- function(dat, year=NULL, country=NULL) {
  if(!is.null(year)) {
    dat <- dat[dat$year %in% year, ]
  }
  if (!is.null(country)) {
    dat <- dat[dat$country %in% country,]
  }
  gdp <- dat$pop * dat$gdpPercap

  new <- cbind(dat, gdp=gdp)
  return(new)
}
~~~
{: .r}

Uma tarefa comum que encontrará ao trabalhar com dados, é que se você desejará
executar cálculos em diferentes grupos dentro dos dados. Acima, estávamos
calculando simplesmente o PIB multiplicando duas colunas juntas. Mas e se
quisermos calcular o PIB médio por continente?

Deveríamos executar `calcGPD` e então pegar as médias por cada continente:


~~~
withGDP <- calcGDP(gapminder)
mean(withGDP[withGDP$continent == "Africa", "gdp"])
~~~
{: .r}



~~~
[1] 20904782844
~~~
{: .output}



~~~
mean(withGDP[withGDP$continent == "Americas", "gdp"])
~~~
{: .r}



~~~
[1] 379262350210
~~~
{: .output}



~~~
mean(withGDP[withGDP$continent == "Asia", "gdp"])
~~~
{: .r}



~~~
[1] 227233738153
~~~
{: .output}

Mas isso não é *agradável*. Sim, usando uma função, você pode reduzir uma
quantidade substancial de repetições. Isso **é** bom. Porém ainda há
repetição. Repetindo custará-lhe tempo, agora e mais tarde, e
potencialmente introduzirá alguns erros desagradáveis.

Poderíamos escrever uma nova função que seja flexível como `calcGDP`, mas isso
também requer uma quantidade substancial de esforço e testes até estar pronta.

O problema que estamos revelando aqui é conhecido como "split-apply-combine":

![Split apply combine](../fig/splitapply.png)

Nós queremos *split (dividir)* nossos dados em grupos, neste caso continentes, 
*apply (aplicar)* alguns cálculos nesse grupo, e depois opcionalmente 
*combine (combinar)* os resultados.

## O pacote `plyr`

Para alguns de vocês que tem usado R antes, poderia estar familiarizado com o
uso de família de funções `apply`. Agora, vamos apresentar outro método para 
resolver o problema "split-apply-combine". O pacote [plyr](http://had.co.nz/plyr/) fornece 
um conjunto de funções que encontraremos mais agradáveis para resolver este problema.

Instalamos este pacote em uma tarefa anterior. Vamos carregá-lo agora:


~~~
library("plyr")
~~~
{: .r}

Plyr tem funções para operações entre `lists`, `data.frames` e `arrays`
(matrizes, ou n-dimensional vetores). Cada função executa:

1. Uma operação de divisão (**split**ting).
2. Aplique uma função em cada divisão (**Apply**).
3. Recombine os dados da saída como um único objeto de dados (Re**combine**).

As funções são chamadas baseadas na estrutura dos dados elas esperam como entrada,
e a estrutura dos dados que se deseja como saída: [a]rray, [l]ist, ou
[d]ata.frame. A primeira letra corresponde à estrutura dos dados da entrada,
a segunda letra à estrutura dos dados da saída, e o resto da função
é chamada "ply".

Isso nós dá 9 funções principais **ply. Existem três funções adicionais que só
executarão as etapas de divisão e aplicação e não qualquer etapa de combinação.
Eles são chamados por seu tipo de dados de entrada e representam uma saída nula por um `_` 
(veja a tabela)

Note aqui que o uso dos "arrays" de plyr é diferente que em R,
um array em ply pode incluir um vetor ou uma matriz.

![Full apply suite](../fig/full_apply_suite.png)


Cada uma das funções de xxply (`daply`, `ddply`, `llply`, `laply`, ...) tem a
mesma estrutura e tem 4 características e estruturas chaves:


~~~
xxply(.data, .variables, .fun)
~~~
{: .r}

* A primeira letra do nome da função dá o tipo de entrada e o segundo dá o tipo de dado da saída.
* .data - dá o objeto de dados a ser processado
* .variables - identifica as variáveis de divisão
* .fun - dá a função que será chamada em cada fase

Agora podemos rapidamente calcular a média do PIB por continente:


~~~
ddply(
 .data = calcGDP(gapminder),
 .variables = "continent",
 .fun = function(x) mean(x$gdp)
)
~~~
{: .r}



~~~
  continent           V1
1    Africa  20904782844
2  Americas 379262350210
3      Asia 227233738153
4    Europe 269442085301
5   Oceania 188187105354
~~~
{: .output}

Vamos revisar o código anterior:

- A função `ddply` se alimenta de um `data.frame` (função começa com **d**) e
retorna outro `data.frame` (segunda letra é **d**)
- O primeiro argumento que demos foi a data.frame com o qual queríamos operar: neste
  caso os dados de gapminder. Chamamos primeiro `calcGDP` tal que ele tería agregada
  uma coluna adicional `gdp`.
- O segundo argumento indicou nossos critérios de divisão: neste caso a coluna "continent".
  Note que nós demos o nome da coluna, não o valor da coluna como a gente tem feito anteriormente 
  com subconjuntos. Plyr cuida desses detalhes de implementação por você.
- O terceiro argumento é a função que queremos aplicar a cada grupo de dados.
  Temos que definir aqui nossa propria função curta: cada subconjunto de dados
  é armazenado em `x`, o primeiro argumento de nossa função. Esta é uma função 
  anônima: não a definimos em outro lugar, e ela não tem nome. Ela só existe
  dentro do âmbito de nossa chamada a `ddply`.

E se quisermos uma estrutura de dados de saída diferente?:


~~~
dlply(
 .data = calcGDP(gapminder),
 .variables = "continent",
 .fun = function(x) mean(x$gdp)
)
~~~
{: .r}



~~~
$Africa
[1] 20904782844

$Americas
[1] 379262350210

$Asia
[1] 227233738153

$Europe
[1] 269442085301

$Oceania
[1] 188187105354

attr(,"split_type")
[1] "data.frame"
attr(,"split_labels")
  continent
1    Africa
2  Americas
3      Asia
4    Europe
5   Oceania
~~~
{: .output}

Nós chamamos a mesma função novamente, mas mudamos a segunda letra para uma `l`,
assim a saída obtida foi uma lista.

Podemos também especificar várias colunas para agrupar:


~~~
ddply(
 .data = calcGDP(gapminder),
 .variables = c("continent", "year"),
 .fun = function(x) mean(x$gdp)
)
~~~
{: .r}



~~~
   continent year           V1
1     Africa 1952   5992294608
2     Africa 1957   7359188796
3     Africa 1962   8784876958
4     Africa 1967  11443994101
5     Africa 1972  15072241974
6     Africa 1977  18694898732
7     Africa 1982  22040401045
8     Africa 1987  24107264108
9     Africa 1992  26256977719
10    Africa 1997  30023173824
11    Africa 2002  35303511424
12    Africa 2007  45778570846
13  Americas 1952 117738997171
14  Americas 1957 140817061264
15  Americas 1962 169153069442
16  Americas 1967 217867530844
17  Americas 1972 268159178814
18  Americas 1977 324085389022
19  Americas 1982 363314008350
20  Americas 1987 439447790357
21  Americas 1992 489899820623
22  Americas 1997 582693307146
23  Americas 2002 661248623419
24  Americas 2007 776723426068
25      Asia 1952  34095762661
26      Asia 1957  47267432088
27      Asia 1962  60136869012
28      Asia 1967  84648519224
29      Asia 1972 124385747313
30      Asia 1977 159802590186
31      Asia 1982 194429049919
32      Asia 1987 241784763369
33      Asia 1992 307100497486
34      Asia 1997 387597655323
35      Asia 2002 458042336179
36      Asia 2007 627513635079
37    Europe 1952  84971341466
38    Europe 1957 109989505140
39    Europe 1962 138984693095
40    Europe 1967 173366641137
41    Europe 1972 218691462733
42    Europe 1977 255367522034
43    Europe 1982 279484077072
44    Europe 1987 316507473546
45    Europe 1992 342703247405
46    Europe 1997 383606933833
47    Europe 2002 436448815097
48    Europe 2007 493183311052
49   Oceania 1952  54157223944
50   Oceania 1957  66826828013
51   Oceania 1962  82336453245
52   Oceania 1967 105958863585
53   Oceania 1972 134112109227
54   Oceania 1977 154707711162
55   Oceania 1982 176177151380
56   Oceania 1987 209451563998
57   Oceania 1992 236319179826
58   Oceania 1997 289304255183
59   Oceania 2002 345236880176
60   Oceania 2007 403657044512
~~~
{: .output}


~~~
daply(
 .data = calcGDP(gapminder),
 .variables = c("continent", "year"),
 .fun = function(x) mean(x$gdp)
)
~~~
{: .r}



~~~
          year
continent          1952         1957         1962         1967
  Africa     5992294608   7359188796   8784876958  11443994101
  Americas 117738997171 140817061264 169153069442 217867530844
  Asia      34095762661  47267432088  60136869012  84648519224
  Europe    84971341466 109989505140 138984693095 173366641137
  Oceania   54157223944  66826828013  82336453245 105958863585
          year
continent          1972         1977         1982         1987
  Africa    15072241974  18694898732  22040401045  24107264108
  Americas 268159178814 324085389022 363314008350 439447790357
  Asia     124385747313 159802590186 194429049919 241784763369
  Europe   218691462733 255367522034 279484077072 316507473546
  Oceania  134112109227 154707711162 176177151380 209451563998
          year
continent          1992         1997         2002         2007
  Africa    26256977719  30023173824  35303511424  45778570846
  Americas 489899820623 582693307146 661248623419 776723426068
  Asia     307100497486 387597655323 458042336179 627513635079
  Europe   342703247405 383606933833 436448815097 493183311052
  Oceania  236319179826 289304255183 345236880176 403657044512
~~~
{: .output}

Você pode usar essas funções em lugar de `for` loops (é usualmente mais rápido).
Para substituir um for loop, ponha o código que está no corpo do `for` loop em lugar da função anônima.


~~~
d_ply(
  .data=gapminder,
  .variables = "continent",
  .fun = function(x) {
    meanGDPperCap <- mean(x$gdpPercap)
    print(paste(
      "The mean GDP per capita for", unique(x$continent),
      "is", format(meanGDPperCap, big.mark=",")
   ))
  }
)
~~~
{: .r}



~~~
[1] "The mean GDP per capita for Africa is 2,193.755"
[1] "The mean GDP per capita for Americas is 7,136.11"
[1] "The mean GDP per capita for Asia is 7,902.15"
[1] "The mean GDP per capita for Europe is 14,469.48"
[1] "The mean GDP per capita for Oceania is 18,621.61"
~~~
{: .output}

> ## Dica: apresentação de números
>
> A função `format` pode ser usada para "melhorar" a apresentação
> dos valores numéricos em mensagens.
{: .callout}


> ## Desafio 1
>
> Calcule a média da esperança de vida por continente. Qual tem a maior?
> Qual tem a menor?
{: .challenge}

> ## Desafio 2
>
> Calcule a média da esperança de vida por continente e ano. Qual tinha a
> maior e a menor em 2017? Qual apresenta a maior mudança entre os anos 1952
> e 2007?
{: .challenge}


> ## Desafio Avançado
>
> Calcule a diferença das médias da esperança de vida entre
> os anos 1952 e 2007 usando os resultados do desafio 2
> use uma das funções de `plyr`.
{: .challenge}

> ## Desafio Alternativo se a aula parece perdida
>
> Sem executá-lo, qual dos segintes códigos calculará a média
> da esperança de vida por continente:
>
> 1.
> 
> ~~~
> ddply(
>   .data = gapminder,
>   .variables = gapminder$continent,
>   .fun = function(dataGroup) {
>      mean(dataGroup$lifeExp)
>   }
> )
> ~~~
> {: .r}
>
> 2.
> 
> ~~~
> ddply(
>   .data = gapminder,
>   .variables = "continent",
>   .fun = mean(dataGroup$lifeExp)
> )
> ~~~
> {: .r}
>
> 3.
> 
> ~~~
> ddply(
>   .data = gapminder,
>   .variables = "continent",
>   .fun = function(dataGroup) {
>      mean(dataGroup$lifeExp)
>   }
> )
> ~~~
> {: .r}
>
> 4.
> 
> ~~~
> adply(
>   .data = gapminder,
>   .variables = "continent",
>   .fun = function(dataGroup) {
>      mean(dataGroup$lifeExp)
>   }
> )
> ~~~
> {: .r}
>
{: .challenge}
