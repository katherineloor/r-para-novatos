---
title: "IntroduÃ§Ã£o ao R e RStudio"
Quest<f5>es:
- Como encontrar o seu camihno ao redor de RStudio?
- Como interagir com R?
- Como gerenciar seu ambiente?
- Como instalar pacotes?
exercises: 10
source: Rmd
teaching: 45
Objetivos:
- Obter familiaridade com os vÃ¡rios panÃ©is no IDE do RStudio
- Obter familiaridade com os botÃµes, atalhos e opÃ§Ãµes no IDE do RStudio
- Entender variÃ¡veis e como atribuir a ela
- Ser capaz de gerenciar seu espaÃ§o de trabalho em uma sessaÃ£o interativa de R
- Ser capaz de usar operaÃ§oÃµes matemÃ¡ticas e de comparaÃ§Ã£o
- Ser capaz de chamar funÃ§Ãµes
- Introduzir o gerenciamento de pacotes
---




## Motiva<e7><e3>o

A ci<ea>ncia <e9> um processo de v<e1>rias etapas: uma vez que voc<ea> criou uma experi<ea>ncia e coletou dados, a verdadeira divers<e3>o come<e7>a! Esta li<e7><e3>o ensinar<e1> como iniciar este processo usando R e RStudio. Come<e7>aremos com dados brutos, realizaremos an<e1>lises explorat<f3>rias e aprenderemos a tra<e7>ar resultados graficamente. Este exemplo come<e7>a com um conjunto de dados de [gapminder.org](https://www.gapminder.org) que cont<e9>m informa<e7><f5>es de popula<e7><e3>o para muitos pa<ed>ses ao longo do tempo. Voc<ea> pode ler os dados em R? Pode tra<e7>ar a popula<e7><e3>o para o Senegal? Voc<ea> pode calcular a renda m<e9>dia dos pa<ed>ses do continente asi<e1>tico? No final dessas li<e7><f5>es voc<ea> ser<e1> capaz de fazer coisas como tramar as popula<e7><f5>es de todos esses pa<ed>ses em menos de um minuto!

## Antes de iniciar o Workshop

Certifique-se de que tem a vers<e3>o mais recente do R e do RStudio instalados na sua m<e1>quina. Isso <e9> importante, pois alguns pacotes usados no workshop podem n<e3>o ser instalados corretamente (ou em todos) se R n<e3>o estiver atualizado.

[Download and install the latest version of R here](https://www.r-project.org/)
[Download and install RStudio here](https://www.rstudio.com/)

## Introdu<e7>a<f5> ao RStudio 


Bem-vindo <e0> parte do R do Software Carpentry.

Ao longo desta li<e7><e3>o, vamos ensinar-lhe alguns dos fundamentos da linguagem R, bem como algumas pr<e1>ticas recomendadas para organizar o c<f3>digo para projetos cient<ed>ficos que facilitar<e3>o a sua vida.

Estaremos usando o RStudio: um ambiente de desenvolvimento integrado livre e de c<f3>digo aberto. Ele fornece um editor embutido, funciona em todas as plataformas (incluindo servidores) e oferece muitas vantagens, como integra<e7><e3>o com controle de vers<e3>o e gerenciamento de projetos. 



**Layout b<e1>sico**

Quando voc<ea> abrir RStudio pela primeira vez, voc<ea> ser<e1> recebido por tr<ea>s pain<e9>is: 

  * O console R interativo (todo <e0> esquerda)
  * Ambiente/Hist<f3>ria (tabulada no canto direito)
  * Archivos/Gr<e1>ficos/Pacotes/Ajuda/Visualizador (com abas na parte inferior direita)

![](http://swcarpentry.github.io/r-novice-gapminder/fig/01-rstudio.png)

Depois de abrir arquivos, como R scripts, um painel de editor tamb<e9>m ser<e1> aberto no canto superior esquerdo. 


![](http://swcarpentry.github.io/r-novice-gapminder/fig/01-rstudio-script.png) 


## Fluxo de trabalho no RStudio
Existem duas maneiras principais de se trabalhar dentro do RStudio.

1. Teste e jogue dentro do console R interativo, em seguida, copie o c<f3>digo para um arquivo .R para ser executado posteriormente.
   *  Isso funciona bem ao fazer pequenos testes e inicialmente come<e7>ar.
   *  Torna-se rapidamente trabalhoso.
2. Comece a escrever em um arquivo .R e use o comando / atalho RStudio para pressionar a linha atual, as linhas selecionadas ou as linhas modificadas para o console R interativo. .
   * Esta <e9> uma <f3>tima maneira de come<e7>ar; Todo o seu c<f3>digo <e9> salvo para mais tarde.
   * Voc<ea> poder<e1> executar o arquivo criado a partir de RStudio ou usando a fun<e7><e3>o **source ()** de R.

> ## Dica: Executando segmentos do seu c<f3>digo
>
> O RStudio oferece-lhe uma grande flexibilidade na execu<e7><e3>o de c<f3>digo 
>a partir da janela do editor. Existem bot<f5>es, op<e7><f5>es de menu e atalhos 
>de teclado. Para executar a linha atual, voc<ea> pode: 1. pressionar no bot<e3>o 
>Executar acima do painel do editor ou 2. selecionar "Run lines" no menu "Code",
>ou 3. pressionar Ctrl-Enter no Windows ou Linux ou Command-Enter no OS X.
>(Este atalho tamb<e9>m pode ser visto ao passar o mouse sobre o bot<e3>o).
>Para executar um bloco de c<f3>digo, selecione-o e, em seguida, Executar. 
>Se voc<ea> modificou uma linha de c<f3>digo dentro de um bloco de c<f3>digo que 
>voc<ea> acabou de executar, n<e3>o h<e1> necessidade de reajustar a se<e7><e3>o e Run, 
>voc<ea> pode usar o pr<f3>ximo bot<e3>o, Re-Run the previous code region. Isso 
>executar<e1> o bloco de c<f3>digo anterior incluindo as modifica<e7><f5>es feitas.
>
{: .callout}

## Introdu<e7><e3>o a R

A maior parte do seu tempo em R ser<e1> gasto no console interativo R. Isto <e9> onde voc<ea> ir<e1> executar todo o seu c<f3>digo, e pode ser um ambiente <fa>til para experimentar id<e9>ias antes de adicion<e1>-los a um arquivo de script R. Este console no RStudio <e9> o mesmo que voc<ea> obteria se voc<ea> digitou R em seu ambiente de linha de comando.

A primeira coisa que voc<ea> vai ver na sess<e3>o interativa R <e9> um monte de informa<e7><f5>es, seguido por um ">" e um cursor piscando. Em muitos aspectos isso <e9> semelhante ao ambiente de shell que voc<ea> aprendeu durante as li<e7><f5>es do shell: ele opera na mesma id<e9>ia de um "Read, evaluate, print loop": voc<ea> digita comandos, R tenta execut<e1>-los e, em seguida, retorna um resultado.

## Usando R como uma calculadora 

A coisa mais simples que voc<ea> pode fazer com R <e9> aritm<e9>tica: 


~~~
1 + 100
~~~
{: .r}



~~~
[1] 101
~~~
{: .output}

E R vai imprimir a resposta, com um precedente "[1]". N<e3>o se preocupe com isso por agora, vamos explicar isso mais tarde. Por agora, pense nisso como uma sa<ed>da indicadora.  

Como bash, se voc<ea> digitar um comando incompleto, R esperar<e1> que voc<ea> o complete: 

~~~
> 1 +
~~~
{: .r}

~~~
+
~~~
{: .output}

Cada vez que voc<ea> pressionar a tecla Enter e a sess<e3>o R mostre um "+" em vez de um ">", isso significa que est<e1> esperando que voc<ea> complete o comando. Se voc<ea> deseja cancelar um comando, voc<ea> pode simplesmente presionar em "Esc" e rstudio vai lhe dar de volta o ">" alerta.

> ## Dica: Cancelamento de comandos
>
> Se voc<ea> estiver usando R a partir da linha de comando em vez de 
>dentro RStudio, voc<ea> precisar<e1> usar `Ctrl+C` em vez de `Esc` 
> para cancelar o comando. Isso tamb<e9>m se aplica aos usu<e1>rios de Mac!
>
> Cancelar um comando n<e3>o <e9> <fa>til somente para matar comandos incompletos: 
> voc<ea> tamb<e9>m pode us<e1>-lo para dizer a R para parar o c<f3>digo em execu<e7><e3>o
> (por exemplo, se estiver demorando muito mais do que voc<ea> espera) 
>ou para se livrar do c<f3>digo que voc<ea> est<e1> escrevendo. 
>
{: .callout}

Ao usar R como uma calculadora, a ordem das opera<e7><f5>es <e9> a mesma que voc<ea> teria aprendido na escola.  

Da preced<ea>ncia mais alta <e0> mais baixa:

 * Par<ea>nteses: `(`, `)`
 * Exponentes: `^` or `**`
 * Dividir: `/`
 * Multiplicar: `*`
 * Adicionar: `+`
 * Subtrair: `-`


~~~
3 + 5 * 2
~~~
{: .r}



~~~
[1] 13
~~~
{: .output}

Use par<ea>nteses para agrupar opera<e7><f5>es, a fim de for<e7>ar a ordem de avalia<e7><e3>o se difere do padr<e3>o, ou para tornar claro o que voc<ea> pretende. .


~~~
(3 + 5) * 2
~~~
{: .r}



~~~
[1] 16
~~~
{: .output}

Isso pode ficar complicado quando n<e3>o <e9> necess<e1>rio, mas esclarece suas inten<e7><f5>es. Lembre-se de que os outros podem ler o seu c<f3>digo mais tarde. 


~~~
(3 + (5 * (2 ^ 2))) # difü¾™†”¼cil de ler
3 + 5 * 2 ^ 2       # claro, se vocü¾˜–”¼ se lembrar das regras
3 + 5 * (2 ^ 2)     # se vocü¾˜–”¼ esquecer algumas regras, isso pode ajudar
~~~
{: .r}


O texto ap<f3>s cada linha de c<f3>digo <e9> chamado de "coment<e1>rio". Qualquer coisa que se segue ap<f3>s o hash (ou octothorpe) s<ed>mbolo `#` <e9> ignorado por R quando ele executa c<f3>digo.

N<fa>meros realmente pequenos ou grandes obt<ea>m uma nota<e7><e3>o cient<ed>fica:


~~~
2/10000
~~~
{: .r}



~~~
[1] 2e-04
~~~
{: .output}

Qual <e9> taquigrafia para "multiplicado por `10^XX`". Ent<e3>o `2e-4` <e9> abrevia<e7><e3>o para `2 * 10^(-4)`.


Voc<ea> tamb<e9>m pode escrever n<fa>meros em nota<e7><e3>o cient<ed>fica: 


~~~
5e3  # Note a falta de menos aqui
~~~
{: .r}



~~~
[1] 5000
~~~
{: .output}

## Fun<e7><f5>es matem<e1>ticas

R tem muitas fun<e7><f5>es matem<e1>ticas constru<ed>das. Para chamar uma fun<e7><e3>o, simplesmente digite seu nome, seguido por par<ea>nteses abertos e fechados. Qualquer coisa que digitemos dentro dos par<ea>nteses <e9> chamada de argumentos da fun<e7><e3>o:


~~~
sin(1)  # funü¾¶”¼ü¾–˜¼es trigonomü¾Ž–”¼tricas
~~~
{: .r}



~~~
[1] 0.841471
~~~
{: .output}


~~~
log(1)  # logaritmo natural
~~~
{: .r}



~~~
[1] 0
~~~
{: .output}


~~~
log10(10) # logaritmo base-10
~~~
{: .r}



~~~
[1] 1
~~~
{: .output}


~~~
exp(0.5) # e^(1/2)
~~~
{: .r}



~~~
[1] 1.648721
~~~
{: .output}

N<e3>o se preocupe em tentar lembrar cada fun<e7><e3>o em R. Voc<ea> pode simplesmente procur<e1>-los no Google, ou se lembrar do in<ed>cio do nome da fun<e7><e3>o, use a conclus<e3>o da guia no RStudio.

Esta <e9> uma vantagem que RStudio tem sobre R por conta pr<f3>pria, tem auto-conclus<e3>o habilidades que lhe permitem procurar mais facilmente as fun<e7><f5>es, os seus argumentos e os valores que eles tomam.

Digite um "?" antes do nome dum comando e abrir<e1> uma p<e1>gina de ajuda para esse comando. Assim como fornecer<e1> uma descri<e7><e3>o detalhada do comando e como ele funciona, baixando na a detailed description of the command and how it works, a rolagem para a parte inferior da p<e1>gina de ajuda normalmente mostrar<e1> uma cole<e7><e3>o de exemplos de c<f3>digo que ilustram o uso do comando. Passaremos por um exemplo mais adiante.

## Comparando coisas

Podemos tamb<e9>m fazer compara<e7><e3>o em R:


~~~
1 == 1  # igualdade (nota dois sinais iguais, leai-se como "ü¾Ž–”¼ igual a")
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}


~~~
1 != 2  # desigualdade (leai-se como "nü¾Œ¶”¼o ü¾Ž–”¼ igual a")
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}


~~~
1 <  2  # menor que
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}


~~~
1 <= 1  # Menor ou igual que
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}


~~~
1 > 0  # maior que
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}


~~~
1 >= -9 #Maior ou igual que
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}

> ## Dica: Comparando N<fa>meros
>
>Uma palavra de advert<ea>ncia sobre a compara<e7><e3>o de n<fa>meros: 
>voc<ea> nunca deve usar `==` para comparar dois n<fa>meros a
> menos que sejam inteiros (um tipo de dados que pode representar
> especificamente apenas n<fa>meros inteiros).
>
> Os computadores s<f3> podem representar n<fa>meros decimais com 
>um certo grau de precis<e3>o, ent<e3>o dois n<fa>meros que parecem os mesmos
>quando impressos por R, podem realmente ter diferentes representa<e7><f5>es 
>subjacentes e, portanto, ser diferentes por uma pequena margem de erro 
>(chamada toler<e2>ncia num<e9>rica da m<e1>quina).
>
> Em vez disso, voc<ea> deve usar a fun<e7><e3>o `all.equal`.
> Leitura adicional:     [http://floating-point-gui.de/](http://floating-point-gui.de/)
> 
{: .callout}

## Vari<e1>veis e atribui<e7><e3>o 

Podemos armazenar valores em vari<e1>veis usando o operador de atribui<e7><e3>o `<-`, assim: 


~~~
x <- 1/40
~~~
{: .r}

Observe que a atribui<e7><e3>o n<e3>o imprime um valor. Em vez disso, n<f3>s armazenamos isso para mais tarde em algo chamado de **vari<e1>vel**, `x` agora cont<e9>m o **valor** `0.025`:


~~~
x
~~~
{: .r}



~~~
[1] 0.025
~~~
{: .output}

Mais precisamente, o valor armazenado <e9> uma *aproxima<e7><e3>o decimal* dessa fra<e7><e3>o chamada uma [N<fa>mero de ponto flutuante](http://en.wikipedia.org/wiki/Floating_point).

Procure a guia `Ambiente` em um dos pain<e9>is do RStudio, e voc<ea> ver<e1> que `x` e seu valor apareceram. Nossa vari<e1>vel `x` pode ser usada no lugar de um n<fa>mero em qualquer c<e1>lculo que espera um n<fa>mero: 


~~~
log(x)
~~~
{: .r}



~~~
[1] -3.688879
~~~
{: .output}

Observe tamb<e9>m que as vari<e1>veis podem ser reatribu<ed>das:


~~~
x <- 100
~~~
{: .r}

`x` usado para conter o valor 0.025 e agora ele tem o valor 100.  

valores asignados podem conter a vari<e1>vel sendo assignidada:


~~~
x <- x + 1 #observe como o RStudio atualiza sua descriü¾¶”¼ü¾Œ¶”¼o de x na guia superior direita
~~~
{: .r}

O lado direito da atribui<e7><e3>o pode ser qualquer express<e3>o R v<e1>lida. 
O lado direito <e9> *totalmente avaliado* antes da atribui<e7><e3>o ocorrer.

Nomes de vari<e1>veis podem conter letras, n<fa>meros, sublinhados e pontos. Eles n<e3>o podem come<e7>ar com um n<fa>mero nem conter espa<e7>os em tudo. Diferentes pessoas usam conven<e7><f5>es diferentes para nomes de vari<e1>veis longas,

  * pontos.entre.palavras
  * sublinha\_entre_palavras
  * cameloCasoParaSeparadoPalavras

O que voc<ea> usa depende de voc<ea>, mas seja **consistente**.

Tamb<e9>m <e9> poss<ed>vel usar o operador = para atribui<e7><e3>o: 


~~~
x = 1/40
~~~
{: .r}

Mas isso <e9> muito menos comum entre os usu<e1>rios de R. O mais importante <e9> ser **consistente** com o operador que voc<ea> usa. H<e1> ocasionalmente lugares onde <e9> menos confuso usar `<-` than `=`, e <e9> o s<ed>mbolo mais comum usado na comunidade. Portanto, a recomenda<e7><e3>o <e9> usar `<-`. 

## Vetoriza<e7><e3>o

<c9> de extrema relev<e2>ncia compreender que o R <e9> um software vetorizado. Isso simplesmnte quer dizer que vari<e1>veis e fun<e7><f5>es podem receber vetores. 
Veja os exemplos abaixo:


~~~
1:5
~~~
{: .r}



~~~
[1] 1 2 3 4 5
~~~
{: .output}



~~~
2^(1:5)
~~~
{: .r}



~~~
[1]  2  4  8 16 32
~~~
{: .output}



~~~
x <- 1:5
2^x
~~~
{: .r}



~~~
[1]  2  4  8 16 32
~~~
{: .output}

A possibilidade de vari<e1>veis e fun<e7><f5>es receber valores faz do R um software extremamente poderoso. Este t<f3>pico ser<e1> abordado em detalhes adiante.


## Controlando o ambiente de trabalho

Existem alguns comandos que o usu<e1>rio pode utilizar para interagir com o ambiente de trabalho do R.

A fun<e7><e3>o `ls`, por exemplo, lista todas as vari<e1>veis e fun<e7><f5>es gravadas no ambiente de trabalho.



~~~
ls()
~~~
{: .r}



~~~
[1] "x" "y"
~~~
{: .output}

> ## Dica: Objetos escondidos
>
> A fun<e7><e3>o `ls()`n<e3>o mostra vari<e1>veis e fun<e7><f5>es que come<e7><e3>o com `.`.
> A fun<e7><e3>o  Para listar todos os objetos, inclusive os que   iniciam com `.`, digite `ls(all.names=TRUE)` e n<e3>o `ls()`.
> 
{: .callout}

Note que na fun<e7><e3>o `ls` n<e3>o <e9> necess<e1>rio fornecer argumentos, mas <e9> necess<e1>rio colocar par<ea>ntesis para que o R entenda que se trata de uma fun<e7><e3>o.

Se digitarmos apenas `ls` o R nos fornecer<e1> o c<f3>digo fonte da fun<e7><e3>o!


~~~
ls
~~~
{: .r}



~~~
function (name, pos = -1L, envir = as.environment(pos), all.names = FALSE, 
    pattern, sorted = TRUE) 
{
    if (!missing(name)) {
        pos <- tryCatch(name, error = function(e) e)
        if (inherits(pos, "error")) {
            name <- substitute(name)
            if (!is.character(name)) 
                name <- deparse(name)
            warning(gettextf("%s converted to character string", 
                sQuote(name)), domain = NA)
            pos <- name
        }
    }
    all.names <- .Internal(ls(envir, all.names, sorted))
    if (!missing(pattern)) {
        if ((ll <- length(grep("[", pattern, fixed = TRUE))) && 
            ll != length(grep("]", pattern, fixed = TRUE))) {
            if (pattern == "[") {
                pattern <- "\\["
                warning("replaced regular expression pattern '[' by  '\\\\['")
            }
            else if (length(grep("[^\\\\]\\[<-", pattern))) {
                pattern <- sub("\\[<-", "\\\\\\[<-", pattern)
                warning("replaced '[<-' by '\\\\[<-' in regular expression pattern")
            }
        }
        grep(pattern, all.names, value = TRUE)
    }
    else all.names
}
<bytecode: 0x7fe73b2ce8b0>
<environment: namespace:base>
~~~
{: .output}

Pode ser interessante deletar objetos que n<e3>o ser<e3>o utilizados adiante. Para isso utilizamos a fun<e7><e3>o `rm`.


~~~
rm(x)
~~~
{: .r}

Se muitos objetos devem ser exclu<ed>dos de uma s<f3> vez n<e3>o h<e1> necessidade de excluir um por um. Basta utilizar a fun<e7><e3>o `ls` conjugada com a `rm` da seguinte forma:


~~~
rm(list = ls())
~~~
{: .r}

Neste caso duas fun<e7><f5>es foram utilizadas em conjunto. Sempre que este for o caso o que est<e1> localizado dentro do par<ea>ntesis mais interno ser<e1> avaliado primeiro e assim em diante.

No caso acima foi especificado que o resultado da fun<e7><e3>o `ls` deveria ser usado na forma de lista `list` como argumento da fun<e7><e3>o `rm`. Quando destinados valores de argumentos a fun<e7><f5>es por nome, o operador a ser utilizado dever<e1> ser o s<ed>mbolo `=`.

Se utilizarmos `<-` teremos efeitos indesejados ou  mensagens de erro.

~~~
rm(list <- ls())
~~~
{: .r}



~~~
Error in rm(list <- ls()): ... must contain names or character strings
~~~
{: .error}

> ## Dica: Avisos vs Erros
>
> Fique atento pois o R poder<e1> fazer algo inesperado.
> Erros, como no caso acima, ocorrem quando o R n<e3>o consegue efetuar as opera<e7><f5>es.
> Avisos, por outro lado, indicam que o R conseguiu efetuar as opera<e7><f5>es, mas algo  provavelmente n<e3>o ocorreu como esperado.
>
> Em ambos os casos, as mensagens que o R fornece ajudam a consertar o problema.
>
{: .callout}

## Pacotes do R

<c9> poss<ed>vel adicionar fun<e7><f5>es ao R atrav<e9>s da escrita de pacote escritos por voc<ea> mesmo, ou por pacotes escritos por outras pessoas. Hoje existem mais de 7000 pacotes dispon<ed>veis no CRAN (*the comprehensive R archive network*). O R e o Rstudio possuem elevada funcionalidade no quesito de gerenciamento de pacotes:

* <c9> poss<ed>vel ver quais pacotes est<e3>o instalados. Para isso devemos digitar `installed.packages()`.
* <c9> poss<ed>vel instalar novos pacotes ao digitar `install.packages("packagename")`, onde `packagename` <e9> o nome do pacote a ser instalado. 
* <c9> poss<ed>vel atualizar pacotes j<e1> instalados. A fun<e7><e3>o utilizada <e9> `update.packages()`.
* <c9> poss<ed>vel remover pacotes com a fun<e7><e3>o `remove.packages("packagename")`. 
* <c9> poss<ed>vel tornar um pacote habilitado ao uso. A fun<e7><e3>o utilizada neste caso <e9> `library(packagename)`.

> ## Desafio 1
>
> Quais nomes a seguir s<e3>o v<e1>lidos para vari<e1>veis no R ?
> 
> ~~~
> min_height
> max.height
> _age
> .mass
> MaxLength
> min-length
> 2widths
> celsius2kelvin
> ~~~
> {: .r}
>
> > ## Solu<e7><e3>o do desafio 1
> >
> > Os nomes a seguir s<e3>o v<e1>lidos:
> > 
> > ~~~
> > min_height
> > max.height
> > MaxLength
> > celsius2kelvin
> > ~~~
> > {: .r}
> >
> > O nome a seguir cria uma vari<e1>vel escondida
> > 
> > ~~~
> > .mass
> > ~~~
> > {: .r}
> >
> > Os nomes a seguir n<e3>o podem ser usados para nomear objetos
> > 
> > ~~~
> > _age
> > min-length
> > 2widths
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}

> ## Desafio 2
>
> Quais ser<e3>o os valores das vari<e1>veis definidas abaixo ?
>
> 
> ~~~
> mass <- 47.5
> age <- 122
> mass <- mass * 2.3
> age <- age - 20
> ~~~
> {: .r}
>
> > ## Solu<e7><e3>o do desafio 2
> >
> > 
> > ~~~
> > mass <- 47.5
> > ~~~
> > {: .r}
> > Este comando atribui o valor 47.5 para a vari<e1>vel ` mass` for the variable mass
> >
> > 
> > ~~~
> > age <- 122
> > ~~~
> > {: .r}
> > Este comando atribiu o valor 122 para a vari<e1>vel ` age` 
> >
> > 
> > ~~~
> > mass <- mass * 2.3
> > ~~~
> > {: .r}
> > O valor da vari<e1>vel ` mass`ser<e1> dividido por 2.3 e a vari<e1>vel ` mass` passar<e1> a ter um novo valor.
> > 
> >
> > 
> > ~~~
> > age <- age - 20
> > ~~~
> > {: .r}
> > Ser<e1> realizada uma subtra<e7><e3>o de 20 unidades na vari<e1>vel ` age`.
> > 
> {: .solution}
{: .challenge}


> ## Desafio 3
>
> Rode os c<f3>digos do desafio anterior, escreva um comando e compare massa com idade. 
> A massa <e9> maior que a idade ?
>
> > ## Solu<e7><e3>o do desafio 3
> >
> > Uma maneira de realizar esta tarefa <e9> utilizar `>`:
> > 
> > ~~~
> > mass > age
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> > [1] TRUE
> > ~~~
> > {: .output}
> > O resultado a ser retornado deve ser `TRUE` porque a vari<e1>vel ` mass` <e9> maior que ` age`.
> {: .solution}
{: .challenge}


> ## Desafio 4
>
> Limpe o seu ambiente de trabalho. Delete as vari<e1>veis massa e idade.
>
>
> > ## Solu<e7><e3>o do desafio 4
> >
> > O comando `rm` pode ser utilizado para realizar essa tarefa
> > 
> > ~~~
> > rm(age, mass)
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}

> ## Desafio 5
>
> Instale os pacotes `ggplot2`, `plyr` e `gapminder`.
>
> > ## Solu<e7><e3>o do desafio 5
> >
> > Podemos utilizar o comando `install.packages()` para instalar os referidos pacotes.
> > 
> > ~~~
> > install.packages("ggplot2")
> > install.packages("plyr")
> > install.packages("gapminder")
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}
