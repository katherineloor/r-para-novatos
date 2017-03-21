---
title: Dataframe Manipulation with tidyr
teaching: 30
exercises: 15
questions:
- "How can I change the format of dataframes?"
objectives:
- "To be understand the concepts of ‘long’ and ‘wide’ data formats and be able to convert between them with `tidyr`."
keypoints:
- "Use the `tidyr` package to change the layout of dataframes."
- "Use `gather()` to go from wide to long format."
- "Use `scatter()` to go from long to wide format."
source: Rmd
---



Pesquisadores muitas vezes querem alterar seus dados do formato 'wide' para 'long', ou vice-versa. O formato 'long' é onde:

 - Cada coluna é uma variável
 - Cada linha é uma observação

No formato 'long', normalmente você tem uma coluna para a variável observada e as outras colunas são variáveis  ID.


No formato 'wide' cada linha é geralmente um local / assunto / paciente e você tem variáveis de observações múltiplas contendo o mesmo tipo de dados. Estas podem ser observações repetidas ao longo do tempo, ou observações de múltiplas variáveis (ou uma mistura de ambos). Você pode achar que a entrada de dados pode ser mais simples ou algumas outras aplicações podem preferir o formato 'wide'. Entretanto, muitas das funções do R foram projetadas assumindo que você tem dados de formato 'long'. Este tutorial irá ajudá-lo a transformar seus dados de forma eficiente, independentemente do formato original.

![](../fig/14-tidyr-fig1.png)

Esses formatos de dados influenciam principalmente a legibilidade. Para nós seres humanos, o formato 'wide' é, em geral, mais intuitivo, já que na maioria das vezes podemos visualizar mais dados na tela devido à sua forma. No entanto, o formato 'long' é mais fácilmente lido pela máquina e está mais próximo da formatação de bases de dados. As variáveis ID em nossos dataframes são semelhantes aos campos em um banco de dados e as variáveis observadas são como os valores do banco de dados.

## Guia de Introdução

Primeiro instale os pacotes caso você não tenha feito isso antes. (você provavelmente já terá instalado o dplyr na lição anterior):


~~~
#install.packages("tidyr")
#install.packages("dplyr")
~~~
{: .r}

Carregue os pacotes


~~~
library("tidyr")
~~~
{: .r}



~~~
Warning: package 'tidyr' was built under R version 3.3.2
~~~
{: .error}



~~~
library("dplyr")
~~~
{: .r}

Primeiro, vamos olhar a estrutura de nosso dataframe original:


~~~
str(gapminder)
~~~
{: .r}



~~~
'data.frame':	1704 obs. of  6 variables:
 $ country  : chr  "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
 $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
 $ pop      : num  8425333 9240934 10267083 11537966 13079460 ...
 $ continent: chr  "Asia" "Asia" "Asia" "Asia" ...
 $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
 $ gdpPercap: num  779 821 853 836 740 ...
~~~
{: .output}

> ## Desafio 1
>
> O gapminder está em um formato puramente 'long', puramente 'wide', ou algum formato >intermediário?
>
> > ## Solução do Desafio 1
> >
> > O nosso data.frame original está em um formato intermediário. Não está no
> > formato puramente 'long', já que possui variáveis com múltiplas observações
> > (`pop`,`lifeExp`,`gdpPercap`).
> {: .solution}
{: .challenge}


Algumas vezes, como no conjunto de dados 'gapminder', temos múltiplos tipos de dados observados. São dados que estão em um formato entre puramente 'long' e puramente 'wide'. Temos 3 variáveis 'ID' (`continent`, `country`, `year`) e 3 variáveis de observação (`pop`,`lifeExp`,`gdpPercap`). Eu usualmente prefiro meus dados neste formato intermediário na maioria dos casos, apesar de não ter todas observações em 1 coluna, dado que todas as 3 variáveis de observação têm unidades diferentes. Existem algumas operações que nós precisaríamos para alongar um pouco mais este dataframe.(ou seja, 4 variáveis ID e 1 variável de observação.)

Ao usar muitas funções no R, que geralmente são baseadas em vetores, normalmente você quer evitar fazer operações matemáticas com valores em unidades diferentes. Por exemplo, usando o formato puramente 'long', uma única média para todos os valores de população, expectativa de vida e PIB*(GDP)* não teria muito sentido, pois retornaria a média de valores com 3 unidades incompatíveis. A solução é que primeiro manipulamos os dados, seja agrupando-os (veja a lição sobre o *dplyr*), ou mudando a estrutura do dataframe. **Nota:** Algumas funções de plotagem em R na verdade funcionam melhor nos dados em formato do tipo 'wide'.

## Mudando de formato 'wide' para 'long' utilizando o gather()

Até agora, estamos usando o conjunto original de dados 'gapminder', que está bem formatado. Mas os dados "reais" (ou seja, nossos próprios dados de pesquisa) nunca estarão tão bem organizados. Vamos começar aqui com a versão do conjunto de dados 'gapminder' no formato 'wide'.

Vamos carregar o arquivo dos dados e olhar para ele. Nota: não queremos que as colunas continente*('continent')* e país*('country')* sejam do tipo fator, portanto usamos o argumento *'stringsAsFactors'* para desabilitar isso no *'read.csv ()'* 


~~~
gap_wide <- read.csv("https://raw.githubusercontent.com/estatcomp/carpentrybr/master/gapminder_wide.csv" , stringsAsFactors = F   )
str(gap_wide)
~~~
{: .r}



~~~
'data.frame':	142 obs. of  38 variables:
 $ continent     : chr  "Africa" "Africa" "Africa" "Africa" ...
 $ country       : chr  "Algeria" "Angola" "Benin" "Botswana" ...
 $ gdpPercap_1952: num  2449 3521 1063 851 543 ...
 $ gdpPercap_1957: num  3014 3828 960 918 617 ...
 $ gdpPercap_1962: num  2551 4269 949 984 723 ...
 $ gdpPercap_1967: num  3247 5523 1036 1215 795 ...
 $ gdpPercap_1972: num  4183 5473 1086 2264 855 ...
 $ gdpPercap_1977: num  4910 3009 1029 3215 743 ...
 $ gdpPercap_1982: num  5745 2757 1278 4551 807 ...
 $ gdpPercap_1987: num  5681 2430 1226 6206 912 ...
 $ gdpPercap_1992: num  5023 2628 1191 7954 932 ...
 $ gdpPercap_1997: num  4797 2277 1233 8647 946 ...
 $ gdpPercap_2002: num  5288 2773 1373 11004 1038 ...
 $ gdpPercap_2007: num  6223 4797 1441 12570 1217 ...
 $ lifeExp_1952  : num  43.1 30 38.2 47.6 32 ...
 $ lifeExp_1957  : num  45.7 32 40.4 49.6 34.9 ...
 $ lifeExp_1962  : num  48.3 34 42.6 51.5 37.8 ...
 $ lifeExp_1967  : num  51.4 36 44.9 53.3 40.7 ...
 $ lifeExp_1972  : num  54.5 37.9 47 56 43.6 ...
 $ lifeExp_1977  : num  58 39.5 49.2 59.3 46.1 ...
 $ lifeExp_1982  : num  61.4 39.9 50.9 61.5 48.1 ...
 $ lifeExp_1987  : num  65.8 39.9 52.3 63.6 49.6 ...
 $ lifeExp_1992  : num  67.7 40.6 53.9 62.7 50.3 ...
 $ lifeExp_1997  : num  69.2 41 54.8 52.6 50.3 ...
 $ lifeExp_2002  : num  71 41 54.4 46.6 50.6 ...
 $ lifeExp_2007  : num  72.3 42.7 56.7 50.7 52.3 ...
 $ pop_1952      : num  9279525 4232095 1738315 442308 4469979 ...
 $ pop_1957      : num  10270856 4561361 1925173 474639 4713416 ...
 $ pop_1962      : num  11000948 4826015 2151895 512764 4919632 ...
 $ pop_1967      : num  12760499 5247469 2427334 553541 5127935 ...
 $ pop_1972      : num  14760787 5894858 2761407 619351 5433886 ...
 $ pop_1977      : num  17152804 6162675 3168267 781472 5889574 ...
 $ pop_1982      : num  20033753 7016384 3641603 970347 6634596 ...
 $ pop_1987      : num  23254956 7874230 4243788 1151184 7586551 ...
 $ pop_1992      : num  26298373 8735988 4981671 1342614 8878303 ...
 $ pop_1997      : num  29072015 9875024 6066080 1536536 10352843 ...
 $ pop_2002      : int  31287142 10866106 7026113 1630347 12251209 7021078 15929988 4048013 8835739 614382 ...
 $ pop_2007      : int  33333216 12420476 8078314 1639131 14326203 8390505 17696293 4369038 10238807 710960 ...
~~~
{: .output}

![](../fig/14-tidyr-fig2.png)

O primeiro passo para obter os dados em um bom formato intermediário é converter primeiro do formato *wide* para o formato *long*. A função `gather()` do `tidyr` irá 'reunir' suas variáveis de observação em uma única variável.



~~~
gap_long <- gap_wide %>%
    gather(obstype_year, obs_values, starts_with('pop'),
           starts_with('lifeExp'), starts_with('gdpPercap'))
str(gap_long)
~~~
{: .r}



~~~
'data.frame':	5112 obs. of  4 variables:
 $ continent   : chr  "Africa" "Africa" "Africa" "Africa" ...
 $ country     : chr  "Algeria" "Angola" "Benin" "Botswana" ...
 $ obstype_year: chr  "pop_1952" "pop_1952" "pop_1952" "pop_1952" ...
 $ obs_values  : num  9279525 4232095 1738315 442308 4469979 ...
~~~
{: .output}

Aqui usamos sintaxe de encadeamento que é semelhante ao que estávamos fazendo na lição anterior com *dplyr*. Na verdade, eles são compatíveis e você pode usar uma mistura de funções do *tidyr* e *dplyr*  encadeando eles juntos.

Dentro do `gather()`, primeiro nomeamos a nova coluna para a nova variável ID (`obstype_year`), o nome da nova variável de observação adicionada (`obs_value`) e, em seguida, os nomes das variáveis de observação antigas. Poderíamos ter digitado todas as variáveis de observação, mas como na função `select()` (ver a lição do `dplyr`), podemos usar o argumento `starts_with()` para selecionar todas as variáveis que começam com a string de caracteres desejada. O Gather também permite a sintaxe alternativa de usar o símbolo - para identificar quais variáveis não devem ser coletadas (isto é, variáveis ID).

![](../fig/14-tidyr-fig3.png)


~~~
gap_long <- gap_wide %>% gather(obstype_year,obs_values,-continent,-country)
str(gap_long)
~~~
{: .r}



~~~
'data.frame':	5112 obs. of  4 variables:
 $ continent   : chr  "Africa" "Africa" "Africa" "Africa" ...
 $ country     : chr  "Algeria" "Angola" "Benin" "Botswana" ...
 $ obstype_year: chr  "gdpPercap_1952" "gdpPercap_1952" "gdpPercap_1952" "gdpPercap_1952" ...
 $ obs_values  : num  2449 3521 1063 851 543 ...
~~~
{: .output}

Isso pode parecer trivial com este dataframe em particular, mas às vezes você tem 1 variável ID e 40 variáveis de Observação com nomes de variáveis irregulares. A flexibilidade nos faz ganhar muito tempo!

Agora, `obstype_year` na verdade contém duas informações, o tipo de observação (`pop`,`lifeExp`, or `gdpPercap`) e o ano`year`. Podemos usar a função `separate()` para dividir as seqüências de caracteres em múltiplas variáveis.



~~~
gap_long <- gap_long %>% separate(obstype_year,into=c('obs_type','year'),sep="_")
gap_long$year <- as.integer(gap_long$year)
~~~
{: .r}


> ## Desafio 2
>
> Usando `gap_long`, calcular a expectativa de vida média, população e PIB per capita para cada continente. :
>**Sugestão:** use as funções `group_by()` e `summarize()` que aprendemos na lição do `dplyr`
>
> > ## Solução do Desafio 2
> >
> >~~~
> >gap_long %>% group_by(continent,obs_type) %>%
> >    summarize(means=mean(obs_values))
> >~~~
> >{: .r}
> >
> >
> >
> >~~~
> >Source: local data frame [15 x 3]
> >Groups: continent [?]
> >
> >   continent  obs_type        means
> >       <chr>     <chr>        <dbl>
> >1     Africa gdpPercap 2.193755e+03
> >2     Africa   lifeExp 4.886533e+01
> >3     Africa       pop 9.916003e+06
> >4   Americas gdpPercap 7.136110e+03
> >5   Americas   lifeExp 6.465874e+01
> >6   Americas       pop 2.450479e+07
> >7       Asia gdpPercap 7.902150e+03
> >8       Asia   lifeExp 6.006490e+01
> >9       Asia       pop 7.703872e+07
> >10    Europe gdpPercap 1.446948e+04
> >11    Europe   lifeExp 7.190369e+01
> >12    Europe       pop 1.716976e+07
> >13   Oceania gdpPercap 1.862161e+04
> >14   Oceania   lifeExp 7.432621e+01
> >15   Oceania       pop 8.874672e+06
> >~~~
> >{: .output}
> {: .solution}
{: .challenge}

## Mudando do formato 'long' para um intermediário utilizando o spread()

É sempre bom verificar o trabalho. Então, vamos usar o oposto do `gather()` para espalhar nossas variáveis de Observação de volta com o chamado `spread()`. Podemos então espalhar nosso `gap_long()` para o formato intermediário original ou o formato mais amplo. Vamos começar com o formato intermediário.



~~~
gap_normal <- gap_long %>% spread(obs_type,obs_values)
dim(gap_normal)
~~~
{: .r}



~~~
[1] 1704    6
~~~
{: .output}



~~~
dim(gapminder)
~~~
{: .r}



~~~
[1] 1704    6
~~~
{: .output}



~~~
names(gap_normal)
~~~
{: .r}



~~~
[1] "continent" "country"   "year"      "gdpPercap" "lifeExp"   "pop"      
~~~
{: .output}



~~~
names(gapminder)
~~~
{: .r}



~~~
[1] "country"   "year"      "pop"       "continent" "lifeExp"   "gdpPercap"
~~~
{: .output}

Agora temos um dataframe `gap_normal` intermediário com as mesmas dimensões do original `gapminder`, mas a ordem das variáveis é diferente. Vamos corrigir isso antes de verificar se eles são todos iguais `all.equal()`.



~~~
gap_normal <- gap_normal[,names(gapminder)]
all.equal(gap_normal,gapminder)
~~~
{: .r}



~~~
[1] "Component \"country\": 1704 string mismatches"              
[2] "Component \"pop\": Mean relative difference: 1.634504"      
[3] "Component \"continent\": 1212 string mismatches"            
[4] "Component \"lifeExp\": Mean relative difference: 0.203822"  
[5] "Component \"gdpPercap\": Mean relative difference: 1.162302"
~~~
{: .output}



~~~
head(gap_normal)
~~~
{: .r}



~~~
  country year      pop continent lifeExp gdpPercap
1 Algeria 1952  9279525    Africa  43.077  2449.008
2 Algeria 1957 10270856    Africa  45.685  3013.976
3 Algeria 1962 11000948    Africa  48.303  2550.817
4 Algeria 1967 12760499    Africa  51.407  3246.992
5 Algeria 1972 14760787    Africa  54.518  4182.664
6 Algeria 1977 17152804    Africa  58.014  4910.417
~~~
{: .output}



~~~
head(gapminder)
~~~
{: .r}



~~~
      country year      pop continent lifeExp gdpPercap
1 Afghanistan 1952  8425333      Asia  28.801  779.4453
2 Afghanistan 1957  9240934      Asia  30.332  820.8530
3 Afghanistan 1962 10267083      Asia  31.997  853.1007
4 Afghanistan 1967 11537966      Asia  34.020  836.1971
5 Afghanistan 1972 13079460      Asia  36.088  739.9811
6 Afghanistan 1977 14880372      Asia  38.438  786.1134
~~~
{: .output}

Estamos quase lá, o original foi ordenado por país, continente, e depois por ano.


~~~
gap_normal <- gap_normal %>% arrange(country,continent,year)
all.equal(gap_normal,gapminder)
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}

Ótimo! Passamos do formato mais longo de volta ao intermediário e não introduzimos erros no nosso código.

Agora vamos converter o formato *'long'*  de volta para o formato *'wide'*. No formato *'wide'*, manteremos país e continente como variáveis ID e espalharemos as observações através das 3 medidas (`pop`,`lifeExp`,`gdpPercap`) e tempo (`year`). Primeiro, precisamos criar nomes apropriados para todas as novas variáveis e também precisamos unificar nossas variáveis ID para simplificar o processo para definir o `gap_wide`.


~~~
gap_temp <- gap_long %>% unite(var_ID,continent,country,sep="_")
str(gap_temp)
~~~
{: .r}



~~~
'data.frame':	5112 obs. of  4 variables:
 $ var_ID    : chr  "Africa_Algeria" "Africa_Angola" "Africa_Benin" "Africa_Botswana" ...
 $ obs_type  : chr  "gdpPercap" "gdpPercap" "gdpPercap" "gdpPercap" ...
 $ year      : int  1952 1952 1952 1952 1952 1952 1952 1952 1952 1952 ...
 $ obs_values: num  2449 3521 1063 851 543 ...
~~~
{: .output}



~~~
gap_temp <- gap_long %>%
    unite(ID_var,continent,country,sep="_") %>%
    unite(var_names,obs_type,year,sep="_")
str(gap_temp)
~~~
{: .r}



~~~
'data.frame':	5112 obs. of  3 variables:
 $ ID_var    : chr  "Africa_Algeria" "Africa_Angola" "Africa_Benin" "Africa_Botswana" ...
 $ var_names : chr  "gdpPercap_1952" "gdpPercap_1952" "gdpPercap_1952" "gdpPercap_1952" ...
 $ obs_values: num  2449 3521 1063 851 543 ...
~~~
{: .output}

Usando `unite()` agora temos uma única variável ID que é uma combinação de continente, país e temos os nomes das variáveis definidas. Agora estamos prontos para encadear isso utilizando o `spread()`.


~~~
gap_wide_new <- gap_long %>%
    unite(ID_var,continent,country,sep="_") %>%
    unite(var_names,obs_type,year,sep="_") %>%
    spread(var_names,obs_values)
str(gap_wide_new)
~~~
{: .r}



~~~
'data.frame':	142 obs. of  37 variables:
 $ ID_var        : chr  "Africa_Algeria" "Africa_Angola" "Africa_Benin" "Africa_Botswana" ...
 $ gdpPercap_1952: num  2449 3521 1063 851 543 ...
 $ gdpPercap_1957: num  3014 3828 960 918 617 ...
 $ gdpPercap_1962: num  2551 4269 949 984 723 ...
 $ gdpPercap_1967: num  3247 5523 1036 1215 795 ...
 $ gdpPercap_1972: num  4183 5473 1086 2264 855 ...
 $ gdpPercap_1977: num  4910 3009 1029 3215 743 ...
 $ gdpPercap_1982: num  5745 2757 1278 4551 807 ...
 $ gdpPercap_1987: num  5681 2430 1226 6206 912 ...
 $ gdpPercap_1992: num  5023 2628 1191 7954 932 ...
 $ gdpPercap_1997: num  4797 2277 1233 8647 946 ...
 $ gdpPercap_2002: num  5288 2773 1373 11004 1038 ...
 $ gdpPercap_2007: num  6223 4797 1441 12570 1217 ...
 $ lifeExp_1952  : num  43.1 30 38.2 47.6 32 ...
 $ lifeExp_1957  : num  45.7 32 40.4 49.6 34.9 ...
 $ lifeExp_1962  : num  48.3 34 42.6 51.5 37.8 ...
 $ lifeExp_1967  : num  51.4 36 44.9 53.3 40.7 ...
 $ lifeExp_1972  : num  54.5 37.9 47 56 43.6 ...
 $ lifeExp_1977  : num  58 39.5 49.2 59.3 46.1 ...
 $ lifeExp_1982  : num  61.4 39.9 50.9 61.5 48.1 ...
 $ lifeExp_1987  : num  65.8 39.9 52.3 63.6 49.6 ...
 $ lifeExp_1992  : num  67.7 40.6 53.9 62.7 50.3 ...
 $ lifeExp_1997  : num  69.2 41 54.8 52.6 50.3 ...
 $ lifeExp_2002  : num  71 41 54.4 46.6 50.6 ...
 $ lifeExp_2007  : num  72.3 42.7 56.7 50.7 52.3 ...
 $ pop_1952      : num  9279525 4232095 1738315 442308 4469979 ...
 $ pop_1957      : num  10270856 4561361 1925173 474639 4713416 ...
 $ pop_1962      : num  11000948 4826015 2151895 512764 4919632 ...
 $ pop_1967      : num  12760499 5247469 2427334 553541 5127935 ...
 $ pop_1972      : num  14760787 5894858 2761407 619351 5433886 ...
 $ pop_1977      : num  17152804 6162675 3168267 781472 5889574 ...
 $ pop_1982      : num  20033753 7016384 3641603 970347 6634596 ...
 $ pop_1987      : num  23254956 7874230 4243788 1151184 7586551 ...
 $ pop_1992      : num  26298373 8735988 4981671 1342614 8878303 ...
 $ pop_1997      : num  29072015 9875024 6066080 1536536 10352843 ...
 $ pop_2002      : num  31287142 10866106 7026113 1630347 12251209 ...
 $ pop_2007      : num  33333216 12420476 8078314 1639131 14326203 ...
~~~
{: .output}

> ## Desafio 3
>
> Tome um passo adiante e crie um formato de dados, chamado `gap_ludicrously_wide` .Faça isso espalhando os dados em países, ano e as 3 medidas.
>**Dica** este novo dataframe deve ter apenas 5 linhas.
>
> > ## Solução do Desafio 3
> >
> >~~~
> >gap_ludicrously_wide <- gap_long %>%
> >    unite(var_names,obs_type,year,country,sep="_") %>%
> >    spread(var_names,obs_values)
> >~~~
> >{: .r}
> {: .solution}
{: .challenge}

Agora temos um excelente dataframe no formato *'wide'*, mas o `ID_var` pode ser mais útil. Vamos então separá-lo em duas variáveis utilizando o `separate()`.


~~~
gap_wide_betterID <- separate(gap_wide_new,ID_var,c("continent","country"),sep="_")
gap_wide_betterID <- gap_long %>%
    unite(ID_var, continent,country,sep="_") %>%
    unite(var_names, obs_type,year,sep="_") %>%
    spread(var_names, obs_values) %>%
    separate(ID_var, c("continent","country"),sep="_")
str(gap_wide_betterID)
~~~
{: .r}



~~~
'data.frame':	142 obs. of  38 variables:
 $ continent     : chr  "Africa" "Africa" "Africa" "Africa" ...
 $ country       : chr  "Algeria" "Angola" "Benin" "Botswana" ...
 $ gdpPercap_1952: num  2449 3521 1063 851 543 ...
 $ gdpPercap_1957: num  3014 3828 960 918 617 ...
 $ gdpPercap_1962: num  2551 4269 949 984 723 ...
 $ gdpPercap_1967: num  3247 5523 1036 1215 795 ...
 $ gdpPercap_1972: num  4183 5473 1086 2264 855 ...
 $ gdpPercap_1977: num  4910 3009 1029 3215 743 ...
 $ gdpPercap_1982: num  5745 2757 1278 4551 807 ...
 $ gdpPercap_1987: num  5681 2430 1226 6206 912 ...
 $ gdpPercap_1992: num  5023 2628 1191 7954 932 ...
 $ gdpPercap_1997: num  4797 2277 1233 8647 946 ...
 $ gdpPercap_2002: num  5288 2773 1373 11004 1038 ...
 $ gdpPercap_2007: num  6223 4797 1441 12570 1217 ...
 $ lifeExp_1952  : num  43.1 30 38.2 47.6 32 ...
 $ lifeExp_1957  : num  45.7 32 40.4 49.6 34.9 ...
 $ lifeExp_1962  : num  48.3 34 42.6 51.5 37.8 ...
 $ lifeExp_1967  : num  51.4 36 44.9 53.3 40.7 ...
 $ lifeExp_1972  : num  54.5 37.9 47 56 43.6 ...
 $ lifeExp_1977  : num  58 39.5 49.2 59.3 46.1 ...
 $ lifeExp_1982  : num  61.4 39.9 50.9 61.5 48.1 ...
 $ lifeExp_1987  : num  65.8 39.9 52.3 63.6 49.6 ...
 $ lifeExp_1992  : num  67.7 40.6 53.9 62.7 50.3 ...
 $ lifeExp_1997  : num  69.2 41 54.8 52.6 50.3 ...
 $ lifeExp_2002  : num  71 41 54.4 46.6 50.6 ...
 $ lifeExp_2007  : num  72.3 42.7 56.7 50.7 52.3 ...
 $ pop_1952      : num  9279525 4232095 1738315 442308 4469979 ...
 $ pop_1957      : num  10270856 4561361 1925173 474639 4713416 ...
 $ pop_1962      : num  11000948 4826015 2151895 512764 4919632 ...
 $ pop_1967      : num  12760499 5247469 2427334 553541 5127935 ...
 $ pop_1972      : num  14760787 5894858 2761407 619351 5433886 ...
 $ pop_1977      : num  17152804 6162675 3168267 781472 5889574 ...
 $ pop_1982      : num  20033753 7016384 3641603 970347 6634596 ...
 $ pop_1987      : num  23254956 7874230 4243788 1151184 7586551 ...
 $ pop_1992      : num  26298373 8735988 4981671 1342614 8878303 ...
 $ pop_1997      : num  29072015 9875024 6066080 1536536 10352843 ...
 $ pop_2002      : num  31287142 10866106 7026113 1630347 12251209 ...
 $ pop_2007      : num  33333216 12420476 8078314 1639131 14326203 ...
~~~
{: .output}



~~~
all.equal(gap_wide, gap_wide_betterID)
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}

Pronto e de volta outra vez!


## Outros excelentes recursos

* [R para Data Science](r4ds.had.co.nz)
* [Arquivo de Consulta-Manipulação de Dados](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)
* [Introdução ao tidyr](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html)
* [Manipulação de Dados no R e RStudio](https://www.rstudio.com/resources/webinars/data-wrangling-with-r-and-rstudio/)
