---
course: "RHCSA"
module: "Understand and use essential tools"
title: "input-output redirections"
date: 2025-12-29
level: "Intermediate"
tags: [shell, cli]
---

## 0) Objectifs
- []
- []


## 1) Concepts clés
### Définitions
Flux et Descripteurs de flux 
- STDIN 0
- STDOUT 1
- STDERR 2

Redirections
- **>** ecrase
- **>>**  concatene
- **<** `cmd < fic.txt` alimente cmd avec le contenu de fic.txt 
- **|** `cmd1 | cmd2` cmd1 alimente STDOUT et cmd2 le lit via STDIN 

### Exemples
`cmd 1> stdout.log 2> stderr.log`
`sort < noms.txt`
`ps aux | grep sshd`

## 2) Redirections combinees
⚠️  `cmd 2>&1 > log-partiel.log` stderr va vers **la ou va stdout** qui pour le moment est l'ecran, PUIS stdout va dans le fichier
⚠️  `cmd > log-tout.log 2>&1` stdout va dans le fichier, PUIS stderr va **la au va** stdout
📌 `cmd &> log-tout.log`

## 3) Here-strings et Here-documents : <<< et <<

### Entree inline
`wc -w <<< "bonjour tout le monde"`

### Entree multi-lignes
`cat << EOF > fichier.txt
ligne 1
ligne 2
EOF`

📌 variante
`cat << 'EOF'
$HOME ne sera pas expanse !
EOF
`

## 4) Commande tee
`dmesg | tee -a dmesg.log`
affiche et ecrit a l'ecran

⚠️  Attention la redirection est initie par le shell donc le sudo doit etre mis sur la commande qui ecrit
`echo "test" | sudo tee /root/test.txt`

===
📌 À retenir
- Toujours distinguer STDOUT vs STDERR.

- Le pipe | ne prend que STDOUT (sauf si on merge avec 2>&1).

- Ordre des redirections est important 

- tee pour diagnostiquer sans perdre l’affichage.

- Attention au piège sudo ... > fichier : la redirection n’est pas “sudo”.
===

## 5) Flashcaard
<details>
<summary><strong>Question :</strong><code>cmd 2> fic.log >&1 </code>Ou sont affichees les erreurs de cmd : ecran ou log ?</summary>
**Reponse**
Les erreurs dans le log et la sortie stdout va ou va stdout, donc l'ecran ici. En fait ici '>&1' = '1>&1'. Inutile donc en dehors de cet exercice de comprehension.
</details>

<!---
📌 À retenir / concepts
🛠️ Commandes / actions
🧪 Test / validation
⚠️  Attention / pièges
🔍 Debug / investigation
🔒 Sécurité / hardening
✅ OK / validé / succès
❌ Erreur / à éviter
-->
