# **[SHODAN.IO](https://tryhackme.com/r/room/shodan)**

### TÂCHE 2 - FILTRES

Tout d'abord, voyons ce qu'est [Eternal Blue](https://fr.wikipedia.org/wiki/EternalBlue).
<br>L'article est court mais il contient tout ce dont nous avons besoin.

C'est une vulnérabilité qui exploite une faille `SMBv1` et dont Microsoft a sorti un correctif. Cette faille a été utilisée par l'un des ransomwares les plus prolifiques de l'Histoire : [Wannacry](https://fr.wikipedia.org/wiki/WannaCry)

<pre>
<b>v***:*****-***</b>
</pre>

**IL EST A NOTER** que vous ne pourrez pas tester cette recherche directement dans `Shodan` car elle est restreinte, uniquement, aux professionnels et aux académies afin d'éviter aux utilisateurs malveillants de faire n'importe quoi !

<br>

### TÂCHE 3 - GOOGLE ET FILTRAGE
#### QUESTION 1

What is the top operating system for MYSQL servers in Google's ASN?
<br>Quel est le système d'exploitation le plus utilisé pour les serveurs MYSQL dans l'ASN de Google ? 

> [!TIP]
> Search " asn:AS15169 product:"MySQL" " to get 5.6.40-84.0-log.
> <br>Recherchez " asn:AS15169 product:"MySQL" " pour obtenir 5.6.40-84.0-log.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

<pre>
-----> <b>5.6.40-84.0-log</b>
</pre>

#### QUESTION 2

What is the 2nd most popular country for MYSQL servers in Google's ASN?
<br>Quel est le deuxième pays le plus populaire pour les serveurs MYSQL dans l'ASN de Google ?

> [!TIP]
> Search " asn:AS15169 product:"MySQL" " to get Netherlands
> <br>Recherchez " asn:AS15169 product:"MySQL" " pour obtenir les Pays-Bas.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

<pre>
-----> <b>Netherlands</b>
</pre>

#### QUESTION 3

Under Google's ASN, which is more popular for nginx, Hypertext Transfer Protocol or Hypertext Transfer Protocol with SSL?
<br>Dans le cadre de l'ASN de Google, quel est le protocole le plus populaire pour nginx, Hypertext Transfer Protocol ou Hypertext Transfer Protocol avec SSL ?

> [!TIP]
> Search " asn:AS15169 product:"nginx" " to get Hypertext Transfer Protocol
> <br>Recherchez " asn:AS15169 product:"nginx" " pour obtenir le protocole de transfert hypertexte.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

<pre>
-----> <b>Hypertext Transfer Protocol</b>
</pre>

#### QUESTION 4

Under Google's ASN, what is the most popular city?
<br>Selon l'ASN de Google, quelle est la ville la plus populaire ?

> [!TIP]
> Search " asn:AS15169 country:"US" " to get Kansas City
> <br>Recherchez " asn:AS15169 country:"US" " pour obtenir Kansas City

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

<pre>
-----> <b>Kansas City</b>
</pre>

#### QUESTION 5

Under Google's ASN in Los Angeles, what is the top operating system according to Shodan?
<br>Dans l'ASN de Google à Los Angeles, quel est le meilleur système d'exploitation selon Shodan ?

> [!TIP]
> Search " asn:AS15169 country:"US" city:"Los Angeles" " to get Debian
> <br>Recherchez " asn:AS15169 country:"US" city:"Los Angeles" " pour obtenir Debian

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

<pre>
-----> <b>Debian</b>
</pre>

#### QUESTION 6

Using the top Webcam search from the explore page, does Google's ASN have any webcams? Yay/nay.
<br>En utilisant la recherche de webcam en haut de la page d'exploration, est-ce que l'ASN de Google a des webcams ? Oui/non.

> [!TIP]
> Nay

<br>Bon ben là, on ne peut pas faire plus clair comme indice, je crois..

<pre>
-----> <b>Nay</b>
</pre>

<br>

> [!NOTE]
>Les données ayant évoluées depuis la création de la room, je suppose que `TryHackMe` nous donne les réponses car il nous serait impossible d'avancer.

<br>

### TÂCHE 4 - MONITEUR SHODAN

What URL takes you to Shodan Monitor?
<br>Quelle URL vous conduit au Moniteur Shodan ?

Pour suivre cette tâche, il semble qu'il faut être abonné à `Shodan`, cependant, il n'est pas nécessaire de l'être pour répondre à la question puisque la réponse est dans l'exercice.

<pre>
-----> <b>*****://*******.******.**/*********</b>
</pre>

<br>

### TÂCHE 5 - SHODAN DORKING

What dork lets us find PCs infected by Ransomware?
<br>Quel "dork" nous permet de trouver des PC infectés par des ransomwares ?

Ici encore, il n'est pas nécessaire d'aller chercher très loin, soyez juste attentif à l'exercice, faites les tests proposés, vous finirez par répondre naturellement à cette question

<pre>
-----> <b>**************:**** ********* *********</b>
</pre>

<br>

> [!IMPORTANT]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
