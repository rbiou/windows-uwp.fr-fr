---
title: 'Étude de cas de portage d’application Windows Runtime 8.x vers UWP : Bookstore1'
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: Cette rubrique présente une étude de cas concernant le portage d’une application 8,1 universelle très simple vers une application Windows 10 plateforme Windows universelle (UWP).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecbc4d3fcdff9d06469e7a274fd56ef453a81e02
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260129"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Étude de cas de portage d’application Windows Runtime 8.x vers UWP : Bookstore1


Cette rubrique présente une étude de cas concernant le portage d’une application 8,1 universelle très simple vers une application Windows 10 plateforme Windows universelle (UWP). Une application 8,1 universelle est une application qui génère un package d’application pour Windows 8.1 et un package d’application différent pour Windows Phone 8,1. Avec Windows 10, vous pouvez créer un package d’application unique que vos clients peuvent installer sur un large éventail d’appareils, et c’est ce que nous allons faire dans cette étude de cas. Voir [Guide des applications UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

L’application que nous porterons se compose d’une classe **ListBox** liée à un modèle d’affichage. Ce modèle comporte une liste de livres qui indique leur titre, leur auteur et leur couverture. Les images de couverture de livre possèdent l’attribut **Action de génération** défini sur **Contenu** et l’attribut **Copier dans le répertoire de sortie** défini sur **Ne pas copier**.

Les rubriques précédentes de cette section décrivent les différences entre les plateformes et fournissent des détails et des recommandations sur le processus de portage des différents aspects d’une application dans le balisage XAML, de la liaison à un modèle d’affichage à l’accès aux données. Une étude de cas vise à compléter ces recommandations en les appliquant à un exemple concret. Elle part du principe que vous avez lu les recommandations, qui ne sont donc pas répétées.

**Notez**   lors de l’ouverture de Bookstore1Universal\_10 dans Visual Studio, si vous voyez le message « mise à jour requise de Visual Studio », suivez les étapes décrites dans [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Téléchargements

[Téléchargez l’application 8,1 universelle Bookstore1\_81](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1_81).

[Téléchargez l’application Bookstore1Universal\_10 Windows 10](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-universal-81-app"></a>Application 8.1 universelle

Voici à quoi ressemble Bookstore1\_81 — l’application que nous allons porter. Il s’agit simplement d’une zone de liste à défilement vertical répertoriant des livres au-dessous de l’en-tête constitué du nom de l’application et du titre de la page.

![apparence de bookstore1\-81 sur Windows](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Bookstore1\_81 sur Windows

![Comment bookstore1\-81 regarde sur Windows Phone](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Bookstore1\_81 sur Windows Phone

##  <a name="porting-to-a-windows10-project"></a>Portage vers un projet Windows 10

La solution Bookstore1\_81 est un projet d’application universelle 8,1 et contient ces projets.

-   Bookstore1\_81. Windows. Il s’agit du projet qui génère le package d’application pour Windows 8.1.
-   Bookstore1\_81. WindowsPhone. Il s’agit du projet qui crée le package d’application pour Windows Phone 8.1.
-   Bookstore1\_81. Shared. Il s’agit du projet qui contient le code source, les fichiers de balisage et d’autres actifs et ressources qui sont utilisés par les deux autres projets.

Pour cette étude de cas, nous disposons des options habituelles décrites dans la section [Si vous disposez d’une application 8.1 universelle](w8x-to-uwp-root.md), relative aux appareils à prendre en charge. La décision ici est un simple : cette application a les mêmes fonctionnalités, et pour l’essentiel avec le même code, à la fois dans ses Windows 8.1 et Windows Phone les formulaires 8,1. Par conséquent, nous allons déplacer le contenu du projet partagé (et tout ce dont nous avons besoin à partir des autres projets) vers un Windows 10 qui cible la famille d’appareils universels (l’un d’entre eux peut être installé sur la plus large gamme d’appareils).

Il s’agit d’une tâche très rapide pour créer un nouveau projet dans Visual Studio, y copier des fichiers à partir de Bookstore1\_81 et inclure les fichiers copiés dans le nouveau projet. Commencez par créer un projet Application vide (universelle Windows). Nommez-le Bookstore1Universal\_10. Il s’agit des fichiers à copier à partir de Bookstore1\_81 vers Bookstore1Universal\_10.

**À partir du projet partagé**

-   Copiez le dossier contenant les fichiers PNG de l’image de couverture du livre (le dossier est \\ressources\\CoverImages). Une fois le dossier copié, dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit de la souris sur le dossier que vous avez copié et sélectionnez **Inclure dans le projet**. Cette commande correspond à ce que nous avons voulu dire par « insertion » de fichiers ou de dossiers dans un projet. Chaque fois que vous copiez un fichier ou un dossier, pour chaque copie, sélectionnez **Actualiser** dans **Explorateur de solutions**, puis incluez le fichier ou dossier dans le projet. Cette opération n’est pas nécessaire pour les fichiers dont vous modifiez la destination.
-   Copiez le dossier contenant le fichier source du modèle d’affichage (le dossier est \\ViewModel).
-   Copiez le fichier MainPage.xaml et remplacez le fichier dans la destination.

**À partir du projet Windows**

-   Copiez le fichier BookstoreStyles.xaml. Nous allons l’utiliser comme point de départ, car toutes les clés de ressources de ce fichier seront résolues dans une application Windows 10 ; certains d’entre eux dans le fichier WindowsPhone équivalent ne le seront pas.

Modifiez le code source et les fichiers de balisage que vous venez de copier, puis remplacez les références à l’espace de noms Bookstore1\_81 par Bookstore1Universal\_10. Une méthode rapide consiste à utiliser la fonctionnalité **Remplacer dans les fichiers**. Aucune modification du code n’est nécessaire dans le modèle d’affichage, ni dans tout autre code impératif. Mais, juste pour faciliter la visibilité de la version de l’application en cours d’exécution, remplacez la valeur retournée par la propriété **Bookstore1Universal\_10. BookstoreViewModel. AppName** « BOOKSTORE1\_81 » par « Bookstore1Universal\_10 ».

Vous pouvez maintenant générer l’application et l’exécuter. Voici à quoi ressemble notre nouvelle application UWP après avoir effectué aucun travail explicite pour la porter vers Windows 10.

![Application Windows 10 avec le code source initial modifié](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

L’application Windows 10 avec les modifications de code source initiales s’exécutant sur un appareil de bureau

![Application Windows 10 avec le code source initial modifié](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

L’application Windows 10 avec les modifications de code source initiales s’exécutant sur un appareil mobile

L’association de l’affichage et du modèle d’affichage fonctionne correctement, tout comme la classe **ListBox**. Nous devons simplement corriger le style. Sur un appareil mobile dans le thème à dominante claire, nous pouvons voir la bordure de la zone de liste, mais qui sera facile à masquer. La typographie est également trop grande. Nous modifierons donc les styles utilisés. En outre, l’application doit être de couleur claire lorsqu’elle est exécutée sur un ordinateur de bureau si nous voulons qu’elle ressemble à la valeur par défaut. Par conséquent, nous la modifierons.

## <a name="universal-styling"></a>Stylisation universelle

L’application Bookstore1\_81 a utilisé deux dictionnaires de ressources différents (BookstoreStyles. Xaml) pour adapter ses styles aux systèmes d’exploitation Windows 8.1 et Windows Phone 8,1. Aucun de ces deux fichiers BookstoreStyles. XAML ne contient exactement les styles dont nous avons besoin pour notre application Windows 10. Or, la bonne nouvelle est que nous voulons un mécanisme beaucoup plus simple que l’un ou l’autre de ces fichiers. Par conséquent, les étapes suivantes impliquent principalement la suppression et la simplification de nos fichiers de projet et du balisage. Les étapes sont les suivantes. Et vous pouvez utiliser les liens en haut de cette rubrique pour télécharger les projets et afficher les résultats de toutes les modifications entre ce stade et la fin de l’étude de cas.

-   Pour bien faire la différence entre les éléments, recherchez le modèle de données `BookTemplate` dans le fichier MainPage.xaml et supprimez la chaîne `Margin="0,0,0,8"` de l’élément **Grid** racine.
-   Également dans `BookTemplate`, il existe des références à `BookTemplateTitleTextBlockStyle` et `BookTemplateAuthorTextBlockStyle`. Bookstore1\_81 a utilisé ces clés comme une indirection, de sorte qu’une clé unique avait des implémentations différentes dans les deux applications. Nous n’avons plus besoin de cette indirection ; nous pouvons référencer directement les styles du système. Par conséquent, remplacez ces références par `TitleTextBlockStyle` et `SubtitleTextBlockStyle`, respectivement.
-   À présent, nous devons définir l’arrière-plan de `LayoutRoot` sur la valeur par défaut appropriée afin que l’application se présente correctement lorsqu’elle est exécutée sur tous les appareils, quel que soit le thème. Remplacez sa valeur `"Transparent"` par la valeur `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Dans l’élément `TitlePanel`, remplacez la référence à `TitleTextBlockStyle` (un peu trop importante, désormais) par une référence à `CaptionTextBlockStyle`. `PageTitleTextBlockStyle` est une autre Bookstore1\_de l’indirection de 81 dont nous n’avons plus besoin. Modifiez-la pour faire référence à `HeaderTextBlockStyle` à la place.
-   Nous n’avons plus besoin de définir un arrière-plan, un style ni un ItemContainerStyle spécial dans l’élément **ListBox**, mais simplement de supprimer ces trois attributs et leurs valeurs du balisage. Toutefois, nous souhaitons masquer la bordure de l’élément **ListBox**. Donc, ajoutez `BorderBrush="{x:Null}"` à celui-ci.
-   Nous n’allons plus faire référence à des ressources du fichier BookstoreStyles.xaml **ResourceDictionary**. Vous pouvez supprimer toutes ces ressources. Mais ne supprimez pas le fichier BookstoreStyles.xaml : nous devons encore l’utiliser, comme vous le verrez dans la section suivante.

Une fois cette séquence d’opérations de stylisation effectuée, l’application ressemble à ceci.

![Application Windows 10 quasiment portée](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

L’application Windows 10 presque portée s’exécutant sur un appareil de bureau

![Application Windows 10 quasiment portée](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

L’application Windows 10 presque portée s’exécutant sur un appareil mobile

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>Ajustement facultatif de la zone de liste pour les appareils mobiles

Lorsque l’application s’exécute sur un appareil mobile, l’arrière-plan d’une zone de liste est clair par défaut dans les deux thèmes. Vous préférerez peut-être ce style et, dans ce cas, vous n’avez plus rien à faire si ce n’est nettoyer : supprimez le fichier de dictionnaire de ressources BookstoreStyles.xaml de votre projet et supprimez le balisage qui le fusionne dans le fichier MainPage.xaml.

Toutefois, les contrôles sont conçus pour que vous puissiez personnaliser leur apparence tout en préservant leur comportement. Par conséquent, si vous souhaitez que la zone de liste soit sombre dans le thème à dominante foncée (l’apparence de l’application d’origine), cette section décrit la manière de procéder.

Les modifications que nous apportons ne doivent pas affecter l’application lorsqu’elle s’exécute sur des appareils mobiles. Nous utiliserons donc un style de zone de liste très légèrement personnalisé lors d’une exécution sur la famille d’appareils mobiles, et nous continuerons à utiliser le style par défaut sur tous les autres. Pour ce faire, nous allons effectuer une copie de BookstoreStyles.xaml et lui donner un nom complet MRT spécial qui entraînera son chargement sur les appareils mobiles uniquement.

Ajoutez un nouvel élément de projet **ResourceDictionary** et nommez-le BookstoreStyles.DeviceFamily-Mobile.xaml. Vous disposez maintenant de deux fichiers dont le nom logique est BookstoreStyles.xaml (c’est le nom que vous utiliserez dans votre balisage et dans le code). Les fichiers portent des noms physiques différents pour pouvoir contenir un balisage différent. Vous pouvez utiliser ce schéma de nommage qualifié MRT avec n’importe quel fichier xaml, mais n’oubliez pas que tous les fichiers xaml ayant le même nom logique partagent un fichier code-behind xaml.cs unique (le cas échéant).

Modifiez une copie du modèle de contrôle de la zone de liste et stockez-la avec la clé de `BookstoreListBoxStyle` dans le nouveau dictionnaire de ressources, BookstoreStyles.DeviceFamily-Mobile.xaml. Maintenant, nous allons apporter des modifications simples à trois méthodes setter.

-   Dans la méthode setter de premier plan, remplacez la valeur par `"{x:Null}"`. Notez que la définition d’une propriété sur `"{x:Null}"` directement dans un élément revient à définir la valeur `null` dans le code. Mais l’utilisation de la valeur `"{x:Null}"` dans une méthode setter a un seul effet : elle remplace la méthode setter dans le style par défaut (pour la même propriété) et rétablit la valeur par défaut de la propriété sur l’élément cible.
-   Dans la méthode setter d’arrière-plan, remplacez la valeur par `"Transparent"` pour supprimer cet arrière-plan clair.
-   Dans la méthode setter de modèle, recherchez l’état visuel nommé `Focused` et supprimez sa table de montage séquentielle, la rendant ainsi dans une balise vide.
-   Supprimez toutes les autres méthodes setter du balisage.

Pour finir, copiez l’élément `BookstoreListBoxStyle` dans BookstoreStyles.xaml et supprimez ses trois méthodes setter, ce qui le convertit en une balise vide. Nous effectuons cette opération pour que, sur les appareils autres que mobiles, notre référence à BookstoreStyles.xaml et à `BookstoreListBoxStyle` soit résolue, mais qu’elle n’ait aucun effet.

![Application Windows 10 portée](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil mobile

## <a name="conclusion"></a>Conclusion

Cette étude de cas vous a décrit le processus de portage d’une application très simple, probablement non réaliste. Par exemple, une zone de liste peut être utilisée pour la sélection ou pour la création d’un contexte de navigation. L’application accède à une page contenant plus de détails sur l’élément sélectionné. Cette application particulière ne fait rien avec la sélection de l’utilisateur et est dépourvue de navigation. Malgré tout, l’étude de cas a servi à briser la glace, afin d’introduire le processus de portage et d’illustrer les techniques importantes que vous pouvez utiliser dans les applications UWP réelles.

Nous avons également pu constater que le processus de portage des modèles d’affichage était généralement fluide. Cependant, la prise en charge du facteur de forme et l’interface utilisateur risquent fort de nécessiter une certaine attention lors du portage.

Dans l’étude de cas suivante, [Bookstore2](w8x-to-uwp-case-study-bookstore2.md), nous examinons l’accès aux données groupées et l’affichage de ces dernières.
