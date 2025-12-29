# RHCSA (EX200) — Notes de révision (RHEL 10)

> Référence officielle : objectifs EX200 (RHEL 10).  
> Rappel important : à l’examen, les configs doivent **survivre à un reboot**. 

## Checklist globale (à cocher en fin de préparation)
- [ ] Je sais exécuter chaque objectif **sans aide**
- [ ] Je sais refaire les tâches **vite**, sans “tâtonner”
- [ ] Je vérifie systématiquement : `systemctl status`, logs, `ss -tulpn`, `firewall-cmd --list-all`
- [ ] Après chaque config : je **teste**, je rends **persistant**, puis je **reboote** et je re-teste

---

## 1) Essential tools (outils essentiels) 
### Objectifs
- [ ] Shell + syntaxe correcte
- [ ] Redirections & pipes (`> >> | 2>`)
- [ ] `grep` + regex
- [ ] SSH
- [ ] Multi-user targets + switch user
- [ ] Archives/compression (`tar`, `gzip`, `bzip2`)
- [ ] Édition de fichiers
- [ ] Fichiers/dossiers (create/copy/move/delete)
- [ ] Liens hard/soft
- [ ] Permissions ugo/rwx
- [ ] Documentation (`man`, `info`, `/usr/share/doc`)

### Commandes à maîtriser
`bash`, `history`, `type`, `which`, `find`, `locate`, `grep -E`, `sed`, `awk` (basique), `tar`, `gzip`, `bzip2`,
`ssh`, `scp`, `sftp`, `su`, `sudo`, `chmod`, `chown`, `chgrp`, `ln`, `stat`, `umask`

### Fichiers / concepts clés
- Permissions, umask, ownership
- Différences **hardlink vs symlink**
- Quoting bash (`'` vs `"`), expansion, exit codes `$?`

### Labs
- [ ] Reproduire une arborescence + permissions exactes
- [ ] Extraire des infos de logs avec pipes + grep -E

---

## 2) Manage software (gestion des logiciels) 
 ## Objectifs
- [ ] Configurer l’accès aux dépôts RPM
- [ ] Installer/supprimer des paquets RPM
- [ ] Configurer l’accès aux dépôts Flatpak
- [ ] Installer/supprimer des applis Flatpak 

### Commandes
- RPM/Repo : `dnf`, `rpm`, `dnf provides`, `dnf repoquery`
- Flatpak : `flatpak remotes`, `flatpak install/remove`, `flatpak list`

### Fichiers
- `/etc/yum.repos.d/*.repo`

### Labs
- [ ] Ajouter un repo + installer un paquet + vérifier version/fichiers
- [ ] Ajouter un remote Flatpak + installer une app + vérifier

---

## 3) Create simple shell scripts (scripts bash simples) 
### Objectifs
- [ ] `if`, `test`, `[ ]`
- [ ] Boucles (`for`, éventuellement `while`)
- [ ] Arguments `$1`, `$2`, etc.
- [ ] Capturer la sortie d’une commande (`$(...)`) 

### Snippets à avoir
- Pattern “si fichier existe”, “si commande réussit”, “boucle sur fichiers”
- Parsing simple (ex: lire une liste, générer un rapport)

### Labs
- [ ] Script qui prend un user en argument et vérifie (existe ? groups ?)
- [ ] Script qui archive un dossier et logge le résultat

---

## 4) Operate running systems (exploitation du système) 
### Objectifs
- [ ] Boot/reboot/shutdown
- [ ] Targets systemd (changer/booter)
- [ ] Interrompre le boot pour récupérer l’accès
- [ ] Process : repérer + kill, priorité/scheduling
- [ ] Tuning profiles
- [ ] Logs/journald + persistance
- [ ] Gérer services réseau (start/stop/status)
- [ ] Transfert sécurisé de fichiers 

### Commandes
`systemctl`, `journalctl`, `loginctl`, `ps`, `top`, `pidstat` (si dispo), `kill/killall`, `nice/renice`,
`tuned-adm`, `shutdown`, `reboot`

### Fichiers
- Journald persistant : `/var/log/journal/` (principe)
- Targets : `multi-user.target`, `graphical.target`, `rescue.target`, `emergency.target`

### Labs
- [ ] Activer logs persistants + prouver après reboot
- [ ] Dépanner un service qui ne démarre pas via logs

---

## 5) Configure local storage (stockage local) 
### Objectifs
- [ ] GPT : partitions (create/delete)
- [ ] LVM : PV/VG/LV create/remove
- [ ] Mount au boot via UUID/label
- [ ] Ajouter partitions/LV/swap **sans casser** 

### Commandes
`lsblk`, `blkid`, `parted`/`fdisk` (selon habitude), `pvcreate`, `vgcreate`, `lvcreate`,
`mkswap`, `swapon`, `swapoff`

### Fichiers
- `/etc/fstab` (indispensable)

### Labs
- [ ] Créer PV/VG/LV + FS + mount persistant (fstab)
- [ ] Ajouter du swap et le rendre persistant

---

## 6) Create and configure file systems (systèmes de fichiers) 
### Objectifs
- [ ] VFAT/ext4/xfs : create/mount/unmount/use
- [ ] NFS : mount/unmount
- [ ] autofs
- [ ] Étendre un LV existant
- [ ] Diagnostiquer/corriger problèmes de permissions 

### Commandes
`mkfs.xfs`, `mkfs.ext4`, `mount`, `umount`, `df -hT`, `xfs_growfs`, `resize2fs`,
`showmount`, `mount -t nfs`, `systemctl status autofs`

### Fichiers
- `/etc/fstab`
- autofs : `/etc/auto.master` (+ maps)

### Labs
- [ ] Monter un NFS + rendre persistant (ou via autofs)
- [ ] Étendre LV + agrandir FS (xfs/ext4) sans perte

---

## 7) Deploy, configure, and maintain systems (déploiement & maintenance) 
### Objectifs
- [ ] Planification : `at`, `cron`, timers systemd
- [ ] Services : start/stop/enable
- [ ] Boot sur un target par défaut
- [ ] Client NTP/time sync
- [ ] Installer/mettre à jour depuis CDN, repo distant, ou local
- [ ] Modifier le bootloader 

### Commandes
`crontab`, `at`, `systemctl list-timers`, `timedatectl`, (NTP : `chronyc` selon config),
`grubby` (selon usage), `dnf`

### Labs
- [ ] Créer un timer systemd qui lance un script
- [ ] Changer target par défaut + vérifier après reboot

---

## 8) Manage basic networking (réseau basique) 
### Objectifs
- [ ] IPv4/IPv6 : configurer
- [ ] Résolution hostname
- [ ] Services réseau auto-start
- [ ] Restreindre accès via firewalld/firewall-cmd 

### Commandes
`ip a`, `ip r`, `nmcli`, `hostnamectl`, `resolvectl` (selon stack),
`ss -tulpn`, `firewall-cmd`

### Fichiers / concepts
- Zones firewalld, services vs ports, runtime vs permanent

### Labs
- [ ] Config IP + DNS + ping + résolution
- [ ] Ouvrir un service/port firewall **en permanent** + tester

---

## 9) Manage users and groups (utilisateurs & groupes) 
### Objectifs
- [ ] Créer/supprimer/modifier users
- [ ] Password aging
- [ ] Groupes + memberships
- [ ] Accès privilégié 

### Commandes
`useradd/usermod/userdel`, `passwd`, `chage`, `groupadd/groupmod/groupdel`, `id`, `getent`,
`visudo`, `sudo -l`

### Fichiers
`/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/sudoers`, `/etc/sudoers.d/*`

### Labs
- [ ] Créer un user avec contraintes (shell, home, groupes)
- [ ] Donner sudo “proprement” via `/etc/sudoers.d/`

---

## 10) Manage security (sécurité) 
### Objectifs
- [ ] Firewall (bis)
- [ ] Permissions par défaut
- [ ] SSH key-based auth
- [ ] SELinux : enforcing/permissive
- [ ] Contextes fichiers/process
- [ ] Restaurer contextes
- [ ] Labels de ports
- [ ] Booleans SELinux 

### Commandes
SSH : `ssh-keygen`, `ssh-copy-id` (ou manuel), `chmod 700 ~/.ssh`, `chmod 600 authorized_keys`  
SELinux : `getenforce`, `setenforce`, `sestatus`, `ls -Z`, `ps -eZ`, `restorecon`,
`semanage fcontext`, `semanage port`, `getsebool`, `setsebool -P`

### Labs
- [ ] Mettre en place SSH keys + interdire password (si demandé) + tester
- [ ] Dépanner un blocage SELinux (contexte/port/boolean) proprement

---

## 11) Stratégie “jour d’examen”
- Ordre de travail : *config → test → persistance → re-test après reboot*
- Toujours garder un “journal de commandes” (history + notes rapides)
- Debug minimal : `journalctl -xeu <service>`, `ss -tulpn`, `firewall-cmd --list-all`, `getenforce`
- Gestion du temps : commencer par les tâches à gros points / plus longues

---

## Annexes
### A) Commandes “météo du système”
- CPU/RAM : `top`, `free -h`
- Disques : `lsblk -f`, `df -hT`, `blkid`
- Réseau : `ip a`, `ip r`, `ss -tulpn`
- Logs : `journalctl -b`

### B) Mini check “persistance”
- systemd: `systemctl is-enabled <svc>`
- firewall: `firewall-cmd --runtime-to-permanent` (ou `--permanent` + reload)
- fstab: `mount -a` (avant reboot)
- SELinux booleans: `setsebool -P ...`

[1]: https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam "Red Hat Certified System Administrator exam | EX200"

