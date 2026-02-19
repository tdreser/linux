# Cours de Linux

## mon Adresse IP Réseau en Binaire

voici mon adresse IP normale :

### et enfin voici sa version binaire : 

- 00001010.00100001.01000110.11011110

10 → 00001010 en binaire (128 64 32 16 8 4 2 1 → 0 0 0 0 1 0 1 0)

33 → 00100001 en binaire

70 → 01000110 en binaire

222 → 11011110 en binaire

On garde toujours 8 bits par octet, en complétant avec des zéros à gauche si besoin.


# B1 Linux - TP2

## I. Exploration locale en solo
### 1. Affichage d'informations sur la pile TCP/IP locale
####  En ligne de commande : 

![](./images/image%20copy%205.png)

### Infos cartes réseau (CLI)

**Commande utilisée :** `ip addr show`

**Interface WiFi :**
- Nom : `wlp3s0`
- Adresse MAC : `00:11:22:33:44:55`
- Adresse IP : `172.16.0.100/20`

**Interface Ethernet :**
- Nom : `enp2s0`
- Adresse MAC : `aa:bb:cc:dd:ee:ff`
- Adresse IP : `Aucune (interface DOWN)`

**Calculs réseau WiFi :**

- commande : ipcalc -n 172.16.0.100/20

Address:   172.16.0.100         10101100.00010000.00000000.01100100
           +---------------------+
Netmask:   255.255.240.0        = 20                           [20 bits]
Wildcard:  0.0.15.255           [12 bits]
Nth-host:  16th-host            = 4096 hosts total
=> Network: 172.16.0.0/20       10101100.00010000.00000000.00000000
    Broadcast: 172.16.15.255    10101100.00010000.00001111.11111111
    => Usable: 172.16.0.1 - 172.16.15.254 (4094 adresses utilisables)

#### En graphique (GUI : Graphical User Interface)


![](./images/image%20copy%206.png)

à quoi sert la gateway dans le réseau d'Ingésup ?

- Ma réponse :

La gateway (passerelle) sert de point de sortie pour toutes les requêtes qui ne sont pas destinées au réseau local (LAN). À Ingésup, elle permet à ton ordinateur de communiquer avec des réseaux extérieurs, comme Internet, en acheminant tes paquets de données vers le routeur de l'école.

### 2. Modifications des informations
### A. Modification d'adresse IP - pt. 1

Pour un réseau en 10.33.70.222 avec un masque 255.255.240.0 :


    Adresse de réseau : 10.33.64.0

    Adresse de broadcast : 10.33.79.255 

    Première IP utilisable : 10.33.64.1

    Dernière IP utilisable : 10.33.79.254 


- j'ai ensuite modifié manuellement l'adresse ip de ma carte WIFI : 

![](./images/image%20copy%207.png)

### B. nmap

pour cette partie la du tp, la connection de l'école ne fonctionnait plus donc j'ai du me faire un part de co avec mon téléphone , voici donc les informations :

IP Address : 172.20.10.2

Netmask : 255.255.255.240 (soit un /28)

Default Route (Gateway) : 172.20.10.1

Broadcast : 172.20.10.15

- commande : nmap -sn 172.20.10.0/28

![](./images/image%20copy%208.png)

### C. Modification d'adresse IP - pt. 2

## II. Exploration locale en duo
### 1. Cablage effectué

### 2. Création du réseau

### 3. Modification d'adresse IP
bien effectuée:

![ll](./images/image%20copy.png)


### 4. Utilisation d'un des deux comme gateway

j'ai decidé de prendre Antonin comme duo pour ce TP, ici ce sera lui qui jouera le role de gateway pendant que moi je joue le role du pc qui n'a plus internet.

je peux bien ping antonin :

![ll](./images/image.png)

et voilà bien un ping 8.8.8.8 qui fonctionne sans carte WiFi :

![ll](./images/image%20copy%202.png)

### 5. Petit chat privé
je suis sur linux donc j'ai deja netcat, et la communication depuis le port 8888 est bien effectuée : 

![ll](./images/mini_chat.png)

### 6. Wireshark


voici ce qu'il se passe dans mon wireshark quand Antonin me ping : 


![ll](./images/image%20copy%203.png)

voici ce qu'il se passe dans mon wireshark quand Antonin netcat : 

![ll](./images/image%20copy%204.png)


### 7. Firewall


## III. Manipulations d'autres outils/protocoles côté client

## pour cette partie là du TP je suis sur la connection de chez moi

### 1. DHCP
- afficher l'adresse IP du serveur DHCP du réseau WiFi et sa date d'expiration :

![](./images/image%20copy%209.png)

L'adresse IP de mon serveur DHCP est 192.168.1.1

![](./images/image%20copy%2010.png)

et la date d'expiration du bail DHCP est donc le Vendredi 20 février 2026 à 08:50:53.

####  Demandez une nouvelle adresse IP (en ligne de commande)

``` nmcli device reapply wlo1 ```  --> est la commande pour demander une nouvelle adresse IP (en ligne de commande)



### 2. DNS

- voici les lookups et les reverses lookups demandés :

![](./images/image%20copy%2011.png)

- interpréter les résultats :

L’outil dig a servi à interroger l'annuaire DNS. Les Lookups sur google.com et ynov.com ont traduit ces noms en adresses IP, prouvant que le DNS permet d'éviter de mémoriser des suites de chiffres. On note que Google renvoie plusieurs IPs pour la répartition de charge (Load Balancing). À l'inverse, les Reverse Lookups sur 78.78.21.21 et 92.16.54.88 n'ont rien renvoyé : cela signifie qu'aucun enregistrement PTR (nom associé à l'IP) n'a été configuré pour ces adresses, ce qui est courant pour des IPs de particuliers ou des serveurs sans identité publique.


# B1 Linux - TP3