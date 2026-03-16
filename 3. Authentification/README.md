# Authentification

Dossier de travail : `formation-ansible/atelier-03/`

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

**Output**

```bash
vagrant@control:~$ for HOST in target01 target02 target03; do     ssh-keyscan -t rsa ${HOST} >> ~/.ssh/know_hosts;     ssh-keyscan -t rsa ${HOST}.sandbox.lan >> ~/.ssh/know_hosts;     ssh-copy-id vagrant@${HOST}; done
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target01.sandbox.lan:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
The authenticity of host 'target01 (192.168.56.20)' can't be established.
ED25519 key fingerprint is SHA256:CifvuKnCFUxaTaHjJPvXJRQcfQVeROGRu5rhIY1FQYs.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.

# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02.sandbox.lan:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
The authenticity of host 'target02 (192.168.56.30)' can't be established.
ED25519 key fingerprint is SHA256:CifvuKnCFUxaTaHjJPvXJRQcfQVeROGRu5rhIY1FQYs.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target02'"
and check to make sure that only the key(s) you wanted were added.

# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target03.sandbox.lan:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
The authenticity of host 'target03 (192.168.56.40)' can't be established.
ED25519 key fingerprint is SHA256:CifvuKnCFUxaTaHjJPvXJRQcfQVeROGRu5rhIY1FQYs.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
    ~/.ssh/known_hosts:4: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target03'"
and check to make sure that only the key(s) you wanted were added
```

## Ping des machines  

```json
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```