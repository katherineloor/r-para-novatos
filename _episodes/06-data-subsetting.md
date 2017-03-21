---
title: Subsetting Data
teaching: 35
exercises: 15
questions:
- "How can I work with subsets of data in R?"
objectives:
- "To be able to subset vectors, factors, matrices, lists, and data frames"
- "To be able to extract individual and multiple elements: by index, by name, using comparison operations"
- "To be able to skip and remove elements from various data structures."
keypoints:
- "Indexing in R starts at 1, not 0."
- "Access individual values by location using `[]`."
- "Access slices of data using `[low:high]`."
- "Access arbitrary sets of data using `[c(...)]`."
- "Use `which` to select subsets of data based on value."
source: Rmd
---




O R tem vários operadores poderosos para subconjuntos e o domínio deles irá
permitir que você facilmente utilize operações complexas em qualquer tipo de
conjunto de dados.

Existem seis diferentes maneiras de nós criarmos subconjuntos de qualquer tipo de
objeto, e três diferentes operadores de subconjuntos para as diferentes
estruturas de dados.

Vamos começar com o carro chefe do R: vetores atômicos.


~~~
x <- c(5.4, 6.2, 7.1, 4.8, 7.5)
names(x) <- c('a', 'b', 'c', 'd', 'e')
x
~~~
{: .r}



~~~
  a   b   c   d   e 
5.4 6.2 7.1 4.8 7.5 
~~~
{: .output}

Então, agora que nós criamos um vetor *dummy* para brincar, como nós temos acesso
ao seu conteúdo?

## Acessando elementos usando seus índices

Para extrair elementos de um vetor nós podemos dar seus correspondentes índices,
começando por um:


~~~
x[1]
~~~
{: .r}



~~~
  a 
5.4 
~~~
{: .output}


~~~
x[4]
~~~
{: .r}



~~~
  d 
4.8 
~~~
{: .output}

Isso pode parecer diferente, mas o operador par de colchetes é uma função. Para
vetores atômicos (e matrizes), isso significa "me passe o *n*-ésimo elemento".

Nós podemos pedir por múltiplos elementos de uma única vez:


~~~
x[c(1, 3)]
~~~
{: .r}



~~~
  a   c 
5.4 7.1 
~~~
{: .output}

Ou fatias do vetor:


~~~
x[1:4]
~~~
{: .r}



~~~
  a   b   c   d 
5.4 6.2 7.1 4.8 
~~~
{: .output}

O operador : cria uma sequência de números do elemento da esquerda até o da
direita.


~~~
1:4
~~~
{: .r}



~~~
[1] 1 2 3 4
~~~
{: .output}



~~~
c(1, 2, 3, 4)
~~~
{: .r}



~~~
[1] 1 2 3 4
~~~
{: .output}


Nós podemos pedir pelo mesmo elemento múltiplas vezes:


~~~
x[c(1,1,3)]
~~~
{: .r}



~~~
  a   a   c 
5.4 5.4 7.1 
~~~
{: .output}

Se nós pedirmos por um número fora do vetor, o R retornará valores faltantes:


~~~
x[6]
~~~
{: .r}



~~~
<NA> 
  NA 
~~~
{: .output}

Este é um vetor de comprimento um contendo um `NA`, cujo nome é também `NA`.

Se nós pedirmos pelo elemento de índice 0, nós temos um vetor vazio:


~~~
x[0]
~~~
{: .r}



~~~
named numeric(0)
~~~
{: .output}

> ## Numeração de vetores no R começa em 1
>
> Em várias linguagens de programação (C e python, por exemplo), o primeiro
> elemento de um vetor possuí um indexador igual a 0. Em R, o primeiro elemento
> é 1.
{: .callout}

## Pulando e removendo elementos

Se nós usarmos como indexador de um vetor um número negativo, o R irá retornar
todo elemento *exceto* o elemento específicado:


~~~
x[-2]
~~~
{: .r}



~~~
  a   c   d   e 
5.4 7.1 4.8 7.5 
~~~
{: .output}


Nós podemos pular múltiplos elementos:


~~~
x[c(-1, -5)]  # ou x[-c(1,5)]
~~~
{: .r}



~~~
  b   c   d 
6.2 7.1 4.8 
~~~
{: .output}

> ## Dica: Ordem de operações
>
> Uma experiência em comum para os novatos ocorre quando se tenta pular
> pedaços de um vetor. A maioria das pessoas primeiro tenta negar uma
> sequência, como por exemplo:
>
> 
> ~~~
> x[-1:3]
> ~~~
> {: .r}
>
> Isto retorna uma espécie de erro crítico:
>
> 
> ~~~
> Error in x[-1:3]: only 0's may be mixed with negative subscripts
> ~~~
> {: .error}
>
> Mas lembre da ordem das operações. : é realmente uma função, então o que
> acontece é que ele pega seu primento argumento como -1, o segundo como 3,
> e então gera a sequência de números: `c(-1, 0, 1, 2, 3)`.
>
> A solução correta é colocar o que a função chama em parênteses, assim o
> operador `-` é aplicado ao resultado:
>
> 
> ~~~
> x[-(1:3)]
> ~~~
> {: .r}
> 
> 
> 
> ~~~
>   d   e 
> 4.8 7.5 
> ~~~
> {: .output}
{: .callout}


Para remover elementos de um vetor, nós precisamos atribuir o resultado
novamente na variável:


~~~
x <- x[-4]
x
~~~
{: .r}



~~~
  a   b   c   e 
5.4 6.2 7.1 7.5 
~~~
{: .output}

> ## Desafio 1
>
> Dado o código a seguir:
>
> 
> ~~~
> x <- c(5.4, 6.2, 7.1, 4.8, 7.5)
> names(x) <- c('a', 'b', 'c', 'd', 'e')
> print(x)
> ~~~
> {: .r}
> 
> 
> 
> ~~~
>   a   b   c   d   e 
> 5.4 6.2 7.1 4.8 7.5 
> ~~~
> {: .output}
>
> Forneça ao menos 3 diferentes comandos que produzem o seguinte resultado:
>
> 
> ~~~
>   b   c   d 
> 6.2 7.1 4.8 
> ~~~
> {: .output}
>
> Depois de você encontrar 3 diferentes comandos, compare anotações com seu colega. Vocês tiveram diferentes estratégias?
>
> > ## Resposta do desafio 1
> >
> > 
> > ~~~
> > x[2:4]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> >   b   c   d 
> > 6.2 7.1 4.8 
> > ~~~
> > {: .output}
> > 
> > ~~~
> > x[-c(1,5)]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> >   b   c   d 
> > 6.2 7.1 4.8 
> > ~~~
> > {: .output}
> > 
> > ~~~
> > x[c("b", "c", "d")]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> >   b   c   d 
> > 6.2 7.1 4.8 
> > ~~~
> > {: .output}
> > 
> > ~~~
> > x[c(2,3,4)]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> >   b   c   d 
> > 6.2 7.1 4.8 
> > ~~~
> > {: .output}
> >
> {: .solution}
{: .challenge}

## Subconjuntos por nome

Nós podemos extrair elementos através de seu nome, invés do indexador:


~~~
x[c("a", "c")]
~~~
{: .r}



~~~
  a   c 
5.4 7.1 
~~~
{: .output}

Esta é uma maneira muito mais confiável de dividir objetos: a posição de vários
elementos pode frequentemente mudar quando encadeamos operações de subconjuntos,
mas os nomes sempre permanecerão os mesmos!

Infelizmente não podemos pular ou remover elementos tão facilmente.

Para pular (ou remover) um único elemento nomeado:


~~~
x[-which(names(x) == "a")]
~~~
{: .r}



~~~
  b   c   d   e 
6.2 7.1 4.8 7.5 
~~~
{: .output}

A função `which` retorna os índices de todos os elementos `TRUE` de seu
argumento. Lembre que expressões são avaliadas antes de serem passadas para
funções. Vamos mostrar passo à passo para ficar claro o que está acontecendo.

Primeiro isso acontece:


~~~
names(x) == "a"
~~~
{: .r}



~~~
[1]  TRUE FALSE FALSE FALSE FALSE
~~~
{: .output}

O operador condicional é aplicado a todo nome do vetor `x`. Apenas o primeiro
nome é `a` e por isso seu elemento é `TRUE`.

`which` então converte isto para um indexador:


~~~
which(names(x) == "a")
~~~
{: .r}



~~~
[1] 1
~~~
{: .output}



Apenas o primeiro elemento é `TRUE`, então `which` retorna 1. Agora que temos
índices podemos pular um elemento, pois temos um index negativo!

Pular múltiplos índices nomeados é similar, mas usa um operador de comparação
diferente:


~~~
x[-which(names(x) %in% c("a", "c"))]
~~~
{: .r}



~~~
  b   d   e 
6.2 4.8 7.5 
~~~
{: .output}

O `%in%` vai em cada elemento de seu argumento à esquerda, nesse caso os nomes
de `x`, e pergunta, "Esse elemento ocorre no segundo argumento?"

> ## Desafio 2
>
> Rode o código a seguir para definir o vetor `x` como acima:
>
> 
> ~~~
> x <- c(5.4, 6.2, 7.1, 4.8, 7.5)
> names(x) <- c('a', 'b', 'c', 'd', 'e')
> print(x)
> ~~~
> {: .r}
> 
> 
> 
> ~~~
>   a   b   c   d   e 
> 5.4 6.2 7.1 4.8 7.5 
> ~~~
> {: .output}
>
> Dado este vetor `X`, o que você espera que o código a seguir faça?
>
>~~~
> x[-which(names(x) == "g")]
>~~~
>{: .r}
>
> Teste este comando e veja o que você recebe. É o que você esperava?
> Por que nós recebemos este resultado? (Dica: teste cada parte do comando - esta é uma ferramenta útil de depuração, *debugging*)
>
> Quais das afirmações a seguir são verdadeiras:
>
> * A) se não existem valores `TRUE` passados ao `witch`, um vetor vazio é retornado
> * B) se não existem valores `TRUE` passados ao `witch`, uma mensagem de erro é retornada
> * C) `integer` é um vetor vazio
> * D) fazendo um vetor vazio negativo é retornado um vetor "com todo mundo"
> * E) `x[]` dá o mesmo resultado que `x[integer()]`
>
> > ## Resposta do desafio 2
> >
> > A e C estão corretas.
> >
> > O comando `which` retorna o indexador de todo valor `TRUE` em sua entrada,
> > *input*. O comando `names(x) == "g"` não retorna cada valor `TRUE`. Se não 
> > existem valores `TRUE` passados ao comando `which`, é retornado um vetor vazio.
> > Transformado este vetor em negativo com um sinal de menos não altera seu
> > significado. Pois nós usamos este vetor vazio para recuperar valores de `x`, o
> > que produz um vetor numérico vazio. Ele é um vetor `numérico nomeado` vazio
> > porque o tipo do vetor `x` é "numérico nomeado" desde que nós atribuímos nomes
> > aos valores (tente `str(x)`).
> {: .solution}
{: .challenge}

> ## Dica: Nomes não-únicos
>
> Você deve estar consciente de que é possível que múltiplos elementos de um vetor
> tenham o mesmo nome. (Para um *data frame*, colunas podem ter o mesmo nome -
> embora o R tente evitar isso - mas o nome das linhas deve ser único). Considere
> estes exemplos:
>
>
>~~~
> x <- 1:3
> x
>~~~
>{: .r}
>
>
>
>~~~
>[1] 1 2 3
>~~~
>{: .output}
>
>
>
>~~~
> names(x) <- c('a', 'a', 'a')
> x
>~~~
>{: .r}
>
>
>
>~~~
>a a a 
>1 2 3 
>~~~
>{: .output}
>
>
>
>~~~
> x['a']  # retorna apenas o primeiro valor
>~~~
>{: .r}
>
>
>
>~~~
>a 
>1 
>~~~
>{: .output}
>
>
>
>~~~
> x[which(names(x) == 'a')]  # retorna todos os três valores
>~~~
>{: .r}
>
>
>
>~~~
>a a a 
>1 2 3 
>~~~
>{: .output}
{: .callout}


> ## Dica: Tendo ajuda com operadores
>
> Lembre-se que você pode procurar por ajuda para operadores colocando eles entre aspas:
> `help("%in%")` ou `?"%in%"`.
>
{: .callout}


Então por que não podemos usar `==` igual antes? Essa é uma excelente pergunta.

Vamos dar uma olhada no componente de comparação deste código:


~~~
names(x) == c('a', 'c')
~~~
{: .r}



~~~
Warning in names(x) == c("a", "c"): longer object length is not a multiple
of shorter object length
~~~
{: .error}



~~~
[1]  TRUE FALSE  TRUE
~~~
{: .output}

Obviamente "c" está como um dos nomes de `x`, então por que isto não funcionou?
`==` trabalha ligeiramente diferente de `%in%`. Ele irá comparar cada elemento de
seu argumento a esquerda com o correspondente elemento do argumento a direita.

Aqui uma ilustração:


~~~
c("a", "b", "c", "e")  # nomes de x
   |    |    |    |    # Os elementos == são comparados
c("a", "c")
~~~
{: .r}

Quando um vetor é menor que o outro, ele é *reciclado*:


~~~
c("a", "b", "c", "e")  # nomes de x
   |    |    |    |    # Os elementos == são comparados
c("a", "c", "a", "c")
~~~
{: .r}

Neste caso o R simplesmente repete c("a", "c") duas vezes. Se o
tamando do maior vetor não for múltiplo do tamanho do menor vetor,
o R também irá retornar uma mensagem de aviso:


~~~
names(x) == c('a', 'c', 'e')
~~~
{: .r}



~~~
[1]  TRUE FALSE FALSE
~~~
{: .output}

Essa diferença entre `==` e `%in%` é importante de se lembrar, pois
pode gerar *bugs* sutis e difíceis de encontrar!

## Subconjuntos através de outras operações lógicas

Nós podemos também criar subconjuntos mais simples atráves de
operações lógicas:


~~~
x[c(TRUE, TRUE, FALSE, FALSE)]
~~~
{: .r}



~~~
a a 
1 2 
~~~
{: .output}

Note que neste caso, o vetor lógico é também reciclado para o
tamanho do vetor que estamos fazendo o subconjunto!


~~~
x[c(TRUE, FALSE)]
~~~
{: .r}



~~~
a a 
1 3 
~~~
{: .output}

Já que operadores de comparação avaliam para vetores lógicos, nós
podemos usá-los para sucintamente criar subconjuntos de vetores:


~~~
x[x > 7]
~~~
{: .r}



~~~
named integer(0)
~~~
{: .output}

> ## Dica: Combinanndo condições lógicas
>
> Existem várias situações em que você vai desejar combinar múltiplos critérios
> lógicos. Por exemplo, nós podemos querer encontrar todos os países da Ásia *ou*
> da Europa *e* que possuem expectativa de vida dentro de certo intervalo.
> Existem no R várias operações para combinar vetores lógicos:
>
>  * `&`, o operador lógico "E": retorna `TRUE` se tanto a esquerda quanto a
     direita forem `TRUE`.
>  * `|`, o operador lógico "OU": retorna `TRUE`, se a esquerda ou direita (ou 
     ambas) forem `TRUE`.
>
> A regra de reciclagem se aplica em ambos, assim, `TRUE` `&`
> `c(TRUE, FALSE, TRUE)` compara o primeiro `TRUE` na esquerda do sinal `&` com
> cada uma das três condições da direita.
>
> As vezes você pode ver `&&` e `||` invés de `&` e `|`. Estes operadores não usam
> a regra de reciclagem: eles olham apenas para o primeiro elemento de cada vetor
> e ignoram os demais elementos. Operadores longos são usados principalmente em
> programação, invés de análise de dados.
>
>  * `!`, o operador lógico "NÃO": converte `TRUE` em `FALSE` e `FALSE` em `TRUE`.
>    Ele pode negar uma única condição lógica (por exemplo, `!TRUE` se torna
>    `FALSE`), ou um vetor inteiro de condições (por exemplo, `!c(TRUE, FALSE)` se
>    torna `c(FALSE, TRUE`).
>
> Adicionalmente, você pode comparar os elementos dentro de um único vetor usando a
> função `all` (que retorna `TRUE` se todos os elementos do vetor forem `TRUE`) e
> a função `any` (que retorna `TRUE` se um ou mais elementos do vetor for `TRUE`).
{: .callout}

> ## Desafio 3
>
> Dado o código a seguir:
>
> 
> ~~~
> x <- c(5.4, 6.2, 7.1, 4.8, 7.5)
> names(x) <- c('a', 'b', 'c', 'd', 'e')
> print(x)
> ~~~
> {: .r}
> 
> 
> 
> ~~~
>   a   b   c   d   e 
> 5.4 6.2 7.1 4.8 7.5 
> ~~~
> {: .output}
>
> Escreva um comando que retorne os valores em `x` que são maiores que 4 e menores que 7.
>
> > ## Resposta do desafio 3
> >
> > 
> > ~~~
> > x_subset <- x[x<7 & x>4]
> > print(x_subset)
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> >   a   b   d 
> > 5.4 6.2 4.8 
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

## Lidando com valores especiais

Em algum dado momento você irá encontrar funções em R que não sabem lidar com
dados faltantes, infinito, ou indefinidos.

Existem funções especiais que você pode usar para filtrar estes dados:

 * `is.na` irá retornar todas as posições do vetor, matriz, ou *data frame*
   que contém `NA`.
 * Da mesma maneira, `is.nan` e `is.infinite` fará o mesmo para `NaN` e `Inf`.
 * `is.finite` irá retornar todas as posições do vetor, matriz, ou *data frame*
  que não contém `NA`, `NaN` ou `Inf`.
 * `na.omit` irá filtrar todos os valores faltantes de um vetor

## Subconjuntos de fatores

Agora que já exploramos as diferentes maneiras de fazer subconjuntos de vetores,
como nós fazemos subconjuntos de outras estruturas de dados?

Subconjuntos de fatores funciona da mesma maneira que os subconjuntos de vetores.


~~~
f <- factor(c("a", "a", "b", "c", "c", "d"))
f[f == "a"]
~~~
{: .r}



~~~
[1] a a
Levels: a b c d
~~~
{: .output}



~~~
f[f %in% c("b", "c")]
~~~
{: .r}



~~~
[1] b c c
Levels: a b c d
~~~
{: .output}



~~~
f[1:3]
~~~
{: .r}



~~~
[1] a a b
Levels: a b c d
~~~
{: .output}

Uma importante nota é que pular elementos não irá remover o nível (*level*),
mesmo que não existam mais elementos dessa categoria no fator:


~~~
f[-3]
~~~
{: .r}



~~~
[1] a a c c d
Levels: a b c d
~~~
{: .output}

## Subconjuntos de matrizes

Subconjuntos de matrizes são criados usando a função `[`. Neste caso é usado
dois argumentos: o primeiro aplicado às linhas, o segundo às colunas:


~~~
set.seed(1)
m <- matrix(rnorm(6*4), ncol=4, nrow=6)
m[3:4, c(3,1)]
~~~
{: .r}



~~~
            [,1]       [,2]
[1,]  1.12493092 -0.8356286
[2,] -0.04493361  1.5952808
~~~
{: .output}

Você pode deixar o primeiro ou segundo argumento em branco para recuperar todas
as linhas ou colunas respectivamente:


~~~
m[, c(3,4)]
~~~
{: .r}



~~~
            [,1]        [,2]
[1,] -0.62124058  0.82122120
[2,] -2.21469989  0.59390132
[3,]  1.12493092  0.91897737
[4,] -0.04493361  0.78213630
[5,] -0.01619026  0.07456498
[6,]  0.94383621 -1.98935170
~~~
{: .output}

Se nós acessarmos apenas uma linha ou coluna, o R irá automaticamente converter
o resultado para um vetor:


~~~
m[3,]
~~~
{: .r}



~~~
[1] -0.8356286  0.5757814  1.1249309  0.9189774
~~~
{: .output}

Se você quiser manter a saída como uma matriz, você precisa especificar um 
*terceiro* argumento, `drop = FALSE`:


~~~
m[3, , drop=FALSE]
~~~
{: .r}



~~~
           [,1]      [,2]     [,3]      [,4]
[1,] -0.8356286 0.5757814 1.124931 0.9189774
~~~
{: .output}

Ao contrário de vetores, se nós tentarmos acessar uma linha ou coluna fora da
matriz, o R irá retornar um erro:


~~~
m[, c(3,6)]
~~~
{: .r}



~~~
Error in m[, c(3, 6)]: subscript out of bounds
~~~
{: .error}

> ## Dica: Matrizes de maior dimensão
>
> ao se trabalhar com matrizes de maiores dimensão, cada argumento para `[`
> corresponde a uma dimensão. Por exemplo, numa matriz 3D, os primeiros três
> argumentos correspondem as linhas, colunas, e nível da dimensão.
>
{: .callout}

Por matrizes serem vetores, nós podemos também criar subconjuntos usando apenas
um argumento:


~~~
m[5]
~~~
{: .r}



~~~
[1] 0.3295078
~~~
{: .output}


Isto usualmente não é útil, e frequentemente confundem na hora de ler. Contudo,
é útil para notar que matrizes são estabelecidas por padrão (*default*) no
formato colunas primeiro, *column-major*. Isso significa que os vetores são
arrumados pelas colunas:


~~~
matrix(1:6, nrow=2, ncol=3)
~~~
{: .r}



~~~
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
~~~
{: .output}

Se você deseja preencher a matriz pelas linhas, use `byrow=TRUE`:


~~~
matrix(1:6, nrow=2, ncol=3, byrow=TRUE)
~~~
{: .r}



~~~
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
~~~
{: .output}

Subconjuntos de matrizes também podem ser criados pelos nomes das linhas e 
colunas, invés do índice das linhas e colunas.

> ## Desafio 4
>
> Dado o seguinte código:
>
> 
> ~~~
> m <- matrix(1:18, nrow=3, ncol=6)
> print(m)
> ~~~
> {: .r}
> 
> 
> 
> ~~~
>      [,1] [,2] [,3] [,4] [,5] [,6]
> [1,]    1    4    7   10   13   16
> [2,]    2    5    8   11   14   17
> [3,]    3    6    9   12   15   18
> ~~~
> {: .output}
>
> 1. Qual dos seguintes comandos irá extrair os valores 11 e 14?
>
> A. `m[2,4,2,5]`
>
> B. `m[2:5]`
>
> C. `m[4:5,2]`
>
> D. `m[2,c(4,5)]`
>
> > ## Resposta do desafio 4
> >
> > D
> {: .solution}
{: .challenge}


## Subconjuntos de listas

Agora nós vamos introduzir alguns novos operadores para subconjuntos. Existem
três funções usadas para fazer subconjuntos de listas. `[`, como vimos para
vetores atômicos e matrizes, assim como `[[` e `$`.

Usando `[` sempre será retornada uma lista. Se você quiser um *subconjunto* de
uma lista, mas não *extrair* um elemento, então você provavelmente irá usar `[`.


~~~
xlist <- list(a = "Software Carpentry", b = 1:10, data = head(iris))
xlist[1]
~~~
{: .r}



~~~
$a
[1] "Software Carpentry"
~~~
{: .output}

Isto retorna uma *lista com um elemento*.

Nós podemos selecionar elementos de uma lista exatamente da mesma maneira usada
com vetores atômicos, isto é, `[`. Operadores de comparação entretanto não
funcionam por não serem recursivos, eles irão testar uma condição na estrutura de
dados em cada elemento da lista, não no elemento indivual dentro dessa estrutura
de dados.


~~~
xlist[1:2]
~~~
{: .r}



~~~
$a
[1] "Software Carpentry"

$b
 [1]  1  2  3  4  5  6  7  8  9 10
~~~
{: .output}

Para extrair elementos individuais de uma lista, você precisa usar a função dois
pares de colchetes: `[[`.


~~~
xlist[[1]]
~~~
{: .r}



~~~
[1] "Software Carpentry"
~~~
{: .output}

Repare que agora o resultado é um vetor, não uma lista.

Você não pode extrair mais de elemento de uma vez:


~~~
xlist[[1:2]]
~~~
{: .r}



~~~
Error in xlist[[1:2]]: subscript out of bounds
~~~
{: .error}

Nem usar isso para pular elementos:


~~~
xlist[[-1]]
~~~
{: .r}



~~~
Error in xlist[[-1]]: attempt to select more than one element in get1index <real>
~~~
{: .error}

Mas você pode usar nomes tanto para fazer subconjuntos quanto para extrair
elementos:


~~~
xlist[["a"]]
~~~
{: .r}



~~~
[1] "Software Carpentry"
~~~
{: .output}

A função `$` é uma maneira abreviada de extrair elementos pelo nome:


~~~
xlist$data
~~~
{: .r}



~~~
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
~~~
{: .output}

> ## Desafio 5
> Dada a seguinte lista:
>
> 
> ~~~
> xlist <- list(a = "Software Carpentry", b = 1:10, data = head(iris))
> ~~~
> {: .r}
>
> Usando seus conhecimentos de subconjuntos de listas e vetores, extraia o número
> 2 de xlist. Dica: o número 2 está contido dentro do item "b" da lista.
>
> > ## Resposta do desafio 5
> >
> > 
> > ~~~
> > xlist$b[2]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> > [1] 2
> > ~~~
> > {: .output}
> > 
> > ~~~
> > xlist[[2]][2]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> > [1] 2
> > ~~~
> > {: .output}
> > 
> > ~~~
> > xlist[["b"]][2]
> > ~~~
> > {: .r}
> > 
> > 
> > 
> > ~~~
> > [1] 2
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}


> ## Desafio 6
> Dado um modelo linear:
>
> 
> ~~~
> mod <- aov(pop ~ lifeExp, data=gapminder)
> ~~~
> {: .r}
>
> Extraia os graus de liberdade residuais (dica: `attributes()` irá ajudar você)
>
> > ## Resposta do desafio 6
> >
> > 
> > ~~~
> > attributes(mod) ## `df.residual` é um dos nomes de `mod`
> > ~~~
> > {: .r}
> > 
> > ~~~
> > mod$df.residual
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}


## *Data frames*

Lembre-se que *data frames* são listas disfarçadas, então regras similares se
aplicam. Contudo, eles também são objetos de duas dimensões:

`[` com um argumento irá agir da mesma forma que em listas, em que cada elemento
da lista corresponde a uma coluna. O objeto resultante será um *data frame*:


~~~
head(gapminder[3])
~~~
{: .r}



~~~
       pop
1  8425333
2  9240934
3 10267083
4 11537966
5 13079460
6 14880372
~~~
{: .output}

Similarmente, `[[` irá agir para extrair uma *única coluna*:


~~~
head(gapminder[["lifeExp"]])
~~~
{: .r}



~~~
[1] 28.801 30.332 31.997 34.020 36.088 38.438
~~~
{: .output}

E `$` fornece um caminho mais curto e conveniente para extrair colunas pelo nome:


~~~
head(gapminder$year)
~~~
{: .r}



~~~
[1] 1952 1957 1962 1967 1972 1977
~~~
{: .output}

Com dois argumentos, `[` se comporta da mesma maneira que em matrizes:


~~~
gapminder[1:3,]
~~~
{: .r}



~~~
      country year      pop continent lifeExp gdpPercap
1 Afghanistan 1952  8425333      Asia  28.801  779.4453
2 Afghanistan 1957  9240934      Asia  30.332  820.8530
3 Afghanistan 1962 10267083      Asia  31.997  853.1007
~~~
{: .output}

Se nós selecionarmos uma única linha, o resultado será um *data frame* (pois os
elementos são de tipos variados):


~~~
gapminder[3,]
~~~
{: .r}



~~~
      country year      pop continent lifeExp gdpPercap
3 Afghanistan 1962 10267083      Asia  31.997  853.1007
~~~
{: .output}

Mas para uma única coluna o resultado será um vetor (isso pode ser alterado com o
terceiro argumento, `drop = FALSE`).

> ## Desafio 7
>
> Conserte cada um dos seguintes subconjuntos de *data frames* com erros:
>
> 1. Extraia observações coletadas no ano de 1957
>
>    
>    ~~~
>    gapminder[gapminder$year = 1957,]
>    ~~~
>    {: .r}
>
> 2. Extraia todas as colunas exceto 1 até 4
>
>    
>    ~~~
>    gapminder[,-1:4]
>    ~~~
>    {: .r}
>
> 3. Extraia as linhas em que a expectativa de vida é maior que 80 anos
>
>    
>    ~~~
>    gapminder[gapminder$lifeExp > 80]
>    ~~~
>    {: .r}
>
> 4. Extraia a primeira linha, e a quarta e quinta coluna
>    (`lifeExp` e `gdpPercap`).
>
>    
>    ~~~
>    gapminder[1, 4, 5]
>    ~~~
>    {: .r}
>
> 5. Avançado: extraia as linhas que contêm informações sobre os anos 2002
>    e 2007
>
>    
>    ~~~
>    gapminder[gapminder$year == 2002 | 2007,]
>    ~~~
>    {: .r}
>
> > ## Resposta do desafio 7
> >
> > Conserte cada um dos seguintes subconjuntos de *data frames* com erros:
> >
> > 1. Extraia observações coletadas no ano de 1957
> >
> >    
> >    ~~~
> >    # gapminder[gapminder$year = 1957,]
> >    gapminder[gapminder$year == 1957,]
> >    ~~~
> >    {: .r}
> >
> > 2. Extraia todas as colunas exceto 1 até 4
> >
> >    
> >    ~~~
> >    # gapminder[,-1:4]
> >    gapminder[,-c(1:4)]
> >    ~~~
> >    {: .r}
> >
> > 3. Extraia as linhas em que a expectativa de vida é maior que 80 anos
> >
> >    
> >    ~~~
> >    # gapminder[gapminder$lifeExp > 80]
> >    gapminder[gapminder$lifeExp > 80,]
> >    ~~~
> >    {: .r}
> >
> > 4. Extraia a primeira linha, e a quarta e quinta coluna
> >    (`lifeExp` e `gdpPercap`).
> >
> >    
> >    ~~~
> >    # gapminder[1, 4, 5]
> >    gapminder[1, c(4, 5)]
> >    ~~~
> >    {: .r}
> >
> > 5. Avançado: extraia as linhas que contêm informações sobre os anos 2002
> >    e 2007
> >
> >     
> >     ~~~
> >     # gapminder[gapminder$year == 2002 | 2007,]
> >     gapminder[gapminder$year == 2002 | gapminder$year == 2007,]
> >     gapminder[gapminder$year %in% c(2002, 2007),]
> >     ~~~
> >     {: .r}
> {: .solution}
{: .challenge}

> ## Desafio 8
>
> 1. Por que `gapminder[1:20]` retorna um erro? Como isso difere de `gapminder[1:20, ]`?
>
>
> 2. Crie um novo `data.frame` chamado `gapminder_small` que contenha apenas as linhas
> >  1 até 9 e 19 até 23. Você pode fazer isso em um ou dois passos.
>
> > ## Resposta do desafio 8
> >
> > 1. `gapminder` é um `data.frame`, então ele precisa de duas dimensões para ter um subconjunto. `gapminder[1:20, ]` diz para selecionar as primeiras 20 linhas e todas as colunas.
> >
> > 2. 
> >
> > 
> > ~~~
> > gapminder_small <- gapminder[c(1:9, 19:23),]
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}
