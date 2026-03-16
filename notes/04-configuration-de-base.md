# Configuration de base

Dossier de travail : `formation-ansible/atelier-06/`

| Machine virtuelle | Adresse IP   |
|-------------------|--------------|
| control (ansible) | 192.168.56.10|
| target01 (rocky)  | 192.168.56.20|
| target02 (debian) | 192.168.56.30|
| target03 (suse)   | 192.168.56.40|


## Définition des hosts  

```bash
#>> /etc/hosts
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan   target01
192.168.56.30  target02.sandbox.lan   target02
192.168.56.40  target03.sandbox.lan   target03
```

## Création d'une clés ssh

```bash
ssh-keygen
```

## Ajout des clés ssh sur la machine ansible

```bash
for HOST in target01 target02 target03
do
    ssh-keyscan -t rsa ${HOST} >> ~/.ssh/know_hosts
    ssh-keyscan -t rsa ${HOST}.sandbox.lan >> ~/.ssh/know_hosts
    ssh-copy-id vagrant@${HOST}
done
```

## Installation d'Ansible

> Commande :  

```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible # Package PPA
```

## Ping des machines

> Commande :  

```bash
ansible all -i target01,target02,target03 -m ping
```

## Création d'un répoertoire projet `~/monprojet`

> Commande :  

```bash
mkdir -pv ~/monprojet
cd ~/monprojet
```

## Création d'un fichier ansible.cfg vide

> Commande :  

```bash
touch ansible.cfg
```

## Vérification que le fichier a bien été pris en compte

> Commande :  

```bash
ansible --version | head -n2 | grep "$PWD/ansible.cfg"
```

## Spécification d'un inventaire `hosts.ini`

> Utilisation de l'option `-i` en ligne de commande ou bien dans l'inventaire :  

```ini
[defaults]
inventory = ./hosts.ini
```


## Activation de la journalisation

> Ajout de la ligne suivante dans le fichier `ansible.cfg`

```ini
log_path = ~/journal/ansible.log
```

## Vérification de la journalisation : 

```log
2026-03-16 10:09:23,763 p=3550 u=vagrant n=ansible | [WARNING]: Platform linux on host target01 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change the
meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.

2026-03-16 10:09:23,764 p=3550 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
2026-03-16 10:09:23,764 p=3550 u=vagrant n=ansible | [WARNING]: Platform linux on host target02 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change the
meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.

2026-03-16 10:09:23,765 p=3550 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
2026-03-16 10:09:23,766 p=3550 u=vagrant n=ansible | [WARNING]: Platform linux on host target03 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change the
meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.

2026-03-16 10:09:23,766 p=3550 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
```

## Définition du groupe `[testlab]`

> Groupe Ansible :  

```ini
[testlab]
target01
target02
target03
```

## Définition de l'utilisateur vagrant par defaut

> Utilisation du `:vars`  
> Ajout dans le fichier d'inventaire :  

```ini
[testlab:vars]
ansible_user=vagrant
```

## Vérification d'un ping vers le groupe all

```json
ansible all -m ping
target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## Définition de l'élévation de droit ansible

> Ajout dans le `:vars` du groupe `testlab` la ligne suivante :  

```ini
ansible_become=yes
```

## Affichage de la première ligne du fichier /etc/shadow

```bash
ansible all -m shell 'head -n 1 /etc/shadow -o'
```

**output**
```bash
ansible all -a "head -n 1 /etc/shadow"
target02 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target01 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
```

## Nettoyage de l'atelier

```bash
exit
vagrant destroy -f
```