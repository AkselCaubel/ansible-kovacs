## Test-01 Création des VMs

> Commande de lancement : `vagrant up`

![test-01-vms](images/test-01-vms.png)


### Check d'accessibilité

```bash
[ema@caubel:~] $ ping -c 1 -q 192.168.56.10
PING 192.168.56.10 (192.168.56.10) 56(84) bytes of data.

--- 192.168.56.10 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.354/0.354/0.354/0.000 ms
[ema@caubel:~] $ ping -c 1 -q 192.168.56.20
PING 192.168.56.20 (192.168.56.20) 56(84) bytes of data.

--- 192.168.56.20 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.246/0.246/0.246/0.000 ms
[ema@caubel:~] $ ping -c 1 -q 192.168.56.30
PING 192.168.56.30 (192.168.56.30) 56(84) bytes of data.

--- 192.168.56.30 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.244/0.244/0.244/0.000 ms
[ema@caubel:~] $ ping -c 1 -q 192.168.56.40
PING 192.168.56.40 (192.168.56.40) 56(84) bytes of data.

--- 192.168.56.40 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.218/0.218/0.218/0.000 ms
```

> Commande de suppression : `vagrant destroy -f`

## Test-02

### Ajout des différentes box

```bash
vagrant box add bento/rockylinux-9
vagrant box add bento/debian-12
vagrant box add bento/opensuse-leap-15
vagrant box add bento/ubuntu-22.04
```

### Vérification 

![test-2-vms](images/test-02-vms.png)

> Suppression du test : `vagrant destroy -f`