---
layout: page
title: 103.7 Buscar texto em arquivos usando expressões regulares (Peso 2)
permalink: /103/103-7-buscando-texto-em-arquivos-usando-expressoes-regulares
---


Candidatos devem ter a capacidade de manipular arquivos e dados de texto utilizando expressões regulares. Esse tópico inclui a criação de de expressões regulares simples contendo diversas notações. E também inclui a utilização dessas expressões e ferramentas para realizar buscas pelo sistema de arquivos e conteúdo de arquivos.


O grep busca dentro de arquivos de input por linhas contendo um `match` de um determinado padrão. 
Se nehum arquivo é expecificado, o grep busca no stdin. POr padrão o grep imprimi as linhas onde o `match` é encontada.

Além disso vale ressaltar que os programas variantes como o `egrep`, `fgrep` and `rgrep` são o mesmo que `grep -E`, `grep -F`, e `grep -r`, respectivamente. Esses variantes foram depreciados, mas são disponibilizados por questões de compatibilidade.

## [grep](/103/103.7-buscando-texto-em-arquivos-usando-expressoes-regulares#grep)

O grep imprime as linhas de acordo com um padrão. No exemplo abaixo eu procuro pela palara `dolor` dentro do arquivo `texto.txt`

O comando é escrito dessa forma:

	grep [OPTIONS] PATTERN [FILE...]

Por exemplo:

	usuario@pc:$ grep dolor texto.txt

O comando retorna todas as linhas que contém a ocorrência da palavra `dolor`:

Lorem ipsum <span class="red">dolor</span> sit amet, consectetur adipiscing elit. Integer metus quam, dictum efficitur accumsan ac, fringilla sed leo. Phasellus ut sagittis quam, sit amet cursus ex. Aenean pharetra sagittis purus in sagittis. Aenean sagittis id risus at scelerisque. Pellentesque arcu ex, consectetur vitae tellus vel, efficitur tincidunt sapien. In finibus maximus erat, a maximus tortor volutpat eget. Quisque fringilla dignissim commodo. Cras id ipsum ac dui pretium elementum. Mauris aliquet enim nisi, vel pellentesque quam commodo egestas. Cras dapibus nec sem sed porttitor. Quisque eget elementum ante.
Etiam sed tortor consectetur, facilisis libero quis, consequat eros. Phasellus venenatis bibendum viverra. Sed varius varius massa eget suscipit. Curabitur lacinia eros bibendum sapien porttitor, et tincidunt tortor aliquet. Donec volutpat, leo eu vehicula condimentum, justo lacus tempus nunc, eget fermentum felis velit vel arcu. Sed id tempus magna. Aenean in urna et mi euismod luctus. Quisque ac massa leo. Nulla mollis nisl vitae felis faucibus luctus. Lorem ipsum <span class="red">dolor</span> sit amet, consectetur adipiscing elit. Fusce eget nisl fringilla <span class="red">dolor</span> faucibus sagittis.
Suspendisse in velit a <span class="red">dolor</span> convallis bibendum faucibus et turpis. Nullam rhoncus eros vel aliquet mattis. Phasellus auctor ante vel enim tincidunt, in lacinia metus semper. Vestibulum porta nec urna id blandit. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Vivamus elementum, mauris quis egestas tristique, urna augue tincidunt elit, eu sodales massa urna ut purus. Praesent pulvinar, justo sit amet blandit ultricies, odio justo volutpat <span class="red">dolor</span>, et ornare eros tellus nec neque. Donec tempus pulvinar arcu, ac scelerisque felis eleifend quis. Maecenas vitae ante ut turpis blandit hendrerit. Aenean orci nunc, commodo a diam consectetur, fringilla venenatis felis. Interdum et malesuada fames ac ante ipsum primis in faucibus. Pellentesque et commodo <span class="red">dolor</span>, tempus feugiat neque. Nullam vitae ex a turpis cursus eleifend in a ipsum.

Podemos utilizar o parametro `-c` que conta o número de vezes que a palavra é encontrada.

	usuario@pc:$ grep -c dolor texto.txt
	3

<blockquote>
Aqui surge um problema. O grep conta as ocorrências por linha mas a palavra `dolor` surge diversas verzes na mesma linha e o grep ignora isso. Como fazer para o grep contar o número de ocorrências mesmo que seja na mesma linha. Procurei na internet e não encontrei solução para isso
</blockquote>

## -v

Existe o paramêtro `-v` que inverte o resultado exibindo as linhas que o padrão não é encontrado:

	usuario@maquina:$ grep -v dolor texto.txt

## -i

Podemos também utilizar o parametro `-i` que realiza a busca de modo case insensitive:


	usuario@maquina:$ grep -i Lorem texto.txt	


<span class="red">Lorem</span> ipsum dolor sit amet, consectetur adipiscing elit. Integer metus quam, dictum efficitur accumsan ac, fringilla sed leo. Phasellus ut sagittis quam, sit amet cursus ex. Aenean pharetra sagittis purus in sagittis. Aenean sagittis id risus at scelerisque. Pellentesque arcu ex, consectetur vitae tellus vel, efficitur tincidunt sapien. In finibus maximus erat, a maximus tortor volutpat eget. Quisque fringilla dignissim commodo. Cras id ipsum ac dui pretium elementum. Mauris aliquet enim nisi, vel pellentesque quam commodo egestas. Cras dapibus nec sem sed porttitor. Quisque eget elementum ante.
Etiam sed tortor consectetur, facilisis libero quis, consequat eros. Phasellus venenatis bibendum viverra. Sed varius varius massa eget suscipit. Curabitur lacinia eros bibendum sapien porttitor, et tincidunt tortor aliquet. Donec volutpat, leo eu vehicula condimentum, justo lacus tempus nunc, eget fermentum felis velit vel arcu. Sed id tempus magna. Aenean in urna et mi euismod luctus. Quisque ac massa leo. Nulla mollis nisl vitae felis faucibus luctus. <span class="red">Lorem</span> ipsum dolor sit amet, consectetur adipiscing elit. Fusce eget nisl fringilla dolor faucibus sagittis.
Quisque malesuada sed metus eget fringilla. Curabitur blandit nisi id velit ornare, nec pulvinar dui gravida. Curabitur vel rhoncus lectus, vel accumsan ligula. In scelerisque, ipsum ac convallis scelerisque, nunc urna dapibus tellus, non tincidunt quam risus facilisis neque. Aenean in ante vitae mauris ultricies dapibus a eu magna. Curabitur aliquet faucibus nisl, eget pellentesque ante lacinia non. Nullam et leo condimentum, tempus ligula eu, semper massa. Mauris ut posuere lectus. Nulla tincidunt malesuada arcu, eu ultricies arcu varius a. Nulla id quam augue. Morbi ut mi sed dui egestas tempor. Nulla hendrerit pulvinar ex in luctus. Fusce bibendum quis velit ut molestie. Donec blandit volutpat purus in tristique. Morbi justo <span class="red">Lorem </span>ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum, <span class="red">lorem</span> mollis at ullamcorper ac, posuere mollis odio. Nam pharetra, turpis a pulvinar dapibus, velit libero pulvinar quam, eget commodo magna enim ut ipsum.

## Usando file globbing

O grep aceita o file `globbing` então podemos usar um asterisco para buscar em todos os arquivos do diretório corrente.

	usuario@maquina:$ grep -c Lorem *  
	outro-texto.txt:0
	grep: subdir: É uma directoria
	subdir:0
	texto-mais-simples.txt:0
	texto.txt:3

## -r

Podemos realizar buscas de forma recursiva usando `-r`. O Exemplo abaixo mostra a utilização da busca recursiva em conjunto com o `globbing`.


	alphabraga@sharp:~/lpi/grep$ grep -cr lorem * 
	outro-texto.txt:0
	subdir/another-subdir/texto.txt:1
	subdir/texto.txt:1
	texto-mais-simples.txt:0
	texto.txt:1

Existem várias vertentes de expressões regulares:

### grep -E

É o mesmo que utilizar o comando egrep seria um padrão chamado regular expression extented

## [egrep](/103/103.7-buscando-texto-em-arquivos-usando-expressoes-regulares#grep)


## [fgrep](/103/103.7-buscando-texto-em-arquivos-usando-expressoes-regulares#fgrep)


## [sed](/103/103.7-buscando-texto-em-arquivos-usando-expressoes-regulares#sed)


## [regex(7)](/103/103.7-buscando-texto-em-arquivos-usando-expressoes-regulares#regex-7)