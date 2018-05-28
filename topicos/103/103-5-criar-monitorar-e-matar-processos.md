---
layout: page
title: 103.5 Criar Monitorar e Matar Processos
permalink: 103/103-5-criar-monitorar-e-matar-processos
---

* Ter a capacidade de realizar gerenciamento de básico de processos;
* Executar processos em `foreground` e `background`;
* Fazer com que um programa continue sua execução mesmo após o logoff do usuário que executou o mesmo;
* Monitorar processos ativos;
* Selecionae e ordenar procesos para a visualização;
* Enviar sinais para processos;

## &

Ao executar um comando colocamos um `&` no final do comando para deixar o mesmo em `background`. Abaixo podemos observar que o comando `ping` esta sendo executado em `background`, porém o output do comando continua saindo para para o terminal. O que de certa forma pode prejudicar o usuário em digitar outros comandos, já que a intenção é mandar um determinado comando para ser execudo em background e ficar com o `bash` livre para a execução de outros comandos. 

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>ping 127.0.0.1  & </code>
[1] 4364
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.041 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.053 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.054 ms
64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.067 ms
</pre>

Para contornar essa situação podemos colocar todas as saídas para o `/dev/null` e assim o terminal fica livre.


<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>ping 127.0.0.1 &> /dev/null &</code>
[1] 4364
</pre>

## jobs

Comando `jobs` exibe todos os comando em execução em background: 

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>jobs</code>
[1]-  Executando              ping 8.8.8.8 &> /dev/null &
[2]+  Executando              ping 127.0.0.1 &> /dev/null &
</pre>

Com a opção `-l` são exibidos os PIDS dos processos.

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>jobs -l</code>
[1]-  6005 Executando              ping 127.0.0.1 &> /dev/null &
[2]+  6007 Executando              ping 8.8.8.8 &> /dev/null &
</pre>

## bg

O comando bg joga um comando para background. Ao executar o comando `gnome-calculator` o mesmo fica ocupando a sessão do terminal impedindo a execução de outros comandos. Mas podemos fazer a uma pausa em sua execução utilizando as teclas `CTRL+Z`, dessa forma o comando será `STOPED` como podemos ver abaixo:

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>gnome-calulator</code>
^Z
[3]+  Parado                  gnome-calculator
</pre>

Podemos observar que o mesmo esta parado

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>jobs -l</code>
[1]   6005 Executando              ping 127.0.0.1 &> /dev/null &
[2]-  6007 Executando              ping 8.8.8.8 &> /dev/null &
[3]+  6855 Parado                  gnome-calculator
</pre>


Executando o comando `bg` o mesmo é jogado em background e sua execução é continuada. 

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>bg</code>
</pre>

Nesse caso podemos utilizar o `bg +` ou ate mesmo o `bg 3`, como no fg.

## fg

O comando `fg` pega um comando que esta em background e joga o mesmo para o foreground.

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>jobs</code>
[1]-  Executando              ping 8.8.8.8 &> /dev/null &
[2]+  Executando              ping 127.0.0.1 &> /dev/null &
<code>fg %1</code>
ping 8.8.8.8 &> /dev/null
</pre>

O sinal de `%` pode ser omitido docomando bg.

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>jobs</code>
[1]-  Executando              ping 8.8.8.8 &> /dev/null &
[2]+  Executando              ping 127.0.0.1 &> /dev/null &
<code>fg 1</code>
ping 8.8.8.8 &> /dev/null
</pre>

Podemos também chamar um determinado comando usando o sinal de `+` ou `-`. Dessa forma com o `+`:



<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>fg +</code>
ping 127.0.0.1 &> /dev/null
</pre>

Com o `-`:

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>fg -</code>
ping 8.8.8.8 &> /dev/null
</pre>

## kill


## nohup

 O comando `nohup` ignora o sinal de `HUP` (hang up), fazendo com que seja possivel executar comandos em background ate mesmo se o terminal acabar, no caso um logoff.

O comando abaixo vai continuar em execução mesmo que o terminal seja fechado ou o usuário faça logoff.

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>nohup ping 127.0.0.1 &> /dev/null &</code>
[1] 4364
</pre>

## ps

Exibe informações sobre processos em execução. o **ps** pode trabalha com 3 tipos de parametros:

* Unix98, precedidos de um `-` e uma letra;
* BSD, composto apenas por letras;
* GNU Long Options, precedido com `--` e palavras;


O `ps` possui muitos parametros. Os principais cobrados na LPI são:



### u

Com esse parametro o ps exibe as informações em modo usuário. Mostrando mais dados no output. Veja abaixo o comando ps sem parametros

<pre class="command-line language-bash">
	<code>ps</code>
PID TTY          TIME CMD
3553 pts/2    00:00:00 zsh
9808 pts/2    00:00:00 ps
</pre>

Agora com o parametro `u`

<pre class="command-line language-bash">
	<code>ps u</code>
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
alphabr+  3553  0.0  0.0  47816  7032 pts/2    Ss   08:26   0:00 zsh
alphabr+  9879  0.0  0.0  37368  3244 pts/2    R+   10:00   0:00 ps u

</pre>

Com o parametro `u` são exibidos o nome do usuario, memoria etc...

### x

Exibe processos que não foram inicialiados pelo seu tty ou seja mais informações são exibidas.

<pre class="command-line language-bash">
	<code>ps ux</code>
alphabr+  1844  0.0  0.0  45276  4560 ?        Ss   May25   0:00 /lib/systemd/systemd --user
alphabr+  1845  0.0  0.0 245868  2656 ?        S    May25   0:00 (sd-pam)
alphabr+  1851  0.0  0.1 285192 13532 ?        SLl  May25   0:09 /usr/bin/gnome-keyring-daemon --daemonize --login
alphabr+  1857  0.0  0.2 419148 23680 ?        Ssl  May25   0:00 mate-session
alphabr+  1990  0.0  0.0  11136   312 ?        Ss   May25   0:00 /usr/bin/ssh-agent /usr/bin/dbus-launch --exit-with-session /usr/bin/im-launch mate-session
alphabr+  1993  0.0  0.0  43600  2568 ?        S    May25   0:00 /usr/bin/dbus-launch --exit-with-session /usr/bin/im-launch mate-session
alphabr+  1994  0.0  0.0  43744  4064 ?        Ss   May25   0:03 /usr/bin/dbus-daemon --fork --print-pid 5 --print-address 7 --session

</pre>

Veja a coluna 7. Ela só tem a informação `?` isso quer diser que os processos listados não foram originados do meu tty.

### a

Com o parametro `a` os processos de todos os usuário são listados. Veja abaixo:

<pre class="language-bash command-line">
	<code>ps uxa</code>
root       465  0.0  0.0      0     0 ?        S<   May25   0:00 [loop3]
root       477  0.0  0.0      0     0 ?        S<   May25   0:00 [loop4]
root       480  0.0  0.0      0     0 ?        S<   May25   0:00 [loop5]
www-data   492  0.0  0.3 489340 31904 ?        S    07:35   0:00 /usr/sbin/apache2 -k start
www-data   493  0.0  0.3 488788 27992 ?        S    07:35   0:00 /usr/sbin/apache2 -k start
www-data   494  0.0  0.3 415712 31068 ?        S    07:35   0:00 /usr/sbin/apache2 -k start
www-data   495  0.0  0.3 411268 25744 ?        S    07:35   0:00 /usr/sbin/apache2 -k start
www-data   496  0.0  0.1 410448 12424 ?        S    07:35   0:00 /usr/sbin/apache2 -k start
root       523  0.0  0.1  95392  9668 ?        Ss   07:35   0:00 /usr/sbin/cupsd -l
root       524  0.0  0.1 274960  9784 ?        Ssl  07:35   0:00 /usr/sbin/cups-browsed
root       546  0.0  0.0      0     0 ?        S    May25   0:00 [irq/129-mei_me]
root       553  0.0  0.0      0     0 ?        S<   May25   0:00 [cfg80211]
root       622  0.0  0.0      0     0 ?        S    May25   0:27 [irq/130-iwlwifi]
root       706  0.0  0.0      0     0 ?        S<   May25   0:00 [kworker/u17:0]
root       814  0.0  0.0      0     0 ?        S<   May25   0:00 [hci0]
root       815  0.0  0.0      0     0 ?        S<   May25   0:00 [hci0]
root       816  0.0  0.0      0     0 ?        S<   May25   0:00 [kworker/u17:2]
root      1102  0.0  0.0  29012  2988 ?        Ss   May25   0:00 /usr/sbin/cron -f
avahi     1111  0.0  0.0  45428  4092 ?        Ss   May25   0:01 avahi-daemon: running [helix.local]
</pre>

### -C

Podemos ainda usar o parametro -C que lista os processos pelo nome. Veja o exemplo abaixo:

<pre class="language-bash command-line">
	<code>ps u -C chrome</code>
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
alphabr+  5857  2.5  3.3 1298572 272384 ?      SLl  08:40   3:06 /opt/google/chrome/chrome
alphabr+  5868  0.0  0.6 419828 56000 ?        S    08:40   0:00 /opt/google/chrome/chrome --type=zygote
alphabr+  5872  0.0  0.1 419828 14104 ?        S    08:40   0:00 /opt/google/chrome/chrome --type=zygote
alphabr+  5924  2.2  1.8 589204 145444 ?       Sl   08:40   2:44 /opt/google/chrome/chrome --type=gpu-process --field-trial-handle=1849731511986804062,1867325293027947282,13
alphabr+  5927  0.0  0.1 449400 14296 ?        S    08:40   0:00 /opt/google/chrome/chrome --type=-broker
</pre>



## top

Exibe informações sobre processos em execução.

## free

Exibe informações sobre a memória volátil do sistema.

## uptime

O comando `uptime` informa quanto tempo a máquina esta ligada.


<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>uptime --help</code>
Usage:
 uptime [options]

Options:
 -p, --pretty   show uptime in pretty format
 -h, --help     display this help and exit
 -s, --since    system up since
 -V, --version  output version information and exit

For more details see uptime(1).
</pre>

Sem argumentos:

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>uptime</code>
11:06:53 up 1 day,  2:41,  1 user,  load average: 0,07, 0,11, 0,09
</pre>

COm o output mais bonito.

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>uptime -p</code>
up 1 day, 2 hours, 42 minutes
</pre>

Ligado desde:

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>uptime -s</code>
2018-04-09 08:25:03
</pre>

Para ver a versão do comando:

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>uptime -V</code>
uptime from procps-ng 3.3.10
</pre>



## pgrep


Esse comando retorna informações a respeito de processos. Recebendo pro padrão uma string para o grep

No exemplo abaixo são retornados os PIDS dos procesos q tem chrome no nome:


<pre class="command-line language-bash">
	<code>psgrep chrome</code>
5857
5868
5872
5924
5927
</pre>

Com o parametro -l mais delalhes são exibidos:


<pre class="command-line language-bash">
	<code>psgrep -l chrome</code>
5857 chrome
5868 chrome
5872 chrome
5924 chrome
5927 chrome
6004 chrome
6099 chrome
6679 chrome
7399 chrome
11441 chrome
11466 chrome
11491 chrome
11591 chrome
11730 chrome
</pre>





## pkill


## killall


## screen


## Tabela de sinais


<table border="2" cellpadding="7" cellspacing="0">
<tbody>
<tr>
<th>Signal</th>
<th>Name</th>
<th>Description</th>
</tr>
<tr>
<td>SIGHUP</td> <td>1</td> <td>Hangup (POSIX)</td>
</tr>
<tr>
<td>SIGINT</td> <td>2</td> <td>Terminal interrupt (ANSI)</td>
</tr>
<tr>
<td>SIGQUIT</td> <td>3</td> <td>Terminal quit (POSIX)</td>
</tr>
<tr>
<td>SIGILL</td> <td>4</td> <td>Illegal instruction (ANSI)</td>
</tr>
<tr>
<td>SIGTRAP</td> <td>5</td> <td>Trace trap (POSIX)</td>
</tr>
<tr>
<td>SIGIOT</td> <td>6</td> <td>IOT Trap (4.2 BSD)</td>
</tr>
<tr>
<td>SIGBUS</td> <td>7</td> <td>BUS error (4.2 BSD)</td>
</tr>
<tr>
<td>SIGFPE</td> <td>8</td> <td>Floating point exception (ANSI) </td>
</tr>
<tr>
<td>SIGKILL</td> <td>9</td> <td>Kill(can't be caught or ignored) (POSIX)</td>
</tr>
<tr>
<td>SIGUSR1</td> <td>10</td> <td>User defined signal 1 (POSIX)</td>
</tr>
<tr>
<td>SIGSEGV</td> <td>11</td> <td>Invalid memory segment access (ANSI)</td>
</tr>
<tr>
<td>SIGUSR2</td> <td>12</td> <td>User defined signal 2 (POSIX)</td>
</tr>
<tr>
<td>SIGPIPE</td> <td>13</td> <td>Write on a pipe with no reader, Broken pipe (POSIX)</td>
</tr>
<tr>
<td>SIGALRM</td> <td>14</td> <td>Alarm clock (POSIX)</td>
</tr>
<tr>
<td>SIGTERM</td> <td>15</td> <td>Termination (ANSI)</td>
</tr>
<tr>
<td>SIGSTKFLT</td> <td>16</td> <td>Stack fault</td>
</tr>
<tr>
<td>SIGCHLD</td> <td>17</td> <td>Child process has stopped or exited, changed (POSIX)</td>
</tr>
<tr>
<td>SIGCONTv	</td><td>18	 </td><td>Continue executing, if stopped (POSIX)</td>
</tr>
<tr>
<td>SIGSTOP</td> <td>19	</td><td>Stop executing(can't be caught or ignored) (POSIX)</td>
</tr>
<tr>
<td>SIGTSTP</td> <td>20</td> <td>Terminal stop signal (POSIX)</td>
</tr>
<tr>
<td>SIGTTIN</td> <td>21</td> <td>Background process trying to read, from TTY (POSIX)</td>
</tr>
<tr>
<td>SIGTTOU</td> <td>22	</td><td>Background process trying to write, to TTY (POSIX)</td>
</tr>
<tr>
<td>SIGURG</td> <td>23	</td><td>Urgent condition on socket (4.2 BSD)</td>
</tr>
<tr>
<td>SIGXCPU</td> <td>24</td> <td>CPU limit exceeded (4.2 BSD)</td>
</tr>
<tr>
<td>SIGXFSZ</td> <td>25</td> <td>File size limit exceeded (4.2 BSD)</td>
</tr>
<tr>
<td>SIGVTALRM</td> <td>26</td> <td>Virtual alarm clock (4.2 BSD)</td>
</tr>
<tr>
<td>SIGPROF</td> <td>27</td> <td>Profiling alarm clock (4.2 BSD)</td>
</tr>
<tr>
<td>SIGWINCH</td> <td>28</td> <td>Window size change (4.3 BSD, Sun)</td>
</tr>
<tr>
<td>SIGIO</td> <td>29</td> <td>I/O now possible (4.2 BSD)</td>
</tr>
<tr>
<td>SIGPWR</td> <td>30</td> <td>Power failure restart (System V)</td>
</tr>
</tbody>
</table>
