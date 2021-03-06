---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Découvrez comment ajouter l’ID d’application et les valeurs d’ID d’unité ad de l’espace partenaires à votre application avant de soumettre votre application au Windows Store.
title: Configurer des unités publicitaires dans votre application
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, publicités, publicité, unités publicitaires, tests
ms.localizationpriority: medium
ms.openlocfilehash: c7bafdc7d21814a03d6f7da7132d8017d7f238e5
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210265"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurer des unités publicitaires dans votre application

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Chaque contrôle de publicité dans votre application de plateforme Windows universelle (UWP) possède une *unité publicitaire* correspondante utilisée par nos services pour servir des publicités au contrôle. Chaque unité publicitaire se compose d’une *ID d’unité publicitaire* et d'une *ID d’application* que vous devez affecter au code dans votre application.

Nous fournissons des [valeurs de test d’unité publicitaire](#test-ad-units) que vous pouvez utiliser lors des tests pour vérifier que votre application affiche les tests de publicités. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Si vous essayez d’utiliser des valeurs de test dans votre application après l’avoir publiée, votre application dynamique ne recevra pas de publicités.

Une fois que vous avez terminé de tester votre application UWP et que vous êtes prêt à la soumettre à l’espace partenaires, vous devez [créer une unité ad active](#live-ad-units) à partir de la page [ADS dans l’application](../publish/in-app-ads.md) dans l’espace partenaires, puis mettre à jour votre code d’application pour utiliser les valeurs ID d’application et ID d’unité Active Directory pour cette unité Active Directory.

Pour plus d’informations sur l’attribution des valeurs d'ID d'application et d'ID d'unité publicitaire dans le code de votre application, consultez les articles suivants :
* [Classe AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Spots](../monetize/interstitial-ads.md)
* [Publicités natives](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Test des unités publicitaires

Lors du développement de votre application, utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire de cette section pour voir comment votre application restitue les publicités au cours du test.

### <a name="banner-ads-using-the-adcontrol-class"></a>Bannières publicitaires (à l’aide de la classe AdControl)

* ID d’unité Active Directory : ```test```
* ID de l’application : ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Pour un **AdControl**, la taille d’une publicité dynamique est définie par les propriétés **Width** (largeur) et **Height** (hauteur). Pour obtenir de meilleurs résultats, vérifiez que les propriétés **Width** et **Height** de votre code font partie des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md). Les propriétés **Width** et **Height** ne changent pas en fonction de la taille d’une publicité dynamique.

### <a name="interstitial-ads-and-native-ads"></a>Spots publicitaires et publicités natives

* ID d’unité Active Directory : ```test```
* ID de l’application : ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Unités publicitaires dynamiques

Pour obtenir une unité ad active de l’espace partenaires et l’utiliser dans votre application :

1.  [Créez une unité ad](../publish/in-app-ads.md#create-ad-unit) sur la page **ADS dans l’application** dans l’espace partenaires. Veillez à spécifier le type d’unité publicitaire approprié au contrôle de publicité que vous utilisez dans votre app.
    > [!NOTE]
    > Vous pouvez, si vous le souhaitez, activer la médiation publicitaire pour votre unité publicitaire en configurant ces paramètres dans la section [Paramètres de médiation](../publish/in-app-ads.md#mediation). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des publicités issues de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux publicitaires payés et les publicités des campagnes de promotion d’applications Microsoft. Par défaut, nous choisissons automatiquement les paramètres de médiation de publicité de votre application à l’aide d’algorithmes d’apprentissage machine afin de vous aider à optimiser vos revenus publicitaires sur les marchés pris en charge par votre application, mais vous pouvez éventuellement configurer manuellement vos paramètres de médiation.

2.  Une fois que vous avez créé la nouvelle unité ad, récupérez l’ID de l' **application** et l' **ID** de l’unité ad pour l’unité ad dans la table des unités ad disponibles dans la page des **publicités de** **monétis** &gt; dans l’application.
    > [!NOTE]
    > Les valeurs d'ID d’application pour les unités publicitaires de test et les unités publicitaires dynamiques UWP ont des formats différents. Les valeurs d’ID d'application tests sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

3.  Affectez les valeurs ID d’application et ID d’unité publicitaire au code de votre application. Pour plus d’informations, consultez les articles suivants :
    * [Classe AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
    * [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
    * [Spots](../monetize/interstitial-ads.md)
    * [Publicités natives](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles publicitaires dans votre application

Vous pouvez utiliser plusieurs contrôles de bannière, spot et publicité native dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous proposons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriques connexes

* [Classe AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Spots](interstitial-ads.md)
* [Publicités natives](native-ads.md)


 

 
