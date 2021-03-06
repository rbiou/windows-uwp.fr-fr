---
description: Commencer le processus de portage en créant un nouveau projet Windows 10 dans Visual Studio et de copie de vos fichiers dans.
title: Déplacement de projets Windows Phone Silverlight pour les projets UWP
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c909517a9c48ba5088f52476373a6bb659bd6001
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372434"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>Déplacement de projets Windows Phone Silverlight pour les projets UWP


Rubrique précédente : [Mappages des espaces de noms et des classes](wpsl-to-uwp-namespace-and-class-mappings.md).

Commencer le processus de portage en créant un nouveau projet Windows 10 dans Visual Studio et de copie de vos fichiers dans.

## <a name="create-the-project-and-copy-files-to-it"></a>Création du projet et copie de fichiers dans ce dernier

1.  Lancez Microsoft Visual Studio 2015 et créez un nouveau projet Application vide (Universal Windows). Pour plus d’informations, consultez [Jumpstart votre Runtime Windows application 8.x à l’aide de modèles (C#, C++, Visual Basic)](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10)). Votre nouveau projet crée un package d’application (fichier APPX) exécutable sur toutes les familles d’appareils.
2.  Dans votre projet d’application Windows Phone Silverlight, identifiez tous les fichiers de code source et les fichiers actifs visuels que vous souhaitez réutiliser. Au moyen de l’Explorateur de fichiers, copiez les modèles de données, les modèles d’affichage, les ressources visuelles, les dictionnaires de ressources, la structure des dossiers et toute information que vous souhaitez réutiliser dans votre nouveau projet. Copiez ou créez des sous-dossiers sur le disque, si nécessaire.
3.  Copiez également les affichages (par exemple, les fichiers MainPage.xaml et MainPage.xaml.cs) dans le nœud du nouveau projet. Là encore, créez autant de sous-dossiers que nécessaire, puis supprimez les affichages existants du projet. Cependant, avant de remplacer ou de supprimer un affichage généré par Visual Studio, créez-en une copie, car vous pourrez avoir besoin de vous y référer ultérieurement. La première phase de portage d’une application Windows Phone Silverlight se concentre sur la mise en route pour rechercher affichent et fonctionnent correctement sur la famille d’un appareil. Par la suite, vous ferez en sorte que les vues s’adaptent bien à tous les facteurs de forme et aurez la possibilité d’ajouter du code adaptatif pour tirer le meilleur parti d’une famille d’appareils spécifique.
4.  Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Sélectionnez les fichiers que vous avez copiés, cliquez dessus avec le bouton droit de la souris et sélectionnez **Inclure dans le projet**. Les dossiers conteneurs sont automatiquement inclus. Vous pouvez ensuite désactiver l’option **Afficher tous les fichiers**, si vous le souhaitez. Vous pouvez également opter pour un flux de travail alternatif, qui repose sur l’utilisation de la commande **Ajouter un élément existant** après la création des sous-dossiers requis dans l’**Explorateur de solutions** de Visual Studio. Pour les ressources visuelles, vérifiez que l’option **Action de génération** est définie sur **Contenu** et que l’option **Copier dans le répertoire de sortie** est définie sur **Ne pas copier**.
5.  À ce stade, toute différence dans les noms de classe ou d’espace de noms peut entraîner un grand nombre d’erreurs de génération. Par exemple, si vous ouvrez les affichages générés par Visual Studio, vous verrez qu’ils sont de type [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), et non de type **PhoneApplicationPage**. Il existe un grand nombre de différences concernant le code impératif et le balisage XAML, que les rubriques suivantes de ce guide de portage étudient en détail. Toutefois, il vous suffit de suivre les étapes générales suivantes pour progresser rapidement : remplacez la commande « clr-namespace » par « using » dans les déclarations de préfixe d’espace de noms du balisage XML. Consultez la rubrique [Mappages des espaces de noms et des classes](wpsl-to-uwp-namespace-and-class-mappings.md) et utilisez la commande **Rechercher et remplacer** de Visual Studio pour apporter des modifications globales à votre code source (par exemple, remplacez l’élément « System.Windows » par « Windows.UI.Xaml »). Dans l’éditeur de code impératif de Visual Studio, utilisez les commandes **Résoudre** et **Organiser les instructions Using** du menu contextuel pour procéder à davantage de modifications ciblées.

## <a name="extension-sdks"></a>Kits de développement logiciel (SDK) d’extension

La plupart des API de plateforme Windows universelle (UWP) que votre application portée appellera sont implémentées dans l’ensemble d’API désigné sous le terme de famille d’appareils universelle. Toutefois, certaines d’entre elles sont implémentées dans des SDK d’extension, et Visual Studio ne reconnaît que les API implémentées par la famille d’appareils cible de votre application ou par les SDK d’extension que vous avez référencés.

Si vous obtenez des erreurs de compilation à propos d’espaces de noms, de types ou de membres introuvables, cela en est probablement la cause. Ouvrez la rubrique concernant l’API dans la documentation de référence sur les API et accédez à la section Configuration requise pour connaître la famille d’appareils d’implémentation. Si celle-ci ne correspond pas à votre famille d’appareils cible, vous avez besoin d’ajouter une référence au SDK d’extension pour cette famille d’appareils afin que l’API soit disponible pour votre projet.

Cliquez sur **projet** &gt; **ajouter une référence** &gt; **Windows universel** &gt; **Extensions** et Sélectionnez le kit SDK d’extension approprié. Par exemple, si les API que vous voulez appeler sont uniquement disponibles dans la famille d’appareils mobiles et qu’elles ont été introduites dans la version 10.0.x.y, cochez **Extensions Windows Mobile pour UWP**.

La référence suivante sera ajoutée à votre fichier de projet :

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

Le nom et le numéro de version correspondent à ceux des dossiers dans l’emplacement d’installation de votre SDK. Par exemple, les informations ci-dessus correspondent à ce nom de dossier :

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

À moins que votre application cible la famille d’appareils qui implémente l’API, vous devez utiliser la classe [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) pour vérifier la présence de l’API avant de l’appeler (on parle alors de « code adaptatif »). Cette condition sera ensuite évaluée chaque fois que votre application est exécutée, mais elle renverra la valeur « true » uniquement sur les appareils où l’API est présente et peut donc être appelée. Utilisez uniquement des SDK d’extension et du code adaptatif après avoir vérifié si une API universelle existe. Quelques exemples sont fournis dans la section ci-dessous.

Voir également [Manifeste du package de l’application](#the-app-package-manifest).

## <a name="maximizing-markup-and-code-reuse"></a>Valorisation de la réutilisation du code et du balisage

Vous constaterez qu’une légère refactorisation et/ou l’ajout de code adaptatif (expliqué ci-dessous) vous permettront d’optimiser la réutilisation du code et du balisage fonctionnant sur toutes les familles d’appareils. Voici quelques informations supplémentaires.

-   Les fichiers communs à toutes les familles d’appareils ne requièrent aucune considération particulière. Ces fichiers seront utilisés par l’application sur toutes les familles d’appareils sur lesquelles elle est exécutée. Cela inclut les fichiers de balisage XAML, les fichiers de code source impératif et les fichiers de ressources.
-   Il est possible de faire en sorte que votre application détecte la famille d’appareils sur laquelle elle est exécutée et navigue vers une vue spécialement conçue pour cette famille d’appareils. Pour plus d’informations, voir [Détection de la plateforme d’exécution de votre application](wpsl-to-uwp-input-and-sensors.md).
-   Une technique similaire qui peut s’avérer utile s’il n’existe aucune autre solution consiste à donner à un fichier de balisage ou à un fichier **ResourceDictionary** (ou au dossier contenant le fichier) un nom spécifique de manière qu’il soit chargé automatiquement à l’exécution uniquement lorsque votre application s’exécute sur une famille d’appareils spécifique. Cette technique est illustrée dans l’étude de cas [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md).
-   Pour utiliser des fonctionnalités qui ne sont pas disponibles sur toutes les familles d’appareils (imprimantes, scanneurs, bouton de l’appareil photo, etc.), vous pouvez écrire du code adaptatif. Voir le troisième exemple de la section [Compilation conditionnelle et code adaptatif](#conditional-compilation-and-adaptive-code) dans cette rubrique.
-   Si vous souhaitez prendre en charge Windows Phone Silverlight et Windows 10, vous pourrez partager des fichiers de code source entre projets. Pour ce faire, procédez comme suit : dans Visual Studio, cliquez avec le bouton droit sur le projet dans l’**Explorateur de solutions**, sélectionnez **Ajouter un élément existant**, sélectionnez les fichiers à partager, puis cliquez sur **Ajouter en tant que lien**. Stockez vos fichiers de code source dans un dossier commun du système de fichiers dans lequel les projets associés peuvent les voir, et n’oubliez pas de les ajouter au contrôle de code source. Si vous pouvez factoriser le code source impératif afin que la majorité du contenu (si ce n’est l’ensemble) d’un fichier puisse fonctionner sur les deux plateformes, vous n’avez pas besoin de disposer de deux versions de ce contenu. Vous pouvez encapsuler une logique spécifique à la plateforme dans le fichier, dans les directives de compilation conditionnelle, lorsque c’est possible, ou dans les conditions d’exécution, si nécessaire. Pour plus d’informations, voir la section ci-dessous et [Directives de préprocesseur C#](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/preprocessor-directives/index).
-   Pour une réutilisation au niveau binaire, plutôt qu’au niveau du code source, il existe des bibliothèques de classes portables, qui prennent en charge le sous-ensemble des API .NET qui sont disponibles dans Windows Phone Silverlight, ainsi que le sous-ensemble pour les applications Windows 10 (.NET Core). Les assemblies des bibliothèques de classes portables sont des fichiers binaires compatibles avec ces plateformes .NET. Utilisez Visual Studio pour créer un projet qui cible une bibliothèque de classes portable. Voir [Développement interplateforme avec la bibliothèque de classes portable](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilation conditionnelle et code adaptatif

Si vous souhaitez prendre en charge Windows Phone Silverlight et Windows 10 dans un seul fichier de code, puis vous pouvez le faire. Si vous regardez dans votre projet Windows 10 sur les pages de propriétés de projet, vous verrez que le projet définit WINDOWS\_UAP comme un symbole de compilation conditionnelle. En règle générale, vous pouvez utiliser la logique suivante pour effectuer une compilation conditionnelle.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Si vous avez le code que vous avez effectué le partage entre une application Windows Phone Silverlight et une application de 8.x Windows Runtime, puis que vous avez peut-être déjà code source avec une logique comme suit :

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

Cas dans ce, et si vous voulez prendre en charge de Windows 10 en outre, puis vous pouvez le faire, trop.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

Il est possible que vous ayez utilisé la compilation conditionnelle pour limiter la gestion du bouton matériel Précédent au Windows Phone. Dans Windows 10, l’événement de bouton précédent est un concept universels. Les boutons Précédent implémentés de manière matérielle ou logicielle déclenchent tous l’événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested), qui est donc l’élément à gérer.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

Vous pouvez avoir utilisé la compilation conditionnelle pour limiter la gestion du bouton matériel d’appareil photo au Windows Phone. Dans Windows 10, le bouton de la caméra de matériel est un concept spécifique à la famille de périphériques mobiles. Comme un même package d’application s’exécutera sur tous les appareils, nous transformons notre condition de compilation en condition d’exécution en utilisant ce que l’on appelle du code adaptatif. Pour ce faire, nous utilisons la classe [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) afin d’envoyer une requête au moment de l’exécution pour vérifier si la classe [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) est présente. **HardwareButtons** étant défini dans le SDK d’extension mobile, nous devons ajouter une référence à ce SDK dans notre projet pour permettre la compilation de ce code. Notez cependant que le gestionnaire sera uniquement exécuté sur les appareils qui implémentent les types définis dans le SDK d’extension mobile, c’est-à-dire appartenant à la famille d’appareils mobiles. Par conséquent, le code ci-après prend soin de n’utiliser que des fonctionnalités présentes, bien que la méthode utilisée pour y parvenir soit différente de la compilation conditionnelle.

```csharp
       // Note: Cache the value instead of querying it more than once.
        bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
            ("Windows.Phone.UI.Input.HardwareButtons");

        if (isHardwareButtonsAPIPresent)
        {
            Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
                this.HardwareButtons_CameraPressed;
        }

    ...

    private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
    {
        // Handle the event.
    }
```

Voir également [Détection de la plateforme d’exécution de votre application](wpsl-to-uwp-input-and-sensors.md).

## <a name="the-app-package-manifest"></a>Manifeste du package de l’application

Les paramètres de votre projet (y compris les références aux SDK d’extension) déterminent la surface d’exposition d’API que votre application peut appeler. Mais le manifeste de votre package d’application est ce qui détermine l’ensemble réel d’appareils sur lesquels vos clients peuvent installer votre application à partir du Windows Store. Pour plus d’informations, voir la section d’exemples de [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily).

Il peut s’avérer utile de savoir comment modifier le manifeste de package d’application, car les rubriques qui suivent portent sur l’utilisation de ce dernier pour diverses déclarations et fonctionnalités et d’autres paramètres requis par certaines fonctions. Vous pouvez utiliser l’éditeur de manifeste de package d’application proposé par Visual Studio pour effectuer vos modifications. Si l’**Explorateur de solutions** ne s’affiche pas, sélectionnez-le dans le menu **Affichage**. Double-cliquez sur **Package.appxmanifest**. Cela ouvre la fenêtre de l’éditeur de manifeste. Sélectionnez l’onglet approprié pour effectuer vos modifications, puis enregistrez-les. Vous pouvez vous assurer que l’élément **pm:PhoneIdentity** du manifeste de l’application portée correspond au contenu du manifeste de l’application que vous portez (pour plus d’informations, voir la rubrique [**pm:PhoneIdentity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity)).

Consultez [référence de schéma de manifeste de Package pour Windows 10](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Rubrique suivante : [Résolution des problèmes](wpsl-to-uwp-troubleshooting.md).

