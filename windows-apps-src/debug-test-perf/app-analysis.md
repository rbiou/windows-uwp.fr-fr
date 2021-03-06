---
title: Analyse d’application
description: Analysez votre application pour détecter les problèmes de performances.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e2977877b839f40e07b3eaa03b8349fb8439a401
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73062754"
---
# <a name="app-analysis-overview"></a>Vue d’ensemble de l’analyse d’application

L’analyse d’application est un outil qui fournit aux développeurs une notification les avertissant de problèmes de performances. L’analyse d’application teste le code de votre application par rapport à un ensemble de bonnes pratiques et de recommandations en matière de performances.

L’analyse d’application identifie les problèmes à partir d’un ensemble de règles de problèmes de performances courants rencontrés par les applications. Le cas échéant, l’analyse d’application pointe sur l’outil chronologie de Visual Studio, les informations sur la source et la documentation pour vous donner les moyens de rechercher les causes.

Les règles de l’analyse d’application font référence à une recommandation ou une bonne pratique par rapport à laquelle votre application est testée.

## <a name="decoded-image-size-larger-than-render-size"></a>Taille d’image décodée supérieure à la taille de rendu

Les images sont capturées dans des résolutions très élevées, par conséquent, les applications peuvent être amenées à utiliser davantage de processeur pendant le décodage des données d’image et de mémoire après leur chargement à partir du disque. Mais cela ne fait aucun sens de décoder et d’enregistrer une image en haute résolution en mémoire si elle doit être affichée uniquement dans une taille plus petite que sa taille d’origine. Créez plutôt une version de l’image avec une taille correspondant exactement à celle qui sera affichée à l’écran à l’aide des propriétés DecodePixelWidth et DecodePixelHeight.

### <a name="impact"></a>Impact

L’affichage d’images dans une taille non native peut avoir un effet négatif à la fois sur le temps processeur (décodage de la taille appropriée et temps de téléchargement) et sur la mémoire.

### <a name="causes-and-solutions"></a>Causes et solutions

#### <a name="image-is-not-being-set-asynchronously"></a>L’image n’est pas définie en mode asynchrone

L’application utilise SetSource() au lieu de SetSourceAsync(). Évitez toujours d’utiliser [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) et utilisez plutôt [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) quand vous définissez un flux pour décoder des images de manière asynchrone. 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>L’image est appelée quand ImageSource n’est pas dans l’arborescence live

Le BitmapImage est associé à l’arborescence XAML live après la définition du contenu avec SetSourceAsync ou UriSource. Vous devez toujours associer un [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) à l’arborescence live avant de définir la source. Dès lors qu’un élément ou pinceau image est spécifié dans le balisage, ce sera automatiquement le cas. Vous trouverez ci-dessous des exemples. 

**Exemples d’arborescences live**

Exemple 1 (correct) : Uniform Resource Identifier (URI) spécifié dans le balisage.

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Exemple 2 balisage : URI spécifié dans le code-behind.

```xml
<Image x:Name="myImage"/>
```

Exemple 2 code-behind (correct) : association de la BitmapImage à l’arborescence avant de définir son UriSource.

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Exemple 2 code-behind (incorrect) : définition de l’UriSouce de BitmapImage avant de l’associer à l’arborescence.

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>Le pinceau image n’est pas rectangulaire 

Quand une image est utilisée pour un pinceau non rectangulaire, elle utilise un chemin de rastérisation logicielle qui ne met pas du tout les images à l’échelle. En outre, elle doit stocker une copie de l’image dans la mémoire logicielle et matérielle. Par exemple, si une image est utilisée comme pinceau pour une ellipse, l’image complète, éventuellement de grande taille, sera stockée deux fois en interne. Quand vous utilisez un pinceau non rectangulaire, votre application doit effectuer une mise à l’échelle préalable de ses images à leur taille de rendu approximative.

Vous pouvez aussi définir une taille de décodage explicite pour créer une version de l’image à la taille exacte qui sera affichée à l’écran à l’aide des propriétés [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) et [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

Les unités pour [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) et [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) sont en pixels physiques par défaut. La propriété [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) peut être utilisée pour modifier ce comportement : en définissant **DecodePixelType** sur **Logical**, la taille de décodage tient compte automatiquement du facteur d’échelle actuel du système, comme c’est le cas pour d’autres contenus XAML. Il est donc généralement approprié de définir **DecodePixelType** sur **Logical** si, par exemple, vous voulez que **DecodePixelWidth** et **DecodePixelHeight** correspondent aux propriétés de hauteur et de largeur du contrôle d’image dans lequel s’affiche l’image. Si vous utilisez le comportement par défaut qui consiste à utiliser des pixels physiques, vous devez tenir compte du facteur d’échelle du système actuel et être attentif aux notifications de modification d’échelle au cas où l’utilisateur modifie ses préférences d’affichage.

Dans certains cas, quand une taille de décodage appropriée ne peut pas être déterminée à l’avance, vous devez vous en remettre au décodage automatique à la taille adéquate du code XAML qui tente de décoder au mieux l’image à la taille appropriée si aucun paramètre DecodePixelWidth/DecodePixelHeight explicite n’est spécifié.

Vous devez définir une taille de décodage explicite si vous connaissez à l’avance la taille du contenu image. Vous devez également définir [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) sur **Logical** si la taille de décodage fournie dépend de la taille d’autres éléments XAML. Par exemple, si vous définissez explicitement la taille du contenu avec Image.Width et Image.Height, vous pouvez définir DecodePixelType sur DecodePixelType.Logical pour utiliser les mêmes dimensions de pixels logiques qu’un contrôle d’image, puis utiliser explicitement BitmapImage.DecodePixelWidth et/ou BitmapImage.DecodePixelHeight pour contrôler la taille de l’image afin de pouvoir éventuellement économiser une grande capacité de mémoire.

Remarque : Prenez en considération Image.Stretch quand vous déterminez la taille du contenu décodé.

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>Les images utilisées à l’intérieur de BitmapIcons utilisent le décodage à la taille naturelle 

Définissez une taille de décodage explicite pour créer une version de l’image à la taille exacte qui sera affichée à l’écran à l’aide des propriétés [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) et [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>Les images qui s’affichent très grandes à l’écran utilisent le décodage à la taille naturelle 

Les images qui s’affichent très grandes à l’écran utilisent le décodage à la taille naturelle. Définissez une taille de décodage explicite pour créer une version de l’image à la taille exacte qui sera affichée à l’écran à l’aide des propriétés [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) et [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

#### <a name="image-is-hidden"></a>L’image est masquée

L’image est masquée en définissant Opacity sur 0 ou Visibility sur Collapsed sur l’élément ou pinceau image hôte, ou tout autre élément parent. Les images qui ne sont pas visibles à l’écran en raison du détourage ou de la transparence peuvent utiliser le décodage à la taille naturelle. 

#### <a name="image-is-using-ninegrid-property"></a>L’image utilise la propriété NineGrid

Quand une image est utilisée pour un [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid), elle utilise un chemin de rastérisation logicielle qui ne met pas du tout les images à l’échelle. En outre, elle doit stocker une copie de l’image dans la mémoire logicielle et matérielle. Quand vous utilisez **NineGrid**, votre application doit effectuer une mise à l’échelle préalable de ses images à leur taille de rendu approximative.

Les images qui utilisent la propriété NineGrid utilisent le décodage à la taille naturelle. Ajoutez l’effet ninegrid à l’image d’origine.

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>DecodePixelWidth ou DecodePixelHeight sont définis sur une taille plus grande que celle de l’image qui s’affiche à l’écran 

Si les paramètres DecodePixelWidth/Height sont explicitement définis sur des valeurs plus grandes que la taille de l’image affichée à l’écran, l’application utilise inutilement de la mémoire supplémentaire (jusqu’à 4 octets par pixel), ce qui devient rapidement coûteux pour les grandes images. L’image est également réduite à l’aide d’une mise à l’échelle bilinéaire, ce qui risque de la faire apparaître floue pour les grands facteurs d’échelle.

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>L’image est décodée dans le cadre de la production d’une image de Glisser- déplacer

Définissez une taille de décodage explicite pour créer une version de l’image à la taille exacte qui sera affichée à l’écran à l’aide des propriétés [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) et [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

## <a name="collapsed-elements-at-load-time"></a>Éléments réduits au moment du chargement

Un modèle courant dans les applications consiste à d’abord masquer des éléments dans l’interface utilisateur pour les afficher plus tard. Dans la plupart des cas, ces éléments doivent être différés à l’aide de x:Load ou de x:DeferLoadStrategy pour éviter de payer le coût de création de l’élément au moment du chargement.

C’est le cas notamment quand un convertisseur de valeur booléenne en valeur visibility est utilisé pour masquer des éléments afin de les afficher plus tard.

### <a name="impact"></a>Impact

Les éléments réduits sont chargés avec d’autres éléments et contribuent à une augmentation du temps de chargement.

### <a name="cause"></a>Cause

Cette règle a été déclenchée, car un élément a été réduit au moment du chargement. Le fait de réduire un élément ou de définir son opacité sur 0 n’empêche pas la création de l’élément. Cette règle peut être déclenchée par une application qui utilise un convertisseur de valeur booléenne en valeur Visibility défini par défaut sur false.

### <a name="solution"></a>Solution

[x:Load attribute](../xaml-platform/x-load-attribute.md) et [x:DeferLoadStrategy](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute) permettent de différer le chargement d’un élément d’interface utilisateur jusqu’au moment opportun. C’est un bon moyen de retarder le traitement de l’interface utilisateur qui n’est pas visible dans le premier frame. Vous pouvez choisir de charger l’élément selon les besoins, ou dans le cadre d’un ensemble de logique différée. Pour déclencher le chargement, appelez findName sur l’élément que vous voulez charger. x:Load étend les fonctionnalités de x:DeferLoadStrategy en permettant de décharger les éléments et de contrôler l’état de chargement par le biais de x:Bind.

Dans certains cas, l’utilisation de findName pour afficher un élément d’interface utilisateur n’est peut-être pas appropriée. Cela est vrai si vous prévoyez de réaliser une partie significative de l’interface utilisateur à partir d’un clic sur un bouton avec une très faible latence. Dans ce cas, vous pouvez éventuellement accepter une latence plus courte de l’interface utilisateur au prix de mémoire supplémentaire ; utilisez alors x:DeferLoadStrategy et définissez Visibility sur Collapsed au niveau de l’élément que vous voulez réaliser. Une fois que la page est chargée et que le thread d’interface utilisateur est libre, vous pouvez appeler findName selon vos besoins pour charger les éléments. Les éléments ne sont pas visibles pour l’utilisateur si la propriété Visibility de l’élément n’est pas définie sur Visible.

## <a name="listview-is-not-virtualized"></a>ListView n’est pas virtualisé

La virtualisation de l’interface utilisateur est le point le plus important que vous pouvez améliorer pour augmenter les performances des collections. Elle implique la création à la demande des éléments d’interface utilisateur. Pour un contrôle d’éléments lié à une collection de 1 000 éléments, créer l’interface utilisateur simultanément pour tous les éléments constituerait un gaspillage de ressources, car il n’est pas possible d’afficher tous les éléments en même temps. Les contrôles ListView et GridView (et d’autres contrôles standard dérivés de ItemsControl) effectuent la virtualisation de l’interface utilisateur à votre place. Quand des éléments vont bientôt défiler dans l’affichage (quelques pages plus loin), l’infrastructure génère l’interface utilisateur pour ces éléments et les met en cache. Lorsqu’il est peu probable que les éléments soient de nouveau affichés, l’infrastructure récupère la mémoire qui leur était allouée.

La virtualisation de l’interface utilisateur est l’un des nombreux facteurs clés qui permettent d’améliorer les performances des collections. La réduction de la complexité des éléments d’une collection et la virtualisation des données sont deux autres aspects importants qui contribuent à l’amélioration des performances des collections. Pour plus d’informations sur l’amélioration des performances des collections dans les contrôles ListView et GridView, voir les articles sur l’[Optimisation des options d’interface ListView et GridView](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) et [Virtualisation des données ListView et Gridview](https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Impact

Un ItemsControl non virtualisé augmente le temps de chargement et l’utilisation des ressources en chargeant plus d’éléments enfants que nécessaire.

### <a name="cause"></a>Cause

La fenêtre d’affichage est un concept essentiel de la virtualisation de l’interface utilisateur, car l’infrastructure doit créer les éléments qui sont susceptibles d’être affichés. La fenêtre d’affichage d’un contrôle ItemsControl est généralement l’extension du contrôle logique. Par exemple, la fenêtre d’affichage d’un contrôle ListView correspond à la largeur et à la hauteur de l’élément ListView. Certains volets offrent un espace illimité aux éléments enfants (par exemple, ScrollViewer et Grid) avec un dimensionnement automatique des lignes ou des colonnes. Quand un contrôle ItemsControl virtualisé est placé dans un panneau de ce type, il occupe suffisamment d’espace pour afficher tous ses éléments, au détriment de la virtualisation. 

### <a name="solution"></a>Solution

Restaurez la virtualisation en définissant une largeur et une hauteur pour le ItemsControl que vous utilisez.

## <a name="ui-thread-blocked-or-idle-during-load"></a>Thread d’interface utilisateur bloqué ou inactif pendant le chargement

Le thread d’interface utilisateur est bloqué en cas d’appels synchrones à des fonctions qui s’exécutent en dehors du thread.  

Pour obtenir la liste complète des bonnes pratiques qui permettent d’améliorer les performances de démarrage de votre application, voir [Meilleures pratiques en matière de performances de démarrage de votre application](https://docs.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance) et [Assurer la réactivité du thread de l’interface utilisateur](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive).

### <a name="impact"></a>Impact

Un thread d’interface utilisateur bloqué ou inactif au moment du chargement empêche l’exécution de la disposition et d’autres opérations de l’interface utilisateur, ce qui augmente le temps de démarrage.

### <a name="cause"></a>Cause

Le code de la plateforme et le code de votre application pour l’interface utilisateur sont exécutés sur le même thread d’interface utilisateur. Une seule instruction à la fois peut être exécutée sur ce thread. Par conséquent, si le code de votre application met trop longtemps à traiter un événement, l’infrastructure ne peut pas exécuter la disposition ou déclencher de nouveaux événements représentant une interaction utilisateur. La réactivité de votre application est liée à la disponibilité du thread d’interface utilisateur pour exécuter les tâches.

### <a name="solution"></a>Solution

Votre application peut être interactive même si certaines de ses parties ne sont pas totalement fonctionnelles. Par exemple, si votre application affiche des données qui sont longues à récupérer, vous pouvez faire en sorte que le code chargé de récupérer ces données s’exécute indépendamment du code de démarrage de l’application en récupérant les données de façon asynchrone. Lorsque les données sont disponibles, fournissez-les à l’interface utilisateur de l’application. Pour que l’application reste réactive, la plateforme fournit des versions asynchrones de plusieurs de ses API. Une API asynchrone permet de garantir que votre thread d’exécution actif n’est jamais bloqué pendant un laps de temps significatif. Si vous appelez une API à partir du thread d’interface utilisateur, utilisez la version asynchrone si elle est disponible.

## <a name="binding-is-being-used-instead-of-xbind"></a>{Binding} est utilisé au lieu de {x:Bind}

Cette règle est déclenchée quand votre application utilise une instruction {Binding}. Pour améliorer leurs performances, les applications doivent utiliser {x:Bind}.

### <a name="impact"></a>Impact

L’exécution de {Binding} demande plus de temps et de mémoire que celle de {x:Bind}.

### <a name="cause"></a>Cause

L’application utilise {Binding} plutôt que {x:Bind}. {Binding} entraîne une plage de travail non triviale et une surcharge de processeur. La création d’une {Binding} entraîne une série d’allocations, et la mise à jour d’une cible de liaison peut provoquer boxing et réflexion.

### <a name="solution"></a>Solution

Utilisez l’extension de balisage {x:Bind}, qui compile les liaisons au moment de la génération. Les liaisons {x:Bind} (souvent appelées liaisons compilées) offrent d’excellentes performances, valident vos expressions de liaison au moment de la compilation et prennent en charge le débogage en vous permettant de définir des points d’arrêt dans les fichiers de code générés en tant que classe partielle pour votre page. 

Notez que x:Bind ne convient pas dans tous les cas, comme dans les scénarios à liaison tardive. Pour obtenir la liste complète des cas non couverts par {x:Bind}, voir la documentation de {x:Bind}.

## <a name="xname-is-being-used-instead-of-xkey"></a>x:Name est utilisé au lieu de x:Key

Les ResourceDictionaries sont généralement utilisés pour stocker vos ressources à un niveau global, c’est-à-dire les ressources que votre application veut référencer à plusieurs endroits. Par exemple, les styles, les pinceaux, les modèles et ainsi de suite. En règle générale, nous optimisons les ResourceDictionaries pour ne pas instancier de ressources sauf demande contraire. Cependant, vous devez être prudent dans quelques emplacements.

### <a name="impact"></a>Impact

Toutes les ressources comprenant x:Name sont instanciées dès que le ResourceDictionary est créé. En effet, x:Name indique à la plateforme que votre application doit accéder à cette ressource. La plateforme doit donc créer un élément à référencer.

### <a name="cause"></a>Cause

Votre application définit x:Name sur une ressource.

### <a name="solution"></a>Solution

Utilisez x:Key à la place de x:Name quand vous ne référencez pas de ressources à partir de code-behind.

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>Le contrôle Collections utilise un panneau de non-virtualisation

Si vous fournissez un modèle de panneau d’éléments personnalisé (voir ItemsPanel), utilisez un panneau de virtualisation tel que ItemsWrapGrid ou ItemsStackPanel. Si vous utilisez VariableSizedWrapGrid, WrapGrid ou StackPanel, vous ne pouvez pas bénéficier de la virtualisation. Par ailleurs, les événements ListView suivants sont déclenchés uniquement quand un ItemsWrapGrid ou un ItemsStackPanel sont utilisés : ChoosingGroupHeaderContainer, ChoosingItemContainer et ContainerContentChanging.

La virtualisation de l’interface utilisateur est le point le plus important que vous pouvez améliorer pour augmenter les performances des collections. Elle implique la création à la demande des éléments d’interface utilisateur. Pour un contrôle d’éléments lié à une collection de 1 000 éléments, créer l’interface utilisateur simultanément pour tous les éléments constituerait un gaspillage de ressources, car il n’est pas possible d’afficher tous les éléments en même temps. Les contrôles ListView et GridView (et d’autres contrôles standard dérivés de ItemsControl) effectuent la virtualisation de l’interface utilisateur à votre place. Quand des éléments vont bientôt défiler dans l’affichage (quelques pages plus loin), l’infrastructure génère l’interface utilisateur pour ces éléments et les met en cache. Lorsqu’il est peu probable que les éléments soient de nouveau affichés, l’infrastructure récupère la mémoire qui leur était allouée.

La virtualisation de l’interface utilisateur est l’un des nombreux facteurs clés qui permettent d’améliorer les performances des collections. La réduction de la complexité des éléments d’une collection et la virtualisation des données sont deux autres aspects importants qui contribuent à l’amélioration des performances des collections. Pour plus d’informations sur l’amélioration des performances des collections dans les contrôles ListView et GridView, voir les articles sur l’[Optimisation des options d’interface ListView et GridView](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) et [Virtualisation des données ListView et Gridview](https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Impact

Un ItemsControl non virtualisé augmente le temps de chargement et l’utilisation des ressources en chargeant plus d’éléments enfants que nécessaire.

### <a name="cause"></a>Cause

Vous utilisez un panneau qui ne prend pas en charge la virtualisation.

### <a name="solution"></a>Solution

Utilisez un panneau de virtualisation tel que ItemsWrapGrid ou ItemsStackPanel.

## <a name="accessibility-uia-elements-with-no-name"></a>Accessibilité : Éléments UIA sans nom

En XAML, vous pouvez indiquer un nom en définissant AutomationProperties.Name. De nombreux pairs Automation fournissent un nom par défaut à UIA si AutomationProperties.Name n’est pas défini. 

### <a name="impact"></a>Impact

Si un utilisateur accède à un élément sans nom, il n’a souvent aucun moyen de savoir ce à quoi ce dernier fait référence. 

### <a name="cause"></a>Cause

Le nom UIA de l’élément est null ou vide. Cette règle vérifie ce que voit UIA, et non la valeur d’AutomationProperties.Name.

### <a name="solution"></a>Solution

Définissez la propriété AutomationProperties.Name dans le code XAML du contrôle sur une chaîne localisée appropriée.

Parfois, le correctif d’application approprié n’est pas de fournir un nom, mais de supprimer l’élément UIA de tous les emplacements à part l’arborescence brute. Vous pouvez le faire en XAML en définissant `AutomationProperties.AccessibilityView = "Raw"`.

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>Accessibilité : Les éléments UIA avec le même Controltype ne doivent pas avoir le même nom

Deux éléments UIA avec le même parent UIA ne doivent pas avoir le même nom ni le même ControlType. Il est possible d’avoir deux contrôles du même nom s’ils ont des ControlTypes différents. 

Cette règle ne vérifie pas si les noms en double ont des parents différents. Toutefois, dans la plupart des cas, vous ne devez pas dupliquer les noms et les ControlTypes au sein d’une fenêtre, même avec des parents différent. Les noms dupliqués au sein d’une fenêtre sont acceptés en cas de deux listes avec des éléments identiques. Dans ce cas, les éléments de liste doivent avoir des noms et des ControlTypes identiques.

### <a name="impact"></a>Impact

Si un utilisateur accède à un élément de même nom et ControlType qu’un autre élément qui a le même parent UIA, l’utilisateur peut ne pas être capable de distinguer la différence entre les éléments.

### <a name="cause"></a>Cause

Les éléments UIA qui ont le même parent UIA ont le même nom et le même ControlType.

### <a name="solution"></a>Solution

Définissez un nom dans le code XAML à l’aide d’AutomationProperties.Name. Dans les listes où cette situation se produit fréquemment, utilisez une liaison pour lier la valeur d’AutomationProperties.Name à une source de données.


