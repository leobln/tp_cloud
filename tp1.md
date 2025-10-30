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

🌞 Connectez-vous en SSH à la VM pour preuve

```
PS C:\Users\leobe\.ssh> ssh custom@4.211.110.173
[...]
custom@custom:~$
```

```
PS C:\Users\leobe> az vm create `
>>    --resource-group custom_group `
>>    --name cloud.tp1 `
>>    --image Ubuntu2404 `
>>    --size Standard_B1s `
>>    --admin-username azureuser `
>>    --ssh-key-values "C:\Users\leobe\.ssh\cloud_t.pub"`
>>    --public-ip-sku Standard `
>>    --location spaincentral `
>>    --output table
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
ResourceGroup    PowerState    PublicIpAddress    Fqdns    PrivateIpAddress    MacAddress         Location
---------------  ------------  -----------------  -------  ------------------  -----------------  ------------
custom_group     VM running    158.158.50.205              10.0.0.4            7C-ED-8D-16-3D-05  spaincentral
```

🌞 Assurez-vous que vous pouvez vous connecter à la VM en SSH sur son IP publique

```
PS C:\Users\leobe> ssh azureuser@158.158.50.205
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1012-azure x86_64)
[...]
azureuser@cloud:~$
```

🌞 Une fois connecté, prouvez la présence du service walinuxagent.service, du service cloud-init.service

```
zureuser@cloud:~$ sudo systemctl status walinuxagent
● walinuxagent.service - Azure Linux Agent
     Loaded: loaded (/usr/lib/systemd/system/walinuxagent.service; enab>
    Drop-In: /run/systemd/system.control/walinuxagent.service.d
             └─50-CPUAccounting.conf, 50-MemoryAccounting.conf
     Active: active (running) since Wed 2025-10-29 11:44:57 UTC; 7min a>

#on voit que c'est activer 
```

```
azureuser@cloud:~$ sudo systemctl status cloud-init
● cloud-init.service - Cloud-init: Network Stage
     Loaded: loaded (/usr/lib/systemd/system/cloud-init.service; enable>
     Active: active (exited) since Wed 2025-10-29 11:44:56 UTC; 10min a>
   Main PID: 706 (code=exited, status=0/SUCCESS)
        CPU: 1.435s
```

🌞 Créez une deuxième VM : azure2.tp1

```
PS C:\Users\leobe> az network nic create `
>>   --resource-group custom_group `
>>   --name azure2NIC `
>>   --vnet-name custom_group-vnet `
>>   --subnet default `
>>   --location spaincentral `
>>   --output table

PS C:\Users\leobe> az vm create `
>>   --resource-group custom_group `
>>   --name azure2.tp1 `
>>   --nics azure2NIC `
>>   --image Ubuntu2404 `
>>   --size Standard_B1s `
>>   --admin-username azureuser `
>>   --ssh-key-values "C:\Users\leobe\.ssh\cloud_t.pub" `
>>   --location spaincentral `
>>   --output table
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
ResourceGroup    PowerState    PublicIpAddress    Fqdns    PrivateIpAddress    MacAddress         Location
---------------  ------------  -----------------  -------  ------------------  -----------------  ------------
custom_group     VM running
```

🌞 Affichez des infos au sujet de vos deux VMs

```
PS C:\Users\leobe> az vm list-ip-addresses -g custom_group -o table
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
custom            4.211.110.173        172.17.0.4
azure2.tp1                             10.0.0.4
cloud.tp1         158.158.50.205       10.0.0.4
```

🌞 Configuration SSH client pour les deux machines

```
notepad $env:USERPROFILE\.ssh\config
```

```
Host az1
    HostName 158.158.50.205
    User azureuser
    IdentityFile C:\Users\leobe\.ssh\cloud_t
    IdentitiesOnly yes

Host az2
    HostName 10.0.0.5
    User azureuser
    IdentityFile C:\Users\leobe\.ssh\cloud_t
    ProxyJump az1
    IdentitiesOnly yes
```

```
PS C:\Users\leobe> ssh az1
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1012-azure x86_64)
[...]
azureuser@cloud:~$
```

```
azureuser@cloud:~$ exit
logout
Connection to 158.158.50.205 closed.
PS C:\Users\leobe> ssh az2
The authenticity of host '10.0.0.4 (<no hostip for proxy command>)' can't be established.
[...]
Last login: Thu Oct 30 08:08:26 2025 from 159.117.224.24
azureuser@cloud:~$
```

