---
layout: page
title: 103.4 Utilizar Fluxos Pipes e Redirecionamentos
permalink: 103/103-4-utilizar-fluxos-pipes-e-redirecionamentos
---

## bash

O bash é o nome do tipo de shell que estamos utilizando. O `bash`, tambem conhecido como Bourne Again Shell é o que é cobrado na LPI. Existem outros tipos de shell como `csh`, `sh`, `ksh`.

## Executando comandos em sequencia

O shell tambem tem a opção de executar os comandos em sequencia. E para isso nós utilizamos os operadores:

#### `;` : esse operador permite a execução em sequencia dos comandos, os comandos são simplesmnete executados não importando se eles esão corretos ou ate mesmo sua execução foi realizada com sucesso. Como no exemplo abaixo:

Comandos executados em sequencia

	usuario@maquina:$ comando1; comando2; comando3

### `&&` : Só executa o proximo comando caso o anterior tenha sido executado com sucesso

	usuario@maquina: dsdddddddddddddddddddddd

## echo 

Imprime uma string na tela

## env

Lista todas as variáveis de ambiente (environment) setadas:

	usuario@maquina:~ env #lista todos as variaveis de ambiente

Você pode tambem usar o comando `printenv` para exibir a mesma informação. 

Para remover uma variável utilizamos o comando: 

	usuario@maquina:$ unset NOMEVAR

## export 

Coloca uma variável no ambiente, para colocar ela definitivamente no enviroment você deve colocar esse export dentro de .bash_profile ou em /etc/environment .


## pwd 

pwd significa `print working directory`, ou seja, exibir o diretorio corrente


## set

O comando `set` quando executado sem parametros imprime variáveis de ambiente e variáveis locais além de funcões. O output dele é bem maior comparado ao `env`.

Acho que isso tá errado!!
**Comando relacionado com a capacidade de evitar sobrescrita de arquivos.**


## man 

Exibe os manuais dos comandos exemplo:

	man cp

Os dados refentes aos manuais ficam no diretorio /usr/share/man

## uname 

Exibe dados do so. Como versão do kernel distribuicao etc.

Com o parametro `-a` todas as informações, o `a` é de `all` .

	usuario@maquina:$ uname -a

	Linux sharp 4.13.0-37-generic #42~16.04.1-Ubuntu SMP Wed Mar 7 16:03:28 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux

Só a informação a respeito do release informe o `-r`

	usuario@maquina:$ uname -r

	4.13.0-37-generic


## history

O comando history é um comando interno do bash. Ele lista o conteudo do ~/.bash_history númerando as linhas. do output.

	usuario@maquina:$

	1115  ldd jekyll 
	1116  whereis jekyll 
	1117  ldd /usr/bin/jekyll
	1118  whereis java
	1119  ldd /usr/bin/java
	1120  whatis ldd
	1121  exit
	1122  cd Projects/my-awesome-site/
	1123  jekyll serve
	1124  whereis ln
	1125  ldd /bin/ln
	1126  locate sln
	1127  sln
	1128  cd /lib64/
	1129  ls -la
	1130  cat ld-linux-x86-64.so.2 
	1131  q!
	1132  sudo aptitude update


## .bash_history

É o arquivo que contem o historico dos comandos. É esse aquivo que exibido pelo comando `history` . 

### Diferença entre .bashrc e .bash_profile

O .bash_profile é executado quando é realizado o login no bash, já o .bashrc é executado sempre se abre o bash dentro do gnome. é comum ver distribuições que colocam no final do arquivo .bash_profile o comando "source ~/.bashrc" dessa forma variáveis e configrações definidas em ambos os arquivos ficam disponiveis.