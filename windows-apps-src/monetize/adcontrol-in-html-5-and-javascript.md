---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Découvrez comment utiliser la classe AdControl pour afficher des bannières publicitaires dans une application JavaScript/HTML pour Windows 10 (UWP).
title: AdControl en HTML 5 et JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, annonces publicitaires, publicité, AdControl, contrôle de publicité, javascript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: 7614265048945ddc9f1a1c32338e8446ee3ce7ba
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507183"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 et JavaScript

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pour afficher des bannières publicitaires dans une application de plateforme Windows universelle (UWP) JavaScript/HTML pour Windows 10.

Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application HTML/JavaScript, voir [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Composants requis

* Installer le [SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec Visual Studio 2015 ou une version ultérieure de Visual Studio. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Si vous avez installé le kit de développement logiciel (SDK) Windows 10 version 10.0.14393 (mise à jour anniversaire) ou une version ultérieure du SDK Windows, vous devez également installer la bibliothèque [WinJS](https://github.com/winjs/winjs) . Cette bibliothèque était incluse dans les versions précédentes du Kit de développement logiciel Windows (Kit SDK Windows) pour Windows 10, mais depuis le SDK Windows 10 version 10.0.14393 (mise à jour anniversaire), elle doit être installée séparément. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Intégrer une bannière publicitaire dans votre application

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package.appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (Client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités tests et des publicités dynamiques.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au SDK Microsoft Advertising de votre projet :

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

6.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet).

7.  Dans la section **&lt;head&gt;** , après les références JavaScript des fichiers default.css et main.js du projet, ajoutez la référence à ad.js.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Vous devez placer cette ligne dans la section **&lt;head&gt;** après avoir inclus main.js. Sinon, vous risquez de rencontrer une erreur lors de la création de votre projet.

8.  Modifiez la section **&lt;body&gt;** dans le fichier default.html (ou un autre fichier html, selon votre projet) pour inclure l’élément **div** correspondant à la classe **AdControl**. Affectez les propriétés **applicationId** et **adUnitId** de la classe **AdControl** aux valeurs de test indiquées dans [Valeurs de l'unité publicitaire test](set-up-ad-units-in-your-app.md#test-ad-units). Ajustez également la **height** (hauteur) et la **width** (largeur) de la commande pour qu’elle corresponde à l’une des [tailles de bannière publicitaire prises en charge](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Chaque **AdControl** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compilez et exécutez l’application pour la voir avec une publicité.

L’exemple suivant montre le fichier index.html complet pour une application simple.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Créez un contrôle AdControl par programmation en JavaScript.

Les étapes précédentes montrent comment déclarer un **AdControl** dans votre balisage HTML. Vous pouvez aussi créer par programmation un contrôle **AdControl** en JavaScript. Cet exemple part du principe que vous utilisez un élément **div** existant dans votre code HTML avec l’ID **MyAd**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

Cet exemple suppose que vous avez déjà déclaré les méthodes de gestionnaires d’événements **myAdError**, **myAdRefreshed** et **myAdEngagedChanged**.

Si vous utilisez ce code et que vous ne voyez pas de publicités, vous pouvez essayer d’insérer un attribut de **position:relative** dans l’élément **div** qui contient le **AdControl**. Cela remplace le paramètre par défaut de l’élément **IFrame**. Les publicités apparaissent correctement, sauf si elles ne sont pas affichées en raison de la valeur de cet attribut. Notez que les nouvelles unités publicitaires peuvent ne pas être disponibles pendant 30 minutes.

> [!NOTE]
> Les valeurs *applicationId* et *adUnitId* indiquées dans cet exemple sont des [valeurs du mode test](set-up-ad-units-in-your-app.md#test-ad-units). Vous devez [remplacer ces valeurs par des valeurs dynamiques](set-up-ad-units-in-your-app.md#live-ad-units) de l’espace partenaires avant de soumettre votre application pour envoi.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités dynamiques

1. Assurez-vous que votre utilisation des bannières publicitaires dans votre app respecte nos [recommandations en matière de bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

1.  Dans l’espace partenaires, accédez à la page [annonces dans l’application](../publish/in-app-ads.md) et [créez une unité ad](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d'ID d’application pour les unités publicitaires de test et les unités publicitaires dynamiques UWP ont des formats différents. Les valeurs d’ID d'application tests sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

2. Vous pouvez, si vous le souhaitez, activer la médiation publicitaire pour **AdControl** en configurant ces paramètres dans la section [Paramètres de médiation](../publish/in-app-ads.md#mediation) de la page [Publicités dans l'app](../publish/in-app-ads.md). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft.

3.  Dans votre code, remplacez les valeurs de l’unité ad du test (**ApplicationID** et **adUnitId**) par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

4.  [Soumettez votre application](../publish/app-submissions.md) au magasin à l’aide de l’espace partenaires.

5.  Passez en revue les [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles publicitaires dans votre application

Vous pouvez utiliser plusieurs objets **AdControl** dans une seule application (par exemple, chaque page de votre application peut héberger un objet **AdControl** différent). Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous proposons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriques connexes

* [Instructions pour les bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurer des unités AD pour votre application](set-up-ad-units-in-your-app.md)
* [Gestion des erreurs dans la procédure pas à pas JavaScript](error-handling-in-javascript-walkthrough.md)
