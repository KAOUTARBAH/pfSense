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
   - **Méthode** : `Create an internal Authorithy`
   - **Taille de la clé** : `2048 bits`  
   - **Durée** : `3650 jours (10 ans)`  
   - **Nom commun** : `OpenVPN-CA` 
   - **Country Code** :  `fr` 
   - **Organisation** : Mets le nom de ton entreprise ou laisse vide  
   - **Pays/Région/Localité** : Remplis selon ta localisation  
- Enregistrez.

![CA](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/CA.png)

![CA-ranka](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/ca-ranka.png)

- L'autorité de certification doit apparaître dans l'interface, comme ceci :
![CA-ranka](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/ca-ranka2.png)
Cette CA servira à signer les certificats du serveur OpenVPN et des clients.

### 3. Générer un Certificat Serveur
- Allez dans → Système > Certificats.
- Onglet "Certificats" 
- Cliquez sur "Ajouter".
- Remplissez les champs :
    - Méthode : Créer un certificat interne
    - Nom descriptif : OpenVPN-Serveur-Certificat
    - Type de certificat : Certificat de serveur
    - Autorité de certification : Sélectionne OpenVPN_CA (l'Autorité de Certification que tu as créée auparavant).
    - Clé : Laisse la valeur par défaut (2048 bits).
    - Durée : 3650 jours (10 ans) ou une durée plus courte selon ta préférence.
    - Nom commun : WINSERV (exemple :openvpn.mondomaine.com) (ou le nom d'hôte de ton serveur si nécessaire).
- Clique ensuite sur Enregistrer pour créer le certificat.

![ca-server](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/ca-server.png)
![ca-server2](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/ca-server2.png)
Cette CA servira à signer les certificats du serveur OpenVPN et des clients.


### 4. Configurer le Serveur OpenVPN sur pfSense

- Aller dans **VPN > OpenVPN > server .  pour utiliser l'assistant de configuration.  
- Choisir * Local User Access **.  
- Sélectionner la **CA**   `OpenVPN_CA`  créée précédemment.  
- Sélectionner le **certificat du serveur**  `OpenVPN-Serveur-Certificat` créé.  

#### Paramètres réseau :  

- **Interface** : `WAN`  
- **Protocole** : `UDP`  
- **Port local** : `1194`  
- **Réseau Tunnel** : `10.8.0.0/24` (ou un autre réseau privé non utilisé).  
- **Réseau Local** : `192.168.1.0/24` (remplacer par votre réseau LAN).  
- ✅ **Activer Redirect Gateway** pour forcer tout le trafic via le VPN.  
- ✅ **Activer Client-to-Client** pour permettre la communication entre les clients VPN.  

- **Sauvegarder et appliquer**.  
![openvpnserver](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/openvpnserver.png)
![serverOpenVPN](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/serverOpenVPN.png)




### 5. Créer les utilisateurs locaux
# Création d’un utilisateur dans User Manager sur pfSense

## 1️⃣ Accéder à User Manager  
1. Connectez-vous à pfSense.  
2. Allez dans **System > User Manager**.  
3. Cliquez sur **+ Add** pour ajouter un nouvel utilisateur.  

## 2️⃣ Remplir les informations de l’utilisateur  
- **Username** : Saisissez un nom d’utilisateur (ex : `vpn_user01`).  
- **Password** : Créez un mot de passe sécurisé.  
- **Full Name** (optionnel) : Indiquez le nom complet de l’utilisateur.  
- **Group Membership** :  
  - Cliquez sur **Add** et sélectionnez `admins` si l’utilisateur doit avoir des droits d’administration.  
  - Sinon, laissez vide ou attribuez-lui un groupe spécifique.  
- **Expiration date** (optionnel) : Définissez une date d’expiration du compte si nécessaire.  

## 3️⃣ Associer un certificat à l’utilisateur (si OpenVPN est utilisé)  
1. Descendez jusqu'à la section **User Certificates**.  
2. Cliquez sur **Add**.  
3. Choisissez **"Create a user certificate"**.  
4. Sélectionnez la **Certificate Authority (CA)** déjà créée.  
5. Remplissez les champs comme suit :  
   - **Descriptive name** : `vpn_user01_cert`.  
   - **Key Length** : `2048 bits`.  
   - **Digest Algorithm** : `SHA256`.  
   - **Lifetime** : `3650 jours (10 ans)` (ou selon votre politique de sécurité).  

![ca-client](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/ca-client.png)
![clt-ca](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/clt-ca.png)


### 6. Configurer les règles de pare-feu
- Aller dans Firewall > Rules > WAN et ajouter une règle :
    - Protocol: UDP
    - Port: 1194
    - Source: Any
    - Destination: WAN address
    - Sauvegarder et appliquer.

![r-vers-wan](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/r-vers-wan.png)

- Aller dans Firewall > Rules > OpenVPN et ajouter une règle :
    - Action: Pass
    - Protocol: Any
    - Source: OpenVPN network
    - Destination: LAN net
    - Sauvegarder et appliquer.

![vers-clt](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/vers-clt.png)


### 7.Tester l'accès distant depuis un poste client
- Sur mon PC Windows 10, je commence par installer le client OpenVPN... Ce qui se fait très facilement, sans difficulté particulière !

- Télécharger OpenVPN

- Dans le dossier "C:\Programmes\OpenVPN\Config" vous devez extraire le contenu de l'archive ZIP téléchargée depuis le Pfsense et qui contient la configuration. Vous pouvez créer un sous-dossier dans le dossier "config" si vous voulez.

- Ensuite, sur l'icône OpenVPN effectuez un clic droit et cliquez sur "Connecter".

![conn](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/conn.png)
![conn-vpn](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/conn-vpn.png)
![test-vpn](https://github.com/KAOUTARBAH/pfSense/blob/main/imagesVPN/test-vpn.png)
