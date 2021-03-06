---
title: API REST pour le portail d'appareil Xbox
description: Informations de référence sur les API pour UWP sur Xbox One.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: d8fdcf01d7d1f72624d73acf2d10ce28dfb75e04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656994"
---
# <a name="xbox-device-portal-rest-api"></a>API REST pour le portail d'appareil Xbox

Cette section contient des rubriques de référence pour l’API REST pour Xbox Device Portal, utilisée pour configurer et gérer votre console à distance.

| URI        | Description |
|------------|-------------|
|[/API/App/PackageManager/Register](wdp-loose-folder-register-api.md)| Inscrit une application qui est contenue dans un dossier isolé. |
|[/API/App/PackageManager/Upload](wdp-folder-upload.md)| Charge un dossier complet sur la console. |
|[/Ext/App/sshpins](uwp-sshpins-api.md)| Désactivez tous les codes confidentiels SSH approuvés à distance. Vous devrez refaire le couplage de code PIN pour le développement UWP Visual Studio. |
|[/Ext/App/deployinfo](uwp-deployinfo-api.md)| Nécessite des informations de déploiement pour un ou plusieurs packages installés. |
|[/ ext/fiddler](wdp-fiddler-api.md)| Activer et désactiver le suivi réseau de Fiddler. |
|[/Ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Obtenir le trafic HTTP à partir de l'application focalisée sur Xbox. |
|[/ ext/networkcredential](uwp-networkcredentials-api.md)| Ajouter, supprimer ou mettre à jour les informations d’identification réseau. |
|[/ ext/remoteinput](uwp-remoteinput-api.md)| Envoyer à distance l'entrée du clavier, de la souris ou de la manette sur une Xbox. |
|[/Ext/remoteinput/Controllers](uwp-remoteinput-controllers-api.md)| Obtenir le nombre de contrôleurs physiques connectés ou désactiver tous les contrôleurs physiques. |
|[capture d’écran/ext /](wdp-media-capture-api.md)| Capture une représentation PNG de l’écran actuellement affiché sur la console. |
|[/ ext/paramètres](wdp-xboxsettings-api.md)| Accède aux paramètres du développeur Xbox One. |
|[/Ext/SMB/developerfolder](wdp-smb-api.md)| Accède au dossier de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. |
|[/ ext/utilisateur](wdp-user-management.md)| Gère les utilisateurs sur la console Xbox One. |
|[/Ext/Xbox/info](wdp-xboxinfo-api.md)| Donne des informations sur l’appareil Xbox One. |
|[/Ext/xboxlive/sandbox](wdp-sandbox-api.md)| Gère votre sandbox Xbox Live. |

## <a name="see-also"></a>Voir également

- [UWP sur Xbox One](index.md)
- [Portail d’appareil Windows](../debug-test-perf/device-portal.md)