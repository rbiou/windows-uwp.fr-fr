---
Description: Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés.
title: Vignettes, badges et notifications
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb65acf606ffa44f075016720ebcd055ba5febc8
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234464"
---
# <a name="tiles-badges-and-notifications-for-windows-apps"></a>Vignettes, badges et notifications pour les applications Windows
 

Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés.

> **API importantes** : [Package NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Une vignette est la représentation d’une application dans le menu Démarrer. Chaque application Windows a une vignette. Vous pouvez activer différentes tailles de vignettes (petite, moyenne, large et grande).</p>

<p>Vous pouvez utiliser une <em>notification par vignette</em> pour mettre à jour la vignette afin de communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités ou l’objet du dernier message non lu.</p>

<p>Vous pouvez utiliser un <em>badge</em> pour fournir des informations d’état ou de résumé sous la forme d’un glyphe fourni par le système ou d’un nombre compris entre 1 et 99. Les badges apparaissent également sur l’icône de barre des tâches d’une application. </p>

<p>Une <em>notification toast</em> est une notification que votre application envoie à l’utilisateur via un élément d’interface utilisateur contextuel appelé <em>toast</em> (ou <em>bannière</em>). La notification est visible, que l’utilisateur se trouve dans votre application ou non.</p>
<p>Une <em>notification Push</em>, ou <em>notification brute</em>, est une notification envoyée à votre application à partir du service de notifications Push Windows (WNS) ou d’une tâche en arrière-plan. Votre application peut répondre à ces notifications soit en informant l’utilisateur qu’un événement l’intéressant s’est produit (par le biais d’une mise à jour de badge, d’une mise à jour de vignette ou d’un toast), soit de la manière de votre choix.</p>

 
## <a name="tiles"></a>Vignettes
| Article | Description |
| --- | --- |
| [Créer des vignettes](creating-tiles.md) | Personnalisez la vignette par défaut de votre application et fournissez des ressources pour différentes tailles d’écran. |
| [Ressources d’icônes de l’application](app-assets.md) | Les ressources d’icône d’application, qui s’affichent sous différentes formes dans le système d’exploitation Windows 10, sont les cartes de visite de votre application Windows. Ces recommandations précisent où apparaissent les ressources d’icône d’application dans le système et fournissent des conseils de conception détaillés pour vous aider à créer les plus belles icônes. |
| [API de vignette principale](primary-tile-apis.md) | Demandez à épingler la vignette principale de votre application, puis vérifiez que la vignette principale est actuellement épinglée. |
| [Contenu de vignette](create-adaptive-tiles.md) | Le contenu des notifications par vignette utilise les modèles de vignette adaptative, une nouvelle fonctionnalité de Windows 10 qui vous permet de concevoir votre propre contenu de notification par vignette à l’aide d’un langage de balisage simple et flexible adapté, à différentes densités d’écran. Cet article vous indique comment créer des vignettes dynamiques adaptatives pour votre application Windows. |
| [Schéma de contenu de vignette](../tiles-and-notifications/tile-schema.md) | Voici les éléments et attributs permettant de créer des vignettes adaptatives. |
| [Modèles de vignette spéciaux](special-tile-templates-catalog.md) | Les modèles de vignette spéciaux sont des modèles uniques qui sont animés, ou qui vous permettent simplement d’effectuer des opérations qui ne sont pas possibles avec des vignettes adaptatives. |
| [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md) | Découvrez comment envoyer une notification par vignette locale, en ajoutant du contenu dynamique riche à votre vignette dynamique. |


## <a name="notifications"></a>Notifications

| Article | Description |
| --- | --- |
| [Notifications toast](adaptive-interactive-toasts.md) | Les notifications toast adaptatives et interactives contextuelles et flexibles (plus de contenu, des images incluses/une interaction utilisateur facultatives). |
| [Envoyer une notification toast locale](send-local-toast.md) | Découvrez comment envoyer une notification toast interactive. |
| [Notifications Visualizer](notifications-visualizer.md) | Notifications Visualizer est une nouvelle application Windows du [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) qui permet aux développeurs de concevoir des vignettes dynamiques adaptatives pour Windows 10. |
| [Choisir une méthode de remise de notification](choosing-a-notification-delivery-method.md) | Ce article présente les quatre options de notification (locale, planifiée, périodique et Push) disponibles pour remettre des mises à jour de vignette et de badge, ainsi que du contenu de notification toast. |
| [Vue d’ensemble des notifications périodiques](periodic-notification-overview.md) | Les notifications périodiques, également appelées notifications interrogées, mettent à jour les vignettes et les badges à intervalle fixe en téléchargeant du contenu à partir d’un service cloud. |
| [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md) | Les services de notification Push Windows (WNS) permettent aux développeurs tiers d’envoyer des mises à jour de toast, de vignette et de badge, ainsi que des mises à jour brutes à partir de leur propre service cloud. Il en résulte un mécanisme fiable et optimal de remise des nouvelles mises à jour aux utilisateurs. |
| [Code généré par l’Assistant Notification Push](the-code-generated-by-the-push-notification-wizard.md) | Grâce à un Assistant Visual Studio, vous pouvez générer des notifications Push à partir d’un service mobile créé par le biais de Microsoft Azure Mobile Services. L’Assistant Visual Studio génère du code qui devrait vous aider à démarrer. Cette rubrique explique comment l’Assistant modifie votre projet, ce que le code généré fait, comment utiliser ce code et ce que vous pouvez faire ensuite pour tirer le meilleur parti des notifications Push. Consultez [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md). |
| [Vue d’ensemble des notifications brutes](raw-notification-overview.md) | Les notifications brutes sont des notifications Push courtes à usage général. Elles ont une finalité exclusivement didactique et n’incluent aucun composant d’interface utilisateur. Comme dans le cas d’autres notifications Push, la fonctionnalité WNS transmet les notifications brutes de votre service cloud à votre application. |
