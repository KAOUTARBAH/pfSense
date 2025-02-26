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
- Allez dans → Système > Certificats.
- Onglet "Autorités" 
- Cliquez sur "Ajouter".
- Remplissez les champs :
   - **Nom de l’autorité** : `OpenVPN_CA`  
   - **Méthode** : Créer une Autorité de Certification interne  
   - **Taille de la clé** : `2048 bits`  
   - **Durée** : `3650 jours (10 ans)`  
   - **Nom commun** : `OpenVPN-CA`  
   - **Organisation** : Mets le nom de ton entreprise ou laisse vide  
   - **Pays/Région/Localité** : Remplis selon ta localisation  
- Enregistrez.

![CA](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/CA.png)
Cette CA servira à signer les certificats du serveur OpenVPN et des clients.

### 3. Générer un Certificat Serveur
- Allez dans → Système > Certificats.
- Onglet "Certificats" 
- Cliquez sur "Ajouter".
- Remplissez les champs :
    - Méthode : Créer un certificat interne
    - Nom descriptif : OpenVPN-Serveur-Cert
    - Type de certificat : Certificat de serveur
    - Autorité de certification : Sélectionne OpenVPN_CA (l'Autorité de Certification que tu as créée auparavant).
    - Clé : Laisse la valeur par défaut (2048 bits).
    - Durée : 3650 jours (10 ans) ou une durée plus courte selon ta préférence.
    - Nom commun : openvpn.mondomaine.com (ou le nom d'hôte de ton serveur si nécessaire).
- Clique ensuite sur Enregistrer pour créer le certificat.

![ca-server](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/ca-server.png)

Cette CA servira à signer les certificats du serveur OpenVPN et des clients.


### 4. Configurer le Serveur OpenVPN sur pfSense

- Aller dans **VPN > OpenVPN > Wizards .  pour utiliser l'assistant de configuration.  
- Choisir * Local User Access **.  
- Sélectionner la **CA**   `OpenVPN_CA`  créée précédemment.  
- Sélectionner le **certificat du serveur**  `OpenVPN-Serveur-Cert` créé.  

#### Paramètres réseau :  

- **Interface** : `WAN`  
- **Protocole** : `UDP`  
- **Port local** : `1194`  
- **Réseau Tunnel** : `10.8.0.0/24` (ou un autre réseau privé non utilisé).  
- **Réseau Local** : `192.168.1.0/24` (remplacer par votre réseau LAN).  
- ✅ **Activer Redirect Gateway** pour forcer tout le trafic via le VPN.  
- ✅ **Activer Client-to-Client** pour permettre la communication entre les clients VPN.  

- **Sauvegarder et appliquer**.  

![ca-server](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/serverOpenVPN
.png)


