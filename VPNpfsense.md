# Installation d'un serveur VPN sur pfSense

- Configurer un serveur VPN avec pfSense - - - permettant l'accès distant à des utilisateurs aux ressources du réseau interne.

- La machine cliente de test peut être une machine Windows ou GNU/Linux.

## 1. Pré-requis
- Un pare-feu pfSense installé et configuré avec un accès à Internet.
- Un réseau interne LAN avec des serveurs et machines accessibles uniquement via le VPN.
- Une machine cliente sous Windows ou Linux pour tester la connexion.
- Un nom de domaine ou une IP publique pour accéder au VPN.

## 2. Installation et Configuration du Serveur OpenVPN sur pfSense
### 1. Installer le package OpenVPN (si nécessaire)
- Se connecter à pfSense via l’interface web.
- Aller dans System > Package Manager > Available Packages.
- Rechercher OpenVPN Client Export et l’installer.

![Installation VPN](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/openVPN.png)


### 2. Créer une Autorité de Certification (CA) manuellement
1️⃣. Allez dans → Système > Certificats.
2️⃣. Onglet "Autorités de Certification" 
3️⃣. Cliquez sur "Ajouter".
4️⃣.Remplissez les champs :
   - **Nom de l’autorité** : `OpenVPN_CA`  
   - **Méthode** : Créer une Autorité de Certification interne  
   - **Taille de la clé** : `2048 bits`  
   - **Durée** : `3650 jours (10 ans)`  
   - **Nom commun** : `OpenVPN-CA`  
   - **Organisation** : Mets le nom de ton entreprise ou laisse vide  
   - **Pays/Région/Localité** : Remplis selon ta localisation  
5️⃣. Enregistrez.

![CA](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/CA.png)
Cette CA servira à signer les certificats du serveur OpenVPN et des clients.
