# **[ROOTME](https://tryhackme.com/r/room/rrootme)**

## TACHE 1 - Déploiement

Suivez simplement les instructions pour déployer la machine virtuelle.

## TACHE 2 - Reconnaissance

First, let's get information about the target.
<br>Commençons par obtenir des informations sur la cible.

**1. Scan the machine, how many ports are open ?**
<br>**1. Scanner la machine, combien de ports sont ouverts ?**

<br>

> [!TIP]
> Question Hint
>
> Use nmap to do a port scan.
> <br>Utilisez nmap pour effectuer le scanne de port

<br>

```bash
nmap -Pn 10.10.182.207
```

<br>

> [!WARNING]
> Adaptez la commande à l'adresse IP de VOTRE machine virtuelle. Elle change à chaque connexion, il est donc peu probable qu'elle soit identique à la mienne au moment où j'écris ce guide.

<br>La première réponse apparaît.

<br>

**2. What version of Apache is running?**
<br>**2. Quelle version d'Apache est active?**

<br>

```bash
apache2 -v | grep "version"
```

<br>La seconde réponse apparaît.



## TACHE 3 - 