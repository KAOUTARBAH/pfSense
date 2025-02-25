# pfSense

# Installation de pfSense avec netgate-installer-v1.0-RC-amd64-20240919-1435.iso dans VirtualBox

## 1. Préparer VirtualBox

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

## 2. Ajouter l'ISO et configurer les interfaces réseau

1. Sélectionnez la VM **pfSense** → **Paramètres**.
2. Allez dans **Stockage** → Sélectionnez le **contrôleur IDE** → Cliquez sur **Ajouter un disque optique**.
3. Choisissez un disque et sélectionnez votre **ISO** téléchargé.

### Configurer les interfaces réseau :
- **Adapter 1 (WAN)** : Mode **Accès par pont (Bridge)** → Cela connecte pfSense au réseau réel.
- **Adapter 2 (LAN)** : Mode **Réseau interne** → Pour simuler un réseau interne.

## 3. Démarrer et installer pfSense

1. Démarrez la VM et bootez sur l'ISO.
2. Sélectionnez **"Install pfSense"**.
3. **Langue et clavier** : Choisissez **Français** si nécessaire.
4. **Type de partitionnement** : Sélectionnez **Auto (ZFS)** pour une installation simple.
5. Confirmez l'installation et attendez la fin du processus.
6. Une fois l'installation terminée, retirez l'ISO et redémarrez la VM.

![installation pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/installation-pfsense)

## 4. Configurer les interfaces WAN et LAN

1. Après le redémarrage, dans la console pfSense :
   - Attribuez les interfaces :
     - **WAN** : vtnet0 ou em0 (selon VirtualBox).
     - **LAN** : vtnet1 ou em1.
   - Une adresse IP sera automatiquement attribuée au LAN : **192.168.1.1/24**.

## 5. Accéder à l'interface Web

1. Créez une **VM client** (Windows/Linux) avec **Réseau interne** et connectez-la au **LAN**.
2. Attribuez à la VM client une adresse IP en **DHCP** ou manuellement (par exemple, **192.168.1.2/24**).
3. Ouvrez un navigateur et entrez l'URL suivante : https://192.168.1.1
4. Identifiants par défaut :
- **Utilisateur** : admin
- **Mot de passe** : pfsense
5. Configurez pfSense via l’assistant.



















Je vois que vous êtes sur l'interface de commande de pfSense 2.7.2-RELEASE. Vous pouvez exécuter des commandes depuis le Shell (option 8) pour diagnostiquer votre problème de connexion.

1. Installer pfSense
- Télécharger et installer pfSense : Allez sur le site officiel de pfSense (https://www.pfsense.org/download/) et téléchargez la dernière version de pfSense. Vous pouvez installer pfSense sur une machine physique ou une machine virtuelle.

- Configuration initiale : Une fois pfSense installé, accédez à l'interface Web de pfSense en utilisant l'adresse par défaut (172.16.15.254/24 ). Vous devrez vous connecter avec le nom d'utilisateur admin et le mot de passe pfsense par défaut.

- Vous serez ensuite guidé pour la configuration initiale. Assurez-vous que pfSense est correctement connecté à vos interfaces réseau (LAN et WAN).

- **VM pfSense :**
    
    - Pare-feu du lab
    - 3 interfaces :
        - WAN : 192.168.1.250/24 => Interface pont VirtualBox
        - LAN : 172.16.15.254/24 => Réseau interne VirtualBox LAN
        - DMZ : 172.16.200.254/24 => Réseau interne VirtualBox DMZ

2. S'assurer d'une configuration réseau valide permettant aux machines internes d’accéder à l'extérieur
- **VM Client1 :**
    
    - PC client dans le LAN pour :
        - Accès à la console pfsense du LAN
        - Test avec les autres VM
    - Configuration IP :
        - Adresse : 172.16.15.20/24 => Réseau interne VirtualBox LAN
        - Passerelle : 172.16.15.254
    - Compte wilder dans le groupes des administrateurs locaux

- **VM webserver1 :**
    
    - Serveur web dans le LAN
    - Configuration IP :
        - Adresse : 172.16.15.10/24 => Réseau interne VirtualBox LAN
        - Passerelle : 172.16.15.254
- **VM webserver2 :**
    
    - Serveur web dans la DMZ
    - Configuration IP :
        - Adresse : 172.16.200.10/24 => Réseau interne VirtualBox DMZ
        - Passerelle : 172.16.200.254
- **VM webserver3 :**
    
    - Serveur web dans le WAN (dans le réseau 192.168.1.0/24)
    - Configuration IP :
        - Adresse : 192.168.1.120/24 => Réseau Pont
        - Passerelle : 192.168.1.250

3. Tester que la machine client peut accéder à l'extérieur


4. Mettre en place une règle de filtrage réseau pour interdire à la machine client de sortir du réseau interne
5. Vérifier que la machine client ne peut plus communiquer avec l'extérieur, mais que le poste d'administration peut encore communiquer avec l'extérieur
6. Expliquer la règle de filtrage mise en place dans le bloc de texte solution de la quête