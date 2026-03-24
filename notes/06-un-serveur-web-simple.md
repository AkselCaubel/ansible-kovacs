# Un serveur web simple

Dossier de travail : `formation-ansible/atelier-10/`

Répertoire de travail dans la vm vagrant ansible : `~/ansible/projets/ema`

Pour cet atelier, je vais utiliser 

## Debian

Utilisation du playbook Ansible à disposition :  

```yml
---  # apache-debian.yml

- hosts: debian

  tasks:

    - name: Update package information
      apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache
      apt:
        name: apache2

    - name: Start & enable Apache
      service:
        name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Debian</title>
            </head>
            <body>
              <h1>Debian -- My first Ansible-managed website</h1>
            </body>
          </html>
```

## Rocky

> Rocky utilise le gestionnaire de paquet `dnf` avec le nom de paquet `httpd` pour le service apache2  

Changement : 

```yml

    - name: Install Apache
      dnf:
        name: httpd
```

## Suse

> Suse utilise le gestionnaire de paquet `zypper` avec le nom de paquet `apache2`.  
> Attention, le path par defaut est `/srv/www/htdocs`  

Changement :  

```yml

    - name: Install Apache
      zypper:
        name: apache2

    - name: Install custom web page
      copy:
        dest: /srv/www/htdocs/index.html
    ...

```