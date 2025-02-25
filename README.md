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

![installation pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/installation-pfsense.png)

## 4. Configurer les interfaces WAN et LAN

1. Après le redémarrage, dans la console pfSense :
   - Attribuez les interfaces :
     - **WAN** : em0 .
     - **LAN** : em1.
   - Une adresse IP sera automatiquement attribuée au LAN : **192.168.1.1/24**.

## 5. Accéder à l'interface Web

1. Créez une **VM client** (Windows/Linux) avec **Réseau interne** et connectez-la au **LAN**.
2. Attribuez à la VM client une adresse IP en **DHCP** ou manuellement (par exemple, **192.168.1.2/24**).
3. Ouvrez un navigateur et entrez l'URL suivante : https://192.168.1.1
![Connexion pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/connPf.png)

4. Identifiants par défaut :
- **Utilisateur** : admin
- **Mot de passe** : pfsense
![Conf pfsense](https://github.com/KAOUTARBAH/pfSense/blob/main/images/confPfsense.png)

5. Configurez pfSense via l’assistant.
















