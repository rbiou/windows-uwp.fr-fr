---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: Cet article vous explique comment lire du contenu multimédia pendant l’exécution de votre application en arrière-plan.
title: Lire du contenu multimédia en arrière-plan
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fb43e5b7006c7c81875651a926e87eb8f76621fe
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254301"
---
# <a name="play-media-in-the-background"></a>Lire du contenu multimédia en arrière-plan
Cet article vous explique comment configurer votre application de telle sorte que le contenu multimédia continue à être lu quand votre application est déplacée du premier plan vers l’arrière-plan. Cela signifie que même après que l’utilisateur a réduit votre application, est revenu à l’écran d’accueil ou a quitté votre application d’une autre manière, votre application peut continuer à lire le contenu audio. 

Scénarios de lecture audio en arrière-plan :

-   **Playslist de longue durée :** l’utilisateur affiche brièvement une application au premier plan pour sélectionner et lancer une playslist, puis veut que la lecture de la playslist continue en arrière-plan.

-   **Utilisation du Sélecteur de tâches :** l’utilisateur affiche brièvement une application au premier plan pour démarrer la lecture d’un contenu audio, puis passe dans une autre application ouverte à l’aide du Sélecteur de tâches. Il veut que la lecture du contenu audio continue en arrière-plan.

L’implémentation audio en arrière-plan décrite dans cet article permettra à votre application de s’exécuter universellement sur tous les appareils Windows, y compris les appareils mobiles, de bureau et Xbox.

> [!NOTE]
> Le code de cet article a été adapté de [l’exemple Contenu audio en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback) UWP.

## <a name="explanation-of-one-process-model"></a>Explication du modèle à processus unique
Avec Windows 10, version 1607, un nouveau modèle à processus unique simplifie considérablement la prise en charge de l’audio d’arrière-plan. Auparavant, votre application devait gérer un processus en arrière-plan en plus de l’application de premier plan. De votre côté, vous deviez communiquer manuellement les modifications d’état de communication entre les deux processus. Sous le nouveau modèle, vous ajoutez simplement la capacité d’audio d’arrière-plan à votre manifeste d’application, de manière à ce que votre application continue à lire le contenu audio lorsqu’elle se déplace vers l’arrière-plan. Deux événements de cycle de vie d’application, [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) et [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground), indiquent à votre application les moments d’entrée et de sortie de l’arrière-plan. Quand votre application se déplace au sein des transitions à destination et en provenance de l’arrière-plan, les contraintes de mémoire mises en place par le système peuvent être modifiées, afin que vous puissiez utiliser ces événements pour évaluer votre consommation courante de mémoire et libérer des ressources vous permettant de rester sous la limite.

En éliminant les activités complexes de communication intraprocessus et de gestion de l’état, le nouveau modèle vous permet d’implémenter l’audio d’arrière-plan bien plus rapidement, via une réduction considérable du code. Toutefois, le modèle à deux processus est toujours pris en charge pour la compatibilité descendante dans la version actuelle. Pour plus d’informations, consultez la page [Contenu audio en arrière-plan](legacy-background-media-playback.md).

## <a name="requirements-for-background-audio"></a>Conditions requises pour l’audio d’arrière-plan
Votre application doit satisfaire les exigences suivantes associées à la lecture de contenu audio durant sa mise en arrière-plan.

* Ajoutez la fonctionnalité de **Lecture de médias en arrière-plan** à votre manifeste d’application, tel que décrit plus bas dans cet article.
* Si votre application désactive l’intégration automatique de **MediaPlayer** avec les contrôles de transport de média système, en définissant par exemple la propriété [**CommandManager.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) sur False, vous devez implémenter l’intégration manuelle avec les contrôles de transport de média système afin de prendre en charge la lecture de médias en arrière-plan. Vous devez également procéder à une intégration manuelle avec les contrôles de transport de média système si vous utilisez une API différente de **MediaPlayer**, telle que [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph), afin de lire du contenu audio de manière ininterrompue durant la mise en arrière-plan de votre application. La configuration minimale requise en matière d’intégration des contrôles de transport de média système est décrite dans la section « Utiliser les contrôles de transport de média système pour le son en arrière-plan » de [Contrôle manuel des contrôles de transport de média système](system-media-transport-controls.md).
* Pendant que votre application est en arrière-plan, vous devez rester sous les limites d’utilisation de la mémoire définies par le système pour les applications en arrière-plan. Les recommandations en matière de gestion de la mémoire pendant la mise en arrière-plan sont fournies plus loin dans cet article.

## <a name="background-media-playback-manifest-capability"></a>Fonctionnalité de manifeste de lecture de médias en arrière-plan
Pour activer l’audio en arrière-plan, vous devez ajouter la fonctionnalité de lecture de médias en arrière-plan au fichier du manifeste d’application, Package.appxmanifest. 

**Pour ajouter des fonctionnalités au manifeste d’application à l’aide du concepteur de manifeste**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Sélectionnez la case **Lecture de médias en arrière-plan**.

Pour définir la fonctionnalité en modifiant manuellement le manifeste xml de l’application, assurez-vous que le préfixe d’espace de noms *uap3* est défini dans l’élément **Package**. Si ce n’est pas le cas, ajoutez-le tel qu’illustré ci-dessous.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

Ensuite, ajoutez la fonctionnalité *backgroundMediaPlayback* à l’élément **Capabilities** :
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

## <a name="handle-transitioning-between-foreground-and-background"></a>Gérer la transition entre le premier plan et l’arrière-plan
Lorsque votre application passe du premier plan à l’arrière-plan, l’événement [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) est déclenché. Quand votre application revient au premier plan, l’événement [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) est déclenché. Ces événements étant liés au cycle de vie de l’application, vous devez enregistrer des gestionnaires dédiés lors de sa création. Dans le modèle de projet par défaut, vous devrez procéder à leur ajout dans le constructeur de la classe **App**, dans App.xaml.cs. 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Créez une variable affectée à la détection de l’exécution en arrière-plan.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Lorsque l’événement [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) est déclenché, définissez la variable de détection afin d’indiquer que l’exécution se déroule actuellement dans l’arrière-plan. Vous n’avez pas intérêt à effectuer de tâches longues dans l’événement **EnteredBackground**, dans la mesure où la transition vers l’arrière-plan pourrait apparaître lente pour l’utilisateur.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Dans le gestionnaire d’événement [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground), vous devez définir la variable de détection afin d’indiquer que votre application ne s’exécute plus en arrière-plan.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>Exigences en matière de gestion de mémoire
La partie la plus importante du traitement de la transition entre le premier plan et l’arrière-plan consiste à gérer la mémoire utilisée par votre application. L’exécution en arrière-plan impliquant la réduction des ressources de mémoire que votre application est autorisée à conserver au niveau du système, vous devez également procéder à une inscription pour les événements [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) et [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging). Lorsque ces événements sont déclenchés, vous devez vérifier la quantité de mémoire actuellement utilisée par votre application ainsi que la limite actuelle, et réduire votre consommation de mémoire si nécessaire. Pour plus d’informations sur la manière de réduire votre consommation de mémoire pendant une exécution en arrière-plan, consultez la page [Libérer de la mémoire lorsque votre application bascule en arrière-plan](../launch-resume/reduce-memory-usage.md).

## <a name="network-availability-for-background-media-apps"></a>Disponibilité du réseau pour les applications multimédias en arrière-plan
L’ensemble des sources multimédias reconnaissant le réseau, celles qui ne sont pas créées à partir d’un flux ou d’un fichier, maintiennent l’activité de la connexion réseau pendant la récupération du contenu à distance et abandonnent l’activité dans le cas contraire. [**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource), en particulier, s’appuie sur l’application pour signaler correctement la plage mise en mémoire tampon correcte à la plateforme à l’aide de [**SetBufferedRange**](https://docs.microsoft.com/uwp/api/windows.media.core.mediastreamsource.setbufferedrange). Une fois que l’intégralité du contenu est mis en tampon, le réseau n’est plus réservé pour le compte de l’application.

SI vous avez besoin d’effectuer des appels réseau intervenant en arrière-plan lorsqu’aucun contenu multimédia n’est en cours de téléchargement, ces opérations doivent être encapsulées dans une tâche appropriée telle que [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) ou [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger). Pour plus d’informations, voir [Prendre en charge votre application avec des tâches en arrière-plan](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lire des fichiers audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Intégration aux contrôles de transport des médias système](integrate-with-systemmediatransportcontrols.md)
* [Exemple audio d’arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 




