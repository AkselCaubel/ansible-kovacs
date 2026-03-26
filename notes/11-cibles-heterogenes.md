# Cibles hétérogènes

Dossier de travail : `formation-ansible/atelier-17/`

Répertoire de travail dans la vm vagrant ansible : `~/ansible/projets/ema`

## Chrony-01 (méthode gros sabots)

```yml
---  # chrony-01.yml

- hosts: all

  tasks:

    ### DEBIAN

    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony
      apt:
        name: chrony
      when: ansible_os_family == "Debian"

    ### ROCKY

    - name: Install Chrony
      dnf:
        name: chrony
      when: ansible_os_family == "Rocky"

    ### SUSE

    - name: Install Chrony
      zypper:
        name: chrony
      when: ansible_os_family == "openSUSE Leap"

    - name: Start & enable Chrony
      service:
        name: chronyd.service
        state: started
        enabled: true

    - name: Set chrony configuration
      copy:
        dest: /etc/chrony.conf
        mode: 0644
        content: |
          # /etc/chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart Chrony

  handlers:

    - name: Restart Chrony
      service:
        name: chronyd.service
        state: restarted
```

## Chrony-02

```yml

---  # chrony-02.yml

- hosts: all

  vars:

    apache:
      Debian:
        package_name: chrony
        service_name: chronyd.service
      Ubuntu:
        package_name: chrony
        service_name: chronyd.service
      Rocky:
        package_name: chrony
        service_name: chronyd.service
      openSUSE Leap:
        package_name: chrony
        service_name: chronyd.service

  tasks:

    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony
      package:
        name: "{{ apache[ansible_distribution].package_name }}"

    - name: Start & enable Chrony
      service:
        name: "{{ apache[ansible_distribution].service_name }}"
        state: started
        enabled: true

    - name: Set chrony configuration
      copy:
        dest: /etc/chrony.conf
        mode: 0644
        content: |
          # /etc/chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart Chrony

  handlers:

    - name: Restart Chrony
      service:
        name: "{{ apache[ansible_distribution].service_name }}"
        state: restarted
```