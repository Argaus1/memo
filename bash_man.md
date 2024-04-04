# BASH

## LIENS

[Building a mini bash: A 42 project](https://m4nnb3ll.medium.com/minishell-building-a-mini-bash-a-42-project-b55a10598218)

-------------------------------------------------
## SOMMAIRE

[Definitions](https://github.com/AnoukBV/memo/blob/main/bash_man.md#definitions)

[Shell Grammar](https://github.com/AnoukBV/memo/blob/main/bash_man.md#shell-grammar)
- [Commande simple](https://github.com/AnoukBV/memo/blob/main/bash_man.md#commande-simple)
- [Pipeline](https://github.com/AnoukBV/memo/blob/main/bash_man.md#pipeline)
- [List](https://github.com/AnoukBV/memo/blob/main/bash_man.md#list)
- [Commandes composees](https://github.com/AnoukBV/memo/blob/main/bash_man.md#compound-commands)
- [coprocesses](https://github.com/AnoukBV/memo/blob/main/bash_man.md#coprocesses)
- [Shell function definition](https://github.com/AnoukBV/memo/blob/main/bash_man.md#shell-function-definitions)

[Comments](https://github.com/AnoukBV/memo/blob/main/bash_man.md#comments)

[Quoting](https://github.com/AnoukBV/memo/blob/main/bash_man.md#quoting)

[Parametres](https://github.com/AnoukBV/memo/blob/main/bash_man.md#parameters)
- [Positional parameters](https://github.com/AnoukBV/memo/blob/main/bash_man.md#positional-parameters)
- [Special parameters](https://github.com/AnoukBV/memo/blob/main/bash_man.md#special-parameters)
- [Shell variables](https://github.com/AnoukBV/memo/blob/main/bash_man.md#shell-variables)
- [Arrays](https://github.com/AnoukBV/memo/blob/main/bash_man.md#arrays)

[Expansion](https://github.com/AnoukBV/memo/blob/main/bash_man.md#expansion)

[Redirection](https://github.com/AnoukBV/memo/blob/main/bash_man.md#redirection)
- [Heredoc](https://github.com/AnoukBV/memo/blob/main/bash_man.md#here-documents)
- [Herestring](https://github.com/AnoukBV/memo/blob/main/bash_man.md#here-strings)

[Aliases](https://github.com/AnoukBV/memo/blob/main/bash_man.md#aliases)

[Functions](https://github.com/AnoukBV/memo/blob/main/bash_man.md#functions)

[Arithmetic evaluation](https://github.com/AnoukBV/memo/blob/main/bash_man.md#arithmetic-evaluation)

[Conditional expressions](https://github.com/AnoukBV/memo/blob/main/bash_man.md#conditional-expressions)

[Simple command expansion](https://github.com/AnoukBV/memo/blob/main/bash_man.md#simple-command-expansion)

[Command execution](https://github.com/AnoukBV/memo/blob/main/bash_man.md#command-execution)

[Command execution environment](https://github.com/AnoukBV/memo/blob/main/bash_man.md#command-execution-environment)

[Environment](https://github.com/AnoukBV/memo/blob/main/bash_man.md#environment)

[Exit Status](https://github.com/AnoukBV/memo/blob/main/bash_man.md#exit-status)

[Signals](https://github.com/AnoukBV/memo/blob/main/bash_man.md#signals)

[Job control](https://github.com/AnoukBV/memo/blob/main/bash_man.md#job-control)

[Prompt](https://github.com/AnoukBV/memo/blob/main/bash_man.md#prompt)

[Readline](https://github.com/AnoukBV/memo/blob/main/bash_man.md#readline)

[History](https://github.com/AnoukBV/memo/blob/main/bash_man.md#history)

[History expansion](https://github.com/AnoukBV/memo/blob/main/bash_man.md#history-expansion)

[Shell builtin commands](https://github.com/AnoukBV/memo/blob/main/bash_man.md#shell-builtin-commands)

([Files](https://github.com/AnoukBV/memo/blob/main/bash_man.md#files))

-------------------------------------------------
## DEFINITIONS

* blank: tab ou space
* word/token: sequence de caracteres consideree comme une unite singuliere
* identifier/name: word/token commencant par underscor ou car alpha et avec seulement caracteres alphanumeriques
* metacharacters: caractere qui separe des mots, SAUF QUAND QUOTED (|  & ; ( ) < > space tab newline)
* control operator: token qui fait operation de controle (|| & && ; ;; ;& ;;& ( ) | |& <nl>)

* certains mots sont des reserved words: signification speciale pour bash
	- si unquoted
	- soit premier word/arg0/cmd to perform
	- soit troisieme mot de cmd case OU select (in)
	- soit troisieme mot de cmd for (in/do)
(! case  coproc  do done elif else esac fi for function if in select then until while { } time [[ ]])


## SHELL GRAMMAR

### commande simple

*Qu'est ce qu'une commande ?*
- assignations de variables
- words/tokens (sequence de caracteres consideree comme une unite singuliere) separes par blank 
(tab ou space) (peuvent etre des redirections) - 1er mot = arg0 = commande a executer, mots suivants 
= arguments de arg0
- operateur de controle termine la commande
- return value: exit status OU 128+n (n=num du signal ayant termine la cmd)

### pipeline

*Qu'est ce qu'une pipeline ?*
- sequence de une ou plusieurs commandes separees entre elles par des operateurs de controle | OU |&
- | :stdout de cmd1 connecte grace a une pipe a stdin de cmd2 - connexion arrive AVANT toute 		
redirection
- |& : stderr de cmd1 aussi connectee a stdin de cmd2 (|& = 2>&1) - connexion arrive APRES toute 
redirection
- return value: exit status de derniere cmd. si "set -o pipefail": exit status de derniere cmd avec 
non-0 exit status
- avant arg0 : on peut trouver resrved word time -p. display le tps, -p = format specifie dans 
TIMEFORMAT
- avant arg0: reserved word ! peut faire que exit status = negation de l'exit status classique

chaque cmd = un processus dans un subshell

si
```bash
set -o lastpipe
```
exit status = derniere cmd executee dans le shell

### list

*Qu'est ce qu'une liste?*
- sequence de une ou plusieurs pipelines separees par operateurs de controle ; & && || et 
potentiellement terminees par op ctrl ; & <nl>
- && || equal precedence -> evaluees avec la meme priorite. sont utilises pour execution
conditionnelle (basee sur succes ou echec des cmd (exit status)). exit status = derniere cmd
- && AND list. cmd2 exec si exit status de precedent == 0
- || OR list. cmd2 exec si exit status de precedente != 0
- ; & equal precedence -> meme priorite.
- ; commandes sequentielles: plusieurs \<nl\> peuvent remplacer un ; shell att fin de cmd
exit status = celui de la derniere cmd.
- & commandes asynchrones: si control oprtr & termine: cmd exec dans un subshell. shell n'att pas fin de cmd, exit status = 0.

### compound commands

*Qu'est ce qu'une commande composee?*
- sequence de plusieurs commandes groupees --> traitees comme une unite singuliere. shell scripts
- ATTENTION : ce qui suit n'est pas du tout demande par minishell

- (list): subshell, exit = celui de la list. permet de ne pas affecter son shell avec des unset par ex
- { list;/\<nl\> }: 0 interet apparemment. mm shell, exit status = celui de la list. = "group command"
- ((expr)): expression arithmetique. 0 si != 0, 1 si 0. correspond a let "expr"
- [[ expr ]]: exit 1 ou 0 selon eval de l'expr (..)
- reserved words exemples :
```bash
for [ [ in [ token ... ] ] ; ] do list ; done
```
```bash
for (( expr1 ; expr2 ; expr3 )) ; do list ; done
```
```bash
select name [ in word ] ; do list ; done
```
```bash
if list; then list; [ elif list; then list; ] ... [ else list; ] fi
```bash
while list-1; do list-2; done
```

### coprocesses

*Qu'est ce qu'un coprocessus?*
```bash
coproc [NAME] command [redirections]
```
- commande precedee par le reserved word coproc. exec est asynchrone dans un subshell
- si commande simple pas besoin de NAME
- shell cree un tableau NAME. stdout de cmd connecte a un fd = NAME[0]. stdin = NAME[1]. pipe etablie
avant toute redirection

### shell function definitions

- nom des fct ne peut pas contenir $
- execute une commande composee definie par l'utilisateur
```bash
fname () compound-command [redirection]
```
```bash
function fname [()] compound-command [redirection]
```


## COMMENTS
\#


## QUOTING

- but = enlever la significations de metacharacters comme $, ou des reserved words,
ou encore parameter expansion
ou ! est a quote pr empecher history expansion
" ' \ permettent de quote, mais les comportements different

- \ = esc character. preserve la valeur litterale du char qui suit SAUF \<nl\> (c une continuation de
ligne et plus un control operator)
- '' = preservation de la valeur litterale de TOUS les char dans les quotes. JAMAIS de '' entre des ''
meme avec \
- "" = preservation de la valeur litterale de tous SAUF $ ' \ et ! si history expansion enabled
\ garde son special meaning si suivi par $ ' " \ \<nl\>
"" peut aller dans "" si \"
\* et @ ont special meaning entre ""
- \$'str' : expansion a str avec \\ANSIC comme \\n ou \\t respectees.
```bash
echo $'anouk\tanouk\nanouk'
```
donne
```bash
anouk	anouk
anouk
```
- $"str" : pas d'exp ANSI C. traduction en acc avec current locale


## PARAMETERS

- entite qui stocke une valeur. name, number, ou special characters
- declaration de variables anouk=8. name=value
- anouk= anouk='' anouk="" anouk+=
- tilde param variable arithmetic exp, quote removal et command substitution mais pas de word 
splitting
- nameref: ref a une autre variable peut etre assignee a une variable ex: declare -n ref=$1

### positional parameters

```bash
set -- 10 20 30
anouk=$2
echo $anouk
```
normalement sort 20

### special parameters

- $\* liste des pos parameters
- "$@" liste avec pleins de ""strs separes ("$\*" = un str)
- $# nbr de pos param
- $? seul pr minishell
- $- flags
- $$ PID du shell
- $! PID du most recent job (avec coproc)
- $0 nom du shell

### shell variables

dans minishell pas utile

### arrays

pas compris


## EXPANSION


### 1brace expansion

avant toute autre expansion. 
```bash
a{d,c,b}e
```
`ade  ace abe'

### 2tilde expansion

- ~/ = home

### 3parameter/variable expansion

### 4cmd substitution

- sortie d'une commande remplace le nom de la commande
- $(cmd) OU 'cmd'
- cmd exec dans un subshell
- \$(cat file) = \$(< file)
- si "$(cmd)"-> If the substitution appears within double quotes, word splitting and pathname 
expansion are not performed on the results.

### 5arithmetic expansion

### process substitution

- <(list) ou >(list)
- list dans un subshell, soit on ecrit dans file qui sert d'input pr la list, soit on lit dans le file
pr connaitre l'output de la list
- simultanement a arithmetic exp, cmd sub et param/variable exp

### word splitting

- effectue sur output de param exp/cmd sub/arithm exp
- donc ne se produit qui si on a une expnasion
- potentiellement important pour minishell

### pathname expansion

- effectue apres word splitting
- \* ? pattern matching - wildcards
- pas besoin pr partie mandatory


## REDIRECTION


- redirection de input et output d'une commande : duplication, ouverture, fermeture
- changer les files dans lesquels une commande ecrit et lit 
- ordre = apparition. operateurs peuvent apparaitre n'importe ou dans une simple commande, ou suivre 
une commande

- chq redir est precedee par un fd ou filename (shell lui alloue un fd)
- si il n'y a pas de fd, on prend fd0 si < ou fd1 si >
- var/filename est sujet a brace expansion, tilde expansion, parameter/variable expansion, command 
substitution, arithmetic expansion, quote removal, pathname expansion, word splitting
- exemple
```bash
ls 2>&1 > dirlist
```
stderr devient une duplication de stdout, PUIS stdout (pas de fd spe avant 
chevron) est redirigee vers dirlist
- fd de bash : /dev/fd/fd, /dev/stdin, /dev/stdout, /dev/stderr, /dev/tcp/host/port (si host est valid
hostname/addr internet & port = port nb -> bash ouvre le TCP socket correspondant), /dev/udp/host/port
(pareil mais avec UDP)
- si fd>9 attention car shell peut les utiliser
- pour ne pas que outfile ecrase, >| ou noclobber
- output > si file existe pas, cree, pareil pour >>
- &>word redirige aussi stderr
- <& dup
- \>- move
- <> les deux

### here documents

```bash
[n]<<[-]word
```
- pas d'expansion
- si rien dans n ok, on prend fd0, mais on peut aussi aller chercher un delimiter dans un autre fd
- si une partie de word est quoted, le delimiter devient le res de quote removal, et le contenu
du here doc n'est pas expanded
- si aucune quote, le contenu du heredoc est sujet a parameter expansion, command substitution, 
arithmetic expansion
- ATTENTION on ignore les \<nl> + utiliser \ pour quote \ $ '
- <<- supprime tab

### here strings

```bash
[n]<<<word
```
- tilde expansion, parameter and variable expansion, command substitution, arithmetic expansion, 
and quote removal
- pas de pathname exp ni word splitting


## ALIASES

## FUNCTIONS

- dynamic scoping : controle de la visibilite d'une variable dans les fonctions
- variables visibles et leur valeur sont le resultat de la sequence d'appel de fonctions qui mene
a la fonction actuelle
- la valeur d'une variable qu'une fonction peut voir depend de sa valeur la ou elle est appellee
- variable local = le plus imp


## ARITHMETIC EVALUATION

## CONDITIONAL EXPRESSIONS

## SIMPLE COMMAND EXPANSION

**IMPORTANT POUR MINISHELL**

Lors de l'execution d'une simple commande, shell fait ces expansions, assignements et redirections
dans l'ordre qui suit, de gauche a droite :
1. le parser a marque certains words/tokens comme des assignements de variables (quand precedent
le nom de la commande) ou des redirectiosn. CES TOKENS SONT SAUVEGARDES POUR LA SUITE
2. les tokens qui n'ont pas ete ainsi sauvegardes par le parser comme des assignements ou des redir
sont EXPANDED si possible. si il y a des restes : 1er token = nom de la cmd, et le reste sont ses 
arguments
3. on fait les redirections
4. assignements de variable : on expand (tilde, param, cmd, arithm, quote removal) et on assigne
ensuite :
- si pas de cmd name : assignements de variables affectent l'environnement du shell
- si cmd name : assignement affectent cmd et pas shell
- si on veut assign une readonly variable : ERROR, cmd exit avec non zero

EXIT STATUS
- si pas de cmd name mais cmd sub : exit = exit de derniere cmd sub
- si pas de cmd name mais pas de cmd sub : exit = 0
- si restes du parsing + expansions = commandes, VOIR CHAP PRO


## COMMAND EXECUTION

- si cmd name n'a pas de / -> shell veut localiser soit fonction soit builtin. si aucun des deux
shell va aller chercher dans le PATH. utilise hash table (SHELL BUILTIN COMMANDS) pr builtins
donc si cmd n'est pas dans hash table : fouille dans path. 

- si echoue : cherche command_not_found_handle, et si cette fonction existe shell l'appelle avec fonction de base et ses arguments en arg, et l'exit status de la fonction devient l'exit status de cet env d'exec different (=/= d'un subshell)
si fct n'existe pas : error message et exit 127

- si succes : exec le prgm dans un autre env d'execution. arg0 devientle name et le reste les 
arguments

- si fonction trouvee n'a pas les droit d'exec/file n'est pas un dir -> c'est un shell script, subshell (bien un subshell) spawn, est reinitialise pr exec le script, SAUF loc des cmdes


## COMMAND EXECUTION ENVIRONMENT

environnement d'execution du shell:

- fichiers ouverts herites par le shell qd invoque (potentiellement modifies si redir fournies a exec
quand shell a ete invoque)
- current working dir (cd) --> a modifier pour minishell ?? lui faire heriter du reste de l'arbre
qui precede le dir du projet ?
- file creation mode ??
- trap ??
- parametres set par assignement de variables ou herites (ou set)
- options (shopt)
- alias
- PID, ceux de background jobs, valeur de $$ et PPID

simple commande diff de builtin ou shell fonction -> exec dans un autre env d'execution qui ne peut
affecter l'env actuel

si cmd sub/grouped cmd ac parenth et cmd asynchrones ->subshell mais dont les valeurs modifiees sont
reinitialisees a la fin selon les valeurs copiees au depart, pour ne pas changer l'env su shell 
principal


## ENVIRONMENT

**IMPORTANT POUR PARSING MINISHELL**

- qd program est invoque on lui donne un tableau de str
- paire name-value sous la forme name=value
- quand shell est invoque, il scanne l'environnement et cree un parametre pour chque 
name qu'il trouve, le marquant automatiquement pour être exporté vers les 
processus enfants.
- les cmd exec heritent de l'env
- export permet de add et supp des variables de l'environnement
- unset supprime


## EXIT STATUS

**IMPORTANT POUR MINISHELL**

*Qu'est ce que l'exit status?*
La valeur retournee par le waitpid. Entre 0 et 255. Certaines valeurs expliquent 
certains echecs.

0 = succes

autre = echec
si cmd termine avec fatal signal n, exit status - 128 + n
si cmd echoue a cause d'une erreur pdt expansion ou redir -> exit status > 0
si cmd not found -> exit = 127
si cmd pas executable -> exit = 126
si bultin utilise incorrectement (option invalide ou arg manquants) -> exit = 2

bash exit status = exit de derniere commande executee
sauf erreur syntax -> non zero exit


## SIGNALS

**IMPORTANT POUR MINISHELL**

on doit avoir un signal handler. cmt le prgrm minishell en lui meme gere les signaux ?

1.SIGHUP: suspension. qd on veut au'un process reload sa config

2.SIGINT: signal d'interruption. trl+c

3.SIGQUIT: ctrl+RIEN. process termine+core dump

4.SIGILL: instruction illegale

6.SIGABRT: erreur detectee

8.SIGFPE: floating point exception. operation arithmetique illegale. overflow, /0...

9.SIGKILL: killm forcer terminer (et pas interrompre), qd ne rep pas aux autres signaux
d'arret

10.SIGUSR1 

11.SIGSEV

12.SIGUSR2: user defined, pour comportement custom

13.SIGPIPE: broken pipe, ecrire dans une pipe quand son reader a terminated

14.SIGALRM: quand un timer set par alarm() expire

15.SIGTERM:termination signal. process peut ignorer ou prendre en compte(impossible 
d'ignorer sigkill)

18.SIGCONT: resume execution

19.SIGSTOP

21.SIGTTIN: envoye a background process qui veut lire depuis son terminl "controlant" 
(quadoit etre le seul a utiliser son input)

22.SIGTTOU: mm chose si background veut ecrire dans terminal controlant

20.SIGTSTP: ctrl+Z

- bash ignore sigterm si en interactif
- sigint gere
- ignore sigquit, sigttin, sigttou, sigtstp si job control in effect
- les signal handler de bash ont des valeurs herites du parent.
- si job ctrl pas la, cmd asynchrone ignorent sigint et sigquit
- pr cmd substitution : ces commandes ignorent les sig generes par le clavier et job ctrl : ttin, ttou et tstp
- si sighup le shell exit apres avoir envoyer sighub a tous les jobs en cours. lesjobs en pause recoivent aussi sigcont


## JOB CONTROL

- capacite a stopper execution puis la continuer
- chaque pipeline = un job
- si job asynchrone  [1] 25647 (PID = dernier process associe a ce job)

## PROMPT

bash display plusieurs prompts:
- PS1: celui quand est pret pour une cmd
- PS2: quand veut d'autre input (typiquement un chevron)
- PS0:apres avoir lu command et avant de l'executer


## READLINE

### readline initialization

rl met des commandes dans un initialization file. variable INPUTRC si on unset INPUTRC -> ~/.inputrc
ou /etc/inputrc
qd prgrm utilisant cette lib commence, ce fichier est lum et on set les bindings et variables cles

### readline key bindings


bcp de listes d'infos potentiellement importants mais absurdes a noter: line 2715


## HISTORY

Initialise a partir de la variable HISTFILE(~/.bash_history). On tronque le fichier nomme d'apres la valeur de histfile pour qu'il ne contienne pas plus que le nbr de lignes demande par histfilesize.


## HISTORY EXPANSION

- performed apres qu'une ligne est lue par bash.
- partie 1: determiner quelle ligne du doc histfile utiliser = l'event
- partie 2: selectionner quelles parties de la ligne est a inclure dans la ligne actuelle (celle qui 
vient d'etre lue) = words
- pour selectionner les words, la ligne est separee en words de la meme maniere que la CL
- ! char pour expansion de l'historique (donc penser au \ ou '' OU si ! est juste avant le " de
fermeture)


## SHELL BUILTIN COMMANDS

l3801

- echo [-n] [arg ...]: affiche les arguments speares par des espaces, fini par <nl> sauf avec -n. 
0 si pas de write error. si $'':il fqut interpreter ANSI C (l4233)
- cd [dir]: le cwd devient dir. tout ce qui suit dir est ignore. return true si succes, false sinon
t_bool??? 
- pwd: print le chemin absolu vers cwd. exit = 0 sauf si erreur en lisant le chemin ou inv option.
- export [name[=word]]: name ajoute a l'env. sans name on print toutes les variables exportees. 
(comment recuperer ce tableau de str?) exit=0 sauf si invalid option, un name n'est pas un name
valide pour shell
- unset [name]: pour chque name, on enleve la variable correspondante. un name = une variable. si 
pas de variable avec ce name, une fonction est supprimee si il en existe une. exit = true sauf si un 
name est readonly.
- env [name=value]... [command [arg]...]: lance un prgrm dans un env modifie. chq name=value, puis
on exec cmd avec ces args.
- exit [n]: shell exit avec n statut, si pas de n avec statut de derniere commande executee.


## SEE ALSO

man env
man readline

## FILES

/bin/bash: The bash executable
/etc/profile: The systemwide initialization file, executed for login shells
/etc/bash.bashrc: The systemwide per-interactive-shell startup file
/etc/bash.bash.logout: The systemwide login shell cleanup file, executed when a login shell exits
~/.bash_profile: The personal initialization file, executed for login shells
~/.bashrc: The individual per-interactive-shell startup file
~/.bash_logout: The individual login shell cleanup file, executed when a login shell exits
~/.inputrc: Individual readline initialization file
