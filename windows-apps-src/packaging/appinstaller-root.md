---
author: laurenhughes
title: Installer des applications UWP avec le Programme d’installation d’application
description: Cette section contient ou associe des articles sur le Programme d’installation d’application et comment utiliser les fonctionnalités du Programme d’installation d’application.
ms.author: lahugh
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, programme d’installation d’application, appinstaller, charger une version test, ensemble connexe, packages facultatifs
ms.localizationpriority: medium
ms.openlocfilehash: 0f82c319ca812af9296895711f2b546a8b0332c5
ms.sourcegitcommit: 618741673a26bd718962d4b8f859e632879f9d61
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "1992105"
---
# <a name="install-uwp-apps-with-app-installer"></a>Installer des applications UWP avec le Programme d’installation d’application

## <a name="purpose"></a>Objectif
Cette section contient ou associe des articles sur le Programme d’installation d’application et comment utiliser les fonctionnalités du Programme d’installation d’application. 

Le Programme d’installation d’application permet d'installer des applications UWP en double-cliquant sur le package de l’application. Cela signifie que les utilisateurs n’ont pas besoin d’utiliser PowerShell ou d’autres outils de développement pour déployer des applications UWP. Le Programme d’installation d’application permet également d’installer une application à partir du web, des packages facultatifs et des ensembles connexes. Pour savoir comment utiliser le Programme d’installation d’application pour installer votre application, consultez les rubriques dans le tableau.

| Rubrique | Description |
|-------|-------------|
| [Créer un fichier de programme d'installation d'application avec VisualStudio](create-appinstallerfile-vs.md)| Découvrez comment utiliser VisualStudio pour permettre des mises à jour automatiques à l'aide du fichier .appinstaller. |
| [Installer des applications UWP à partir d’une page web](installing-UWP-apps-web.md) | Dans cette section, nous allons examiner les étapes à suivre pour permettre aux utilisateurs d’installer vos applications directement à partir de la page web. |
| [Installer un ensemble connexe à l’aide d’un fichier du Programme d’installation d’application](install-related-set.md) | Dans cette section, découvrez comment autoriser l’installation d’un ensemble connexe via le Programme d’installation d’application. Nous effectuerons également les étapes nécessaires pour créer un fichier du Programme d’installation d’application qui définira votre ensemble connexe. |
| [Résoudre les problèmes d’installation avec le fichier du programme d'installation d'application](troubleshoot-appinstaller-issues.md) | Problèmes fréquents et solutions en cas de chargement de version test des applications avec le fichier du programme d'installation d'application. |
| [Référence du fichier du programme d’installation d’application (.appinstaller)](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file) | Afficher le schéma XML complet pour le fichier du Programme d’installation d’application. |

## <a name="tutorials"></a>Didacticiels 

Suivez ces didacticiels et apprenez à héberger et installer une application UWP à partir de différentes plateformes de distribution. Ces didacticiels sont utiles pour les entreprises et les développeurs qui ne veulent pas ou n'ont pas besoin de publier leurs applications dans le Microsoft Store, mais qui souhaitent tirer parti de la plateforme Windows10 de déploiement et de création de packages.

| Didacticiel | Description |
|----------|-------------|
| [Installer une application UWP à partir d’une application Web Azure](web-install-azure.md) | Créez une application Web Azure et utilisez-la pour héberger et distribuer votre package d’application UWP. |
| [Installer une application UWP à partir d’un serveur IIS](web-install-IIS.md) | Configurez un serveur IIS, vérifiez que votre application web peut héberger des packages d’application et utilisez le Programme d'installation d'application de manière efficace. |
| [Hébergement de packages d'application UWP pour l'installation web](web-install-aws.md) | Découvrez comment configurer Amazon Simple Storage Service pour héberger votre package d’application UWP à partir d’un site Web. |
