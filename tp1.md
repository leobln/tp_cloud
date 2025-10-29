🌞 Déterminer quel algorithme de chiffrement utiliser pour vos clés

```https://www.ethersys.fr/actualites/20241022-quelle-cle-ssh-generer/```

🌞 Générer une paire de clés pour ce TP

```
azureuser@totototo:~$ ssh-keygen -t ed25519 -f ~/.ssh/cloud_tp -C "clé TP Cloud"
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/cloud_tp
Your public key has been saved in /home/azureuser/.ssh/cloud_tp.pub
The key fingerprint is:
SHA256:5/zBfRzfairxKxoK/JynV+bqC/OboFhGhoYh4/uRWQA clé TP Cloud
The key's randomart image is:
+--[ED25519 256]--+
| E               |
|  .              |
|+  .             |
|o+ ..            |
|..o o.  S .    . |
| ..++    ++. . .+|
| . += + .+ooo . =|
|  .+.= Boo+....o |
|  ... *=O= o++.  |
+----[SHA256]-----+
azureuser@totototo:~$ cd ./.ssh
azureuser@totototo:~/.ssh$ ls
authorized_keys  cloud_tp  cloud_tp.pub
```