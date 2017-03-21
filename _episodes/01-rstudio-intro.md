---
title: "Introduction to R and RStudio"
teaching: 45
exercises: 10
questions:
- "How to find your way around RStudio?"
- "How to interact with R?"
- "How to manage your environment?"
- "How to install packages?"
objectives:
- "To gain familiarity with the various panes in the RStudio IDE"
- "To gain familiarity with the buttons, short cuts and options in the RStudio IDE"
- "To understand variables and how to assign to them"
- "To be able to manage your workspace in an interactive R session"
- "To be able to use mathematical and comparison operations"
- "To be able to call functions"
- "Introduction to package management"
keypoints:
- "Use RStudio to write and run R programs."
- "R has the usual arithmetic operators and mathematical functions."
- "Use `<-` to assign values to variables."
- "Use `ls()` to list the variables in a program."
- "Use `rm()` to delete objects in a program."
- "Use `install.packages()` to install packages (libraries)."
---




## Motiva??o

A ci?ncia ? um processo de v?rias etapas: uma vez que voc? criou uma experi?ncia e coletou dados, a verdadeira divers?o come?a! Esta li??o ensinar? como iniciar este processo usando R e RStudio. Come?aremos com dados brutos, realizaremos an?lises explorat?rias e aprenderemos a tra?ar resultados graficamente. Este exemplo come?a com um conjunto de dados de [gapminder.org](https://www.gapminder.org) que cont?m informa??es de popula??o para muitos pa?ses ao longo do tempo. Voc? pode ler os dados em R? Pode tra?ar a popula??o para o Senegal? Voc? pode calcular a renda m?dia dos pa?ses do continente asi?tico? No final dessas li??es voc? ser? capaz de fazer coisas como tramar as popula??es de todos esses pa?ses em menos de um minuto!

## Antes de iniciar o Workshop

Certifique-se de que tem a vers?o mais recente do R e do RStudio instalados na sua m?quina. Isso ? importante, pois alguns pacotes usados no workshop podem n?o ser instalados corretamente (ou em todos) se R n?o estiver atualizado.

[Download and install the latest version of R here](https://www.r-project.org/)
[Download and install RStudio here](https://www.rstudio.com/)

## Introdu?a? ao RStudio 


Bem-vindo ? parte do R do Software Carpentry.

Ao longo desta li??o, vamos ensinar-lhe alguns dos fundamentos da linguagem R, bem como algumas pr?ticas recomendadas para organizar o c?digo para projetos cient?ficos que facilitar?o a sua vida.

Estaremos usando o RStudio: um ambiente de desenvolvimento integrado livre e de c?digo aberto. Ele fornece um editor embutido, funciona em todas as plataformas (incluindo servidores) e oferece muitas vantagens, como integra??o com controle de vers?o e gerenciamento de projetos. 



**Layout b?sico**

Quando voc? abrir RStudio pela primeira vez, voc? ser? recebido por tr?s pain?is: 

  * O console R interativo (todo ? esquerda)
  * Ambiente/Hist?ria (tabulada no canto direito)
  * Archivos/Gr?ficos/Pacotes/Ajuda/Visualizador (com abas na parte inferior direita)

![](http://swcarpentry.github.io/r-novice-gapminder/fig/01-rstudio.png)

Depois de abrir arquivos, como R scripts, um painel de editor tamb?m ser? aberto no canto superior esquerdo. 


![](http://swcarpentry.github.io/r-novice-gapminder/fig/01-rstudio-script.png) 


## Fluxo de trabalho no RStudio
Existem duas maneiras principais de se trabalhar dentro do RStudio.

1. Teste e jogue dentro do console R interativo, em seguida, copie o c?digo para um arquivo .R para ser executado posteriormente.
   *  Isso funciona bem ao fazer pequenos testes e inicialmente come?ar.
   *  Torna-se rapidamente trabalhoso.
2. Comece a escrever em um arquivo .R e use o comando / atalho RStudio para pressionar a linha atual, as linhas selecionadas ou as linhas modificadas para o console R interativo. .
   * Esta ? uma ?tima maneira de come?ar; Todo o seu c?digo ? salvo para mais tarde.
   * Voc? poder? executar o arquivo criado a partir de RStudio ou usando a fun??o **source ()** de R.

> ## Dica: Executando segmentos do seu c?digo
>
> O RStudio oferece-lhe uma grande flexibilidade na execu??o de c?digo 
>a partir da janela do editor. Existem bot?es, op??es de menu e atalhos 
>de teclado. Para executar a linha atual, voc? pode: 1. pressionar no bot?o 
>Executar acima do painel do editor ou 2. selecionar "Run lines" no menu "Code",
>ou 3. pressionar Ctrl-Enter no Windows ou Linux ou Command-Enter no OS X.
>(Este atalho tamb?m pode ser visto ao passar o mouse sobre o bot?o).
>Para executar um bloco de c?digo, selecione-o e, em seguida, Executar. 
>Se voc? modificou uma linha de c?digo dentro de um bloco de c?digo que 
>voc? acabou de executar, n?o h? necessidade de reajustar a se??o e Run, 
>voc? pode usar o pr?ximo bot?o, Re-Run the previous code region. Isso 
>executar? o bloco de c?digo anterior incluindo as modifica??es feitas.
>
{: .callout}

## Introdu??o a R

A maior parte do seu tempo em R ser? gasto no console interativo R. Isto ? onde voc? ir? executar todo o seu c?digo, e pode ser um ambiente ?til para experimentar id?ias antes de adicion?-los a um arquivo de script R. Este console no RStudio ? o mesmo que voc? obteria se voc? digitou R em seu ambiente de linha de comando.

A primeira coisa que voc? vai ver na sess?o interativa R ? um monte de informa??es, seguido por um ">" e um cursor piscando. Em muitos aspectos isso ? semelhante ao ambiente de shell que voc? aprendeu durante as li??es do shell: ele opera na mesma id?ia de um "Read, evaluate, print loop": voc? digita comandos, R tenta execut?-los e, em seguida, retorna um resultado.

## Usando R como uma calculadora 

A coisa mais simples que voc? pode fazer com R ? aritm?tica: 


~~~
1 + 100
~~~
{: .r}



~~~
[1] 101
~~~
{: .output}

E R vai imprimir a resposta, com um precedente "[1]". N?o se preocupe com isso por agora, vamos explicar isso mais tarde. Por agora, pense nisso como uma sa?da indicadora.  

Como bash, se voc? digitar um comando incompleto, R esperar? que voc? o complete: 

~~~
> 1 +
~~~
{: .r}

~~~
+
~~~
{: .output}

Cada vez que voc? pressionar a tecla Enter e a sess?o R mostre um "+" em vez de um ">", isso significa que est? esperando que voc? complete o comando. Se voc? deseja cancelar um comando, voc? pode simplesmente presionar em "Esc" e rstudio vai lhe dar de volta o ">" alerta.

> ## Dica: Cancelamento de comandos
>
> Se voc? estiver usando R a partir da linha de comando em vez de 
>dentro RStudio, voc? precisar? usar `Ctrl+C` em vez de `Esc` 
> para cancelar o comando. Isso tamb?m se aplica aos usu?rios de Mac!
>
> Cancelar um comando n?o ? ?til somente para matar comandos incompletos: 
> voc? tamb?m pode us?-lo para dizer a R para parar o c?digo em execu??o
> (por exemplo, se estiver demorando muito mais do que voc? espera) 
>ou para se livrar do c?digo que voc? est? escrevendo. 
>
{: .callout}

Ao usar R como uma calculadora, a ordem das opera??es ? a mesma que voc? teria aprendido na escola.  

Da preced?ncia mais alta ? mais baixa:

 * Par?nteses: `(`, `)`
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

Use par?nteses para agrupar opera??es, a fim de for?ar a ordem de avalia??o se difere do padr?o, ou para tornar claro o que voc? pretende. .


~~~
(3 + 5) * 2
~~~
{: .r}



~~~
[1] 16
~~~
{: .output}

Isso pode ficar complicado quando n?o ? necess?rio, mas esclarece suas inten??es. Lembre-se de que os outros podem ler o seu c?digo mais tarde. 


~~~
(3 + (5 * (2 ^ 2))) # dif?cil de ler
3 + 5 * 2 ^ 2       # claro, se voc? se lembrar das regras
3 + 5 * (2 ^ 2)     # se voc? esquecer algumas regras, isso pode ajudar
~~~
{: .r}


O texto ap?s cada linha de c?digo ? chamado de "coment?rio". Qualquer coisa que se segue ap?s o hash (ou octothorpe) s?mbolo `#` ? ignorado por R quando ele executa c?digo.

N?meros realmente pequenos ou grandes obt?m uma nota??o cient?fica:


~~~
2/10000
~~~
{: .r}



~~~
[1] 2e-04
~~~
{: .output}

Qual ? taquigrafia para "multiplicado por `10^XX`". Ent?o `2e-4` ? abrevia??o para `2 * 10^(-4)`.


Voc? tamb?m pode escrever n?meros em nota??o cient?fica: 


~~~
5e3  # Note a falta de menos aqui
~~~
{: .r}



~~~
[1] 5000
~~~
{: .output}

## Fun??es matem?ticas

R tem muitas fun??es matem?ticas constru?das. Para chamar uma fun??o, simplesmente digite seu nome, seguido por par?nteses abertos e fechados. Qualquer coisa que digitemos dentro dos par?nteses ? chamada de argumentos da fun??o:


~~~
sin(1)  # fun??es trigonom?tricas
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

N?o se preocupe em tentar lembrar cada fun??o em R. Voc? pode simplesmente procur?-los no Google, ou se lembrar do in?cio do nome da fun??o, use a conclus?o da guia no RStudio.

Esta ? uma vantagem que RStudio tem sobre R por conta pr?pria, tem auto-conclus?o habilidades que lhe permitem procurar mais facilmente as fun??es, os seus argumentos e os valores que eles tomam.

Digite um "?" antes do nome dum comando e abrir? uma p?gina de ajuda para esse comando. Assim como fornecer? uma descri??o detalhada do comando e como ele funciona, baixando na a detailed description of the command and how it works, a rolagem para a parte inferior da p?gina de ajuda normalmente mostrar? uma cole??o de exemplos de c?digo que ilustram o uso do comando. Passaremos por um exemplo mais adiante.

## Comparando coisas

Podemos tamb?m fazer compara??o em R:


~~~
1 == 1  # igualdade (nota dois sinais iguais, leai-se como "? igual a")
~~~
{: .r}



~~~
[1] TRUE
~~~
{: .output}


~~~
1 != 2  # desigualdade (leai-se como "n?o ? igual a")
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

> ## Dica: Comparando N?meros
>
>Uma palavra de advert?ncia sobre a compara??o de n?meros: 
>voc? nunca deve usar `==` para comparar dois n?meros a
> menos que sejam inteiros (um tipo de dados que pode representar
> especificamente apenas n?meros inteiros).
>
> Os computadores s? podem representar n?meros decimais com 
>um certo grau de precis?o, ent?o dois n?meros que parecem os mesmos
>quando impressos por R, podem realmente ter diferentes representa??es 
>subjacentes e, portanto, ser diferentes por uma pequena margem de erro 
>(chamada toler?ncia num?rica da m?quina).
>
> Em vez disso, voc? deve usar a fun??o `all.equal`.
> Leitura adicional:     [http://floating-point-gui.de/](http://floating-point-gui.de/)
> 
{: .callout}

## Vari?veis e atribui??o 

Podemos armazenar valores em vari?veis usando o operador de atribui??o `<-`, assim: 


~~~
x <- 1/40
~~~
{: .r}

Observe que a atribui??o n?o imprime um valor. Em vez disso, n?s armazenamos isso para mais tarde em algo chamado de **vari?vel**, `x` agora cont?m o **valor** `0.025`:


~~~
x
~~~
{: .r}



~~~
[1] 0.025
~~~
{: .output}

Mais precisamente, o valor armazenado ? uma *aproxima??o decimal* dessa fra??o chamada uma [N?mero de ponto flutuante](http://en.wikipedia.org/wiki/Floating_point).

Procure a guia `Ambiente` em um dos pain?is do RStudio, e voc? ver? que `x` e seu valor apareceram. Nossa vari?vel `x` pode ser usada no lugar de um n?mero em qualquer c?lculo que espera um n?mero: 


~~~
log(x)
~~~
{: .r}



~~~
[1] -3.688879
~~~
{: .output}

Observe tamb?m que as vari?veis podem ser reatribu?das:


~~~
x <- 100
~~~
{: .r}

`x` usado para conter o valor 0.025 e agora ele tem o valor 100.  

valores asignados podem conter a vari?vel sendo assignidada:


~~~
x <- x + 1 #observe como o RStudio atualiza sua descri??o de x na guia superior direita
~~~
{: .r}

O lado direito da atribui??o pode ser qualquer express?o R v?lida. 
O lado direito ? *totalmente avaliado* antes da atribui??o ocorrer.

Nomes de vari?veis podem conter letras, n?meros, sublinhados e pontos. Eles n?o podem come?ar com um n?mero nem conter espa?os em tudo. Diferentes pessoas usam conven??es diferentes para nomes de vari?veis longas,

  * pontos.entre.palavras
  * sublinha\_entre_palavras
  * cameloCasoParaSeparadoPalavras

O que voc? usa depende de voc?, mas seja **consistente**.

Tamb?m ? poss?vel usar o operador = para atribui??o: 


~~~
x = 1/40
~~~
{: .r}

Mas isso ? muito menos comum entre os usu?rios de R. O mais importante ? ser **consistente** com o operador que voc? usa. H? ocasionalmente lugares onde ? menos confuso usar `<-` than `=`, e ? o s?mbolo mais comum usado na comunidade. Portanto, a recomenda??o ? usar `<-`. 

## Vetoriza??o

? de extrema relev?ncia compreender que o R ? um software vetorizado. Isso simplesmnte quer dizer que vari?veis e fun??es podem receber vetores. 
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

A possibilidade de vari?veis e fun??es receber valores faz do R um software extremamente poderoso. Este t?pico ser? abordado em detalhes adiante.


## Controlando o ambiente de trabalho

Existem alguns comandos que o usu?rio pode utilizar para interagir com o ambiente de trabalho do R.

A fun??o `ls`, por exemplo, lista todas as vari?veis e fun??es gravadas no ambiente de trabalho.



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
> A fun??o `ls()`n?o mostra vari?veis e fun??es que come??o com `.`.
> A fun??o  Para listar todos os objetos, inclusive os que   iniciam com `.`, digite `ls(all.names=TRUE)` e n?o `ls()`.
> 
{: .callout}

Note que na fun??o `ls` n?o ? necess?rio fornecer argumentos, mas ? necess?rio colocar par?ntesis para que o R entenda que se trata de uma fun??o.

Se digitarmos apenas `ls` o R nos fornecer? o c?digo fonte da fun??o!


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
<bytecode: 0x7fd92a9714b0>
<environment: namespace:base>
~~~
{: .output}

Pode ser interessante deletar objetos que n?o ser?o utilizados adiante. Para isso utilizamos a fun??o `rm`.


~~~
rm(x)
~~~
{: .r}

Se muitos objetos devem ser exclu?dos de uma s? vez n?o h? necessidade de excluir um por um. Basta utilizar a fun??o `ls` conjugada com a `rm` da seguinte forma:


~~~
rm(list = ls())
~~~
{: .r}

Neste caso duas fun??es foram utilizadas em conjunto. Sempre que este for o caso o que est? localizado dentro do par?ntesis mais interno ser? avaliado primeiro e assim em diante.

No caso acima foi especificado que o resultado da fun??o `ls` deveria ser usado na forma de lista `list` como argumento da fun??o `rm`. Quando destinados valores de argumentos a fun??es por nome, o operador a ser utilizado dever? ser o s?mbolo `=`.

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
> Fique atento pois o R poder? fazer algo inesperado.
> Erros, como no caso acima, ocorrem quando o R n?o consegue efetuar as opera??es.
> Avisos, por outro lado, indicam que o R conseguiu efetuar as opera??es, mas algo  provavelmente n?o ocorreu como esperado.
>
> Em ambos os casos, as mensagens que o R fornece ajudam a consertar o problema.
>
{: .callout}

## Pacotes do R

? poss?vel adicionar fun??es ao R atrav?s da escrita de pacote escritos por voc? mesmo, ou por pacotes escritos por outras pessoas. Hoje existem mais de 7000 pacotes dispon?veis no CRAN (*the comprehensive R archive network*). O R e o Rstudio possuem elevada funcionalidade no quesito de gerenciamento de pacotes:

* ? poss?vel ver quais pacotes est?o instalados. Para isso devemos digitar `installed.packages()`.
* ? poss?vel instalar novos pacotes ao digitar `install.packages("packagename")`, onde `packagename` ? o nome do pacote a ser instalado. 
* ? poss?vel atualizar pacotes j? instalados. A fun??o utilizada ? `update.packages()`.
* ? poss?vel remover pacotes com a fun??o `remove.packages("packagename")`. 
* ? poss?vel tornar um pacote habilitado ao uso. A fun??o utilizada neste caso ? `library(packagename)`.

> ## Desafio 1
>
> Quais nomes a seguir s?o v?lidos para vari?veis no R ?
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
> > ## Solu??o do desafio 1
> >
> > Os nomes a seguir s?o v?lidos:
> > 
> > ~~~
> > min_height
> > max.height
> > MaxLength
> > celsius2kelvin
> > ~~~
> > {: .r}
> >
> > O nome a seguir cria uma vari?vel escondida
> > 
> > ~~~
> > .mass
> > ~~~
> > {: .r}
> >
> > Os nomes a seguir n?o podem ser usados para nomear objetos
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
> Quais ser?o os valores das vari?veis definidas abaixo ?
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
> > ## Solu??o do desafio 2
> >
> > 
> > ~~~
> > mass <- 47.5
> > ~~~
> > {: .r}
> > Este comando atribui o valor 47.5 para a vari?vel ` mass` for the variable mass
> >
> > 
> > ~~~
> > age <- 122
> > ~~~
> > {: .r}
> > Este comando atribiu o valor 122 para a vari?vel ` age` 
> >
> > 
> > ~~~
> > mass <- mass * 2.3
> > ~~~
> > {: .r}
> > O valor da vari?vel ` mass`ser? dividido por 2.3 e a vari?vel ` mass` passar? a ter um novo valor.
> > 
> >
> > 
> > ~~~
> > age <- age - 20
> > ~~~
> > {: .r}
> > Ser? realizada uma subtra??o de 20 unidades na vari?vel ` age`.
> > 
> {: .solution}
{: .challenge}


> ## Desafio 3
>
> Rode os c?digos do desafio anterior, escreva um comando e compare massa com idade. 
> A massa ? maior que a idade ?
>
> > ## Solu??o do desafio 3
> >
> > Uma maneira de realizar esta tarefa ? utilizar `>`:
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
> > O resultado a ser retornado deve ser `TRUE` porque a vari?vel ` mass` ? maior que ` age`.
> {: .solution}
{: .challenge}


> ## Desafio 4
>
> Limpe o seu ambiente de trabalho. Delete as vari?veis massa e idade.
>
>
> > ## Solu??o do desafio 4
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
> > ## Solu??o do desafio 5
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
