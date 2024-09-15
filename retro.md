# **[RETRO](https://tryhackme.com/r/room/retro)**

Outils utilisés: [NMAP](https://nmap.org/) - [GOBUSTER](https://github.com/OJ/gobuster) - [XFREERDP](https://www.freerdp.com/)

<br>

### Question 1
<pre>
A web server is running on the target. What is the hidden directory which the website lives on?

Un serveur web fonctionne sur la cible. Quel est le répertoire caché dans lequel se trouve le site web ?
</pre>

<br>

> [!TIP]
> dirbuster 2.3 medium

<br>

Lancez la machine cible et l'`Attackbox`.
<br>Nous allons commencer par énumérer les ports ouverts grâce à l’outils `nmap`.

<br>

> [!WARNING]
> L’adresse IP de la cible peut varier, c'est pourquoi je ne la montre pas dans la commande mais adaptez la à votre IP.

<br>

```bash
nmap -Pn -sV <IP_CIBLE>
```

<pre>
Starting Nmap 7.60 ( https://nmap.org ) at 2024-09-15 08:27 BST
Nmap scan report for ip-10-10-109-14.eu-west-1.compute.internal (10.10.109.14)
Host is up (0.00071s latency).
Not shown: 998 filtered ports
PORT     STATE SERVICE       VERSION
<b>80/tcp   open  http          Microsoft IIS httpd 10.0
3389/tcp open  ms-wbt-server Microsoft Terminal Services</b>
MAC Address: 02:11:C4:C1:26:0D (Unknown)
Service Info: <b>OS: Windows</b>; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.81 seconds
</pre>

Nous constatons qu’il y a deux ports ouverts, le 80 et le 3389 et que la machine est basée sur Windows.

Facultatif: Pour éviter d’avoir à retenir l’adresse IP de la machine cible, nous allons la convertir par son nom de scène dans le fichier `/etc/hosts` de la machine hôte:

```bash 
echo "<IP_CIBLE> retroweb | sudo tee -a /etc/hosts"
```

Maintenant que c’est fait, quand nous tapons `http://retroweb` dans la barre d’adresse de firefox, nous avons bien accès à la page du serveur web de la cible, qui, pour rappel, est sous Windows Server:

<br>

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_homepage.png"
        style="width:100%">
</div>

<br>

L'indice nous conseille d’utiliser la wordlist `dirbuster 2.3 medium`:
<br>Pour utiliser `Dirbuster`, nous allons avoir besoin de l’outils `Gobuster`:

```bash
gobuster dir -u retroweb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

<pre>
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            <b>http://retroweb</b>
[+] Threads:        10
[+] Wordlist:       <b>/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt</b>
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2024/09/15 08:41:30 Starting gobuster
===============================================================
<b>/retro (Status: 301)</b>
/Retro (Status: 301)
===============================================================
2024/09/15 08:41:54 Finished
===============================================================
</pre>

> [!NOTE]
> - `dir` - Indique que nous allons utiliser un répertoire ou un fichier pour le brute-force
> - `-u` (url) – Indique l’url de l’attaque.
> - `-w` (wordlist) – Indique le chemin de la wordlist conseillée par l’astuce

Une fois le scanne terminé, le dossier caché nous est dévoilé:

<br>

> [!IMPORTANT]
> <b>/retro</b>

<br>

---

### Question 2

Dans ce deuxième exercice, il nous est demandé de trouver le fichier `user.txt`:

<br>

<pre>
[+ 50] - user.txt
</pre>

<br>

> [!TIP]
> Don't leave sensitive information out in the open, even if you think you have control over it.
>
> Ne laissez pas d'informations sensibles à la vue de tous, même si vous pensez pouvoir les contrôler.

<br>Voici ce que vous devriez voir si vous vous rendez sur la page `http://retroweb/retro`

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_retro_page.png"
        style="width:100%">
</div>

<br>En analysant la page que nous venons de découvrir, nous pouvons constater que tous les articles ont été écrits par Wade. Nous allons donc analyser son profil. Il est possible de lire ses derniers posts.

En allant sur son post `Ready Player One` et en analysant le commentaire, nous pouvons voir qu’il contient un mot que Wade semble ne pas vouloir oublier dans le cas où il voudrait l’épeler:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_pass.png</div>
        style="width:100%">
</div>

Nous pouvons légitimement penser qu’il s’agit de son mot de passe, ce qui n’est pas super malin.

Nous allons maintenant exploiter cette information sur le port 3389, l’un des deux ports ouverts que nous avons vus précédemment.
Pour ce faire, nous allons utiliser un programme de prise en main à distance (Remote Desktop Protocol), `xfreerdp`.

Pourquoi ? Parce que le port **3389** EST le port utilisé pour le protocole **RDP**.

Nous savons que Wade semble être le seul utilisateur du site puisqu’il n’y a que lui qui poste des articles, et que son mot de passe est, vraisemblablement: `parzival`.

Nous allons donc nous connecter à la machine à distance avec `xfreerdp`:

```bash
xfreerdp /v:retroweb /u:wade /p:parzival
```

Ce qui aura pour effet d'ouvrir une fenêtre avec la prise en main de la machine distante:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_rdp_win.png</div>
        style="width:100%">
</div>

Il nous est maintenant possible d'ouvrir le fichier `user.txt` pour récupérer le flag:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_user_flag.png</div>
        style="width:100%">
</div>

<br>

> [!IMPORTANT]
> <b>3b99xxxxxxxxxxxxxxxxx</b>

<br>

---

### Question 3

Pour ce troisième exercice, il nous est demandé de trouver un autre fichier texte mais cette fois, celui de l’utilisateur `root`.<br>Pour y avoir accès, il va falloir faire une escalade de privilèges.

<pre>
[+ 100] - root.txt
</pre>

<br>

> [!TIP]
> Figure out what the user last was trying to find. Otherwise, put this one on ice and get yourself a better shell, perhaps one dipped in venom.
>
> Déterminez ce que l'utilisateur essayait de trouver en dernier lieu. Sinon, mettez-le au frais et procurez-vous un meilleur coquillage, peut-être trempé dans du venin.

<br>Avant toute chose, il semble qu’il y ait un fichier dans la corbeille. Ouvrez la et restaurez le fichier. Il apparaît sur le bureau.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_hhupd.png</div>
        style="width:100%">
</div>

Ouvrez le fichier, cette fenêtre apparaît:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_uac.png</div>
        style="width:100%">
</div>

 L’idée est de récupérer les informations du certificat.
 <br>Pour ce faire, cliquez sur le lien `Show more details`, puis, sur `Show information about the publisher’s certificate`:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_certif.png</div>
        style="width:100%">
</div>

Cliquez sur le lien de `Issued by`, le seul lien disponible sur cette page, puis sélectionnez Internet Explorer. Vous pourriez utiliser Chrome mais vous n’obtiendriez pas de session élevée. Il n’est donc pas conseillé d’utiliser Chrome, c’est pourquoi nous choisissons Internet Explorer.

Ce qui va nous ouvrir une page d’erreur puisque nous n’avons pas de connection internet sur cette machine, ne vous inquiétez pas, cela n’a aucune importance :

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_ie_error.png</div>
        style="width:100%">
</div>

L’astuce va être d’accéder au dossier `System32` en simulant une sauvegarde de la page, c’est pourquoi, la page en elle-même n’a aucun interêt.

Faites un `CTRL+S` pour ouvrir la page de sauvegarde puis, rendez-vous dans `C:\Windows\System32`, en l’état, vous ne pouvez voir que les dossiers, pour pouvoir voir les fichiers et les exécutables, donnez comme nom de sauvegarde `*.*` puis cliquez sur `save`, les fichiers devraient s’afficher maintenant.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_sys32.png</div>
        style="width:100%">
</div>

Cherchez `cmd`, faites un clic droit et ouvrez-le en tant qu’administrateur:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_cmd_admin.png</div>
        style="width:100%">
</div>

Maintenant, nous pouvons aller chercher le fichier `root.txt` qui contient le flag.
<br>Tapez la commande suivante pour lire le fichier:

```bash
type ..\..\Users\Administrator\Desktop\root.txt.txt
```

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/tryhackme/blob/main/img/retro_root_flag.png</div>
        style="width:100%">
</div>

Comme vous pouvez le constater, il est possible de lire l’intérieur d’un fichier texte sans avoir à l’ouvrir grâce à la commande `type` qui est l’équivalent Windows de `cat` sous Linux.

`..\..` devant `\Users` indique à la console de revenir deux niveaux avant dans l’arborescence par rapport au chemin actuel, qui est `C:\Windows\System32`.

<br>

> [!IMPORTANT]
> <b>7958bxxxxxxxxxxxxxxxxx</b>

<br>

> [!TIP]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
