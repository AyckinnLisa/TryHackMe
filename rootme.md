# **[ROOTME](https://tryhackme.com/r/room/rrootme)**

## TACHE 1 - Déploiement

Suivez simplement les instructions pour déployer la machine virtuelle.

## TACHE 2 - Reconnaissance

First, let's get information about the target.
<br>Commençons par obtenir des informations sur la cible.

**1. Scan the machine, how many ports are open ?**
<br>**1. Scanner la machine, combien de ports sont ouverts ?**

<pre>
+------------------------------------------------------------------------+   +--------------+   +--------------+
| Answer format: *                                                       |   |    SUBMIT    |   |     HINT     |
+------------------------------------------------------------------------+   +--------------+   +--------------+
</pre>

<br>

> [!TIP]
> Question Hint
>
> Use nmap to do a port scan.
> <br>Utilisez nmap pour effectuer le scanne de port

<br>Ce que nous allons faire:

```bash
nmap -Pn 10.10.182.207
```

<br>

> [!WARNING]
> Adaptez la commande à l'adresse IP de VOTRE machine virtuelle. Elle change à chaque connexion, il est donc peu probable qu'elle soit identique à la mienne au moment où j'écris ce guide.

<br>

<pre>
Starting Nmap 7.60 ( https://nmap.org ) at 2024-08-29 13:32 BST
Nmap scan report for ip-10-10-182-207.eu-west-1.compute.internal (10.10.182.207)
Host is up (0.00090s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
<b>22/tcp open  ssh
80/tcp open  http</b>
MAC Address: 02:5D:3B:68:F9:11 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.64 seconds
</pre>

<br>

<pre>
+------------------------------------------------------------------------+   +----------------+   +--------------+
| <b>x</b>                                                                      |   | CORRECT ANSWER |   |     HINT     |
+------------------------------------------------------------------------+   +----------------+   +--------------+
</pre>
