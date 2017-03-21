---
title: Functions Explained
teaching: 45
exercises: 15
questions:
- "How can I write a new function in R?"
objectives:
- "Define a function that takes arguments."
- "Return a value from a function."
- "Test a function."
- "Set default values for function arguments."
- "Explain why we should divide programs into small, single-purpose functions."
keypoints:
- "Use `function` to define a new function in R."
- "Use parameters to pass values into functions."
- "Load functions into programs using `source`."
source: Rmd
---




If we only had one data set to analyze, it would probably be faster to load the
file into a spreadsheet and use that to plot simple statistics. However, the
gapminder data is updated periodically, and we may want to pull in that new
information later and re-run our analysis again. We may also obtain similar data
from a different source in the future.

In this lesson, we'll learn how to write a function so that we can repeat
several operations with a single command.

> ## What is a function?
>
> Functions gather a sequence of operations into a whole, preserving it for ongoing use. Functions provide:
>
> * a name we can remember and invoke it by
> * relief from the need to remember the individual operations
> * a defined set of inputs and expected outputs
> * rich connections to the larger programming environment
>
> As the basic building block of most programming languages, user-defined
> functions constitute "programming" as much as any single abstraction can. If
> you have written a function, you are a computer programmer.
{: .callout}

## Defining a function

Let's open a new R script file in the `functions/` directory and call it functions-lesson.R.


~~~
my_sum <- function(a, b) {
  the_sum <- a + b
  return(the_sum) 
}
~~~
{: .r}

Let’s define a function fahr_to_kelvin that converts temperatures from Fahrenheit to Kelvin:


~~~
fahr_to_kelvin <- function(temp) {
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}
~~~
{: .r}

Nós definimos `fahr_to_kelvin` by assigning it to the output of `function`.  The
list of argument names are contained within parentheses.  Next, the
[body]({{ page.root }}/reference/#function-body) of the function--the statements that are
executed when it runs--is contained within curly braces (`{}`).  The statements
in the body are indented by two spaces.  This makes the code easier to read but
does not affect how the code operates.

When we call the function, the values we pass to it as arguments are assigned to those
variables so that we can use them inside the function.  Inside the function, we
use a [return statement]({{ page.root }}/reference/#return-statement) to send a result back
to whoever asked for it.

> ## Tip
>
> One feature unique to R is that the return statement is not required.
> R automatically returns whichever variable is on the last line of the body
> of the function. But for clarity, we will explicitly define the
> return statement.
{: .callout}


Let's try running our function.
Calling our own function is no different from calling any other function:


~~~
# freezing point of water
fahr_to_kelvin(32)
~~~
{: .r}



~~~
[1] 273.15
~~~
{: .output}


~~~
# boiling point of water
fahr_to_kelvin(212)
~~~
{: .r}



~~~
[1] 373.15
~~~
{: .output}

> ## Challenge 1
>
> Write a function called `kelvin_to_celsius` that takes a temperature in Kelvin
> and returns that temperature in Celsius
>
> Hint: To convert from Kelvin to Celsius you subtract 273.15
>
> > ## Solution to challenge 1
> >
> > Write a function called `kelvin_to_celsius` that takes a temperature in Kelvin
> > and returns that temperature in Celsius
> >
> > 
> > ~~~
> > kelvin_to_celsius <- function(temp) {
> >  celsius <- temp - 273.15
> >  return(celsius)
> > }
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}

## Combining functions

The real power of functions comes from mixing, matching and combining them
into ever-larger chunks to get the effect we want.

Let's define two functions that will convert temperature from Fahrenheit to
Kelvin, and Kelvin to Celsius:


~~~
fahr_to_kelvin <- function(temp) {
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}

kelvin_to_celsius <- function(temp) {
  celsius <- temp - 273.15
  return(celsius)
}
~~~
{: .r}

> ## Desafio 2
>
> Defina a função que converte diratamente de Fahrenheit para Celsius, 
> reutilizando as duas funções acima (ou usando as funções que preferir).
>
>
> > ## Solução Desafio 2
> >
> > Vamos definir uma função que calcula o Produto Interno Bruto de uma nação a partir 
>> dos dados disponíveis no nosso conjunto de dados.
> >
> >
> > 
> > ~~~
> > fahr_to_celsius <- function(temp) {
> >   temp_k <- fahr_to_kelvin(temp)
> >   result <- kelvin_to_celsius(temp_k)
> >   return(result)
> > }
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}


We're going to define
a function that calculates the Gross Domestic Product of a nation from the data
available in our dataset:


~~~
# Takes a dataset and multiplies the population column
# with the GDP per capita column.
calcGDP <- function(dat) {
  gdp <- dat$pop * dat$gdpPercap
  return(gdp)
}
~~~
{: .r}

Nós definimos calcGDP ao assinalar a saída da função. A lista dos nomes dos argumentos
está contida dentro dos parênteses. Depois, o corpo da função - as afirmações executadas 
quando você chama a função - está contida dentro das chaves ({}).

Nós dividimos as afirmações dentro do corpo por dois espaços. Isso deixa o código mais
fácil de entender, mas não afeta seu modo de operação.

Quando nós chamamos a função, os valores que passamos a ela são assinalados para os argumentos, 
que se tornam variáveis dentro do corpo da função.

Dentro da função, nós usamos a função return para retornar o resultado. A função de retorno
é opcional: o R vai automaticamente retornar os resultados qualquer que seja o comando executado na última linha da função.



~~~
calcGDP(head(gapminder))
~~~
{: .r}



~~~
[1]  6567086330  7585448670  8758855797  9648014150  9678553274 11697659231
~~~
{: .output}

Isso não é muito informativo. Vamos adicionar mais argumentos para que possamos extrair informações por ano e por país.


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

Se você já está escrevendo essas funções em um script do R separado(uma boa ideia!), 
você pode carregar essas funções para a nossa sessão do R usando a função source.


~~~
source("functions/functions-lesson.R")
~~~
{: .r}

OK, agora tem muitas coisas acontecendo com essa função. Em português claro, 
a função agora subdivide os dados por ano, se o argumento year não estiver vazio, 
depois ele subdivide o resultado por país se o argumento country não estiver vazio.
Após tudo isso, ele calcula o PIB para quaisquer subconjuntos que surgem depois dessas
operações dos dois passos anteriores. A função adiciona o PIB como uma nova coluna 
para os dados subdivididos e retorna isso como o resultado final. Você pode ver que a 
saída final é muito mais informativa que um vetor de números.

Vamos ver o que acontece quando especificamos um ano:


~~~
head(calcGDP(gapminder, year=2007))
~~~
{: .r}



~~~
       country year      pop continent lifeExp  gdpPercap          gdp
12 Afghanistan 2007 31889923      Asia  43.828   974.5803  31079291949
24     Albania 2007  3600523    Europe  76.423  5937.0295  21376411360
36     Algeria 2007 33333216    Africa  72.301  6223.3675 207444851958
48      Angola 2007 12420476    Africa  42.731  4797.2313  59583895818
60   Argentina 2007 40301927  Americas  75.320 12779.3796 515033625357
72   Australia 2007 20434176   Oceania  81.235 34435.3674 703658358894
~~~
{: .output}

Ou, para um país específico:


~~~
calcGDP(gapminder, country="Australia")
~~~
{: .r}



~~~
     country year      pop continent lifeExp gdpPercap          gdp
61 Australia 1952  8691212   Oceania  69.120  10039.60  87256254102
62 Australia 1957  9712569   Oceania  70.330  10949.65 106349227169
63 Australia 1962 10794968   Oceania  70.930  12217.23 131884573002
64 Australia 1967 11872264   Oceania  71.100  14526.12 172457986742
65 Australia 1972 13177000   Oceania  71.930  16788.63 221223770658
66 Australia 1977 14074100   Oceania  73.490  18334.20 258037329175
67 Australia 1982 15184200   Oceania  74.740  19477.01 295742804309
68 Australia 1987 16257249   Oceania  76.320  21888.89 355853119294
69 Australia 1992 17481977   Oceania  77.560  23424.77 409511234952
70 Australia 1997 18565243   Oceania  78.830  26997.94 501223252921
71 Australia 2002 19546792   Oceania  80.370  30687.75 599847158654
72 Australia 2007 20434176   Oceania  81.235  34435.37 703658358894
~~~
{: .output}

Ou os dois:


~~~
calcGDP(gapminder, year=2007, country="Australia")
~~~
{: .r}



~~~
     country year      pop continent lifeExp gdpPercap          gdp
72 Australia 2007 20434176   Oceania  81.235  34435.37 703658358894
~~~
{: .output}

Vamos passar pelo corpo da função:


~~~
calcGDP <- function(dat, year=NULL, country=NULL) {
~~~
{: .r}

Aqui nós adicionamos dois argumentos, year e country. Nós os colocamos como argumentos default
usando a função Null usando o operador = na definição. Isso significa que esses argumentos vão 
pegar esses valores a não ser que o usuário especifique de maneira diferente.


~~~
  if(!is.null(year)) {
    dat <- dat[dat$year %in% year, ]
  }
  if (!is.null(country)) {
    dat <- dat[dat$country %in% country,]
  }
~~~
{: .r}

Aqui, verificamos se cada argumento adicional é definido como nulo, e sempre que eles 
não são nulos irão sobrepor o conjunto de dados armazenado pelo argumento não nulo.
Fazemos isso para que nossa função seja mais flexível para mais tarde. Podemos pedir para calcular o GDP para:

 * Conjunto de dados inteiro;
 * Um único ano;
 * Um único país;
 * Uma única combinação de ano e país.

Ao usar% em%, também podemos dar vários anos ou países a esses argumentos.

> ## Dica: PASSANDO O VALOR
>
> As funções no R, criam ópias para que possam operar dentro do corpo de uma função.
> Quando modificamos o dado dentro da função estamos modificando a cópia do conjunto de dados armazenado, 
> não a variável original que demos como o primeiro argumento.
> Isso é chamado de "pass-by-value" e torna o código de escrita muito mais seguro: você sempre pode ter certeza de que, 
> independentemente das mudanças que fizer no corpo da função, fique dentro do corpo da função.

{: .callout}

> ## Dica: ESCOPO DA FUNÇÃO
>
> Outro conceito importante é o escopo: quaisquer variáveis ou funções que você criar ou modificar dentro do corpo de uma
> função só existem para o tempo de vida da execução da função. Quando chamamos calcGDP, as variáveis dat, gdp e new só 
> existem dentrodo corpo da função. 
> Mesmo que tenhamos variáveis do mesmo nome em nossa sessão R interativa,elas não são modificadas de forma alguma 
> ao executar uma função.
{: .callout}


~~~
  gdp <- dat$pop * dat$gdpPercap
  new <- cbind(dat, gdp=gdp)
  return(new)
}
~~~
{: .r}

Finalmente, calculamos o GDP em nosso novo subconjunto e criamos um novo quadro de dados com essa coluna adicionada. 
Isso significa que quando chamamos a função mais tarde, podemos ver o contexto para os valores retornados do GDP,
o que é muito melhor do que em nossa primeira tentativa onde temos um vetor de números.

> ## DESAFIO 3
>
> Teste sua função do GDP calculando o GDP para Nova Zelândia em 1987. Como este diferem do GDP de Nova Zelândia em 1952?
>
> > ## Solução Desfaio 3
> >
> > GDP da Nova Zelândia em 1987: 65050008703
> >
> > GDP da Nova Zelândia em 1952: 21058193787
> {: .solution}
{: .challenge}


> ## Desafio 4
>
> A função de colar pode ser usada para combinar texto, por exemplo:
>
> 
> ~~~
> best_practice <- c("Write", "programs", "for", "people", "not", "computers")
> paste(best_practice, collapse=" ")
> ~~~
> {: .r}
> 
> 
> 
> ~~~
> [1] "Write programs for people not computers"
> ~~~
> {: .output}
>
>  Escreva uma função chamada fence que leva dois vetores como argumentos, chamados 
> text e wrapper, e devolva o texto com o wrapper:
>
> 
> ~~~
> fence(text=best_practice, wrapper="***")
> ~~~
> {: .r}
>
> *Nota:*a função colar tem um argumento chamado 'sep', que especifica o separador entre texto. 
> O padrão é um espaço: "". O padrão para colar não é "" espaço.

>
> > ## Solução Desafio 4
> >
>>  Escreva uma função chamada fence que leva dois vetores como argumentos, chamados 
>> 'text' e 'wrapper', e devolva o texto com o wrapper:
> >
> > 
> > ~~~
> > fence <- function(text, wrapper){
> >   text <- c(wrapper, text, wrapper)
> >   result <- paste(text, collapse = " ")
> >   return(result)
> > }
> > best_practice <- c("Write", "programs", "for", "people", "not", "computers")
> > fence(text=best_practice, wrapper="***")
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> > [1] "*** Write programs for people not computers ***"
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Dica
>
> R tem alguns aspectos únicos que podem ser explorados ao realizar operações mais complicadas. 
> Não estaremos escrevendo nada que exija o conhecimento desses conceitos mais avançados. 
> No futuro, quando você estiver confortável escrevendo funções em R, você pode aprender mais 
> lendo o R Manual de Linguagem ou o capítulo de programação em R Avançada, 
> por Hadley Wickham. Para contexto, R usa a terminologia "ambientes" em vez de quadros.

{: .callout}

[man]: http://cran.r-project.org/doc/manuals/r-release/R-lang.html#Environment-objects
[chapter]: http://adv-r.had.co.nz/Environments.html
[adv-r]: http://adv-r.had.co.nz/


> ## Dica: Testando e Documentando
>
> É importante testar as funções e documentá-las: a documentação ajuda 
> você e outros a entender qual é a finalidade de sua função e como usá-la,
> e é importante certificar-se de que sua função realmente faça o que você acha que ela faz.

>
> Quando você começar pela primeira vez, seu fluxo de trabalho provavelmente será muito parecido com isto:
>
>  1. Escrever uma função
>  2. Partes do comentário da função para documentar seu comportamento
>  3. Carregar no arquivo de origem
>  4. Experimente para se certificar de que ele se comporta como você espera
>  5. Faça as correções de bugs necessárias
>  6. Salve e Repita

>
> A documentação formal para funções, escrita em arquivos .Rd, é transformada na 
> documentação que você vê nos arquivos de ajuda. O pacote roxygen2 permite que
> os codificadores R escrevam a documentação ao lado do código de função e, em seguida, 
> processe-o nos arquivos .Rd apropriados. Você vai querer mudar para este método mais
> formal de escrever documentação quando você começar a escrever projetos R mais complicados.

>
> Testes automatizados podem ser escritos usando o pacote [testthat][] .
{: .callout}

[roxygen2]: http://cran.r-project.org/web/packages/roxygen2/vignettes/rd.html
[testthat]: http://r-pkgs.had.co.nz/tests.html
