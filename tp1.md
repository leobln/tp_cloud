## I. Prérequis

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

##  II. Spawn des VMs

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
    HostName 10.0.0.4
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

## III. Déployer et configurer un machin

🌞 Installer MySQL/MariaDB sur azure2.tp1

```
azureuser@cloud:~$ sudo apt install -y mariadb-server
```

🌞 Démarrer le service MySQL/MariaDB sur azure2.tp1

```
azureuser@cloud:~$ sudo systemctl enable mariadb --now
Synchronizing state of mariadb.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable mariadb
azureuser@cloud:~$ sudo systemctl status mariadb
● mariadb.service - MariaDB 10.11.13 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; >
     Active: active (running) since Thu 2025-10-30 08:18:22 UTC; 2min 6>
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
   Main PID: 21499 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 10 (limit: 6554)
     Memory: 89.3M (peak: 95.0M)
        CPU: 368ms
     CGroup: /system.slice/mariadb.service
             └─21499 /usr/sbin/mariadbd
[...]
azureuser@cloud:~$
```

🌞 Ajouter un utilisateur dans la base de données pour que mon app puisse s'y connecter

```
azureuser@cloud:~$ sudo mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 31
Server version: 10.11.13-MariaDB-0ubuntu0.24.04.1 Ubuntu 24.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CREATE DATABASE meow_database;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> CREATE USER 'meow'@'%' IDENTIFIED BY 'meow';
Query OK, 0 rows affected (0.006 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES ON meow_database.* TO 'meow'@'%';
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> SELECT User, Host FROM mysql.user;
+-------------+-----------+
| User        | Host      |
+-------------+-----------+
| meow        | %         |
| mariadb.sys | localhost |
| mysql       | localhost |
| root        | localhost |
+-------------+-----------+
4 rows in set (0.004 sec)
```

🌞 Ouvrez un port firewall si nécessaire

```
azureuser@cloud:~$ sudo ss -lnpt
State    Recv-Q   Send-Q     Local Address:Port     Peer Address:Port   Process
LISTEN   0        80               0.0.0.0:3306          0.0.0.0:*       users:(("mariadbd",pid=22277,fd=23))
LISTEN   0        4096          127.0.0.54:53            0.0.0.0:*       users:(("systemd-resolve",pid=499,fd=17))
LISTEN   0        4096             0.0.0.0:22            0.0.0.0:*       users:(("sshd",pid=1575,fd=3),("systemd",pid=1,fd=177))
LISTEN   0        4096       127.0.0.53%lo:53            0.0.0.0:*       users:(("systemd-resolve",pid=499,fd=15))
LISTEN   0        4096                [::]:22               [::]:*       users:(("sshd",pid=1575,fd=4),("systemd",pid=1,fd=178))
azureuser@cloud:~$ sudo ufw allow from 10.0.0.0/16 to any port 3306 proto tcp
Rules updated
azureuser@cloud:~$ sudo ufw reload
```

```
azureuser@cloud:~$ mysql -h 10.0.0.4 -u meow -p meow_database
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 31
Server version: 10.11.13-MariaDB-0ubuntu0.24.04.1 Ubuntu 24.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [meow_database]>
```

# 2. Machine azure1.tp1

🌞 Récupération de l'application sur la machine

```
azureuser@cloud:~$ sudo mkdir -p /opt/meow
azureuser@cloud:~$ sudo chown $USER:$USER /opt/meow
azureuser@cloud:~$ cd /opt/meow
azureuser@cloud:/opt/meow$ git clone https://gitlab.com/it4lik/b2-pano-cloud-2025.git .
Cloning into '.'...
remote: Enumerating objects: 381, done.
remote: Counting objects: 100% (301/301), done.
remote: Compressing objects: 100% (298/298), done.
remote: Total 381 (delta 140), reused 0 (delta 0), pack-reused 80 (from 1)
Receiving objects: 100% (381/381), 14.26 MiB | 26.54 MiB/s, done.
Resolving deltas: 100% (163/163), done.
azureuser@cloud:/opt/meow$ cd docs/tp/1/app
azureuser@cloud:/opt/meow/docs/tp/1/app$ ls
app.py  docker-compose.yml  requirements.txt  templates
```

🌞 Installation des dépendances de l'application

```
azureuser@cloud:/opt/meow$ cd /opt/meow/docs/tp/1/app
azureuser@cloud:/opt/meow/docs/tp/1/app$ python -m venv .
Command 'python' not found, did you mean:
  command 'python3' from deb python3
  command 'python' from deb python-is-python3
azureuser@cloud:/opt/meow/docs/tp/1/app$ python3 -m venv .
azureuser@cloud:/opt/meow/docs/tp/1/app$ ./bin/pip install -r requirements.txt
Collecting Flask (from -r requirements.txt (line 1))
  Using cached flask-3.1.2-py3-none-any.whl.metadata (3.2 kB)
[...]
azureuser@cloud:/opt/meow/docs/tp/1/app$
```

🌞 Configuration de l'application

```
azureuser@cloud:/opt/meow/docs/tp/1/app$ nano .env
```

```
# Flask Configuration
FLASK_SECRET_KEY=ewnFw95H7qBeGiVvkQl9YmnJohW6NCMMqR0arxfnWYASeCDvzwQwzL>
FLASK_DEBUG=False
FLASK_HOST=0.0.0.0
FLASK_PORT=8000

# Database Configuration
DB_HOST=10.0.0.4
DB_PORT=3306
DB_NAME=meow_database
DB_USER=meow
DB_PASSWORD=meow
```

🌞 Gestion de users et de droits

```
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo useradd -m -s /bin/bash webapp
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo passwd webapp
New password:
Retype new password:
passwd: password updated successfully
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo groupadd webapp
groupadd: group 'webapp' already exists
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo chown -R webapp:webapp /opt/meow/docs/tp/1/app
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo chmod -R o-rwx /opt/meow/docs/tp/1/app
azureuser@cloud:/opt/meow/docs/tp/1/app$ ls -l /opt/meow/docs/tp/1/app
ls: cannot open directory '/opt/meow/docs/tp/1/app': Permission denied
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo ls -l /opt/meow/docs/tp/1/
app
total 36
-rw-rw---- 1 webapp webapp 3827 Oct 30 08:57 app.py
drwxrwx--- 2 webapp webapp 4096 Oct 30 09:14 bin
-rw-rw---- 1 webapp webapp  223 Oct 30 08:57 docker-compose.yml
drwxrwx--- 4 webapp webapp 4096 Oct 30 09:14 include
drwxrwx--- 3 webapp webapp 4096 Oct 30 09:14 lib
lrwxrwxrwx 1 webapp webapp    3 Oct 30 09:14 lib64 -> lib
-rw-rw---- 1 webapp webapp  162 Oct 30 09:14 pyvenv.cfg
-rw-rw---- 1 webapp webapp   58 Oct 30 08:57 requirements.txt
drwxrwx--- 2 webapp webapp 4096 Oct 30 08:57 templates
drwxrwx--- 5 webapp webapp 4096 Oct 30 09:01 venv
```

🌞 Création d'un service webapp.service pour lancer l'application

```
[Unit]
Description=Super Webapp MEOW

[Service]
User=webapp
WorkingDirectory=/opt/meow
ExecStart=/opt/meow/docs/tp/1/app/bin/python docs/tp/1/app/app.py


[Install]
WantedBy=multi-user.target
```

```
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo systemctl daemon-reload
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo systemctl start webapp.service
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo systemctl status webapp.service
● webapp.service - Super Webapp MEOW
     Loaded: loaded (/etc/systemd/system/webapp.service; disabled; pres>
     Active: active (running) since Thu 2025-10-30 09:37:04 UTC; 2s ago
   Main PID: 25350 (python)
      Tasks: 1 (limit: 993)
     Memory: 41.1M (peak: 41.3M)
        CPU: 427ms
     CGroup: /system.slice/webapp.service
             └─25350 /opt/meow/docs/tp/1/app/bin/python docs/tp/1/app/a>

Oct 30 09:37:04 cloud systemd[1]: Started webapp.service - Super Webapp>
Oct 30 09:37:04 cloud python[25350]:  * Serving Flask app 'app'
Oct 30 09:37:04 cloud python[25350]:  * Debug mode: off
Oct 30 09:37:04 cloud python[25350]: WARNING: This is a development ser>
Oct 30 09:37:04 cloud python[25350]:  * Running on all addresses (0.0.0>
Oct 30 09:37:04 cloud python[25350]:  * Running on http://127.0.0.1:8000
Oct 30 09:37:04 cloud python[25350]:  * Running on http://10.0.0.4:8000
Oct 30 09:37:04 cloud python[25350]: Press CTRL+C to quit
```

🌞 Ouverture du port80 dans le(s) firewall(s)

```
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo ss -lnpt | grep python
LISTEN 0      128          0.0.0.0:8000      0.0.0.0:*    users:(("python",pid=25350,fd=4))
```

```
azureuser@cloud:/opt/meow/docs/tp/1/app$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
3306/tcp                   ALLOW       10.0.0.0/16
8000/tcp                   ALLOW       Anywhere
8000/tcp (v6)              ALLOW       Anywhere (v6)
```

🌞 L'application devrait être fonctionnelle sans soucis à partir de là

```
PS C:\Users\leobe> curl http://158.158.50.205:8000/


StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html>
                    <html lang="en">
                    <head>
                        <meta charset="UTF-8">
                        <meta name="viewport"
                    content="width=device-width, initial-scale=1.0">
                        <title>Purr Messages - Cat Message
                    Board</title>
                        <...
```



