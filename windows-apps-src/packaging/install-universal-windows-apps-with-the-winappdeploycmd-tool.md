---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Installer des applications avec l’outil WinAppDeployCmd.exe
description: Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application de plateforme Windows universelle (UWP) à partir d’un PC Windows 10 et vers tout appareil Windows 10.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6c8383a5b0041d5edf6e0c2c8d94acf82572d13
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "70808447"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>Installer des applications avec l’outil WinAppDeployCmd.exe

Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application de plateforme Windows universelle (UWP) à partir d’un PC Windows 10 et vers tout appareil Windows 10. Vous pouvez utiliser cet outil pour déployer un package d’application quand l’appareil Windows 10 est connecté par le biais d’un port USB ou disponible sur le même sous-réseau, sans avoir besoin de Microsoft Visual Studio ni de la solution pour cette application. Vous pouvez également déployer l’application sans créer de package au préalable vers un ordinateur distant ou une Xbox One. Cet article décrit comment installer des applications UWP à l’aide de cet outil.

Vous devez simplement installer le SDK Windows 10 pour exécuter l’outil WinAppDeployCmd à partir d’une invite de commandes ou d’un fichier de script. Quand vous installez une application avec WinAppDeployCmd.exe, celui-ci utilise le fichier .appx/.msix ou AppxManifest (pour les fichiers isolés) pour le chargement indépendant de votre application sur un appareil Windows 10. Cette commande n’installe pas le certificat nécessaire pour votre application. Pour exécuter l’application, l’appareil Windows 10 doit être en mode développeur ou le certificat doit déjà avoir été installé.

Pour un déploiement vers des appareils mobiles, vous devez d’abord créer un package. Pour plus d’informations, voir [cet article](/windows/msix/package/packaging-uwp-apps).

L’outil **WinAppDeployCmd.exe** se trouve à cet emplacement sur votre PC Windows 10 : **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\&lt;SDK Version&gt;\\x86\\WinAppDeployCmd.exe** (selon votre chemin d’installation pour le SDK).

> [!NOTE]
> Les versions 15063 et ultérieures du SDK sont installées côte-à-côte, dans des dossiers spécifiques à la version. Les SDK précédents (version 14393 et versions antérieures) sont écrits directement dans le dossier parent.

Tout d’abord, connectez votre appareil Windows 10 au même sous-réseau ou connectez-le directement à votre ordinateur Windows 10 par le biais d’un port USB. Utilisez ensuite la syntaxe et les exemples suivants de la commande présentés plus loin dans cet article pour déployer votre application UWP :

## <a name="winappdeploycmd-syntax-and-options"></a>Options et syntaxe WinAppDeployCmd

Il s’agit de la syntaxe générale utilisée pour **WinAppDeployCmd.exe** :

```CMD
WinAppDeployCmd command -option <argument>
```

Voici quelques exemples de syntaxe supplémentaire associée à l’utilisation de diverses commandes :

```CMD
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

Vous pouvez installer ou désinstaller une application sur l’appareil cible, ou vous pouvez mettre à jour une application déjà installée. Pour conserver les données ou les paramètres enregistrés par une application déjà installée, utilisez les options **update** plutôt que les options **install**.

Le tableau suivant décrit les commandes pour **WinAppDeployCmd.exe**.

| **Commande**  | **Description**                                                     |
|--------------|---------------------------------------------------------------------|
| périphériques      | Affiche la liste des périphériques réseau disponibles.                         |
| installer      | Installe un package d’application UWP sur l’appareil cible.                     |
| mise à jour       | Met à jour une application UWP déjà installée sur l’appareil cible.    |
| list         | Affiche la liste des applications UWP installées sur l’appareil cible spécifié. |
| uninstall    | Désinstalle le package d’application spécifié de l’appareil cible.         |
| deployfiles  | Copie les fichiers isolés d’application présents dans le chemin d’accès cible vers le chemin d’accès relatif distant sur l’appareil.|
| registerfiles| Inscrit les fichiers isolés d’application dans le répertoire de déploiement distant.         |
| addcreds     | Ajoute des informations d’identification à une Xbox pour lui permettre d’accéder à un emplacement réseau pour l’enregistrement d’applications.|
| getcreds     | Récupère les informations d’identification réseau utilisées par la cible lors de l’exécution d’une application depuis un partage réseau.|
| deletecreds  | Supprime les informations d’identification réseau utilisées par la cible lors de l’exécution d’une application depuis un partage réseau.|

Le tableau suivant décrit les options pour **WinAppDeployCmd.exe**.

| **Commande**  | **Description**  |
|--------------|------------------|
| -h (-help)       | Affiche les commandes, les options et les arguments. |
| -ip              | Adresse IP de l’appareil cible. |
| -g (-guid)       | Identificateur unique de l’appareil cible.|
| -d (-dependency) | (Facultatif) Spécifie le chemin d’accès de dépendance pour chacune des dépendances du package. Si aucun chemin d’accès n’est spécifié, l’outil recherche des dépendances dans le répertoire racine pour le package d’application et les répertoires du kit de développement logiciel (SDK).|
| -f (-file)       | Chemin d’accès de fichier pour le package d’application à installer, mettre à jour ou désinstaller.|
| -p (-package)    | Le nom complet du package pour le package d’application à désinstaller. (Vous pouvez utiliser la commande « list » pour rechercher le nom complet des packages déjà installés sur l’appareil.) |
| -pin             | Un code confidentiel, s’il est nécessaire pour établir une connexion avec l’appareil cible. (Vous serez invité à réessayer avec l’option -pin si une authentification est nécessaire.) |
| -credserver      | Nom de serveur des informations d’identification réseau à utiliser par la cible. |
| -credusername    | Nom d’utilisateur des informations d’identification réseau à utiliser par la cible. |
| -credpassword    | Mot de passe des informations d’identification réseau à utiliser par la cible. |
| -connecttimeout  | Délai d’expiration en secondes utilisé lors de la connexion à l’appareil. |
| -remotedeploydir | Le chemin/nom relatif du répertoire pour copier les fichiers sur l’appareil distant ; il s’agira d’un dossier de déploiement distant bien connu et automatiquement déterminé. |
| -deleteextrafile | Paramètre booléen servant à indiquer si les fichiers existants du répertoire distant doivent être supprimés pour que ce dernier corresponde au répertoire source. |

Le tableau suivant décrit les options pour **WinAppDeployCmd.exe**.

| **Argument**           | **Description**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | Délai d’expiration en secondes. (La valeur par défaut est 10)                                          |
| &lt;address&gt;        | Adresse IP ou identificateur unique de l’appareil cible.                        |
| &lt;a&gt;&lt;b&gt; ... | Chemin d’accès de dépendance pour chacune des dépendances du package d’application.                    |
| &lt;p&gt;              | Un PIN alphanumérique indiqué dans les paramètres de l’appareil pour établir une connexion. |
| &lt;path&gt;           | Chemin d’accès au système de fichiers.                                                            |
| &lt;name&gt;           | Nom complet du package pour le package d’application à désinstaller.                          |
| &lt;server&gt;         | Serveur sur le réseau de fichiers.                                                  |
| &lt;username&gt;       | Nom d’utilisateur des informations d’identification pour l’accès au serveur sur le réseau de fichiers.      |
| &lt;password&gt;       | Mot de passe des informations d’identification pour l’accès au serveur sur le réseau de fichiers. |
| &lt;remotedeploydir&gt;| Répertoire de l’appareil relatif à l’emplacement de déploiement.                      |

## <a name="winappdeploycmdexe-examples"></a>Exemples de WinAppDeployCmd.exe

Voici quelques exemples montrant comment effectuer un déploiement à partir de la ligne de commande, à l’aide de la syntaxe de **WinAppDeployCmd.exe**.

Affiche les appareils qui sont disponibles pour le déploiement. La commande expire au bout de 3 secondes.

``` CMD
WinAppDeployCmd devices 3
```

Installe l’application à partir du package MyApp.appx qui se trouve dans le répertoire Téléchargements de votre PC pour un appareil Windows 10 avec l’adresse IP 192.168.0.1 et avec le PIN A1B2C3 nécessaire pour établir une connexion avec l’appareil.

``` CMD
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Désinstalle le package spécifié (en fonction de son nom complet) à partir d’un appareil Windows 10 avec l’adresse IP 192.168.0.1. Vous pouvez utiliser la commande list pour afficher le nom complet de tous les packages installés sur un appareil.

``` CMD
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Met à jour l’application qui est déjà installée sur l’appareil Windows 10 avec l’adresse IP 192.168.0.1 à l’aide du package d’application spécifié.

``` CMD
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Déploie les fichiers d’une application présents dans le même dossier que le fichier AppxManifest vers un PC ou une Xbox avec l’adresse IP 192.168.0.1 dans le répertoire app1_F5 sous le chemin d’accès de déploiement de l’appareil.

``` CMD
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Inscrit l’application dans le répertoire app1_F5 sous le chemin de déploiement du PC ou de la Xbox à l’adresse IP 192.168.0.1.

``` CMD
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>Utilisation de WinAppDeployCmd pour configurer un déploiement d’exécution à partir d’un PC sur une console Xbox One

L’exécution à partir d’un PC vous permet de déployer une application UWP sur une console Xbox One sans copier les fichiers binaires dessus. Au lieu de cela, les fichiers binaires sont hébergés sur un partage réseau sur le même réseau que la Xbox.  Pour ce faire, vous avez besoin d’une console Xbox One déverrouillée par le développeur et d’un application UWP à fichier libre sur un lecteur réseau auquel la Xbox peut accéder.

Exécutez la commande suivante pour inscrire l’application :

``` CMD
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
