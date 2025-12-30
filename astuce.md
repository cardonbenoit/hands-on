## Copier-Coller depuis/dans vi dans un terminal sous Mac
On tape dans vi `:version`, s'il y a +clipboard alors on peut selectionner a la souris et coller dans le presse papier avec
`"+y` 
Dans ~/.vimrc :
`set mouse=`
permet que vi n'intercepte pas la souris et laisse faire le terminal

## Vi pour ajouter une ligne au-dessus/au-dessous
`O` nouvelle ligne au-dessus  
`o` nouvelle ligne en-dessous  

## Visualiser un fichier md dans un terminal
`glow -p FIC.md`  
-p permet le pager less

## Substituer sous VI
### substituer **toutes** les occurences **dans la ligne**
`:s/ancien/nouveau/gci`
### substituer **dans une plage de ligne** les occurences 
`:10,20s/ancien/nouveau/gci`
### substituer **toutes** les occurences **dans le fichier**
`:%s/ancien/nouveau/gci`
### substituer **mot entier** seulement
`:%s/\<ancien\>/nouveau/g`
- g pour global
- c pour confirm
- i pour ignorer la casse
ℹ️  \< / \> : début / fin de mot : Pas du POSIX, extension du moteur Vim
### sous vi on peut aussi utiliser sed
``:%!sed -E '47,66{/^[[:space:]]*$/d;}'`  

## Chercher dans man
### Rechercher sans ouvrir le man
⚠️  man utilise un PAGER, souvent 'less'. Associé à un pipe, 'less' sait qu'il n'est pas en mode interactif et qu'il doit envoyer sur stdout.  
⚠️  Toutefois dans le man la mise en forme peut casser un grep.  
La meilleur option est de filtrer avec col pour avoir un format **brut**   
```bash
man commande | col -bx | grep -niC5 "mot"`  
```

grep -n donne le numero de ligne, -i l'insensitive-case  
grep -Cx donne les x lignes avant et apres pour avoir le contexte  

### Recherche dans man
`man -P 'less -N' commande`  
man -P change le PAGER  
Dans less, on atteint la ligne 10 en tapant **10g**   
Penser a la variable d'environnement LESS pour passer les options par defaut (notamment -N, -i, -g , --usecolor et --color=Nc)  
L'option less -g permet un highlight de l'occurence de recherche en cours selection  
On peut aussi utiliser MANPAGER='less <OPTIONS>'  

### Chercher une option précise  
`man rsync | col -bx | grep -n -E "(\-\-recursive|\-r)\b"`  
L'option -E : regexp etendues et \b limite de mot  
⚠️  Pour indiquer a grep la fin des options, utiliser -- 
⚠️  Ca permet d'eviter le backquote des tirets  
`man rsync | col -bx | grep -En -- "(--recursive|-r)\b"` 


### Trouver quel man contient un terme
`whatis commande` But: donner une définition courte d’une commande par son nom  
`apropos commande` But: quelles pages de man parlent d’un mot-clé ?

### Ouvrir directement sur une occurence
`man commande | less +/mot`  
