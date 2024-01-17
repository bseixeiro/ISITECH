# Versionning

## Intro

*https://www.markdownguide.org/cheat-sheet/*

scm = git

versionning = permet de revenir a une version precedante facilement, voir qui a fait quel modif, travailler en parrallele sur des fonctionnalite differente d une meme appli

importance => tracabilite, collaboration facile, backup et retablissement, creation de branche pour travailler sur le meme projet separemment

nomenclature d une version : **Majeure.Mineure.Correctif**

Majeure => gros changement incompatible avec version precedante
Mineure => ajout de fonctionnalite retrocompatible
Correctif =>correction de bug retrocompatible

## Git

SVN => ancien Git => Model Centralise => un serveur ou tout le code source est centralise => besoin d etre connecte au serveur pour modif code source => serveur tombe perte d historique => faiblesse = trop de participant
	
CheckOut => recuperation du code
Commit => sauvegarde localement une version du projet (pas besoin d internet vu que local)
Branching => separer les collaborateur pour travailler individuellement sur leur fonctionnalite
merging => rejoindre les differentes branches
repos => code source (local ou distant)


Git = logiciel distribue => plus ya de gens plus c est robuste

Plupart des scm stocke les modifs effectuer sur le code alors que git stocke tous le code source 

garde l integrite des donnees et voir si des modifs sont apporte => checksum ( somme de controle )

tres peu d operation sont destructive 

3 etats pour les fichiers :
	- modifié
	- indéxé
	- validé
	
	
### Commande

git => savoir si git est installer

**Config**

A la premiere utilisation => configurer git avec Git config, on peut le trouver a 3 endroits :
	- [chemin]/etc/gitconfig => config pour tous les utilisateurs et tous les projets ( forcer l ecriture --system )
	- fichier ~/.gitconfig => specifique a votre utilisateur ( forcer l ecriture --global )
	- fichier config dans le repertoire git d un depot en cours (.git/config) => specifique a un seul depot

```sh
$git config --list --show-origin
```
indique d ou proviens la config git

```sh
$git config --global user.name "John Doe"
$git config --global user.email mon@email.com
```
config le nom et l email

```sh
$git config --global core.editor [chemin complet vers l editeur de texte]
```
config l editeur de texte

```sh
$git config --global init.defaultBranch main
```
config la branche par default

```sh
$git config --list
```
verifier son parametrage

```sh
$git config user.name
```
verifier un parametre unique 

**Aide**

```sh
$git help <command>
$man git <command>
$git <command> --help
```

**Les Bases**

```sh
$git init
```
initialisé un depots 

```sh
$rm -rf .git/
```
supprimé un depots git

**Verifier les modifs**
```sh
$git status
```
verifier l etat du repos, modifie ou nom

```sh
$git diff
```
presente les modif du repos , si pas de modif alors rien en sorti
```sh
$git diff --staged
```
presente les modifs indexe

**Valider les modifs**
```sh
$git commit
```
faire un commit

```sh
$git commit -m "message"
```
faire un commit avec le titre

```sh
$git commit -a -m "message"
```
ajout des modifs et commit

**Indexé/Desindexé un fichier**
```sh
$git add .
```
ajoute tous ce qui se trouve dans le repertoire actuelle dans le prochain commit

```sh
$git rm --cached [fichier]
```
supprimer un fichier du prochain commit

```sh
$ rm [fichier]

$ git status 

$ git rm [fichier]
```
supprimer un fichier

**Clonage d un repos**
```sh
$git clone [url]
```
cloner un depots

```sh
$git clone https://github.com/bendahmanem/ISITECH-2324-B2-DEV-Versioning
```
cloner le cours 

**Push**
```sh
$ git push -u origin main
```
permet d informer git que la branche local main doit etre synchroniser avec la origin/main, si elle n existe pas on la créer

```sh
$ git push
```
pour les push suivant

**Historique**

```sh
$ git log
```
pour voir l historique des commits

**Tag**

```sh
$ git tag -a v1.0 -m "Version 1.0"
```
mettre un tag sur un commit

```sh
$ git tag
```
liste des tags dans l ordre alphabetique

```sh
$ git tag -l "v1.8.5*"
```
liste de tag filtrer

```sh
$ git show v1.4
```
pour visualiser un tag précis 

**SSh Key Github** 

https://docs.github.com/fr/authentication/connecting-to-github-with-ssh

Avec GitBash
```sh
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

Avec PowerShell en Admin
```sh
$ Get-Service -Name ssh-agent | Set-Service -StartupType Manual
$ Start-Service ssh-agent
```

Avec GitBash
```sh
$ ssh-add c:\Users\YOU\.ssh\id_ed25519
```


```sh
$ git remote add origin <ssh>
```
ajout d un remote ssh

```sh
$ git remote rm <remote-name>
```
supprimer un remote

```sh
$ git remote -v
```
lister les remote

**.gitignore**

.gitignore permet de dire a git quel fichier ou dossier ignorer => de facon recursive  

**Branche**

La balise Head nous sert à savoir sur quel commit on se trouve

```sh
$ git branch <BranchName>
```
créer une branche

La création d'une branche ne nous fais pas changer de branche

```sh
$ git checkout <BranchName>
```
basculer sur une autre branche

```sh
$ git checkout -b <BranchName>
```
création et basculement vers la nouvelle branche

```sh
$ git branch -a
$ git branch --all
```
lister toutes les branches y compris celles des remotes

```sh
$ git fetch <distant>
```
permet de synchroniser ses travaux, elle recherche le serveur <distant> et recupere les modifs apporte


Pour récupérer des modifs sur des nouvelles branches :
```sh
$ git fetch <distant>
```

Pour récupérer des modifs sur une branche distante et fusionner avec la branche local :
```sh
$ git pull <distant> <branch>
```

Règle d'or de commencement git:
`commit` -> `pull` -> `push`

Lorsque l'on récupère la branche distante, une branche local ne se créer pas automatiquement, il faut donc en créer une et la lier :
```sh
$ git checkout -b <branchName> <distant>/<branchName>
$ git checkout --track <distant>/<branchName>
```
Pour voir les branches suivies et savoir si votre branche local est en avance ou en retard sur la branche distante:
```sh
$ git fetch --all
$ git branch -vv
```
```sh
$ git push origin --delete <branchName>
```
supprimer une branche distante

**Rebase/Merge**

```sh
$ git checkout main
$ git merge develop
```
permet de fusionner la branche de dev sur la branche principale
Cette action n'est pas destructive et est traçable

contrairement à un rebase :
```sh
$ git checkout develop
$ git rebase main 
```
permet de rebaser la dev sur la principale
Cette action peut être destructive et fait perdre des infos sur la traçabilité car supprime l'historique depuis la création de la branche -> à faire que si l'on est sur de ce que l on fait



