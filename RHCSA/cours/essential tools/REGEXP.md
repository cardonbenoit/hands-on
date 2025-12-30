---
course: "RHCSA"
module: "Understand and use essential tools"
title: "REGEXP: ERE vs BERE"
date: 2025-12-30
level: "Intermediate"
tags: [regexpm, ere, bere, sed, grep, awk]
---

## 0) Objectifs
- [ ] Faire un point rapide sur les REGEXP
- [ ] Identifier les pieges en CLI
- [ ] Application pratique a grep, sed, awk


## 1) Concepts clés
### Définitions
- **BRE** (Basic Regular Expressions) +, ?, |, {m,n} ne sont pas “spéciaux” sauf si on les échappe
- **ERE** (Extended Regular Expressions) +, ?, |, {m,n} sont *directement spéciaux*

⚠️  Le vrai piège : On a 2 (parfois 3) couches d’échappement
- Le shell (quoting)
- La regex (BRE vs ERE vs PCRE)
- (pour sed seulement) la partie remplacement s/REGEX/REPL/ n’obéit pas aux mêmes règles que REGEX

### 📌 À retenir: choisir ERE 
Objectif : réduire ces couches et ne pas se battre avec les backslashes ? → ERE + quotes simples.
En ERE : (...), a|b, a+, a?, a{2,5} marchent sans backslash

⚠️  Et attention : “regex” ≠ “regex PCRE”
grep -P (si dispo) active PCRE (Perl-compatible),
C'est encore different. On ne developpera pas dans ce document.

## 2) Regles de survie
### Regle 1 : Utiliser ERE partout
- grep -E '...' (ERE)
- sed -E 's/.../.../' (ERE) 
- awk / gawk : regex = ERE (superset POSIX) par nature

### Regle 2 : Mettre le Regex entre quotes simples ' '
Evite que le shell (bash) mange une partie des caractères avant même que grep/sed/awk ne voie ta regex ($, \, !, etc.)

## 3) Operateurs et POSIX (valable pour grep -E, sed -E, awk)
### Opérateurs de base
- Début/fin de ligne : ^ $
- N’importe quel char : .
- Répétitions : * (0+), + (1+), ? (0/1), {m,n}
- Alternatives : foo|bar
- Groupes : (abc)

### Classes POSIX : lisibles et portables (mieux que \d, \s, \w) : 
- chiffre : [[:digit:]]
- espace : [[:space:]]
- lettre : [[:alpha:]]
- alphanum : [[:alnum:]]

### Pour grep on s'autorise \b (boundary = limite de mot)
Quand on peut pas le faire POSIX

## 4) Astuces
### Fixed string : recherche avec grep -F 
`grep -F 'texte(littéral)' fichier`  
ne sera pas vu un groupe de caracteres, mais lu tel quel

📌 pas de confusion
`sed -E 's/^([[:alpha:]]+)=([0-9]+)/cle=\1 valeur=\2/'`  
Dans la partie remplacement \1 et \2 sont des backrefs, rien avoir avec posix.

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
