---
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Découvrez comment utiliser la classe AdControl pour afficher des bannières publicitaires dans une application XAML pour Windows 10 (UWP).
title: AdControl en XAML et .NET
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, uwp, annonces, publicité, AdControl, contrôle de publicité, XAML, .net, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: 7472343dcff4ac7f80d3bcea917e38849f513be6
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507203"
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl en XAML et .NET

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pour afficher des bannières publicitaires dans une application de plateforme Windows universelle (UWP) XAML pour Windows 10 implémentées avec C#.

> [!NOTE]
> Le Kit de développement Microsoft Advertising prend également en charge les applications XAML qui sont implémentées à l’aide de C++. Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Composants requis

* Installer le [SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec Visual Studio 2015 ou une version ultérieure de Visual Studio. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Intégrer une bannière publicitaire dans votre application

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package.appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (Client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités tests et des publicités dynamiques.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au SDK Microsoft Advertising de votre projet :

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

4.  Modifiez le code XAML de la page où vous incorporez des publicités pour inclure l’espace de noms **Microsoft.Advertising.WinRT.UI**. Par exemple, dans l’exemple d’application par défaut généré par Visual Studio (nommé, dans cette application, MyAdFundedWindows10AppXAML), la page XAML est **MainPage.XAML**.

    La section **Page** du fichier MainPage.xaml généré par Visual Studio contient le code suivant.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    Ajoutez la référence d’espace de noms **Microsoft.Advertising.WinRT.UI** pour que la section **Page** du fichier MainPage.xaml contienne le code suivant.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. Dans la balise **Grid**, ajoutez le code correspondant à la classe **AdControl**. Affectez les propriétés [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) et [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) aux [valeurs de l'unité publicitaire test](set-up-ad-units-in-your-app.md#test-ad-units). Ajustez également les valeurs **Height** (hauteur) et **Width** (largeur) de la commande pour qu’elle corresponde à l’une des [tailles de publicités prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Chaque **AdControl** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    La balise **Grid** complète ressemble à ce code.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    Le code complet du fichier MainPage.xaml doit ressembler à ceci.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Compilez et exécutez l’application pour la voir avec une publicité.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités dynamiques

1. Assurez-vous que votre utilisation des bannières publicitaires dans votre app respecte nos [recommandations en matière de bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

2.  Dans l’espace partenaires, accédez à la page [annonces dans l’application](../publish/in-app-ads.md) et [créez une unité ad](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d'ID d’application pour les unités publicitaires de test et les unités publicitaires dynamiques UWP ont des formats différents. Les valeurs d’ID d'application tests sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

3. Vous pouvez, si vous le souhaitez, activer la médiation publicitaire pour **AdControl** en configurant ces paramètres dans la section [Paramètres de médiation](../publish/in-app-ads.md#mediation) de la page [Publicités dans l'app](../publish/in-app-ads.md). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft.

4.  Dans votre code, remplacez les valeurs de l’unité ad du test (**ApplicationID** et **AdUnitId**) par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

5.  [Soumettez votre application](../publish/app-submissions.md) au magasin à l’aide de l’espace partenaires.

6.  Passez en revue les [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles publicitaires dans votre application

Vous pouvez utiliser plusieurs objets **AdControl** dans une seule application (par exemple, chaque page de votre application peut héberger un objet **AdControl** différent). Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous proposons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriques connexes

* [Instructions pour les bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Procédure pas à pas pour gérer les erreurs dans XAML/C#](error-handling-in-xamlc-walkthrough.md).
* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurer des unités AD pour votre application](set-up-ad-units-in-your-app.md)
