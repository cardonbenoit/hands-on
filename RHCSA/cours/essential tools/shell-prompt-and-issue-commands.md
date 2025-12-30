---
course: "RHCSA"
module: "Understand and use essential tools"
title: "shell prompt and issue commands"
date: 2025-12-29
level: "Intermediate"
tags: [shell]
---

## Objectifs
- [] quelques generalites basiques a connaitre

> se connecter via ssh
```bash
ssh -p 2222 user@serveur        # port non standard
ssh -i ~/.ssh/id_ed25519 user@serveur   # clé privée
```
> environnement
```bash
whoami
hostname
pwd
id
date
```

> variable
```bash
VAR=hello
echo $VAR
```

> expansion utile
```bash
echo ~
echo $HOME
echo {1..5}
```

> substitution de commande
```bash
echo "Nous sommes le: $(date)"
```

> sous-shell
```bash
(cd /tmp; pwd)
```

📌 en substitution de commande ou sous-shell, le shell appelant n'est pas affecte 

> Wildcards (globbing) : * ? [ ]
**Globbing = motifs qui matchent des noms de fichiers**
|glob|exemple|
|--|--|
|*|`ls *.conf` → liste tous les fichiers finissant par .txt|
|? |`ls photo?.jpg` → photo1.jpg, photoA.jpg |
|[abc]|`ls file[12].log` → UN SEUL caractere parmi ceus-la → file1.log, file2.log|
|[a-z]|`ls img_[a-z].png` → dans la plage  |

> Brace expansion
**génère du texte en plusieurs variantes. Ce n'est pas fait pour matcher**
```bash
echo {a,b}{1,2} → a1 a2 b1 b2
```

> Operateurs logiques
|operateur|comportement|
|--|--|
|;|enchaîne quoi qu’il arrive|
|&&|exécute la suivante si succès|
|||| exécute la suivante si échec|

```bash
mkdir /tmp/test && cd /tmp/test
cd /nope || echo "échec du cd"
```

> 📌 Naviguer dans l'historique
```bash
history
```  
Ctrl + r puis tape un bout de commande
!! : dernière commande
!42 : commande n°42 dans history

Ctrl + a début de ligne
Ctrl + e fin de ligne
Alt + f / Alt + b forward/backward par mot

<!--
📌 À retenir / concepts
🛠️ Commandes / actions
🧪 Test / validation
⚠️  Attention / pièges
🔍 Debug / investigation
🔒 Sécurité / hardening
✅ OK / validé / succès
❌ Erreur / à éviter
-->
