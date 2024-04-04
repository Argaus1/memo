# GIT TUTORIAL

## Liens

[*Qu'est ce que git?*](https://www.nicelydev.com/git/definition-git)

--------------------------------------------------------------------
## Sommaire

[Notions de base](https://github.com/AnoukBV/memo/blob/main/git.md#notions-de-base)

[Branches](https://github.com/AnoukBV/memo/blob/main/git.md#branches)

--------------------------------------------------------------------
## Notions de base

HEAD = endroit ou nous nous trouvons.

![head](https://www.nicelydev.com/img/git/aller-version-2.png)

**installation**
```bash
sudo apt-get update
sudo apt-get install git-all
```

**connaitre ma version**
```bash
git --version
```

**connaitre liste des donnees de configuration**
```bash
git config --list
```

**creer un projet**
```bash
git init
```

**verifier statut de mes fichiers**
```bash
git status
```

**git add**

un fichier peut etre untracked: cree mais pas add
```bash
git add
```
avec cette cmd le fichier est indexe (staging area)

pour desindexer:
```bash
git rm --cached <file>
```

si on modifie un fichier added pas commit -> changes to be committed mais certains change sont "not staged for commit", tout en etant dans un fichier dans la staging area

```bash
git restore <file>
```
on recupere la derniere version indexee

**git commit**

Depuis index/staging area
```bash
git commit -m "str"
```
Pour ne pas avoir besoin de git add **pour les fichiers deja indexes** avec chgmts "not staged"
```bash
git commit -am "str"
```

Si des modifs insatisfaisantes **mais pas de commit depuis ces modifs**
```bash
git reset --hard
```
annule toutes les modifs depuis dernier commit. Ne supprimera pas un fichier non indexe mais supprimera un fichier dans not staged

**git log**
```bash
git log
```
"HEAD-\>master" correspond au point ou nous nous trouvons. master = nom de la branche.
```bash
git log <nom fichier ou dir>
git log -n 2
git log --oneline
git log --all --oneline
```

**retour en arriere**

```bash
git reset <no commit> --soft
```
remet dans staging area/index
```bash
git reset <no commit> --mix
```
remet dans not staged
```bash
git reset <no commit> --hard
```
ne remet nulle part, modif supprimees.
```bash
git checkout <commit ou branche>
git checkout <main>
```
Nous ramene a un commit : **deplace la HEAD**. Mais nous sommes dans l'historique!

**autres commandes**

voir qui a fait quelle modif
```bash
git blame <file>
```
voir differences
```bash
git diff [<no commit> [<no commit>]]
```
donner nom a un commit
```bash
git checkout <no commit>
git tag <nom>
git checkout <nom>
git tag -d <nom>
```
Quand on s'est perdue:
```bash
git reflog
```

Creer un nv commit dans la branche, copie d'un ancien commit (pas de notion de retour en arriere etc, pas mal pour travail avec remote et groupe):
```bash
git revert <no commit>
```

## BRANCHES

```bash
git branch
git branch <name>
git checkout <name>
```

Branches sont utiles pour tester du code. Une fois teste, soit on merge, soit on supprime la branche.

Pour circuler:
```bash
git checkout <branche>^
git checkout <branche>^^
git checkout <branche>~n
```

Creer une branch et s'y deplacer:
```bash
git branch -b <name>
```

**merging**
```bash
git merge <name>
```
/!\ Si code modifie dans main, on va avoir un conflit.
![conflit](https://www.nicelydev.com/img/git/fusion-branches.png)

Le conflit a eu lieu a cause du commit fc41856.

**supprimer une branche**
```bash
git branch -d <branche>
```

**creer une branche et la deplacer vers un commit**
```bash
git branch --force <branche> <no commit>/<branche>
git branch -f <branche> <no commit>/<branche>
```
![deplacement](https://www.nicelydev.com/img/git/deplacer-branche-test.png)

Quand la branche a ete cree elle etait sur commit ef2a68d, mtn elle a ete deplacee.

**selectionner des commits a copier sous la branche**
```bash
git rebase -i <branche>
git cherry-pick <no commit> <no commit> ...
```

## REMOTE

Quand on cree une branche, elle n'est cree que localement. 
Quand on a plusieurs personnes sur un projet, deux options:
- tout le monde est collaborateur
- un createur et plusieurs autres qui ne peuvent pas push sans aval du createur

### One gatekeeper and others group members

![schema group proj](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Vg2W3Aoc7ksZYpXDbMqEyQ.png)

Chaque carre vert est une machine, les carres bleus sont des repositories.

Le createur du projet cree son repo. Ensuite dans son terminal:
```bash
git remote add <remote name> <url>
git push -u <remote name> main
```

Le membre du groupe va fork sur github. Les membres travaillent sur un repo forked.
Ils clonent la version du moment du fork.
```bash
git remote -v
```
Permet d'afficher les repo remote avec lesquels le repo local interagit. fetch = pour recuperer des donnees, push = pour envoyer des donnees.

Ensuite en faisant un git remote add comme d'hab vont se connecter au main project. Cela permet de cloner la version la plus recente pour updater son main local. 

**Chaque membre dispose d'un origin remote connecte a leur fork, et un upstream remote connecte au main project repo.**

**Ne jamais travailler sur la main branch --> si on veut modifier, on cree une nouvelle branche.**

![modifications hierarchie](https://miro.medium.com/v2/resize:fit:720/format:webp/1*J2-6INx5GDg-2JnDFZUOmQ.png)

Je suis un membre du groupe (createur ou autre), j'ai fait mes changements sur la branche aue j'ai cree pour ces changements. Je les commit. 

**je clone la version la plus recente** (meme si je suis le createur)
```bash
git pull upstream main/master
```

Pour tout le monde ensuite:

J'ajoute mes changements sur ma main branche **locale**:
```bash
git merge <branch name> //tjrs avec changes committed
```
Je gere mes merge conflicts localement.

Je push ma main sur **mon fork** (si je suis membre non createur) = D sur schema.
```bash
git push origin main/<branch name>
```

Je cree une pull request (sur github). Le createur accepte. Je n'ai juste pas a faire de pull request. 
