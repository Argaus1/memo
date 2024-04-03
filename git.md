# GIT TUTORIAL

## Links

[*Qu'est ce que git?*](https://www.nicelydev.com/git/definition-git)

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
```
Nous ramene a un commit : **deplace la HEAD**. Mais nous sommes dans l'historique!
