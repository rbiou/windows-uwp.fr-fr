---
title: Exécuter indéfiniment en arrière-plan
description: Utilisez la fonctionnalité extendedExecutionUnconstrained pour exécuter indéfiniment une tâche en arrière-plan ou une session d’exécution étendue en arrière-plan.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: tâche en arrière-plan, exécution étendue, ressources, limites, tâche en arrière-plan
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 55025d0348abdf311ebf020c70ccf9029bf7ec5a
ms.sourcegitcommit: ebd35887b00d94f1e76f7d26fa0d138ec4abe567
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888666"
---
# <a name="run-in-the-background-indefinitely"></a>Exécuter indéfiniment en arrière-plan

Pour fournir la meilleure expérience aux utilisateurs, Windows impose des limites de ressources sur les applications de plateforme Windows universelle (UWP). Les applications de premier plan reçoivent le plus de mémoire et de temps d’exécution, tandis que les applications en arrière-plan en obtiennent moins. Les utilisateurs sont ainsi protégés contre des performances médiocres des applications de premier plan et la décharge rapide de la batterie.

Toutefois, les développeurs qui écrivent des applications UWP pour une utilisation personnelle (autrement dit, des applications chargées indépendamment qui ne seront pas publiées dans le Microsoft Store) ou les développeurs qui écrivent des applications UWP d’entreprise peuvent souhaiter utiliser toutes les ressources disponibles sur l’appareil sans limitation d'exécution étendue ou en arrière-plan. Les applications UWP métier et personnelles peuvent utiliser des API dans Windows Creators Update (version 1703) pour désactiver la fonctionnalité de limitation. N’oubliez pas que vous ne pouvez pas placer une application dans le Microsoft Store si elle utilise ces API.

## <a name="run-while-minimized"></a>Exécuter en mode réduit

Les applications UWP basculent à l’état « suspendue » lorsqu’elles ne s’exécutent pas au premier plan. Sur le Bureau, cela se produit lorsqu’un utilisateur réduit l’application. Les applications utilisent une session d’exécution étendue pour continuer à s’exécuter en mode réduit. Les API d’exécution étendue qui sont acceptées par le Microsoft Store sont détaillées dans [Reporter la suspension d’une application avec l’exécution étendue](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution).

Si vous développez une application qui n’est pas destinée à être soumise dans le Microsoft Store, alors vous pouvez utiliser [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) avec la fonctionnalité restreinte `extendedExecutionUnconstrained` afin que votre application puisse continuer à s’exécuter en mode réduit, quel que soit l’état d’énergie de l’appareil.  

La fonctionnalité `extendedExecutionUnconstrained` est ajoutée en tant que fonctionnalité restreinte dans le manifeste de votre application. Voir [Déclarations des fonctionnalités d’application](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) pour plus d’informations sur les fonctionnalités restreintes.

> **Remarque :** Ajoutez la déclaration d’espace de noms XML *xmlns : ResCap* et utilisez le préfixe *ResCap* pour déclarer la fonctionnalité.

_Package.appxmanifest_
```xml
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
  ...
  <Capabilities>
    <rescap:Capability Name="extendedExecutionUnconstrained"/>
  </Capabilities>
</Package>
```

Lorsque vous utilisez la fonctionnalité `extendedExecutionUnconstrained`, [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) et [ExtendedExecutionForegroundReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) sont utilisés plutôt que [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) et [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). Le même modèle pour créer la session, définir les membres et demander l’extension de façon asynchrone s’applique toujours : 

```cs
var newSession = new ExtendedExecutionForegroundSession();
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;
newSession.Description = "Long Running Processing";
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();
switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```

Vous pouvez demander cette session d’exécution étendue dès que l’application s'affiche au premier plan. Les sessions d’exécution étendue sans contrainte ne sont pas limitées par les quotas d’énergie ou par l’économiseur de batterie du système d’exploitation. Tant qu’il existe une référence à l’objet de session, l’application reste à l’état « en cours d’exécution » et ne bascule pas à l’état « suspendue ». Si l’application est fermée par l’utilisateur, la session est révoquée.

L’enregistrement pour l’événement **Revoked** permet à votre application d’effectuer tout travail de nettoyage requis. Dans l’état « suspendue », vous pouvez créer une session d’exécution étendue avec [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) pour enregistrer les données utilisateur avant que l’application ne soit arrêtée et supprimée de la mémoire.

## <a name="run-background-tasks-indefinitely"></a>Exécuter indéfiniment les tâches en arrière-plan

Dans la plateforme Windows universelle, les tâches en arrière-plan sont des processus qui s’exécutent en arrière-plan sans aucune forme d’interface utilisateur. Les tâches en arrière-plan peuvent s’exécuter en général pour une durée maximale de 25 secondes avant d’être annulées. Certaines des tâches les plus longues en termes d'exécution font également l’objet d’une vérification qui garantit que la tâche en arrière-plan ne reste pas inactive ou utilise de la mémoire. Dans Windows Creators Update (version 1703), la fonctionnalité restreinte [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) a été introduite pour supprimer ces limites. La fonctionnalité **extendedBackgroundTaskTime** est ajoutée en tant que fonctionnalité restreinte dans le fichier manifeste de votre application :

> **Remarque :** Ajoutez la déclaration d’espace de noms XML *xmlns : ResCap* et utilisez le préfixe *ResCap* pour déclarer la fonctionnalité.

_Package.appxmanifest_
```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
...
  <Capabilities>
    <rescap:Capability Name="extendedBackgroundTaskTime"/>
  </Capabilities>
</Package>
```

Cette fonctionnalité supprime les limites de durée d’exécution et la surveillance des tâches inactives. Lorsqu’une tâche en arrière-plan a démarré, que ce soit par un déclencheur ou un appel au service d’application, une fois qu’elle effectue un report sur le [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) fourni par la méthode **Exécuter**, elle peut s’exécuter indéfiniment. Si l’application est définie sur **Géré par Windows**, alors un quota d’énergie peut encore lui être appliqué et ses tâches en arrière-plan ne sont pas activées lorsque l’économiseur de batterie est actif. Cela peut être modifié avec les paramètres de système d’exploitation. Des informations supplémentaires sont disponibles dans [Optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

La plateforme Windows universelle surveille l’exécution des tâches en arrière-plan afin de garantir une bonne autonomie de la batterie et une expérience fluide avec les applications de premier plan. Toutefois, les applications personnelles et les applications métier d’entreprise peuvent utiliser l’exécution étendue et la fonctionnalité **extendedBackgroundTaskTime** pour créer des applications qui seront exécutées tant que nécessaire, quelle que soit la disponibilité des ressources de l’appareil.

N’oubliez pas que les fonctionnalités **extendedExecutionUnconstrained** et **extendedBackgroundTaskTime** peuvent remplacer la stratégie par défaut pour les applications UWP et risquent de décharger rapidement la batterie Avant d’utiliser ces fonctionnalités, confirmez tout d’abord que les stratégies de durée par défaut pour l'exécution étendue et les tâches en arrière-plan ne correspondent pas à vos besoins, et exécutez des tests en conditions de batterie restreinte afin de comprendre l’impact de votre application sur un appareil.

## <a name="see-also"></a>Articles associés

[Supprimer les restrictions de ressources de tâche en arrière-plan](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)
