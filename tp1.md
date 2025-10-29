# I. Prérequis

🌞 Déterminer quel algorithme de chiffrement utiliser pour vos clés

```
https://www.ethersys.fr/actualites/20241022-quelle-cle-ssh-generer/
```

trdftyf  `ls` xcvbn,

🌞 Générer une paire de clés pour ce TP

```bash
PS C:\Users\leobe> ssh-keygen -t ed25519 -f C:\Users\leobe\.ssh\cloud_t -C "clé TP Cloud"
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\leobe\.ssh\cloud_t
Your public key has been saved in C:\Users\leobe\.ssh\cloud_t.pub
The key fingerprint is:
SHA256:Y9KacXMy6gKZDULAwHzEbM0UIuVcJ0KSN0YtMAnocWg clé TP Cloud
The key's randomart image is:
+--[ED25519 256]--+
|%=%B==..         |
|.E*@++o          |
|+ Boo            |
|...    .         |
| . =  o S .      |
|  + .  O *       |
|   .  +          |
|    ..           |
|     ..          |
+----[SHA256]-----+
PS C:\Users\leobe> cd C:\Users\leobe\.ssh
PS C:\Users\leobe\.ssh> ls


    Répertoire : C:\Users\leobe\.ssh


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        29/10/2025     11:23            444 cloud_t
-a----        29/10/2025     11:23             96 cloud_t.pub
-a----        29/10/2025     09:04           2655 id_rsa
-a----        29/10/2025     09:04            570 id_rsa.pub
-a----        29/10/2025     09:18            837 known_hosts
-a----        29/10/2025     09:15             95 known_hosts.old
```

🌞 Configurer un agent SSH sur votre poste

```
PS C:\Users\leobe> ssh-add C:\Users\leobe\.ssh\cloud_t
Enter passphrase for C:\Users\leobe\.ssh\cloud_t:
Identity added: C:\Users\leobe\.ssh\cloud_t (cl├® TP Cloud)

PS C:\Users\leobe\.ssh> ssh azureuser@48.220.33.30
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1012-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Oct 29 10:29:40 UTC 2025

  System load:  0.0               Processes:             115
  Usage of /:   7.0% of 28.02GB   Users logged in:       0
  Memory usage: 33%               IPv4 address for eth0: 172.16.0.4
  Swap usage:   0%


Expanded Security Maintenance for Applications is not enabled.

34 updates can be applied immediately.
19 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


Last login: Wed Oct 29 08:44:14 2025 from 159.117.224.25
azureuser@totototo:~$

# on peut voir que maintenant on me demande plus la passphrase

```

# II. Spawn des VMs

```
PS C:\Users\leobe\.ssh> ssh custom@4.211.110.173
[...]
custom@custom:~$
```
