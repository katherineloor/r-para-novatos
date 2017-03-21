---
title: Dataframe Manipulation with dplyr
teaching: 40
exercises: 15
questions:
- "How can I manipulate dataframes without repeating myself?"
objectives:
- " To be able to use the six main dataframe manipulation 'verbs' with pipes in  `dplyr`."
keypoints:
- "Use the `dplyr` package to manipulate dataframes."
- "Use `select()` to choose variables from a dataframe."
- "Use `filter()` to choose data based on values."
- "Use `group_by()` and `summarize()` to work with subsets of data."
- "Use `mutate()` to create new variables."
source: Rmd
---



Manipulação de dataframes significa muitas coisas para diferentes pesquisadores, frequentemente selecionamos determinadas observações (linhas) ou variáveis (colunas), muitas vezes agrupamos os dados por uma determinada variável(s), ou até mesmo calculamos estatística de resume. Podemos fazer essas operações usando operações básicas do R.


~~~
mean(gapminder[gapminder$continent == "Africa", "gdpPercap"])
~~~
{: .r}



~~~
[1] 2193.755
~~~
{: .output}



~~~
mean(gapminder[gapminder$continent == "Americas", "gdpPercap"])
~~~
{: .r}



~~~
[1] 7136.11
~~~
{: .output}



~~~
mean(gapminder[gapminder$continent == "Asia", "gdpPercap"])
~~~
{: .r}



~~~
[1] 7902.15
~~~
{: .output}

Mas isso não é muito desejável porque há algumas repetições. Se repetir irá lhe custar tempo, tanto agora como mais tarde, e potencialmente introduzir alguns erros desagradáveis.

## O pacote `dplyr` 

Felizmente, o pacote [`dplyr`](https://cran.r-project.org/web/packages/dplyr/dplyr.pdf)
oferece muitas funções úteis para manipulação de dataframes de forma a reduzir a repetição acima, a probabilidade de cometer erros e, provavelemente, até mesmo economizar alguma digitação. Como bonus adicional, você pode achar o `dplyr` mais facil de ler.

Aqui vamos abordar seis das funções mais usadas, bem como usar pipes (`%>%`).

1. `select()`
2. `filter()`
3. `group_by()`
4. `summarize()`
5. `mutate()`

Se ainda você não tem este pacote instalado, por favor faça-o:


~~~
install.packages('dplyr')
~~~
{: .r}

Agora vamos carregar o pacote:


~~~
library("dplyr")
~~~
{: .r}

## Usando select()

Se, por exemplo, quiseremos avançar com apenas algumas das variáveis em nosso dataframe, poderiamos usar a função `select()`. Isso manterá apenas as variáveis selecionadas.


~~~
year_country_gdp <- select(gapminder,year,country,gdpPercap)
~~~
{: .r}

![](../fig/13-dplyr-fig1.png)

Se abrirmos `year_country_gdp` veremos que ele contém somente o ano, país e gdpPercap. Acima usamos a linguagem "normal", mas a força do `dplyr` está na combinação de diversas funcões usando pipes. Como a linguagem do pipe é diferente de qualquer outra que já tenhamos visto no R, vamos repetir o que já fizemos acima usando pipes.



~~~
year_country_gdp <- gapminder %>% select(year,country,gdpPercap)
~~~
{: .r}
Para te ajudar a entender porquê que escrevemos isto desta forma, iremos analizá-lo passo-a-passo. Primeiramente nós importamos o dataframe gapminder e o passamos adiante, usando o simbolo do pipe `%>%`, para o próximo passo, que é a função `select()`. Neste caso não especificamos qual elemento dos dados usamos na função `select()` já que a função o obtém do pipe anterior. **Fato interessante**: Existe uma boa chance de já teres encontrado pipes no Shell. Em R, o símbolo do pipe é `%>%` enquanto que no Shell é `|` , mas o conceito é o mesmo!


## Usando filter()

Se agora quizermos avançar com o contéudo acima, mas somente para países europeos, podemos combinar`select` e `filter`.



~~~
year_country_gdp_euro <- gapminder %>%
    filter(continent=="Europe") %>%
    select(year,country,gdpPercap)
~~~
{: .r}

> ## Desafio 1
>
>
> Escreva um único comando (que poderá ter multiplas linhas e incluir pipes) que
> irá produzir um dataframe que possuí os valores africanos para `lifeExp`, `country`
> e `year`, mas não para outros continentes. Quantas linhas tem o seu dataframe
> e porquê?
>
> > ## Solução do desafio 1
> >
> >~~~
> >year_country_lifeExp_Africa <- gapminder %>%
> >                            filter(continent=="Africa") %>%
> >                            select(year,country,lifeExp)
> >~~~
> >{: .r}
> {: .solution}
{: .challenge}

Tal como da última vez, primeiramente passamos o dataframe gapminder para a função `filter()`, depois passamos a versão filtrada do dataframe gapminder para a função `select()`. **Nota:** Neste caso a ordem das operações é muito importante. Se usarmos `select` primeiro, o filtro não será capaz de encontrar a variável Continent já que o teríamos removido no passo anterior.


## Usando group_by() e summarize()

Agora, nós deveríamos estar reduzindo o erro que pode ser gerado pela repetitividade (repetitividade essa originado pelas operações básicas do R), mas até agora não fizemos isso, já que teríamos que repetir o acima para cada continente. Em vez de `filter()`, que se irá passar observações que atendam aos seus critérios (no acima: `continent=="Europe"`), podemos usar `group_by()`, que irá essencialmente usar todos os critérios exclusivos que você poderia ter usado no filtro.



~~~
str(gapminder)
~~~
{: .r}



~~~
'data.frame':	1704 obs. of  6 variables:
 $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
 $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
 $ pop      : num  8425333 9240934 10267083 11537966 13079460 ...
 $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
 $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
 $ gdpPercap: num  779 821 853 836 740 ...
~~~
{: .output}



~~~
str(gapminder %>% group_by(continent))
~~~
{: .r}



~~~
Classes 'grouped_df', 'tbl_df', 'tbl' and 'data.frame':	1704 obs. of  6 variables:
 $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
 $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
 $ pop      : num  8425333 9240934 10267083 11537966 13079460 ...
 $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
 $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
 $ gdpPercap: num  779 821 853 836 740 ...
 - attr(*, "vars")=List of 1
  ..$ : symbol continent
 - attr(*, "drop")= logi TRUE
 - attr(*, "indices")=List of 5
  ..$ : int  24 25 26 27 28 29 30 31 32 33 ...
  ..$ : int  48 49 50 51 52 53 54 55 56 57 ...
  ..$ : int  0 1 2 3 4 5 6 7 8 9 ...
  ..$ : int  12 13 14 15 16 17 18 19 20 21 ...
  ..$ : int  60 61 62 63 64 65 66 67 68 69 ...
 - attr(*, "group_sizes")= int  624 300 396 360 24
 - attr(*, "biggest_group_size")= int 624
 - attr(*, "labels")='data.frame':	5 obs. of  1 variable:
  ..$ continent: Factor w/ 5 levels "Africa","Americas",..: 1 2 3 4 5
  ..- attr(*, "vars")=List of 1
  .. ..$ : symbol continent
  ..- attr(*, "drop")= logi TRUE
~~~
{: .output}
Você vai notar que a estrutura do dataframe onde usamos `group_by()` (`grouped_df`) não o mesmo que o`gapminder` (`data.frame`) original. Um `grouped_df` pode ser entendido como uma `list` onde cada item na `list` é um `data.frame` que contém somente a linha que corresponde a um valor particular 'continent' (pelo menos no exemplo acima).


![](../fig/13-dplyr-fig2.png)

## Usando summarize()

O caso acima possui algumas desvantágens sendo que o uso de `group_by()` será muito mais interessante em conjunto com `summarize()`. Isso vai permitir o usuário criar nova(s) variáveis usando funções que repetem para cada um dos dataframes específicos para cada continente. Isso quer dizer que, usando a função `group_by()`, dividimos o nosso dataframe original em várias partes, depois podemos rodar funções (ex: `mean()` ou `sd()`) dentro do `summarize()`.



~~~
gdp_bycontinents <- gapminder %>%
    group_by(continent) %>%
    summarize(mean_gdpPercap=mean(gdpPercap))
~~~
{: .r}

![](../fig/13-dplyr-fig3.png)

Que nos permite calcular a média gdpPercap para cada continente, mas isso fica ainda melhor.

> ## Desafio 2
>
>
> Calcule a expetativa de vida média por país.
> Qual tem a maior expetativa de vida média e qual tem a menor expetativa de vida média?
>
> > ## Solução do desafio 2
> >
> >~~~
> >lifeExp_bycountry <- gapminder %>%
> >    group_by(country) %>%
> >    summarize(mean_lifeExp=mean(lifeExp))
> >lifeExp_bycountry %>% 
> >    filter(mean_lifeExp == min(mean_lifeExp) | mean_lifeExp == max(mean_lifeExp))
> >~~~
> >{: .r}
> >
> >
> >
> >~~~
> ># A tibble: 2 × 2
> >       country mean_lifeExp
> >        <fctr>        <dbl>
> >1      Iceland     76.51142
> >2 Sierra Leone     36.76917
> >~~~
> >{: .output}
> Outra forma de fazer isto é usar a função `dplyr` `arrange()`, que organiza as linhas num
> data frame de acordo com a ordem de uma ou mais  variáveis do dataframe. A função `arrange()`
> possui uma syntax similar a outras funções do paote `dplyr`. Você pode usar `desc()` dentro 
> de  `arrange()` para listar em ordem descendente.
> >
> >~~~
> >lifeExp_bycountry %>%
> >    arrange(mean_lifeExp) %>%
> >    head(1)
> >~~~
> >{: .r}
> >
> >
> >
> >~~~
> ># A tibble: 1 × 2
> >       country mean_lifeExp
> >        <fctr>        <dbl>
> >1 Sierra Leone     36.76917
> >~~~
> >{: .output}
> >
> >
> >
> >~~~
> >lifeExp_bycountry %>%
> >    arrange(desc(mean_lifeExp)) %>%
> >    head(1)
> >~~~
> >{: .r}
> >
> >
> >
> >~~~
> ># A tibble: 1 × 2
> >  country mean_lifeExp
> >   <fctr>        <dbl>
> >1 Iceland     76.51142
> >~~~
> >{: .output}
> {: .solution}
{: .challenge}

A funçãoo `group_by()` nos permite agrupar por múltiplas variáveis. Vamos agrupar por `year` e `continent`.



~~~
gdp_bycontinents_byyear <- gapminder %>%
    group_by(continent,year) %>%
    summarize(mean_gdpPercap=mean(gdpPercap))
~~~
{: .r}

Isso já é bastante poderoso, mas fica ainda melhor! Você não está limitado a definir somente uma nova variável no `summarize()`.


~~~
gdp_pop_bycontinents_byyear <- gapminder %>%
    group_by(continent,year) %>%
    summarize(mean_gdpPercap=mean(gdpPercap),
              sd_gdpPercap=sd(gdpPercap),
              mean_pop=mean(pop),
              sd_pop=sd(pop))
~~~
{: .r}

## Usando mutate()

Também podemos criar novas variáveis antes de (ou mesmo depois) resumir informações usando `mutate()`.


~~~
gdp_pop_bycontinents_byyear <- gapminder %>%
    mutate(gdp_billion=gdpPercap*pop/10^9) %>%
    group_by(continent,year) %>%
    summarize(mean_gdpPercap=mean(gdpPercap),
              sd_gdpPercap=sd(gdpPercap),
              mean_pop=mean(pop),
              sd_pop=sd(pop),
              mean_gdp_billion=mean(gdp_billion),
              sd_gdp_billion=sd(gdp_billion))
~~~
{: .r}

## Combinando os pacotes `dplyr` e `ggplot2`

Na lição sobre plotagem de gráficos, vimos como criar uma figura `multi-panel` (painel múltiplo), adicionando uma camada de painéis `facet` usando o pacote `ggplot2`. Aqui está o código que usamos (com alguns comentários extras):



~~~
# Obtendo a letra inicial de cada país
starts.with <- substr(gapminder$country, start = 1, stop = 1)
# Filtrando os países que começam com letra "A" ou "Z"
az.countries <- gapminder[starts.with %in% c("A", "Z"), ]
# Construindo o gráfico
ggplot(data = az.countries, aes(x = year, y = lifeExp, color = continent)) +
  geom_line() + facet_wrap( ~ country)
~~~
{: .r}

<img src="../fig/rmd-13-unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" style="display: block; margin: auto;" />
Esse código cria o gráfico certo, porém ele também cria algumas variáveis (`starts.with` e `az.countries`), que talvez nem as utilizaremos mais. Da mesma forma que usamos `%>%` para passar dados ao longo das funções `dplyr`, podemos utilizá-lo também para passar dados para `ggplot()`. Como `%>%` substitui o primeiro argumento em uma função, não precisamos especificar o argumento `data=` na função `ggplot()`. Combinando as funções dos pacotes `dplyr` e `ggplot2` podemos fazer a mesma figura sem criar novas variáveis ou modificar os dados.



~~~
gapminder %>% 
   # Obtendo a letra inicial de cada país
   mutate(startsWith = substr(country, start = 1, stop = 1)) %>% 
   # Filtrando os países que começam com letra "A" ou "Z"
   filter(startsWith %in% c("A", "Z")) %>%
  # Construindo o gráfico
   ggplot(aes(x = year, y = lifeExp, color = continent)) + 
   geom_line() + 
   facet_wrap( ~ country)
~~~
{: .r}

<img src="../fig/rmd-13-unnamed-chunk-17-1.png" title="plot of chunk unnamed-chunk-17" alt="plot of chunk unnamed-chunk-17" style="display: block; margin: auto;" />

As funções do `dplyr` também nos ajudam a simplificar as coisas, por exemplo, podemos combinar os dois primeiros passos:


~~~
gapminder %>%
     # Filtrando os países que começam com letra "A" ou "Z"
	filter(substr(country, start = 1, stop = 1) %in% c("A", "Z")) %>%
	# Construindo o gráfico
	ggplot(aes(x = year, y = lifeExp, color = continent)) + 
	geom_line() + 
	facet_wrap( ~ country)
~~~
{: .r}

<img src="../fig/rmd-13-unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

> ## Desafio
>
>
> Para cada continente, calcule a expectativa de vida média em 2002 de 2 países
> selecionados aleatoriamente. Em seguida, organize os nomes dos continentes em
> ordem inversa. **Dica:** Use as funções do `dplyr` `arrange()` e `sample_n()`,
> elas têm sintaxe similar a outras funções do `dplyr`.
>
> > ## Solução do desafio
> >
> >~~~
> >lifeExp_2countries_bycontinents <- gapminder %>%
> >    filter(year==2002) %>%
> >    group_by(continent) %>%
> >    sample_n(2) %>%
> >    summarize(mean_lifeExp=mean(lifeExp)) %>%
> >    arrange(desc(mean_lifeExp))
> >~~~
> >{: .r}
> {: .solution}
{: .challenge}

## Outros grandes recursos

* [Arquivo de consulta - manipulação de dados](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)
* [Introdução ao dplyr](https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)
* [Manipulação de dados com R e RStudio](https://www.rstudio.com/resources/webinars/data-wrangling-with-r-and-rstudio/)
