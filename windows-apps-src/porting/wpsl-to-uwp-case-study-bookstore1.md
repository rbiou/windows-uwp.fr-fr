---
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: Cette rubrique présente une étude de cas concernant le portage d’une application Silverlight très simple Windows Phone vers une application Windows 10 plateforme Windows universelle (UWP).
title: Windows Phone l’étude de cas Silverlight pour UWP, Bookstore1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1079d51aa0b013cd40ff585e5baabb61f940a745
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260095"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>Windows Phone l’étude de cas Silverlight-UWP : Bookstore1


Cette rubrique présente une étude de cas concernant le portage d’une application Silverlight très simple Windows Phone vers une application Windows 10 plateforme Windows universelle (UWP). Avec Windows 10, vous pouvez créer un package d’application unique que vos clients peuvent installer sur un large éventail d’appareils, et c’est ce que nous allons faire dans cette étude de cas. Voir [Guide des applications UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

L’application que nous porterons se compose d’une classe **ListBox** liée à un modèle d’affichage. Ce modèle comporte une liste de livres qui indique leur titre, leur auteur et leur couverture. Les images de couverture de livre possèdent l’attribut **Action de génération** défini sur **Contenu** et l’attribut **Copier dans le répertoire de sortie** défini sur **Ne pas copier**.

Les rubriques précédentes de cette section décrivent les différences entre les plateformes et fournissent des détails et des recommandations sur le processus de portage des différents aspects d’une application dans le balisage XAML, de la liaison à un modèle d’affichage à l’accès aux données. Une étude de cas vise à compléter ces recommandations en les appliquant à un exemple concret. Elle part du principe que vous avez lu les recommandations, qui ne sont donc pas répétées.

**Notez**   lors de l’ouverture de Bookstore1Universal\_10 dans Visual Studio, si vous voyez le message « mise à jour requise de Visual Studio », suivez les étapes de sélection d’un contrôle de version de la plateforme cible dans [TargetPlatformVersion](wpsl-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Téléchargements

[Téléchargez l’application Bookstore1WPSL8 Windows Phone Silverlight](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1WPSL8).

[Téléchargez l’application Bookstore1Universal\_10 Windows 10](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-windowsphone-silverlight-app"></a>L’application Windows Phone Silverlight

Voici à quoi ressemble Bookstore1WPSL8, l’application que nous allons porter. Il s’agit simplement d’une zone de liste à défilement vertical répertoriant des livres au-dessous de l’en-tête constitué du nom de l’application et du titre de la page.

![Apparence de l’application Bookstore1WPSL8](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>Portage vers un projet Windows 10

La procédure consistant à créer un projet dans Visual Studio, puis à copier des fichiers de Bookstore1WPSL8 dans ce nouveau projet, est très rapide. Commencez par créer un projet Application vide (universelle Windows). Nommez-le Bookstore1Universal\_10. Il s’agit des fichiers à copier à partir de Bookstore1WPSL8 vers Bookstore1Universal\_10.

-   Copiez le dossier contenant les fichiers PNG de l’image de couverture du livre (le dossier est \\ressources\\CoverImages). Une fois le dossier copié, dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit de la souris sur le dossier que vous avez copié et sélectionnez **Inclure dans le projet**. Cette commande correspond à ce que nous avons voulu dire par « insertion » de fichiers ou de dossiers dans un projet. Chaque fois que vous copiez un fichier ou un dossier, sélectionnez **Actualiser** dans l’**Explorateur de solutions**, puis incluez le fichier ou dossier dans le projet. Cette opération n’est pas nécessaire pour les fichiers dont vous modifiez la destination.
-   Copiez le dossier contenant le fichier source du modèle d’affichage (le dossier est \\ViewModel).
-   Copiez le fichier MainPage.xaml et remplacez le fichier dans la destination.

Nous pouvons conserver l’app. xaml et App.xaml.cs que Visual Studio a généré pour nous dans le projet Windows 10.

Modifiez le code source et les fichiers de balisage que vous venez de copier, puis remplacez toutes les références à l’espace de noms Bookstore1WPSL8 par Bookstore1Universal\_10. Une méthode rapide consiste à utiliser la fonctionnalité **Remplacer dans les fichiers**. Dans le code impératif du fichier source du modèle d’affichage, apportez les modifications de portage suivantes :

-   Remplacez `System.ComponentModel.DesignerProperties` par `DesignMode`, puis appliquez-lui la commande **Résoudre**. Supprimez la propriété `IsInDesignTool` et utilisez IntelliSense pour ajouter le nom de propriété correct : `DesignModeEnabled`.
-   Utilisez la commande **Résoudre** sur `ImageSource`.
-   Utilisez la commande **Résoudre** sur `BitmapImage`.
-   Supprimez using `System.Windows.Media;` et `using System.Windows.Media.Imaging;`.
-   Modifiez la valeur retournée par la propriété **Bookstore1Universal\_10. BookstoreViewModel. AppName** de « BOOKSTORE1WPSL8 » à « Bookstore1Universal ».

Dans le fichier MainPage.xaml, apportez les modifications de portage suivantes :

-   Remplacez l’élément `phone:PhoneApplicationPage` par `Page` (en incluant les occurrences figurant dans la syntaxe des éléments de propriété).
-   Supprimez les déclarations de préfixe d’espace de noms `phone` et `shell`.
-   Remplacez l’élément « clr-namespace » par « using » dans la déclaration de préfixe d’espace de noms restante.

Nous pouvons choisir de corriger les erreurs de compilation de balisage très facilement si nous voulons afficher les résultats le plus tôt possible, même si cela implique la suppression temporaire du balisage. Conservons néanmoins un enregistrement des éléments supprimés en procédant ainsi. Voici ce dont il s’agit dans notre cas.

1.  Dans l’élément **Page** racine du fichier **MainPage.xaml**, supprimez `SupportedOrientations="Portrait"`.
2.  Dans l’élément **Page** racine du fichier **MainPage.xaml**, supprimez `Orientation="Portrait"`.
3.  Dans l’élément **Page** racine du fichier **MainPage.xaml**, supprimez `shell:SystemTray.IsVisible="True"`.
4.  Dans le modèle de données `BookTemplate`, supprimez les références aux styles `PhoneTextExtraLargeStyle` et `PhoneTextSubtleStyle` **TextBlock**.
5.  Dans le modèle de données `TitlePanel` **StackPanel**, supprimez les références aux styles `PhoneTextNormalStyle` et `PhoneTextTitle1Style` **TextBlock**.

Commençons par travailler sur l’interface utilisateur de la famille d’appareils mobiles, après quoi nous pourrons envisager les autres facteurs de forme. Vous pouvez à présent générer et exécuter l’application. Voici comment cette dernière apparaît sur l’émulateur d’appareil mobile.

![Application UWP sur un appareil mobile avec les modifications du code source initial](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

L’association de l’affichage et du modèle d’affichage fonctionne correctement, tout comme la classe **ListBox**. Il nous reste principalement à corriger les styles et à faire en sorte que les images s’affichent.

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>Récupération des éléments supprimés et stylisation initiale

Par défaut, toutes les orientations sont prises en charge. Toutefois, l’application Windows Phone Silverlight se limite explicitement au mode portrait uniquement. par conséquent, les Articles de dette \#1 et \#2 sont remboursés en accédant au manifeste du package d’application dans le nouveau projet et en vérifiant le **portrait** sous **orientations prises en charge**.

Pour cette application, l’élément \#3 n’est pas une dette, car la barre d’État (anciennement appelée « barre d’état système ») est affichée par défaut. Pour les éléments \#4 et \#5, nous devons Rechercher quatre styles **TextBlock** plateforme Windows universelle (UWP) qui correspondent aux styles Windows Phone Silverlight que nous utilisons. Vous pouvez exécuter l’application Windows Phone Silverlight dans l’émulateur et la comparer côte à côte avec l’illustration dans la section de [texte](wpsl-to-uwp-porting-xaml-and-ui.md) . À partir de cela, et de la consultation des propriétés des styles de système Windows Phone Silverlight, nous pouvons créer cette table.

| Clé de style Silverlight pour Windows Phone | Clé de style UWP          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
Pour définir ces styles, il vous suffit de les taper dans l’éditeur de balisage. Vous pouvez également utiliser les outils XAML de Visual Studio et les définir sans rien taper. Pour ce faire, cliquez avec le bouton droit sur un **TextBlock** , puis cliquez sur **modifier le style** &gt; **appliquer la ressource**. Pour ce faire, avec les **TextBlocks**dans le modèle d’élément, cliquez avec le bouton droit sur la **zone de liste** , puis cliquez sur modifier les **modèles supplémentaires** &gt; **modifier les éléments générés (ItemTemplate)** .

Il existe un arrière-plan blanc opaque à 80 % derrière les éléments, car le style par défaut du contrôle **ListBox** définit son arrière-plan sur la ressource système `ListBoxBackgroundThemeBrush`. Définissez `Background="Transparent"` sur l’élément **ListBox** pour effacer cet arrière-plan. Pour aligner à gauche les objets **TextBlock** dans le modèle d’élément, modifiez-les de nouveau comme décrit ci-dessus et définissez un élément **Margin** de `"9.6,0"` dans les deux objets **TextBlock**.

Une fois cette opération effectuée, en raison des [modifications associées aux pixels d’affichage](wpsl-to-uwp-porting-xaml-and-ui.md), nous devons passer en revue et multiplier toutes les dimensions de taille fixe que nous n’avons pas encore modifiées (marges, largeur, hauteur, etc.) par 0,8. Par exemple, les images doivent passer de 70 x 70 px à 56 x 56 px.

Mais récupérons ces images pour le rendu avant de montrer les résultats de notre stylisation.

## <a name="binding-an-image-to-a-view-model"></a>Liaison d’une propriété Image à un modèle d’affichage

Dans Bookstore1WPSL8, nous avons fait cela :

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Dans Bookstore1Universal, nous utilisons le [schéma d’URI](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)) ms-appx. Pour laisser le reste de notre code inchangé, nous pouvons utiliser une surcharge différente du constructeur **System.Uri** pour placer le schéma d’URI dans un URI de base et y ajouter le reste du chemin d’accès. Comme ceci :

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>Stylisation universelle

À présent, il ne nous reste plus qu’à apporter quelques adaptations de stylisation finales et à vérifier que l’application s’affiche correctement sur les facteurs de forme des appareils de bureau (et autres), ainsi que sur les appareils mobiles. Les étapes sont les suivantes. Et vous pouvez utiliser les liens en haut de cette rubrique pour télécharger les projets et afficher les résultats de toutes les modifications entre ce stade et la fin de l’étude de cas.

-   Pour réduire l’espacement entre les éléments, recherchez le modèle de données `BookTemplate` dans le fichier MainPage.xaml et supprimez l’attribut `Margin` de l’élément **Grid** racine.
-   Si vous souhaitez aérer légèrement le titre de la page, vous pouvez réduire la marge inférieure de la valeur `-5.6` à la valeur `0` sur l’élément **TextBlock** du titre de la page.
-   À présent, nous devons définir l’arrière-plan de `LayoutRoot` sur la valeur par défaut appropriée afin que l’application se présente correctement lorsqu’elle est exécutée sur tous les appareils, quel que soit le thème. Remplacez sa valeur `"Transparent"` par la valeur `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.

Dans le cas d’une application plus élaborée, le moment serait bien choisi pour utiliser les recommandations de l’article [Portage d’une application Silverlight pour Windows Phone vers UWP pour différents facteurs de forme et expériences utilisateur](wpsl-to-uwp-form-factors-and-ux.md) et pour optimiser l’utilisation du facteur de forme de chacun des nombreux appareils sur lesquels l’application peut désormais s’exécuter. Mais, dans le cas de cette application simple, nous pouvons nous arrêter là et examiner l’apparence de l’application après cette dernière séquence d’opérations de stylisation. L’application présente le même aspect sur les appareils mobiles que sur les appareils de bureau, bien qu’elle ne tire pas le meilleur parti de l’espace sur les facteurs de forme larges (mais nous étudierons comment rectifier cet aspect dans une étude de cas ultérieure).

Pour connaître la procédure à suivre pour contrôler le thème de votre application, voir [Modifications de thème](wpsl-to-uwp-porting-xaml-and-ui.md).

![Application Windows 10 portée](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil mobile

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>Ajustement facultatif de la zone de liste pour les appareils mobiles

Lorsque l’application s’exécute sur un appareil mobile, l’arrière-plan d’une zone de liste est clair par défaut dans les deux thèmes. S’il s’agit de votre thème de prédilection, vous n’avez rien d’autre à faire. Toutefois, les contrôles sont conçus pour que vous puissiez personnaliser leur apparence tout en préservant leur comportement. Donc, si vous souhaitez que la zone de liste soit sombre dans le thème foncé (apparence de l’application d’origine), suivez [ces instructions](w8x-to-uwp-case-study-bookstore1.md) sous « Ajustement facultatif ».

## <a name="conclusion"></a>Conclusion

Cette étude de cas vous a décrit le processus de portage d’une application très simple, probablement non réaliste. Par exemple, il est possible d’utiliser des contrôles de liste pour la sélection ou pour la création d’un contexte de navigation. L’application accède alors à une page contenant plus de détails sur l’élément sélectionné. Cette application particulière ne fait rien avec la sélection de l’utilisateur et est dépourvue de navigation. Malgré tout, l’étude de cas a servi à briser la glace, afin d’introduire le processus de portage et d’illustrer les techniques importantes que vous pouvez utiliser dans les applications UWP réelles.

Dans l’étude de cas suivante, [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md), nous examinons l’accès aux données groupées et l’affichage de ces dernières.
