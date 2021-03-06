---
title: Applications et appareils connectés (projet Rome)
description: Cette section explique comment utiliser la plateforme Systèmes distants pour identifier les appareils distants, lancer une application sur un appareil distant et communiquer avec un service d’application sur un appareil distant.
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP, appareils connectés, systèmes distants, Rome, projet Rome
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: 742ef56165ea26df3a03d54e99a17d2a00908cb6
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684455"
---
# <a name="connected-apps-and-devices-project-rome"></a>Applications et appareils connectés (projet Rome)

Cette section explique comment connecter des applications sur des appareils et des plateformes à l’aide du [projet Rome](https://developer.microsoft.com/windows/project-rome). Pour savoir comment implémenter le projet Rome dans un scénario multiplateforme, visitez la [page principale docs pour le projet Rome](https://docs.microsoft.com/windows/project-rome/).

La plupart des utilisateurs ont plusieurs appareils et commencent souvent une activité sur un pour la terminer sur un autre. Pour répondre à cette situation, les applications doivent être accessibles sur plusieurs appareils et plateformes. Le projet Rome vous permet de découvrir des appareils distants, de lancer une application sur un appareil distant et de communiquer avec un service d’application sur un appareil distant.

Les [API Systèmes distants](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems) intégrées dans Windows 10 version 1607 vous permettent d’écrire des applications grâce auxquelles les utilisateurs peuvent commencer une tâche sur un appareil et la terminer sur un autre. La tâche reste le point central, et les utilisateurs peuvent faire leur travail sur l’appareil le plus pratique pour eux. Par exemple, un utilisateur écoute la radio sur son téléphone en voiture, mais une fois à la maison, il voudra sans doute transférer la lecture à son Xbox One qui est raccordée à sa chaîne stéréo.

Vous pouvez également utiliser le projet Rome pour les appareils compléments ou des scénarios de contrôle à distance. Utilisez les API de messagerie de service d'application pour créer un canal d’application entre deux appareils afin d’échanger des messages personnalisés. Par exemple, vous pouvez écrire une application pour votre téléphone, qui contrôle la lecture sur votre téléviseur, ou une application complément qui fournit des informations sur les personnages d’une émission de télévision que vous regardez via une autre application.  

Les appareils peuvent être connectés à proximité par Bluetooth et sans fil, ou à distance par le cloud ; ils sont liés par le compte Microsoft (MSA) de la personne qui les utilise.

Pour des exemples illustrant comment détecter le système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages entre des applications qui s’exécutent sur deux systèmes, consultez la section [Exemple de Systèmes distants UWP](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ).

Pour plus d’informations sur le projet Rome en général, y compris les ressources pour l’intégration inter-plateforme, accédez à [aka.ms/project-rome](https://developer.microsoft.com/windows/project-rome).

| Sujet | Description |
|-------|-------------|
| [Lancer une application sur un appareil distant](launch-a-remote-app.md) | Découvrez comment lancer une application sur un appareil distant. Cette rubrique décrit le cas d'utilisation le plus simple et la configuration préliminaire.  |
| [Découvrir des appareils distants](discover-remote-devices.md)  | Découvrez comment détecter les appareils auxquels vous pouvez vous connecter. |
| [Communiquer avec un service d’application distant](communicate-with-a-remote-app-service.md) | Découvrez comment interagir avec une application sur un appareil distant. |
| [Connecter des appareils via des sessions à distance](remote-sessions.md) | Créez des expériences partagées sur plusieurs appareils en les rejoignant dans une session à distance. |
| [Poursuivre l’activité utilisateur, même sur différents appareils](useractivities.md)| Aidez les utilisateurs à reprendre ce qu’ils faisaient dans votre application, même sur plusieurs appareils.|
| [Meilleures pratiques relatives aux activités de l’utilisateur](useractivities-best-practices.md)| Découvrez les pratiques recommandées pour la création et la mise à jour des activités des utilisateurs.|
