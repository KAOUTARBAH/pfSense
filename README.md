# pfSense

# 1. Installer pfSense

## 1. Préparer le iso 
- Télécharger pfSense depuis le site officiel pfSense.
- Installation de pfSense avec netgate-installer-v1.0-RC-amd64-20240919-1435.iso dans VirtualBox

## 2. Préparer VirtualBox

1. Ouvrez VirtualBox et cliquez sur **Nouvelle machine**.
2. Dans les paramètres de la nouvelle machine :
   - **Nom** : pfSense (ou un autre nom de votre choix).
   - **Type** : BSD.
   - **Version** : FreeBSD (64-bit).
3. Attribuez de la mémoire RAM :
   - Minimum **1024 Mo** (2 Go recommandé).
4. Créez un disque dur virtuel :
   - **Sélectionnez** "Créer un disque virtuel maintenant".
   - **Format** : VDI (ou VMDK si vous utilisez VMware plus tard).
   - **Stockage** : Dynamique.
   - **Taille** : Minimum **10 Go** (ou plus selon votre besoin).

## 3. Ajouter l'ISO et configurer les interfaces réseau

1. Sélectionnez la VM **pfSense** → **Paramètres**.
2. Allez dans **Stockage** → Sélectionnez le **contrôleur IDE** → Cliquez sur **Ajouter un disque optique**.
3. Choisissez un disque et sélectionnez votre **ISO** téléchargé.

### Configurer les interfaces réseau :
- **Adapter 1 (WAN)** : Mode **Accès par pont (Bridge)** → Cela connecte pfSense au réseau réel.
- **Adapter 2 (LAN)** : Mode **Réseau interne** → Pour simuler un réseau interne.

## 4. Démarrer et installer pfSense

1. Démarrez la VM et bootez sur l'ISO.
2. Sélectionnez **"Install pfSense"**.
3. **Langue et clavier** : Choisissez **Français** si nécessaire.
4. **Type de partitionnement** : Sélectionnez **Auto (ZFS)** pour une installation simple.
5. Confirmez l'installation et attendez la fin du processus.
6. Une fois l'installation terminée, retirez l'ISO et redémarrez la VM.

![installation pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/installation-pfsense.png)

## 5. Configurer les interfaces WAN et LAN

1. Après le redémarrage, dans la console pfSense :
   - Attribuez les interfaces :
     - **WAN** : em0 .
     - **LAN** : em1.
   - Une adresse IP sera automatiquement attribuée au LAN : **192.168.1.1/24**.

## 6. Accéder à l'interface Web

1. Créez une **VM client** (Windows/Linux) avec **Réseau interne** et connectez-la au **LAN**.
2. Attribuez à la VM client une adresse IP en **DHCP** ou manuellement (par exemple, **192.168.1.2/24**).
3. Ouvrez un navigateur et entrez l'URL suivante : https://192.168.1.1
![Connexion pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/connPf.png)

4. Identifiants par défaut :
- **Utilisateur** : admin
- **Mot de passe** : pfsense
![Conf pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/confPfsense.png)

5. Configurez pfSense via l’assistant.

# 2. S'assurer d'une configuration réseau valide permettant aux machines internes d’accéder à l'extérieur
## Configurer le réseau pour permettre l'accès à Internet

### 1. Accéder à l'interface Web :
1. Connectez un PC à l'interface **LAN**.
2. Ouvrez un navigateur et rendez-vous sur l'URL suivante :  
https://192.168.1.1

3. Connectez-vous avec les identifiants par défaut :
- **Utilisateur** : admin
- **Mot de passe** : pfsense

### 2. Configurer l’interface WAN :
1. Configurez l'interface **WAN** :
- En **DHCP** (si votre fournisseur d'accès utilise le DHCP) ou
- Avec une **IP fixe** (si vous avez une adresse IP statique fournie par votre FAI).

- Configurer l'interface WAN en DHCP
1. Sur la page de configuration de l'interface **WAN**, vous devez configurer l'interface pour obtenir une adresse IP automatiquement via **DHCP**.
2. Dans la section **"General Configuration"** :
   - **Interface** : Sélectionnez **WAN** (c’est déjà sélectionné par défaut).
   - **IPv4 Configuration Type** : Choisissez **DHCP**.
   
Cela permet à pfSense de récupérer automatiquement une adresse IP de votre fournisseur d'accès via le **DHCP** (comme le ferait un routeur classique).

### 3. Configurer l’interface LAN :
1. Assignez un réseau interne pour l'interface **LAN**, par exemple **192.168.1.1/24**.

### 4. Configurer NAT et Pare-feu :
1. **NAT automatique** doit être activé.
2. Créez une règle dans le **pare-feu** pour autoriser l'accès à Internet :
- **LAN > Any > Any** : Permet l'accès total des machines internes vers Internet.

# 3. Tester que la machine client peut accéder à l'extérieur
- Depuis une machine interne, testez la connexion en pinguant une **IP publique** (par exemple, **8.8.8.8**) : 
```sh
ping 8.8.8.8
```

![ping](https://github.com/KAOUTARBAH/pfSense/blob/main/images/ping8.png)

- Testez la **résolution DNS** avec la commande **nslookup** pour vérifier que les noms de domaine sont résolus correctement : 
```sh
nslookup google.com
```
![nslookup](https://github.com/KAOUTARBAH/pfSense/blob/main/images/nslookup.png)


















