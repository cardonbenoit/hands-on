---
course: "RHCSA"
module: "Understand and use essential tools"
title: "sed"
date: 2025-12-29
level: "Intermediate"
tags: [sed]
---

## Objectifs
- Maitriser **sed** (Stream EDitor) 

## 1) Concepts clés
- sed lit le texte ligne par ligne
- applique des commandes
- affiche le resultat

## 2) Syntaxe de base
On peut choisir l'occurence a substituer 
```bash
echo 'Chat ChAt miam chaT' | sed 's/chat/chien/gI' 
chien chien miam chien

echo 'Chat ChAt miam chaT' | sed 's/chat/chien/2I' 
Chat chien miam chaT
```
I pour insentive-case / g pour global / 2 pour 2eme occurence

Pour substituer sur plusieurs occurences, il faut chainer:
```bash
echo 'Chat ChAt miam chaT chat' | sed 's/chat/chien/3I; s/chat/chien/2I'
Chat chien miam chien chat
```
## 3) Manipulations complexes avec sed sous vi
### substitution selon le numero d'occurence et par plage (exemple)
> [[qwerty]]
> [[qwerty]]
> [[qwerty]]
> [[qwerty]]
> [[qwerty]]
```bash
%!sed '1,2s/\[/*/2 ; 3,5s/\[/*/1 ; 2,$s/\]/*/2'
```
> [*qwerty]]
> [*qwerty]*
> *[qwerty]*
> *[qwerty]*
> *[qwerty]*

📌 On peut selectionner des plages de lignes ou une ligne particuliere, ou toutes avec '^,$s/.../...'

### Supprimer des lignes vides dans une place sous vi
`%!sed '87,105{/^[[:space:]]*$/d;}'`
⚠️  Dans cette notation il faut bien mettre un ; avant de fermer le bloc {  ;}

> 🔍 Marquer une position sous Vi et y retourner, utile notamment quand sed sous vi remonte en debut de fichier !
> ma : met une marque a sur la ligne courante
> `a : revient exactement à la position (ligne+colonne)

## 4) Lignes qui matchent une regex comme condition de la substitution
```bash
sed '/ERROR/s/foo/bar/' fichier
```

Lignes qui NE matchent PAS une regex
```bash
sed '/ERROR/!s/foo/bar/' fichier
```
⚠️  penser a backquoter ! si on utilise dans vi :  
:%!sed '/ERROR/\!s/foo/OK/'

## 5) Supprimer/Filtrer des lignes
```bash
sed '5d' fichier
sed '1,10d' fichier
sed '/^$/d' fichier #supprimer lignes vides
```

Grep-like
`sed -n '/ERROR/p' fichier`  

Ajouter **une ligne après** (append) : a
`sed '/motif/a\Ligne ajoutée' fichier`

Insérer **une ligne avant** (insert) : i
`sed '/motif/i\Ligne avant' fichier`  

Remplacer **la ligne entière** : c
`sed '/motif/c\Nouvelle ligne complète' fichier`  


## 6) 📌 Regex utiles dans sed
> ^ début de ligne
> $ fin de ligne
> . n’importe quel caractère
> * répétition (0 ou +)
> \+ (GNU sed) 1 ou + (souvent mieux: -E pour regex étendues)
> [] classe de caractères
> [^] négation
> () groupes (avec sed -E)
> | ou (avec sed -E)- 
-
Regex étendues (recommandé)
`sed -E 's/(chat|chien)/animal/g' fichier`  

## 7) Groupes et backreferences
### Reutiliser une partie matchee
`echo "nom=dupont" | sed -E 's/(.*)=(.*)/cle:\1 valeur:\2/'`  
# -> cle:nom valeur:dupont

### Enlever les espaces en debut et en fin
`echo "   bonjour   " | sed -E 's/^ +//; s/ +$//'`  

## 8) Plusieurs commandes à la suite
### Separe par ;
`sed -E 's/foo/bar/g; s/baz/qux/g' fichier`  

### Separe par -e
`sed -e 's/foo/bar/g' -e '/^#/d' fichier`  

## 9) Cas pratiques “admin / logs / config”
### A) Commenter une ligne de config (si elle commence par PermitRootLogin)
`sed -E 's/^(PermitRootLogin)/#\1/' sshd_config`  

### B) Décommenter une ligne (enlever # au début)
`sed -E 's/^#(PermitRootLogin)/\1/' sshd_config`  

### C) Remplacer une valeur de clé (ex: PORT=80 -> PORT=8080)
`sed -E 's/^(PORT=).*/\18080/' .env`  

### D) Extraire un bloc entre deux motifs
`sed -n '/DEBUT/,/FIN/p' fichier`  
-n rend sed silencieux, puis -p affiche ce qui matche
, = opérateur “intervalle”
DONC = se lit : à partir de la première ligne qui matche DEBUT jusqu’à la première ligne suivante qui matche FIN, imprime (p) toutes les lignes.
- Si DEBUT apparaît plusieurs fois, sed recommence un nouvel intervalle après avoir atteint FIN.
- Si FIN n’apparaît pas après un DEBUT, alors sed imprime jusqu’à la fin du fichier.

## 10) Pièges classiques
- Par défaut, sed affiche tout : utilise -n + p pour filtrer.
- Les / dans les chemins : choisis un autre séparateur :
  `sed 's#/var/www#/srv/www#g'`  
-i modifie le fichier : pense au backup -i.bak.
  Différences GNU/BSD (macOS) surtout sur -i et certaines options.

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
