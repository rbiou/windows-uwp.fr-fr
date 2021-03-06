---
description: Le code qui s’intègre à l’appareil proprement dit et à ses capteurs implique des entrées de l’utilisateur et des sorties vers ce dernier.
title: Portage d’une application Windows Runtime 8.x vers UWP pour le modèle d’E/S, d’appareil et d’application
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d30554c22fbd40a555f51b25011b128072a16165
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372230"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>Portage d’une application Windows Runtime 8.x vers UWP pour le modèle d’E/S, d’appareil et d’application




Rubrique précédente : [Portage du balisage XAML et de la couche interface utilisateur](w8x-to-uwp-porting-xaml-and-ui.md).

Le code qui s’intègre à l’appareil proprement dit et à ses capteurs implique des entrées de l’utilisateur et des sorties vers ce dernier. Il peut également impliquer le traitement des données. Néanmoins, ce code n’est généralement pas pensé comme la couche interface utilisateur *ni* comme la couche de données. Ce code inclut l’intégration au contrôleur de vibrations, à l’accéléromètre, au gyroscope, au microphone et au haut-parleur (qui rejoignent la reconnaissance et la synthèse vocales), à la (géo)localisation et aux modalités d’entrée telles que l’écran tactile, la souris, le clavier et le stylet.

## <a name="application-lifecycle-process-lifetime-management"></a>Cycle de vie des applications (gestion de la durée de vie des processus)


Dans le cas d’une application 8.1 universelle, il existe une « fenêtre de réponse » de deux secondes entre le moment où l’application devient inactive et le moment où le système déclenche l’événement de suspension. L’utilisation de cette fenêtre de réponse comme délai supplémentaire pour suspendre l’état est peu sûre. Par ailleurs, dans le cas d’une application pour la plateforme Windows universelle (UWP), il n’existe pas de telle fenêtre ; l’événement de suspension est déclenché dès que l’application devient inactive.

Pour plus d’informations, voir [Cycle de vie de l’application](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle).

## <a name="background-audio"></a>Contenu audio en arrière-plan


Pour le [ **MediaElement.AudioCategory** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.audiocategory) propriété, **ForegroundOnlyMedia** et **BackgroundCapableMedia** sont déconseillées pour Applications Windows 10. Utilisez plutôt le modèle d’application du Windows Phone Store. Pour plus d’informations, voir [Contenu audio en arrière-plan](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio).

## <a name="detecting-the-platform-your-app-is-running-on"></a>Détection de la plateforme d’exécution de votre application


La façon de penser ciblage d’application des modifications avec Windows 10. Selon le nouveau modèle conceptuel, une application cible la plateforme Windows universelle (UWP) et s’exécute sur tous les appareils Windows. Elle peut ensuite choisir d’activer des fonctionnalités exclusives à certaines familles d’appareils. Si nécessaire, l’application a également la possibilité de restreindre son ciblage à une ou plusieurs familles d’appareils spécifiques. Pour plus d’informations sur les familles d’appareils et savoir comment déterminer les familles d’appareils à cibler, voir le [Guide des applications UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Si votre application 8.1 universelle intègre du code qui détecte le système d’exploitation sur lequel elle est exécutée, vous devrez peut-être changer cela en fonction de la raison de la logique. Si l’application transmet la valeur et n’intervient pas en conséquence, il peut être préférable de continuer à collecter les informations sur le système d’exploitation.

**Remarque**    nous recommandons de n'utiliser pas système d’exploitation ou la famille de périphériques pour détecter la présence de fonctionnalités. En règle générale, l’identification de la famille d’appareils ou du système d’exploitation actuel ne constitue pas le meilleur moyen de déterminer si une fonctionnalité particulière du système d’exploitation ou de la famille d’appareils est présente. Plutôt que de détecter le système d’exploitation ou la famille d’appareils (et le numéro de version), vérifiez directement la présence de la fonctionnalité à l’aide d’un test (voir [Compilation conditionnelle et code adaptatif](w8x-to-uwp-porting-to-a-uwp-project.md)). Si vous devez exiger un système d’exploitation ou une famille d’appareils spécifique, veillez à l’utiliser comme une version minimale prise en charge plutôt que de concevoir le test pour cette version particulière.

 

Pour adapter l’interface utilisateur à différents appareils, plusieurs techniques sont recommandées. Continuez à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans le balisage XAML, continuez à utiliser les tailles en pixels effectifs (auparavant appelés « pixels d’affichage ») afin que votre interface utilisateur s’adapte à différentes résolutions et différents facteurs d’échelle (voir [Pixels effectifs, distance d’affichage et facteurs d’échelle](w8x-to-uwp-porting-xaml-and-ui.md)). Utilisez également les déclencheurs adaptatifs et les méthodes setter du Gestionnaire d’état visuel pour adapter votre interface utilisateur à la taille de la fenêtre (voir le [Guide des applications UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)).

Toutefois, si l’un de vos scénarios implique nécessairement la détection de la famille d’appareils, vous pouvez mettre celle-ci en œuvre. Dans cet exemple, nous faisons appel à la classe [**AnalyticsVersionInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) pour naviguer vers une page adaptée à la famille d’appareils mobiles lorsqu’un tel appareil est utilisé, en veillant à revenir vers une page par défaut dans le cas contraire.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Votre application peut également déterminer la famille d’appareils sur laquelle elle est exécutée à partir des facteurs de sélection de ressources en vigueur. L’exemple ci-dessous indique comment effectuer cette opération de manière impérative, et la rubrique [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) décrit le cas d’utilisation le plus courant de la classe pour le chargement de ressources propres à la famille d’appareils en fonction du facteur de cette dernière.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Voir également [Compilation conditionnelle et code adaptatif](w8x-to-uwp-porting-to-a-uwp-project.md).

## <a name="location"></a>Location


Lorsqu’une application qui déclare la fonctionnalité de localisation dans son application package manifeste s’exécute sur Windows 10, le système vous invite l’utilisateur final à donner son consentement. Cela est vrai si l’application est une application Windows Phone Store ou une application Windows 10. Si votre application affiche sa propre invite de consentement personnalisée ou qu’elle fournit une bascule de type activation/désactivation, vous devrez donc supprimer ces éléments pour que l’utilisateur final ne soit invité qu’une seule fois à autoriser cette fonctionnalité.

 

 




