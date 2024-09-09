# **[HYDRA](https://tryhackme.com/r/room/hydra)**

Outils utilisés: [HYDRA](https://www.kali.org/tools/hydra/) - [BURP](https://portswigger.net/burp)

## Exercice 1

<pre>
Use Hydra to bruteforce molly's web password. What is flag 1?
Utiliser Hydra pour forcer le mot de passe web de Molly. Qu'est-ce que le drapeau 1 ?
</pre>

<br>

> [!TIP]
> If you've tried more than 30 passwords from RockYou.txt, you are doing something wrong!
> <br>Si vous avez essayé plus de 30 mots de passe à partir de RockYou.txt, c'est que vous faites fausse route !

<br>Tout ce dont nous aurons besoin est dans le mini court juste au-dessus des questions.

Nous utiliserons donc `Hydra` mais aussi `Burp`et son navigateur intégré. 

Rendez-vous dans l’onglet `Proxy`, puis cliquez sur `Open browser`. Il se peut que vous ayez un message d’erreur, suivez simplement l’instruction pour débloquer le navigateur.
<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q1_burp_proxy.png"
        style="width:100%">
</div>

<br>La première chose à faire est d’entrer l’adresse IP de la machine VPN dans le navigateur.
<br>Nous pouvons voir un formulaire de connexion.
<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q1_login.png"
        style="width:50%">
</div>

Considérons que nous avons déjà le nom de l’utilisateur `Molly`, il s'agit d'entrer ce nom d’utilisateur et un mot de passe au hasard afin de récupérer le résultat avec Burp.

Pour ce faire, nous allons activer le mode interception en cliquant sur le bouton `Intercept is off` pour le passer sur `on`
<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q1_intercept.png"
        style="width:100%">
</div>

Une fois la page interceptée, nous avons toutes les informations dont nous avons besoin pour cracker le mot de passe de cette chère Molly.

Voici la commande, un peu longue :
```bash
hydra -I IP_CIBLE -l molly -P /usr/share/wordlists/rockyou.txt httpp-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
```

<br>

> [!NOTE]
> - **`-I`** Attaque l'IP cible 
> - **`-l`** Signifie que nous connaissons déjà le nom d’utilisateur. Dans le cas contraire, nous aurions utilisé `-L`
> - **`-P`** Signifie que nous allons donner le chemin du ficchier de mots de passe en paramètre, ici rockyou.txt.
> - **`Http-post-form`** C’est avec cette option que nous pouvons filtrer notre recherche en donnant en paramètre le chemin du formulaire, ici `/login`, les nom d’utilisateur et mot de passe en donnant pour valeur `^USER^`, qui va être remplacé par `molly` et `^PASS^` qu’Hydra va devoir aller chercher, s’il existe, dans le fichier `rockyou.txt`.
> - `**F=incorrect`** Message d’erreur quand nous entrons des logins erronés (argument obligatoire)

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q1_demo.png"
        style="width:100%">
</div>

Après quelques secondes, vous verrez le mot de passe de Molly s’afficher

<br>

> [!IMPORTANT]
> <b>s°°°h°°°</b>

> [!NOTE]
> S’il ne s’affiche pas, comme précisé dans l’astuce, c’est que la commande n’est pas juste, vérifiez bien toute la procédure.

Il est temps de récupérer le flag. Maintenant que nous avons les logs de Molly, il suffit simplement de les entrer dans le formulaire

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q1_flag.png"
        style="width:100%">
</div>
 
<br>

> [!IMPORTANT]
> <b>THM{°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°}</b>

<br><br>

## Exercice 2

<pre>
Use Hydra to bruteforce molly's SSH password. What is flag 2?
Utiliser Hydra pour forcer le mot de passe SSH de Molly. Qu'est-ce que le drapeau 2 ?
</pre>

Il n’y a pas d’astuces ici car la commande est sensiblement la même que la précédente. Seule la cible change.
<br>Je le répète parce que j’aime bien, modifiez l’IP avec celle de VOTRE machine VPN.

```shell
Hydra -l molly -P /usr/share/wordlists/rockyou.txt ssh://IP_CIBLE
```

Là encore, après quelques secondes, vous devriez voir le mot de passe de Molly apparaître

<br>

> [!IMPORTANT]
> <b>b°°°r°°y</b>

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q2_attack_ssh.png"
        style="width:100%">
</div>

Vous pouvez maintenant se connecter en SSH avec ses identificants
<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q2_ssh_access.png"
        style="width:100%">
</div>

Acceptez la connexion puis renseignez le mot de passe que vous venez de découvrir.
<br><b>IL EST A NOTER</b> que vous ne verrez pas le mot de passe s'afficher ! Je sais que vous le savez mais comme tout le monde n'utilise pas Linux, je préfère le préciser ;) 

Un `LS` puis un `CAT` sur le fichier `flag2.txt` et le drapeau est dévoilé

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/TryHackMe/blob/main/Hydra/img/q2_flag.png"
        style="width:100%">
</div>

<br>

> [!IMPORTANT]
> <b>THM{°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°}</b>

<br>

> [!TIP]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
