# ` 📞 `︲Doc-Technique-VoIP-XiVO

---

Ce dépôt présente un **guide complet pour la mise en place d’un service de téléphonie IP (VoIP)** basé sur **XiVO Pollux**, déployé sur un serveur **Debian 12**.  
Il couvre l’installation, la configuration et l’utilisation du service de bout en bout.  
Tu y apprendras à **installer un serveur VoIP**, à **configurer des utilisateurs et des lignes SIP**, à **intégrer des téléphones IP et des softphones**, ainsi qu’à **mettre en place un plan d’appels fonctionnel** avec plusieurs contextes internes.

---

## `📑`︲Sommaire

---

1. [` 🟦 `︲Introduction.](#introduction)
2. [` 🎯 `︲Contexte & objectifs du TP.](#contexte--objectifs-du-tp)
3. [` 🧰 `︲Prérequis & environnement technique.](#prérequis--environnement-technique)

---

4. [` ⚙️ `︲Installation du serveur VoIP XiVO Pollux.](#installation-du-serveur-voip-xivo-pollux)
   - [` 🐧 `︲Installation de Debian 12 (CLI).](#installation-de-debian-12-cli)
   - [` 🌐 `︲Configuration réseau initiale.](#configuration-réseau-initiale)
   - [` 📦 `︲Installation de XiVO Pollux Edition.](#installation-de-xivo-pollux-edition)
   - [` 🔐 `︲Configuration initiale via l’interface web.](#configuration-initiale-via-linterface-web)

---

5. [` 🏗️ `︲Configuration des entités et contextes.](#configuration-des-entités-et-contextes)
   - [` 🧩 `︲Création du contexte interne principal.](#création-du-contexte-interne-principal)
   - [` 🧾 `︲Définition des plages de numéros.](#définition-des-plages-de-numéros)

---

6. [` 👥 `︲Gestion des utilisateurs VoIP.](#gestion-des-utilisateurs-voip)
   - [` 🧑‍💼 `︲Création des utilisateurs du service Comptabilité.](#création-des-utilisateurs-du-service-comptabilité)
   - [` 📞 `︲Association lignes et numéros SIP.](#association-lignes-et-numéros-sip)

---

7. [` 📡 `︲Mise en place du service DHCP.](#mise-en-place-du-service-dhcp)
   - [` ⚙️ `︲Installation du serveur kea-dhcp4.](#installation-du-serveur-kea-dhcp4)
   - [` 🌐 `︲Configuration de l’étendue DHCP.](#configuration-de-létendue-dhcp)
   - [` 🔄 `︲Attribution automatique des paramètres réseau.](#attribution-automatique-des-paramètres-réseau)

---

8. [` ☎️ `︲Configuration d’un téléphone IP Snom D715.](#configuration-dun-téléphone-ip-snom-d715)
   - [` 🔑 `︲Récupération des identifiants SIP.](#récupération-des-identifiants-sip)
   - [` ⚙️ `︲Paramétrage manuel du téléphone.](#paramétrage-manuel-du-téléphone)

---

9. [` 💻 `︲Configuration des softphones.](#configuration-des-softphones)
   - [` 🖥️ `︲Configuration de ZoIPer sur PC.](#configuration-de-zoiper-sur-pc)
   - [` 📱 `︲Configuration de ZoIPer sur smartphone.](#configuration-de-zoiper-sur-smartphone)
   - [` 📶 `︲Configuration du point d’accès Wi-Fi TP-Link.](#configuration-du-point-daccès-wi-fi-tp-link)

---

10. [` 🔁 `︲Tests du plan d’appels.](#tests-du-plan-dappels)
    - [` 📞 `︲Appels internes entre utilisateurs.](#appels-internes-entre-utilisateurs)
    - [` ✅ `︲Validation du fonctionnement global.](#validation-du-fonctionnement-global)

---

11. [` 🤖 `︲Auto-approvisionnement des téléphones Snom.](#auto-approvisionnement-des-téléphones-snom)
    - [` 🔄 `︲Réinitialisation du téléphone.](#réinitialisation-du-téléphone)
    - [` 📦 `︲Installation du plugin xivo-snom.](#installation-du-plugin-xivo-snom)
    - [` ⚙️ `︲Configuration des modèles de terminaison.](#configuration-des-modèles-de-terminaison)
    - [` 🧩 `︲Association MAC / ligne utilisateur.](#association-mac--ligne-utilisateur)

---

12. [` 📬 `︲Configuration de la messagerie vocale.](#configuration-de-la-messagerie-vocale)
    - [` 🎙️ `︲Activation des boîtes vocales.](#activation-des-boîtes-vocales)
    - [` 🔧 `︲Paramétrage avancé.](#paramétrage-avancé)
    - [` 🧪 `︲Tests de dépôt et d’écoute des messages.](#tests-de-dépôt-et-découte-des-messages)

---

13. [` 🏢 `︲Création d’un second contexte interne.](#création-dun-second-contexte-interne)
    - [` 👤 `︲Utilisateurs du contexte Administratif.](#utilisateurs-du-contexte-administratif)
    - [` 📞 `︲Tests des appels internes.](#tests-des-appels-internes)

---

14. [` 🔀 `︲Routage des appels inter-contextes.](#routage-des-appels-inter-contextes)
    - [` 🔧 `︲Configuration du routage.](#configuration-du-routage)
    - [` 🧪 `︲Tests des appels inter-contextes.](#tests-des-appels-inter-contextes)

---

15. [` ♻️ `︲Réinitialisation des équipements.](#réinitialisation-des-équipements)
    - [` ☎️ `︲Réinitialisation des téléphones Snom.](#réinitialisation-des-téléphones-snom)
    - [` 📡 `︲Réinitialisation du point d’accès Wi-Fi.](#réinitialisation-du-point-daccès-wi-fi)

---

16. [` 🧰 `︲Outils & ressources utilisées.](#outils--ressources-utilisées)

---

17. [` ✅ `︲Conclusion & validation du TP.](#conclusion--validation-du-tp)

---

## `🟦`︲Introduction

---

La téléphonie sur IP (VoIP) est aujourd’hui un **service essentiel** au sein des infrastructures informatiques d’entreprise.  
Elle permet de centraliser les communications, de réduire les coûts, d’améliorer la flexibilité et de faciliter l’administration des postes téléphoniques.

Ce TP a pour but de **concevoir, déployer et valider une infrastructure VoIP fonctionnelle**, reposant sur :
- un serveur **XiVO Pollux**,
- des **téléphones IP physiques**,
- des **softphones**,
- un **service DHCP dédié**,
- et un **plan d’appels structuré par contextes**.

L’ensemble de la mise en œuvre est réalisé dans un environnement virtualisé, en respectant une logique **réseau, service et utilisateur**, proche d’un contexte professionnel réel.

## `🎯`︲Objectifs pédagogiques

---

À travers ce TP, les objectifs sont les suivants :

- Comprendre le fonctionnement d’un **système de téléphonie IP**
- Installer et administrer un **serveur XiVO**
- Mettre en œuvre un **service DHCP** adapté à la VoIP
- Gérer des **utilisateurs, lignes et contextes SIP**
- Configurer des **téléphones IP physiques** et des **softphones**
- Mettre en place un **plan d’appels cohérent**
- Tester et valider le bon fonctionnement du service

---

> [!TIP]  
> Cette documentation peut servir de **support d’apprentissage**, de **référence technique** ou de **base pour un déploiement VoIP en environnement réel**.

---

## `🧰`︲Prérequis et environnement technique

---

### `🖥️`︲Environnement matériel et logiciel

La mise en œuvre du TP repose sur l’environnement suivant :

- Un **poste hôte** capable de faire fonctionner un environnement virtualisé
- Un **hyperviseur** (VMware Workstation / VirtualBox)
- Une **machine virtuelle Debian 12 (CLI)** dédiée au serveur XiVO
- Un ou plusieurs **postes clients** (PC / smartphone)
- Un **téléphone IP Snom D715**
- Un **point d’accès Wi-Fi TP-Link**

---

### `🌐`︲Environnement réseau

L’architecture réseau mise en place comprend :

- Un réseau local isolé (NAT ou Host-Only selon l’hyperviseur)
- Un **serveur DHCP** permettant l’attribution automatique des paramètres réseau
- Des équipements VoIP connectés au même segment réseau
- Une communication SIP interne entre les différents utilisateurs

> [!NOTE]  
> L’ensemble des équipements (serveur, téléphones IP, softphones) doit impérativement se trouver sur le **même réseau logique** afin de garantir le bon fonctionnement du service VoIP.

---

### `📦`︲Services et composants utilisés

Les principaux composants utilisés durant ce TP sont :

- **XiVO Pollux Edition** (serveur VoIP)
- **Asterisk** (moteur de téléphonie)
- **kea-dhcp4** (service DHCP)
- **SIP** (protocole de signalisation)
- **Téléphones IP Snom**
- **Softphones ZoIPer** (PC et mobile)

---

> [!TIP]  
> Cette section pose les bases techniques nécessaires avant d’aborder l’installation et la configuration détaillée du serveur XiVO.

---

## `⚙️`︲Installation du serveur VoIP XiVO Pollux

---

Cette section décrit les étapes nécessaires à l’installation du **serveur VoIP XiVO Pollux**, depuis la mise en place du système Debian jusqu’à l’accès à l’interface d’administration web.

L’objectif est d’obtenir un **serveur fonctionnel, accessible sur le réseau**, prêt à accueillir les configurations VoIP (utilisateurs, lignes, contextes).

---

### `🐧`︲Installation de Debian 12 (CLI)

---

Le serveur XiVO repose sur une **installation minimale de Debian 12**, sans interface graphique, afin de garantir :
- de meilleures performances,
- une surface d’attaque réduite,
- une administration orientée serveur.

Lors de l’installation :
- sélectionner la langue et le clavier adaptés,
- configurer le réseau (DHCP ou IP statique selon le TP),
- définir un nom de machine explicite,
- créer le compte administrateur et l’utilisateur standard,
- installer uniquement les **utilitaires standards du système**.

> [!NOTE]  
> Aucune interface graphique n’est requise pour XiVO. Toute l’administration se fait via une interface web.

---

### `🌐`︲Configuration réseau initiale

---

Une fois Debian installé, une vérification de la configuration réseau est nécessaire afin de s’assurer que le serveur :
- dispose d’une adresse IP valide,
- peut communiquer avec les équipements VoIP,
- est accessible depuis le poste client.

Les points à vérifier :
- adresse IP attribuée,
- passerelle par défaut,
- résolution DNS fonctionnelle.

> [!TIP]  
> Une **adresse IP fixe** est fortement recommandée pour un serveur VoIP afin d’éviter toute perte de connectivité avec les téléphones.

---

### `📦`︲Installation de XiVO Pollux Edition

---

XiVO Pollux est installé à l’aide du script officiel fourni par l’éditeur.  
Cette étape permet de déployer automatiquement :
- le moteur de téléphonie (Asterisk),
- les services web,
- les composants nécessaires à la gestion des téléphones et des utilisateurs.

Les étapes principales sont :
- mise à jour du système,
- récupération du script d’installation,
- lancement de l’installation automatique,
- attente de la fin du déploiement des services.

> [!IMPORTANT]  
> L’installation peut prendre plusieurs minutes. Il est essentiel de **ne pas interrompre le processus**.

---

### `🔐`︲Configuration initiale via l’interface web

---

Une fois l’installation terminée, l’administration de XiVO s’effectue via une **interface web** accessible depuis un navigateur.

Cette interface permet :
- de finaliser la configuration du serveur,
- de vérifier l’état des services,
- d’accéder aux menus de gestion des utilisateurs et des lignes.

Les premières vérifications à effectuer :
- accessibilité de l’interface web,
- état des services XiVO,
- connexion avec le compte administrateur.

> [!TIP]  
> Cette étape marque la fin de l’installation du serveur. La suite du TP se concentre sur la **configuration logique du service VoIP**.

---


