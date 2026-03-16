# Idempotence

Dossier de travail : `formation-ansible/atelier-07/`

Répertoire de travail dans la vm vagrant ansible : `~/ansible/projets/ema`

## Installation de paquets

Installation des paquets suivants : 

- tree
- git
- nmap

> Commande :  

```bash
ansible all -m package -a "name=tree,git,nmap state=present"
```

> NOTE :  
> Si on relance la commande, l'output ansible passe en `OK` et non plus en `changed`

## Désinstallation des mêmes paquets

> Commande : 

```bash
ansible all -m package -a "name=tree,git,nmap state=absent"
```

## Copie du fichier local `/etc/fstab` vers les targets host

> Utilisation du module `copy`

```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
```

**Extrait d'output**

```json
debian | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1773658191.0041704-6041-10757197328072/source",
    "state": "file",
    "uid": 0
}
```

## Suppression du fichier distant

> Commande :  

```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent"
```

**Exemple d'output**

```json
debian | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
```

## Affichage de l'espace utilisé

> Commande :  

```bash
ansible all -a "df -h /"
```

**Output**
```bash
debian | CHANGED | rc=0 >>
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/debian--12--vg-root   62G  1.7G   57G   3% /
rocky | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        61G  2.1G   59G   4% /
suse | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        64G  2.4G   58G   4% /
```
On remarque que la commande restera toujours en state `changed` car le contenu du stdin sera ne peut pas être comparé.