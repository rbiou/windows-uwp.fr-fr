---
title: Lancement, reprise et tâches en arrière-plan
description: Cette section décrit ce qui se produit en cas de démarrage, de suspension, de reprise et d’arrêt d’une application de plateforme Windows universelle (UWP).
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp, tâche en arrière-plan, app service, appareils connectés, systèmes distants
ms.localizationpriority: medium
ms.openlocfilehash: 9280a240f35c2fdf5290c94d837e2fafc008dbfd
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80483009"
---
# <a name="launching-resuming-and-background-tasks"></a>Lancement, reprise et tâches en arrière-plan


Cette section contient des informations sur ce qui suit :

- Ce qui se produit en cas de démarrage, de suspension, de reprise ou d’arrêt d’une application de plateforme Windows universelle (UWP).
- Comment lancer des applications à l’aide d’un URI ou par activation de fichiers.
- Comment utiliser des services d’application qui permettent à votre application de plateforme Windows universelle (UWP) de partager des données et des fonctionnalités avec d’autres applications.
- Comment utiliser des tâches d’arrière-plan qui permettent à une application UWP de fonctionner même lorsqu’elle n’est pas au premier plan.
- Comment détecter les appareils connectés, lancer une application sur un autre appareil et communiquer avec un service d’application sur un appareil distant, pour créer des expériences utilisateur fluides sur plusieurs appareils.
- Comment choisir la technologie adéquate pour étendre et agencer votre application.
- Comment ajouter et configurer l’écran de démarrage de votre application.
- Comment écrire une application pour l’étendre via des packages que les utilisateurs peuvent installer à partir du Microsoft Store.

## <a name="the-app-lifecycle"></a>Cycle de vie d’une application

Cette rubrique décrit le cycle de vie d’une application de plateforme Windows universelle (UWP) Windows 10, depuis son activation jusqu’à sa fermeture.

| Rubrique | Description |
|-------|-------------|
| [Cycle de vie de l’application](app-lifecycle.md)               | Découvrez le cycle de vie d’une application UWP et ce qui se passe quand Windows lance, suspend et reprend l’exécution de votre application. |
| [Gérer le prélancement d’une application](handle-app-prelaunch.md) | Découvrez comment gérer le prélancement d’une application.                                                                              |
| [Gérer l’activation d’une application](activate-an-app.md)     | Découvrez comment gérer l’activation d’une application.                                                                             |
| [Gérer l’interruption d’une application](suspend-an-app.md)         | Apprenez à enregistrer d’importantes données d’application lorsque le système suspend votre application.                                 |
| [Gérer la reprise d’une application](resume-an-app.md)           | Apprenez à actualiser le contenu à l’écran lorsque le système reprend l’exécution de votre application.                                        |
| [Libérer de la mémoire quand l’application passe en arrière-plan](reduce-memory-usage.md) | Découvrez comment réduire la quantité de mémoire utilisée par votre application lorsqu’elle est en arrière-plan, afin qu’elle ne soit pas arrêtée.|
| [Reporter la suspension d’une application avec l’exécution étendue](run-minimized-with-extended-execution.md) | Découvrir comment utiliser l’exécution étendue pour que votre application continue de s’exécuter lorsqu’elle est en mode réduit |

## <a name="launch-apps"></a>Lancer des applications

| Rubrique | Description |
|-------|-------------|
| [Créer une application de console de plateforme Windows universelle](console-uwp.md) | Découvrez comment écrire une application de plateforme Windows universelle qui s’exécute dans une fenêtre de console. |
| [Créer une application UWP à instances multiples](multi-instance-uwp.md) | Découvrez comment écrire une application de plateforme Windows universelle à instances multiples. |

La section [Lancer une application avec un URI](launch-app-with-uri.md) explique comment utiliser un URI (Uniform Resource Identifier) pour lancer une application.

| Rubrique | Description |
|-------|-------------|
| [Lancer l’application par défaut pour un URI](launch-default-app.md) | Découvrez comment lancer l’application par défaut d’un URI (Uniform Resource Identifier). Un URI permet de lancer une autre application pour effectuer une tâche spécifique. Cette rubrique fournit également une vue d’ensemble des nombreux schémas d’URI intégrés à Windows. |
| [Gérer l’activation d’un URI](handle-uri-activation.md) | Découvrez comment inscrire une application afin qu’elle devienne le gestionnaire par défaut d’un nom de schéma d’URI (Uniform Resource Identifier). |
| [Lancer une application pour obtenir des résultats](how-to-launch-an-app-for-results.md) | Découvrez comment démarrer une application à partir d’une autre, et échanger des données entre les deux. On parle de démarrage d’une application pour afficher les résultats. |
| [Choisir et enregistrer des tonalités à l’aide du schéma d’URI ms-tonepicker](launch-ringtone-picker.md) | Cette rubrique décrit le schéma d’URI ms-tonepicker et explique comment il affiche un outil permettant de sélectionner une tonalité, d’enregistrer une tonalité et de lui attribuer un nom convivial. |
| [Lancer l’application Paramètres Windows](launch-settings-app.md) | Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques. |
| [Lancer l’application du Microsoft Store](launch-store-app.md) | Cette rubrique décrit le schéma d’URI ms-windows-store. Votre application peut utiliser ce schéma d’URI pour lancer l’application UWP en ouvrant des pages spécifiques dans le Store. |
| [Lancer l’application Cartes Windows](launch-maps-app.md) | Découvrez comment lancer l’application Cartes Windows à partir de votre application. |
| [Lancer l’application Contacts](launch-people-apps.md) | Cette rubrique décrit le schéma d’URI ms-people. Votre application peut utiliser ce schéma d’URI afin de lancer l’application Contacts pour effectuer des actions spécifiques. |
| [Prendre en charge la liaison application-site web avec les gestionnaires d’URI d’application](web-to-app-linking.md) | Rendez votre application plus attractive avec des gestionnaires d’URI d’application. |

La section [Lancer une application par l’activation de fichiers](launch-app-from-file.md) explique comment configurer le démarrage de votre application à l’ouverture d’un fichier d’un type donné.

| Rubrique | Description |
|-------|-------------|
| [Lancer l’application par défaut pour un fichier](launch-the-default-app-for-a-file.md) | Découvrez comment lancer l’application par défaut d’un fichier. |
| [Gérer l’activation des fichiers](handle-file-activation.md) | Découvrez comment inscrire votre application comme gestionnaire par défaut d’un type de fichier. |

Consultez les rubriques liées au lancement d’une application ci-dessous.

| Rubrique | Description |
|-------|-------------|
| [Poursuivre l’activité utilisateur, même sur différents appareils](useractivities.md) | Suscitez de nouveau l’intérêt des utilisateurs avec votre application, même sur différents appareils, en lançant votre application là où les utilisateurs s’étaient arrêtés. |
| [Lancement automatique avec lecture automatique](auto-launching-with-autoplay.md) | Vous pouvez utiliser la lecture automatique pour proposer votre application en tant qu’option lorsque l’utilisateur connecte un périphérique à son PC. Cela inclut les périphériques autres que les périphériques de volume, tels qu’un appareil photo ou un lecteur multimédia, ou les périphériques de volume tels qu’une clé USB, une carte mémoire SD ou un DVD. |
| [Noms de schéma d’URI et de fichier réservés](reserved-uri-scheme-names.md) | Cette rubrique répertorie les noms de schéma d’URI et de fichier réservés, indisponibles pour votre application. |

## <a name="app-services-and-extensions"></a>Services et extensions d’application

La section [Services et extensions d’application](app-services.md) explique comment intégrer des services d’application dans votre application UWP pour autoriser le partage des données et des fonctionnalités entre les applications.

| Rubrique | Description |
|-------|-------------|
| [Créer et consommer un service d’application](how-to-create-and-consume-an-app-service.md) | Découvrez comment écrire une application de plateforme Windows universelle (UWP) fournissant des services à d’autres applications UWP et comment utiliser ces derniers. |
| [Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md) | Convertissez un code de service d’application qui s’exécutait dans un processus distinct en arrière-plan en code qui s’exécute dans le même processus que votre fournisseur de service d’application. |
| [Étendre votre application avec des services, des extensions et des packages d’application](extend-your-app-with-services-extensions-packages.md) | Déterminer les technologies à utiliser pour étendre et agencer votre application, et obtenir une vue d’ensemble de chacune d’entre elles. |
| [Créer et consommer une extension d’application](how-to-create-an-extension.md) | Écrivez et hébergez des extensions d’applications UWP pour étendre votre application via des packages que les utilisateurs peuvent installer à partir du Microsoft Store. |

## <a name="background-tasks"></a>Tâches en arrière-plan

La section [Tâches en arrière-plan](support-your-app-with-background-tasks.md) vous montre comment exécuter du code léger en arrière-plan, en réponse à des déclencheurs.

| Rubrique | Description |
|-------|-------------|
| [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)                                       | Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan. |
| [Accéder à des capteurs et des appareils à partir d’une tâche en arrière-plan](access-sensors-and-devices-from-a-background-task.md)   | [**DeviceUseTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) permet à votre application Windows universelle d’accéder aux capteurs et aux appareils en arrière-plan, même si votre application d’avant-plan est interrompue. |
| [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)       | Créez et inscrivez une tâche en arrière-plan in-process qui s’exécute dans le même processus que votre application au premier plan. |
| [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)           | Créez et inscrivez une tâche en arrière-plan qui s’exécute dans un processus distinct de votre application, et inscrivez-la pour qu’elle s’exécute lorsque votre application ne se trouve pas au premier plan. |
| [Créer et inscrire une tâche COM en arrière-plan pour une application winmain](create-and-register-a-winmain-background-task.md) | Créez une tâche COM en arrière-plan qui peut s’exécuter dans votre processus principal ou hors processus lorsque votre application winmain empaquetée n’est peut-être pas en cours d’exécution. |
| [Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process](convert-out-of-process-background-task.md) | Découvrez comment porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process qui s’exécute dans le même processus que votre application d’avant-plan.|
| [Déboguer une tâche en arrière-plan](debug-a-background-task.md)                                                       | Découvrez comment déboguer une tâche en arrière-plan, notamment dans le cadre de son activation et du suivi de débogage dans le journal des événements Windows. |
| [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md) | Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application. |
| [Regrouper l’inscription de tâches en arrière-plan](group-background-tasks.md)                                             | Isolez l’inscription de tâches en arrière-plan à l’aide de groupes. |
| [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)                                 | Découvrez comment faire en sorte qu’une tâche en arrière-plan reconnaisse les demandes d’annulation et arrête le travail, tout en signalant l’annulation à l’application utilisant le dispositif de stockage persistant. |
| [Superviser la progression et l’exécution des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)       | Découvrez comment votre application peut reconnaître la progression et l’achèvement d’une tâche en arrière-plan. |
| [Optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) |Découvrez comment réduire l’énergie utilisée en arrière-plan et interagir avec les paramètres utilisateur pour l’activité en arrière-plan. |
| [Inscrire une tâche en arrière-plan](register-a-background-task.md)                                                 | Découvrez comment créer une fonction que vous pouvez réutiliser pour inscrire la plupart des tâches en arrière-plan en toute sécurité. |
| [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)         | Découvrez comment créer une tâche en arrière-plan qui répond aux événements [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType). |
| [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)                                    | Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique. |
| [Exécuter en arrière-plan indéfiniment](run-in-the-background-indefinetly.md)                                    | Utilisez une fonctionnalité pour exécuter indéfiniment une tâche en arrière-plan ou une session d’exécution étendue en arrière-plan. |
| [Déclencher une tâche en arrière-plan à partir de votre application](trigger-background-task-from-app.md) | Découvrez comment utiliser [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) pour activer une tâche en arrière-plan à partir de votre application.|
| [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)             | Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera. |
| [Transférer des données en arrière-plan](https://docs.microsoft.com/windows/uwp/networking/background-transfers)                 | Utilisez l’API de transfert en arrière-plan pour copier des fichiers en arrière-plan. |
| [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)                   | Utilisez une tâche en arrière-plan pour mettre à jour une vignette dynamique de votre application avec du contenu actualisé. |
| [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)                                                   | Découvrez comment utiliser la classe [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) pour exécuter du code léger en arrière-plan tandis que l’appareil est branché. |

## <a name="remote-systems"></a>Systèmes distants

La section [Applications et appareils connectés (projet Rome)](connected-apps-and-devices.md) explique comment utiliser la plateforme Systèmes distants pour identifier les appareils distants, lancer une application sur un appareil distant et communiquer avec un service d’application sur un appareil distant.

| Rubrique | Description |
|-------|-------------|
| [Découvrir des appareils distants](discover-remote-devices.md)  | Découvrez comment détecter les appareils auxquels vous pouvez vous connecter. |
| [Lancer une application sur un appareil distant](launch-a-remote-app.md) | Découvrez comment lancer une application sur un appareil distant.  |
| [Communiquer avec un service d’application distant](communicate-with-a-remote-app-service.md) | Découvrez comment interagir avec une application sur un appareil distant. |
| [Connecter des appareils via des sessions à distance](remote-sessions.md) | Créez des expériences partagées sur plusieurs appareils en les rejoignant dans une session à distance. |

## <a name="splash-screens"></a>Écrans de démarrage

La section [Écrans de démarrage](splash-screens.md) explique comment définir et configurer l’écran de démarrage de votre application.

| Rubrique | Description |
|-------|-------------|
| [Ajouter un écran de démarrage](add-a-splash-screen.md) | Définissez l’image et la couleur d’arrière-plan de l’écran de démarrage de votre application. |
| [Afficher un écran de démarrage plus longtemps](create-a-customized-splash-screen.md) | Affichez un écran de démarrage plus longtemps en créant et en affichant un écran de démarrage étendu pour votre application. Cet écran étendu imite l’écran de démarrage affiché quand votre application est lancée. Il peut être personnalisé. |
