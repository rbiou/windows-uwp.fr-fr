---
title: Lancer une application sur un appareil distant
description: Apprenez à lancer un appareil sur un appareil distant à l'aide du projet « Rome ».
ms.date: 02/12/2018
ms.topic: article
keywords: appareils Windows 10, uwp, connectés, les systèmes distants, rome, project rome
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: ac4a5783250f3bd21cb8a3b96a579715830e687d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371712"
---
# <a name="launch-an-app-on-a-remote-device"></a>Lancer une application sur un appareil distant

Cet article explique comment lancer une application Windows sur un appareil distant.

Depuis Windows 10 version 1607, une application UWP peut lancer une application UWP ou une application de bureau Windows à distance sur un autre appareil qui exécute également Windows 10 version 1607 ou ultérieure, pourvu que les deux appareils soient signés avec le même compte Microsoft (MSA). Il s'agit du cas d’utilisation le plus simple du projet Rome.

La fonctionnalité de lancement à distance permet des expériences orientées tâches : un utilisateur peut démarrer une tâche sur un appareil et la terminer sur un autre. Par exemple, si l’utilisateur écoute de la musique sur son téléphone dans sa voiture, il peut ensuite transférer la fonctionnalité de lecture sur sa console Xbox One lorsqu’il arrive chez lui. Le lancement à distance permet à des applications de transmettre des données contextuelles à l’application distante démarrée, afin de reprendre exactement la tâche là où elle a été arrêtée.

## <a name="preliminary-setup"></a>Configuration préliminaire

### <a name="add-the-remotesystem-capability"></a>Ajouter la fonctionnalité remoteSystem

Pour que votre application puisse lancer une application sur un appareil distant, vous devez ajouter la fonctionnalité `remoteSystem` dans le manifeste de votre package d’application. Vous pouvez l’ajouter avec le concepteur de manifeste de package en sélectionnant **Système distant** dans l’onglet **Capacités**, ou manuellement en ajoutant la ligne suivante dans le fichier _Package.appxmanifest_ de votre projet.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Activer le partage sur plusieurs appareils

En outre, l’appareil client doit être configuré pour autoriser le partage sur plusieurs appareils. Ce paramètre, qui est accessible dans **paramètres**: **Système** > **partagé expériences** > **partage entre les appareils**, est activée par défaut. 

![page de paramètres Expériences partagées](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Trouver un appareil distant

Vous devez tout d’abord trouver l’appareil auquel vous souhaitez vous connecter. La section [Détecter les périphériques distants](discover-remote-devices.md) explique en détail comment faire. Nous allons utiliser une approche simple, qui permet un filtrage par type d’appareil ou de connectivité. Nous allons créer un observateur qui recherche des appareils distants et écrire des gestionnaires d’événement qui surviennent lorsque des appareils sont détectés ou supprimés. Ce qui nous fournira un ensemble d’appareils distants.

Le code de ces exemples nécessite que votre ou vos fichier(s) contienne(nt) une instruction `using Windows.System.RemoteSystems`.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

La première chose à faire avant d’effectuer lancement à distance est d’appeler `RemoteSystem.RequestAccessAsync()`. Vérifiez la valeur de retour pour vous assurer que votre application est autorisée à accéder à des appareils distants. Cette vérification peut échouer si vous n’avez ajouté la fonctionnalité `remoteSystem` à votre application.

Les gestionnaires d’événements de l’Observateur du système sont appelés lorsqu’un appareil auquel nous pouvons nous connecter est détecté ou n’est plus disponible. Nous allons utiliser ces gestionnaires d’événements pour conserver une liste mise à jour d’appareils auxquels nous pouvons nous connecter.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


Nous assurons le suivi des appareils par ID de système distant, à l’aide d’un **dictionnaire**. Un objet **ObservableCollection** est utilisé pour stocker la liste des appareils que nous pouvons énumérer. Un objet **ObservableCollection** facilite aussi la liaison de la liste des appareils à l’interface utilisateur, mais nous nous en dispenserons dans cet exemple.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Ajoutez un appel à `BuildDeviceList()` dans le code de démarrage de l’application avant d’essayer de lancer une application à distance.

## <a name="launch-an-app-on-a-remote-device"></a>Lancer une application sur un appareil distant

Lancez une application à distance en transmettant l’appareil auquel vous souhaitez vous connecter à l’API [**RemoteLauncher.LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotelauncher.launchuriasync). Il existe trois surcharges pour cette méthode. La plus simple, reprise dans cet exemple, spécifie l’URI qui active l’application sur l’appareil distant. Dans cet exemple, l’URI ouvre l’application Cartes sur l’ordinateur distant avec une vue 3D sur l’Aiguille de l’espace de Seattle.

Les autres surcharges de **RemoteLauncher.LaunchUriAsync** vous permettent de spécifier des options telles que l’URI du site web à afficher si aucune application capable de gérer l’URI ne peut être lancée sur l’appareil distant, et une liste facultative de noms de famille de package pouvant servir à lancer l’URI sur l’appareil distant. Vous pouvez également fournir des données sous la forme de paires clé/valeur. Vous pouvez transmettre des données à l’application que vous activez pour fournir un contexte à l’application distante, comme le nom de la chanson à lire et le lieu de lecture en cours lorsque vous basculez la lecture d’un appareil à un autre.

Concrètement, vous pouvez fournir l’interface utilisateur pour sélectionner l’appareil que vous souhaitez cibler. Mais pour simplifier cet exemple, nous allons utiliser le premier appareil distant sur la liste.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

L’objet [**RemoteLaunchUriStatus**](https://docs.microsoft.com/uwp/api/windows.system.remotelaunchuristatus) renvoyé par **RemoteLauncher.LaunchUriAsync()** permet de savoir si le lancement à distance a abouti ou non (et dans ce cas, la raison de l’échec).

## <a name="related-topics"></a>Rubriques connexes

[Référence de l’API de systèmes à distance](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)  
[Vue d’ensemble des (Project Rome) applications et des appareils connecté](connected-apps-and-devices.md)  
[Découvrir des appareils distants](discover-remote-devices.md)  
L’[exemple Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) montre comment détecter un système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages avec des applications qui s’exécutent sur deux systèmes.
