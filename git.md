# GIT TUTORIAL

## Liens

[*Qu'est ce que git?*](https://www.nicelydev.com/git/definition-git)

--------------------------------------------------------------------
## Sommaire

[Notions de base](

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

## BRANCHES

```bash
git branch
git branch <name>
git checkout <name>
```

Branches sont utiles pour tester du code. Une fois teste, soit on merge, soit on supprime la branche.

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
git branch --force <branche> <no commit>
```
![deplacement](https://www.nicelydev.com/img/git/deplacer-branche-test.png)

Quand la branche a ete cree elle etait sur commit ef2a68d, mtn elle a ete deplacee.
