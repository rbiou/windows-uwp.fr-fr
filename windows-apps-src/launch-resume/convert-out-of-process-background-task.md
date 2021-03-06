---
title: Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process
description: Une tâche en arrière-plan out-of-process pour une tâche en arrière-plan dans le processus qui s’exécute au sein de votre processus d’application de premier plan du port.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, uwp, tâche en arrière-plan, service d’application
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 42aaa5600b30924acc84aa61c2a15dbb2a320c10
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366260"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process

Pour votre activité d’arrière-plan (OOP) out-of-process pour l’activité dans le processus de port le plus simple consiste à mettre votre [IBackgroundTask.Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) méthode de code à l’intérieur de votre application et lancer à partir [OnBackgroundActivated ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La technique décrite ici n’est pas sur la création d’un shim à partir d’une tâche d’arrière-plan OOP à une tâche en arrière-plan dans le processus ; de réécriture sur (ou portage) une version de la programmation orientée objet pour une version in-process.

Si votre application comporte plusieurs tâches en arrière-plan, [l’exemple d’activation en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) montre comment vous pouvez utiliser `BackgroundActivatedEventArgs.TaskInstance.Task.Name` pour identifier la tâche qui est lancée.

Si vous êtes en train de communiquer entre les processus en arrière-plan et au premier plan, vous pouvez supprimer ce code de communication et de gestion de l’état.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Types de déclencheur et tâches en arrière-plan qui ne peuvent pas être convertis

* Les tâches en arrière-plan intégrées au processus ne prennent pas en charge l’activation d’une tâche VoIP en arrière-plan.
* Tâches en arrière-plan dans le processus ne prennent pas en charge les déclencheurs suivants :  [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) et **IoTStartupTask**
