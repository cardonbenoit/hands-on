    ---
theme: "Mise en place"
module: "Documenter"
titre: "Monter un repository git et GitHub"
date: 2025-12-28
level: ""
tags: [git,GitHub,ssh,markdown,glow]
---

## 0) Objectifs
- Documenter la mise en place de la documentation 
- Creer un repository git local
- Publier sur GitHub via ssh
- Decider un format aux fichiers .md des le depart et mettre en place git


## 1) Concepts clés
### Définitions
- Git: systeme de versioning (local)
- GitHub: plateforme d'hebergement du repository (remote: nomme 'origin' par defaut) 

### 📌 À retenir
**Trois** zones de travail local pour git et **Une** zone remote sur GitHub: 
- working directory : Creation/Modif. fichiers locaux : `git status`
- staging area : Selection de ce qui sera mis a jour : `git add`
- repository : Nouveau Snapshot/Etat dans l'hisorique : `git commit -m 'ce qui change avec cette version'`
- publier sur le remote : `git push`
**Recuperer la derniere version en local** `git pull`

## 2) Creation d'un Repo
### En local
- `mkdir ~/hands-on ; cd ~/hands-on`
- `git init` cette commande va creer un dossier .git contenant le versionning et transformer le dossier en repository
⚠️  A ce stade, il n'y a eu ni commit et encore moins de publication.

### En ligne : Creation d'un repo sur GitHub
- GitHub > Onglet Repository > New
- mettre le repository en public
- cliquer sur 'create repository'

### 🔒 Securiser l'acces via SSH
- `cd`
- `ssh-keygen -t ed25519 -C '248338221+cardonbenoit@users.noreply.github.com'`
- choisir un nom de fichier type `~/.ssh/git_AAAAMMDD_hands-on` (rotation de cle facilite)
- `eval "$(ssh-agent -s)"` pour demarrer si necessaire l'agent et executer des exports de variables d'environnement utilisees par git, ssh, ssh-add
- `ssh-add ~/.ssh/git_AAAAMMDD_hands-on` : charge la cle et on ne tape le mot de passe qu'une fois
- `cd ~/hands-on; git config core.sshCommand 'ssh -i ~/.ssh/git_AAAAMMDD_hands-on'` : Dans le cas ou on cree une cle par repository 
- `git config user.email '248338221+cardonbenoit@users.noreply.github.com'`
- `git commit --amend --reset-author --no-edit` (si on fait la config apres un commit fait trop rapidement avant d'avoir peaufine la config avec la ligne ci-dessus)

- `pbcopy < .ssh/git_AAAAMMDD_hands-on2.pub`
- ** sur GitHib > Compte > Settings > SSH et GPG > New SSH key ** on ajoute la cle publique (toute la ligne dans .pub)
- `ssh -T git@github.com` accepter 

### 🧪 Test rapide
- `echo "# Welcome" >> README.md`
- `git add README.md`
- `git commit -m "1er commit test"`
- `git branch -M master`
- `git remote add origin git@github.com:cardonbenoit/hands-on.git`
- `git push -u origin master`


📌 À retenir / concepts
🛠️ Commandes / actions
🧪 Test / validation
⚠️  Attention / pièges
🔍 Debug / investigation
🔒 Sécurité / hardening
✅ OK / validé / succès
❌ Erreur / à éviter
