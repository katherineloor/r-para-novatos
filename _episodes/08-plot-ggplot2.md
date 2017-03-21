---
title: Creating Publication-Quality Graphics
teaching: 60
exercises: 20
questions:
- "Como posso criar gráficos de qualidade para publicação em R?"
objectives:
- "Aprender a usar o ggplot2 para gerar gráficos de qualidade para publicação."
- "Para entender a gramática básica de gráficos, incluindo as camadas de estética e de geometria, adicionando estatísticas, usando transformações de escala, dividindo com cor ou painéis por grupos."
keypoints:
- "Usar `ggplot2` para criar gráficos."
- "Pense nos gráficos em camadas: *aesthetics*, *geometry*, *statistics*, *scale transformation*, e *grouping*."
source: Rmd
---



Plotar nossos dados é uma das melhores maneiras de explorá-los rapidamente e também ver as várias relações entre variáveis.

Existem três sistemas de plotagem principais no R, o [base plotting system][base], o pacote [lattice][lattice], and o pacote [ggplot2][ggplot2].

[base]: http://www.statmethods.net/graphs/
[lattice]: http://www.statmethods.net/advgraphs/trellis.html
[ggplot2]: http://www.statmethods.net/advgraphs/ggplot2.html

Hoje vamos aprender sobre o pacote ggplot2, porque é o mais eficaz para a criação de gráficos de qualidade de publicação.

ggplot2 é construído sobre a gramática de gráficos, a ideia de que qualquer gráfico pode ser expresso a partir do mesmo conjunto de componentes: um **conjunto de dados**, um **sistema de coordenadas** e um conjunto de **geoms** - a representação visual dos conjunto de pontos.

A chave para entender ggplot2 é pensar em uma figura em camadas. 
Essa ideia pode ser familiar para você se você usou programas de edição de imagem como Photoshop, Illustrator ou Inkscape.

Vamos começar com um exemplo:


~~~
library("ggplot2")
~~~
{: .r}



~~~
Warning: package 'ggplot2' was built under R version 3.3.2
~~~
{: .error}



~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point()
~~~
{: .r}

<img src="../fig/rmd-08-lifeExp-vs-gdpPercap-scatter-1.png" title="plot of chunk lifeExp-vs-gdpPercap-scatter" alt="plot of chunk lifeExp-vs-gdpPercap-scatter" style="display: block; margin: auto;" />

Então a primeira coisa que fazemos é chamar a função `ggplot`. Esta função permite que R saiba que estamos criando um novo gráfico e que qualquer dos argumentos que damos à função `ggplot` são as opções *globais* para este gráfico: elas se aplicam a todas as camadas do gráfico.

Passamos em dois argumentos para `ggplot`. Primeiro, dizemos ao `ggplot` quais dados queremos mostrar na nossa figura, neste exemplo os dados gapminder que lemos anteriormente. 
Para o segundo argumento passamos na função `aes`, que diz ao `ggplot` como variáveis nos **dados** delineiam as propriedades *aesthetic* da figura, neste caso as localizações de **x** e **y**. Aqui nós dissemos ao `ggplot` que queremos graficar a coluna "gdpPercap" do data frame gapminder no eixo x, e a coluna "lifeExp" no eixo y. Observe que não precisamos explicitamente passar essas colunas (por exemplo `x = gapminder[, "gdpPercap"]`), isso ocorre porque o `ggplot` é inteligente o suficiente para saber olhar os **dados** dessa coluna!

Por si só, a chamada para `ggplot` não é suficiente para desenhar uma figura:


~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp))
~~~
{: .r}

<img src="../fig/rmd-08-unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />

Precisamos dizer ao `ggplot` como queremos representar visualmente os dados, o que fazemos adicionando uma nova camada **geom**. No nosso exemplo, usamos `geom_point`, que diz ao `ggplot` que queremos representar visualmente a relação entre **x** e **y** como um scatterplot dos pontos:


~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point()
~~~
{: .r}

<img src="../fig/rmd-08-lifeExp-vs-gdpPercap-scatter2-1.png" title="plot of chunk lifeExp-vs-gdpPercap-scatter2" alt="plot of chunk lifeExp-vs-gdpPercap-scatter2" style="display: block; margin: auto;" />

> ## Desafio 1
>
> Modifique o exemplo para que a figura visualize como a expectativa de vida mudou ao longo do tempo:
>
> 
> ~~~
> ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) + geom_point()
> ~~~
> {: .r}
>
> Dica: o conjunto de dados gapminder tem uma coluna chamada "year", que deve aparecer no eixo x.
>
> > ## Solução para Desafio 1
> >
> > Aqui está uma possível solução:
> >
> > 
> > ~~~
> > ggplot(data = gapminder, aes(x = year, y = lifeExp)) + geom_point()
> > ~~~
> > {: .r}
> > 
> > <img src="../fig/rmd-08-ch1-sol-1.png" title="plot of chunk ch1-sol" alt="plot of chunk ch1-sol" style="display: block; margin: auto;" />
> >
> {: .solution}
{: .challenge}

>
> ## Desafio 2
>
> Nos exemplos anteriores e no desafio usamos a função `aes` para dizer ao **geom** do scatterplot sobre as posições **x** e **y** de cada ponto. Outra propriedade *aesthetic* que podemos modificar é a *cor* do ponto. Modifique o código do desafio anterior para **colorir** os pontos pela coluna "continent". Que tendências você vê nos dados? Eles são o que você esperava?
>
> > ## Solução para Desafio 2
> >
> > Nos exemplos anteriores e no desafio usamos a função `aes` para dizer ao **geom** do scatterplot sobre as posições **x** e **y** de cada ponto. Outra propriedade *aesthetic* que podemos modificar é a *cor* do ponto. Modifique o código do desafio anterior para **colorir** os pontos pela coluna "continent". Que tendências você vê nos dados? Eles são o que você esperava?
> >
> > 
> > ~~~
> > ggplot(data = gapminder, aes(x = year, y = lifeExp, color=continent)) +
> >   geom_point()
> > ~~~
> > {: .r}
> > 
> > <img src="../fig/rmd-08-ch2-sol-1.png" title="plot of chunk ch2-sol" alt="plot of chunk ch2-sol" style="display: block; margin: auto;" />
> >
> {: .solution}
{: .challenge}


## Camadas

Usar um scatterplot provavelmente não é o melhor modo de visualizar a mudança ao longo do tempo. Em vez disso, vamos dizer ao `ggplot` para visualizar os dados como um gráfico de linha:


~~~
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country, color=continent)) +
  geom_line()
~~~
{: .r}

<img src="../fig/rmd-08-lifeExp-line-1.png" title="plot of chunk lifeExp-line" alt="plot of chunk lifeExp-line" style="display: block; margin: auto;" />

Em vez de adicionar uma camada `geom_point`, adicionamos uma camada `geom_line`. Nós adicionamos **pelo** *aesthetic*, que diz ao `ggplot` para desenhar uma linha para cada país.

Mas e se quisermos visualizar linhas e pontos no gráfico? Podemos simplesmente adicionar outra camada ao gráfico:


~~~
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country, color=continent)) +
  geom_line() + geom_point()
~~~
{: .r}

<img src="../fig/rmd-08-lifeExp-line-point-1.png" title="plot of chunk lifeExp-line-point" alt="plot of chunk lifeExp-line-point" style="display: block; margin: auto;" />

É importante notar que cada camada é desenhada em cima da camada anterior. Neste exemplo, os pontos foram desenhados *sobre* as linhas. Aqui está uma demonstração:


~~~
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country)) +
  geom_line(aes(color=continent)) + geom_point()
~~~
{: .r}

<img src="../fig/rmd-08-lifeExp-layer-example-1-1.png" title="plot of chunk lifeExp-layer-example-1" alt="plot of chunk lifeExp-layer-example-1" style="display: block; margin: auto;" />
Neste exemplo, o delineamento *aesthetic* da **cor** foi movido das opções de plotagem global em `ggplot` para a camada `geom_line`, de modo que ela não mais se aplica aos pontos. Agora podemos ver claramente que os pontos são desenhados no topo das linhas.

> ## Dica: Definir um aesthetic para um valor em vez de um mapeamento
>
> Até agora, vimos como usar um aesthetic (como a **cor**) como um *delineamento* para uma variável nos dados. Por exemplo, quando usamos `geom_line(aes(color=continent))`, ggplot dará uma cor diferente para cada continente. Mas e se quisermos mudar a cor de todas as linhas para o azul? Você pode pensar que `geom_line(aes(color="blue"))` deve funcionar, mas não. Como não queremos criar um delineamento para uma variável específica, simplesmente movemos a especificação de cor para fora da função `aes()`, como esta: `geom_line(color="blue")`.
{: .callout}

> ## Desafio 3
>
> Alterne a ordem das camadas de ponto e linha do exemplo anterior. O que aconteceu?
>
> > ## Solução para Desafio 3
> >
> > Alterne a ordem das camadas de ponto e linha do exemplo anterior. O que aconteceu?
> >
> > 
> > ~~~
> > ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country)) +
> >  geom_point() + geom_line(aes(color=continent))
> > ~~~
> > {: .r}
> > 
> > <img src="../fig/rmd-08-ch3-sol-1.png" title="plot of chunk ch3-sol" alt="plot of chunk ch3-sol" style="display: block; margin: auto;" />
> >
> > As linhas agora são desenhadas sobre os pontos!
> >
> {: .solution}
{: .challenge}

## Transformações e estatísticas

O ggplot também facilita a sobreposição de modelos estatísticos sobre os dados. Para demonstrar, vamos voltar ao nosso primeiro exemplo:


~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp, color=continent)) +
  geom_point()
~~~
{: .r}

<img src="../fig/rmd-08-lifeExp-vs-gdpPercap-scatter3-1.png" title="plot of chunk lifeExp-vs-gdpPercap-scatter3" alt="plot of chunk lifeExp-vs-gdpPercap-scatter3" style="display: block; margin: auto;" />

Atualmente, é difícil ver a relação entre os pontos devido a alguns outliers fortes no PIB per capita. Podemos alterar a escala de unidades no eixo x usando as funções *scale*. Este controla o delineamento entre os valores dos dados e os valores visuais de um aesthetic. Podemos também modificar a transparência dos pontos, usando a função *alpha*, que é especialmente útil quando você tem uma grande quantidade de dados que é muito agrupado.


~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = 0.5) + scale_x_log10()
~~~
{: .r}

<img src="../fig/rmd-08-axis-scale-1.png" title="plot of chunk axis-scale" alt="plot of chunk axis-scale" style="display: block; margin: auto;" />

A função `log10` aplicou uma transformação para os valores da coluna gdpPercap antes de os renderizar no gráfico, de modo que cada múltiplo de 10 agora apenas corresponde a um aumento em 1 na escala transformada, por exemplo, um PIB per capita de 1.000 é agora 3 no eixo y, um valor de 10.000 corresponde a 4 no eixo y e assim por diante. Isso torna mais fácil visualizar a disseminação dos dados no eixo x.

> ## Dica lembrete: Definir um *aesthetic* para um valor em vez de um mapeamento
>
> Observe que usamos `geom_point(alpha = 0.5)`. Como a dica anterior mencionada, usando um ajuste fora da função `aes()` fará com que esse valor seja usado para todos os pontos, que é o que queremos neste caso. Mas, como qualquer outro ajuste aesthetic, *alpha* também pode ser delineado para uma variável nos dados. Por exemplo, podemos dar uma transparência diferente para cada continente com `geom_point(aes(alfa = continent))`.
{: .callout}

Podemos ajustar uma relação simples aos dados adicionando outra camada, `geom_smooth`:


~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point() + scale_x_log10() + geom_smooth(method="lm")
~~~
{: .r}

<img src="../fig/rmd-08-lm-fit-1.png" title="plot of chunk lm-fit" alt="plot of chunk lm-fit" style="display: block; margin: auto;" />

Podemos tornar a linha mais espessa, *definindo* o **size** aesthetic na camada `geom_smooth`:


~~~
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point() + scale_x_log10() + geom_smooth(method="lm", size=1.5)
~~~
{: .r}

<img src="../fig/rmd-08-lm-fit2-1.png" title="plot of chunk lm-fit2" alt="plot of chunk lm-fit2" style="display: block; margin: auto;" />

Há duas maneiras que um *aesthetic* pode ser especificado. Aqui *definimos* o **size** aesthetic passando-o como um argumento para `geom_smooth`. Anteriormente, na lição, usamos a função `aes` para definir um *delineamento* entre variáveis de dados e sua representação visual.

> ## Desafio 4a
>
> Modifique a cor e o tamanho dos pontos na camada de pontos no exemplo anterior.
>
> Dica: não use a função `aes`.
>
> > ## Solução para Desafio 4a
> >
> > Modifique a cor e o tamanho dos pontos na camada de pontos no exemplo anterior.
> >
> > Dica: não use a função `aes`.
> >
> > 
> > ~~~
> > ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
> >  geom_point(size=3, color="orange") + scale_x_log10() +
> >  geom_smooth(method="lm", size=1.5)
> > ~~~
> > {: .r}
> > 
> > <img src="../fig/rmd-08-ch4a-sol-1.png" title="plot of chunk ch4a-sol" alt="plot of chunk ch4a-sol" style="display: block; margin: auto;" />
> {: .solution}
{: .challenge}


> ## Desafio 4b
>
> Modifique sua solução para o Desafio 4a para que os pontos fiquem agora com uma forma diferente e sejam coloridos por continente com novas linhas de tendência. Dica: O argumento de cor pode ser usado dentro de aes.
>
> > ## Solução para Desafio 4b
> >
> > Modifique sua solução para o Desafio 4a para que os pontos fiquem agora com uma forma diferente e sejam coloridos por continente com novas linhas de tendência. Dica: O argumento de cor pode ser usado dentro de aes.
> >
> > Hint: The color argument can be used inside the aesthetic.
> >
> >
> >~~~
> > ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp, color = continent)) +
> > geom_point(size=3, shape=17) + scale_x_log10() +
> > geom_smooth(method="lm", size=1.5)
> >~~~
> >{: .r}
> >
> ><img src="../fig/rmd-08-ch4b-sol-1.png" title="plot of chunk ch4b-sol" alt="plot of chunk ch4b-sol" style="display: block; margin: auto;" />
> {: .solution}
{: .challenge}


## Figuras de Painel Múltiplo

Anteriormente, visualizamos a mudança na expectativa de vida ao longo do tempo em todos os países em um único gráfico. Alternativamente, podemos dividir isso em vários painéis adicionando uma camada de painéis **facet**. Focalizando apenas aqueles países com nomes que começam com a letra "A" ou "Z".

> ## Dica
>
> Começamos fazendo um subconjunto dos dados. Usamos a função `substr` para extrair uma parte de uma seqüência de caracteres; Neste caso, as letras que ocorrem das posições `start` até `stop`, inclusive, do vetor `gapminder$country`. O operador `%in%` permite-nos fazer comparações múltiplas ao invés de escrever longas condições de subconjunto (neste caso, `starts.with %in% c("A", "Z")` é equivalente a `starts.with == "A" | starts.with == "Z"`)
{: .callout}



~~~
starts.with <- substr(gapminder$country, start = 1, stop = 1)
az.countries <- gapminder[starts.with %in% c("A", "Z"), ]
ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country)
~~~
{: .r}

<img src="../fig/rmd-08-facet-1.png" title="plot of chunk facet" alt="plot of chunk facet" style="display: block; margin: auto;" />

A camada `facet_wrap` tomou uma "fórmula" como seu argumento, denotado pelo til (~). Isso informa o R para desenhar um painel para cada valor exclusivo na coluna de país do conjunto de dados gapminder.

## Modificando o texto

Para limpar esta figura para uma publicação, precisamos alterar alguns dos elementos de texto. O eixo x é muito desordenado e no eixo y deve estar escrito "Espectativa de vida", em vez do nome da coluna da data frame, além disso recomenda-se, colocar o nome do gráfico, e legendas na língua da publicação.

Podemos fazer isso adicionando um par de camadas diferentes. A camada **theme** controla o texto do eixo e o tamanho geral do texto, e existem camadas especiais para alterar os rótulos dos eixos. Para alterar o título da legenda, precisamos usar a camada **scales**.


~~~
ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
  xlab("Year") + ylab("Life expectancy") + ggtitle("Figure 1") +
  scale_colour_discrete(name="Continent") +
  theme(axis.text.x=element_blank(), axis.ticks.x=element_blank())
~~~
{: .r}

<img src="../fig/rmd-08-theme-1.png" title="plot of chunk theme" alt="plot of chunk theme" style="display: block; margin: auto;" />

Este é um gostinho do que você pode fazer com ggplot2. O RStudio fornece uma [cheat sheet (Folha de macetes)][cheat] realmente útil das diferentes camadas disponíveis, e documentação mais extensa está disponível no [site ggplot2][ggplot-doc]. Finalmente, se você não tem ideia de como mudar algo, uma rápida pesquisa do Google geralmente irá enviar para uma pergunta relevante e resposta no Stack Overflow com código reutilizável para modificar!

[cheat]: http://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf
[ggplot-doc]: http://docs.ggplot2.org/current/


> ## Desafio 5
>
> Criar um gráfico de densidade do PIB per capita preenchido, por continente.
> 
> Avançado:
>  - Transforme o eixo x para visualizar melhor a propagação de dados.
>  - Adicione uma camada facet ao painel de gráficos de densidade por ano.
>
> > ## Solução para Desafio 5
> >
> > Criar um gráfico de densidade do PIB per capita preenchido, por continente.
> >
> > Avançado:
> >  - Transforme o eixo x para visualizar melhor a propagação de dados.
> >  - Adicione uma camada facet ao painel de gráficos de densidade por ano.
> >
> > 
> > ~~~
> > ggplot(data = gapminder, aes(x = gdpPercap, fill=continent)) +
> >  geom_density(alpha=0.6) + facet_wrap( ~ year) + scale_x_log10()
> > ~~~
> > {: .r}
> > 
> > <img src="../fig/rmd-08-ch5-sol-1.png" title="plot of chunk ch5-sol" alt="plot of chunk ch5-sol" style="display: block; margin: auto;" />
> {: .solution}
{: .challenge}
