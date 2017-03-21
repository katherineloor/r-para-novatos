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



































































































