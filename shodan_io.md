# **[SHODAN.IO](https://tryhackme.com/r/room/shodan)**

### TÂCHE 2 - FILTRES

Tout d'abord, voyons ce qu'est [Eternal Blue](https://fr.wikipedia.org/wiki/EternalBlue)
<br>L'article est court mais il contient tout ce dont nous avons besoin.

C'est une vulnérabilité qui exploite une faille `SMBv1` et dont Microsoft a sorti un correctif. Cette faille a été utilisée par l'un des ransomwares les plus prolifiques de l'Histoire : [Wannacry](https://fr.wikipedia.org/wiki/WannaCry)

> [!IMPORTANT]
> <b>v°°°:°°°°°°°°°</b>

**IL EST A NOTER** que vous ne pourrez pas tester cette recherche directement dans `Shodan` car elle est restreinte, uniquement, aux professionnels et aux académies afin d'éviter aux utilisateurs malveillants de faire n'importe quoi !

<br>

### TÂCHE 3 - GOOGLE ET FILTRAGE
#### QUESTION 1
<pre>
What is the top operating system for MYSQL servers in Google's ASN?
Quel est le système d'exploitation le plus utilisé pour les serveurs MYSQL dans l'ASN de Google ? 
</pre>

> [!TIP]
> Search " asn:AS15169 product:"MySQL" " to get 5.6.40-84.0-log.
> <br>Recherchez " asn:AS15169 product:"MySQL" " pour obtenir 5.6.40-84.0-log.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

> [!IMPORTANT]
> <b>5.6.40-84.0-log</b>

<br>

#### QUESTION 2
<pre>
What is the 2nd most popular country for MYSQL servers in Google's ASN?
Quel est le deuxième pays le plus populaire pour les serveurs MYSQL dans l'ASN de Google ?
</pre>

> [!TIP]
> Search " asn:AS15169 product:"MySQL" " to get Netherlands
> <br>Recherchez " asn:AS15169 product:"MySQL" " pour obtenir les Pays-Bas.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

> [!IMPORTANT]
> <b>Netherlands</b>

<br>

#### QUESTION 3
<pre>
Under Google's ASN, which is more popular for nginx, Hypertext Transfer Protocol or Hypertext Transfer Protocol with SSL?
Dans le cadre de l'ASN de Google, quel est le protocole le plus populaire pour nginx, Hypertext Transfer Protocol
ou Hypertext Transfer Protocol avec SSL ?
</pre>

> [!TIP]
> Search " asn:AS15169 product:"nginx" " to get Hypertext Transfer Protocol
> <br>Recherchez " asn:AS15169 product:"nginx" " pour obtenir le protocole de transfert hypertexte.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

> [!IMPORTANT]
> <b>Hypertext Transfer Protocol</b>

<br>

#### QUESTION 4
<pre>
Under Google's ASN, what is the most popular city?
Selon l'ASN de Google, quelle est la ville la plus populaire ?
</pre>

> [!TIP]
> Search " asn:AS15169 country:"US" " to get Kansas City
> <br>Recherchez " asn:AS15169 country:"US" " pour obtenir Kansas City

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

> [!IMPORTANT]
> <b>Kansas City</b>

<br>

#### QUESTION 5
<pre>
Under Google's ASN in Los Angeles, what is the top operating system according to Shodan?
Dans l'ASN de Google à Los Angeles, quel est le meilleur système d'exploitation selon Shodan ?
</pre>

> [!TIP]
> Search " asn:AS15169 country:"US" city:"Los Angeles" " to get Debian
> <br>Recherchez " asn:AS15169 country:"US" city:"Los Angeles" " pour obtenir Debian

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

> [!IMPORTANT]
> <b>Debian</b>

<br>

#### QUESTION 6
<pre>
Using the top Webcam search from the explore page, does Google's ASN have any webcams? Yay/nay.
En utilisant la recherche de webcam en haut de la page d'exploration, est-ce que l'ASN de Google a des webcams? Oui/non.
</pre>

> [!TIP]
> Nay

<br>Bon ben là, on ne peut pas faire plus clair comme indice, je crois..

> [!IMPORTANT]
> <b>Nay</b>

<br>

> [!NOTE]
>Les données ayant évoluées depuis la création de la room, je suppose que `TryHackMe` nous donne les réponses car il nous serait impossible d'avancer.

<br><br>

### TÂCHE 4 - MONITEUR SHODAN
<pre>
What URL takes you to Shodan Monitor?
Quelle URL vous conduit au Moniteur Shodan ?
</pre>

Pour suivre cette tâche, il semble qu'il faut être abonné à `Shodan`, cependant, il n'est pas nécessaire de l'être pour répondre à la question puisque la réponse est dans l'exercice.

> [!IMPORTANT]
> <b>°°°°°://°°°°°°°.°°°°°°.°°/°°°°°°°°°</b>

<br><br>

### TÂCHE 5 - SHODAN DORKING
<pre>
What dork lets us find PCs infected by Ransomware?
Quel "dork" nous permet de trouver des PC infectés par des ransomwares ?
</pre>

Ici encore, il n'est pas nécessaire d'aller chercher très loin, soyez juste attentif à l'exercice, faites les tests proposés, vous finirez par répondre naturellement à cette question

> [!IMPORTANT]
> <b>°°°°°°°°°°°°°°:°°°° °°°°°°°°° °°°°°°°°°</b>

<br>

> [!TIP]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
