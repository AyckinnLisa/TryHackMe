# **[SHODAN.IO](https://tryhackme.com/r/room/shodan)**

### TÂCHE 2 - FILTRES

Tout d'abord, voyons ce qu'est [Eternal Blue](https://fr.wikipedia.org/wiki/EternalBlue).
<br>L'article est court mais il contient tout ce dont nous avons besoin.
<br>C'est une vulnérabilité qui exploite une faille `SMBv1` et dont Microsoft a sorti un correctif. Cette faille a été utilisée par l'un des ransomwares les plus prolifiques : [Wannacry](https://fr.wikipedia.org/wiki/WannaCry)

-----> **`vxxx:xxxxx-xxx`**

**IL EST A NOTER** que vous ne pourrez pas tester cette recherche directement dans `Shodan`, car elle est restreinte, uniquement, aux professionnels ou aux académies afin d'éviter aux utilisateurs malveillants de faire n'importe quoi !

<br>

### TÂCHE 3 - GOOGLE ET FILTRAGE
#### QUESTION 1

What is the top operating system for MYSQL servers in Google's ASN?
<br>Quel est le système d'exploitation le plus utilisé pour les serveurs MYSQL dans l'ASN de Google ? 

> [!TIP]
> Search " asn:AS15169 product:"MySQL" " to get 5.6.40-84.0-log.
> <br>Recherchez " asn:AS15169 product:"MySQL" " pour obtenir 5.6.40-84.0-log.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

-----> **`5.6.40-84.0-log`**

<br>

#### QUESTION 2

What is the 2nd most popular country for MYSQL servers in Google's ASN?
<br>Quel est le deuxième pays le plus populaire pour les serveurs MYSQL dans l'ASN de Google ?

> [!TIP]
> Search " asn:AS15169 product:"MySQL" " to get Netherlands
> <br>Recherchez " asn:AS15169 product:"MySQL" " pour obtenir les Pays-Bas.

<br>La réponse étant donnée par `TryHackMe` eux-même dans l'indice, je la mets en clair:

-----> **`Netherlands`**

<br>

> [!NOTE]
>Les données ayant évoluées depuis la création de la room, je suppose que `TryHackMe` nous donne les réponses car il nous serait impossible d'avancer.

<br>










<br>

> [!IMPORTANT]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
