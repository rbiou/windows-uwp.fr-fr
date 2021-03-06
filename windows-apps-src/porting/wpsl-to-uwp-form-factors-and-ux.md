---
description: Les applications Windows présentent le même aspect, que ce soit sur PC, sur appareil mobile ou sur tout autre type d’appareil. Les modèles d’interaction, d’entrée et d’interface utilisateur sont similaires ; un utilisateur passant d’un type d’appareil à un autre ne pourra que se féliciter de ces similitudes.
title: Portage Windows Phone Silverlight vers UWP pour le facteur de forme et l’expérience utilisateur
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c15621ed34fb358f318549d7987d7c445247aae
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684635"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>Portage Windows Phone Silverlight vers UWP pour le facteur de forme et l’expérience utilisateur


Rubrique précédente : [Portage des couches métier et des couches de données](wpsl-to-uwp-business-and-data.md).

Les applications Windows présentent le même aspect, que ce soit sur PC, sur appareil mobile ou sur tout autre type d’appareil. Les modèles d’interaction, d’entrée et d’interface utilisateur sont similaires ; un utilisateur passant d’un type d’appareil à un autre ne pourra que se féliciter de ces similitudes. Les différences entre les appareils tels que la taille physique, l’orientation par défaut et le facteur de résolution de pixel effectif dans le mode de rendu d’une application plateforme Windows universelle (UWP) par Windows 10. Heureusement, le système gère à votre place une grande partie des tâches les plus difficiles à l’aide de concepts novateurs tels que les pixels effectifs.

## <a name="different-form-factors-and-user-experience"></a>Des facteurs de forme différents pour une expérience utilisateur variée

Les différents appareils peuvent présenter plusieurs résolutions en mode portrait et en mode paysage, ainsi que des proportions variées. Comment les aspects visuels de l’interface, du texte et des ressources de votre application UWP s’adapteront-ils ? Comment faire pour prendre en charge la technologie tactile ainsi que les entrées effectuées à l’aide d’un clavier et d’une souris ? Enfin, lorsque l’application prend en charge la technologie tactile sur des appareils de taille différente, à des distances d’affichage variées, comment faire pour qu’un contrôle joue le rôle d’élément d’une taille adéquate pour diverses densités de pixel *tout en présentant* un contenu lisible à des distances différentes ? Les sections suivantes fournissent des informations que vous devez connaître.

## <a name="what-is-the-size-of-a-screen-really"></a>Quelle est la taille d’un écran, en réalité ?

En un mot comme en cent, la réponse à cette question est subjective. En fait, cela dépend non seulement de la taille physique de l’affichage, mais également de votre distance par rapport à l’écran. Pour cela, il est nécessaire de se mettre à la place de l’utilisateur. Or, un bon développeur doit le faire en toutes circonstances.

Objectivement, la taille d’un écran est mesurée en pouces et en pixels physiques (bruts). La connaissance de ces deux métriques vous permet de déterminer le nombre de pixels inclus dans un pouce. La valeur obtenue porte le nom de densité en pixels (DPI, Dots Per Inch). On parle également de PPP, ou pixels par pouce. L’inverse de la densité en pixels est la taille physique des pixels, sous la forme d’une fraction d’un pouce. La densité en pixels est également appelée *résolution*, même si ce terme est souvent utilisé pour indiquer le nombre de pixels.

Lorsque la distance d’affichage augmente, toutes ces métriques objectives *semblent* correspondre à des valeurs plus petites et sont résolues sous la forme d’une *taille réelle* de l’écran, associée à la *résolution effective* de ce dernier. En général, l’appareil que vous placez le plus près de vos yeux est votre téléphone, suivi de votre tablette, puis de l’écran de votre PC. Enfin, les appareils plus éloignés sont les [Surface Hub](https://www.microsoft.com/surface/devices/surface-hub) et les écrans de télévision. Pour compenser cette distance, les appareils ont tendance à être de plus en plus grands en fonction de la distance d’affichage. Lorsque vous définissez les tailles des éléments de votre interface utilisateur, vous les exprimez en unités appelées pixels effectifs (epx). Et Windows 10 prend en compte la résolution PPP, et la distance d’affichage par défaut d’un appareil, pour calculer la taille optimale de vos éléments d’interface utilisateur en pixels physiques pour offrir la meilleure expérience d’affichage. Voir [Pixels d’affichage/effectifs, distance d’affichage et facteurs d’échelle](wpsl-to-uwp-porting-xaml-and-ui.md).

Même dans ce cas, nous vous recommandons de tester votre application avec différents appareils afin que vous puissiez vérifier chaque expérience par vous-même.

## <a name="touch-resolution-and-viewing-resolution"></a>Résolution tactile et résolution d’affichage

Les affordances (widgets d’interface utilisateur) doivent présenter la taille adéquate pour une interaction tactile. Ainsi, une cible tactile doit conserver grosso modo sa taille physique sur les différents appareils qui sont susceptibles de proposer des densités en pixels variées. Là encore, les pixels effectifs vous viennent en aide, car ils sont mis à l’échelle sur les différents appareils (la densité en pixels étant prise en compte) afin de garantir une taille physique plus ou moins constante idéale pour les cibles tactiles.

Pour pouvoir être lu correctement, le texte doit présenter une taille adéquate : de manière empirique, on considère qu’une distance 50 cm pour lire un texte de 12 points est appropriée. De plus, les images doivent présenter la taille et la résolution réelle les mieux adaptées à la distance d’affichage. Sur les différents appareils, la même valeur de mise à l’échelle des pixels effectifs permet d’assurer une taille adéquate des éléments de l’interface utilisateur, qui sont alors clairement lisibles. Le texte et les autres graphiques vectoriels sont automatiquement mis à l’échelle de manière efficace. Les graphiques raster (bitmap) sont également mis à l’échelle de manière automatique lorsque le développeur fournit une ressource présentant une taille unique élevée. Toutefois, le développeur est invité à proposer différentes tailles pour chaque ressource afin de permettre le chargement automatique de la taille idéale pour le facteur d’échelle d’un appareil cible. Pour plus d’informations, voir [Pixels d’affichage/effectifs, distance d’affichage et facteurs d’échelle](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="layout-and-adaptive-visual-state-manager"></a>Disposition et Gestionnaire d’état visuel adaptatif

Nous avons décrit les facteurs qui entrent en jeu dans une connaissance approfondie de la taille de l’écran. À présent, penchons-nous sur la disposition de votre application et voyons comment utiliser l’éventuel excédent d’espace. Considérons l’exemple de cette page, qui provient d’une application très simple, conçue pour s’exécuter sur un appareil mobile de petite taille. À quoi doit-elle ressembler sur un écran plus grand ?

![Application du Windows Phone Store portée](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

La version mobile de l’application propose uniquement une orientation en mode portrait, car cette dernière offre les meilleures proportions pour afficher la liste de livres. Nous procéderions de même pour afficher une page de texte, qui apparaît mieux sous la forme d’une colonne unique sur les appareils mobiles. Or, un écran de PC ou de tablette est plus grand, quelle que soit l’orientation. Sur ces types d’appareil, les limitations associées aux écrans d’appareils mobiles semblent donc inutiles.

Si vous exécutez un zoom optique sur l’application afin que le contenu s’affiche comme sur un appareil mobile (en plus gros caractères), vous ne tirez pas parti des avantages proposés par l’appareil, ni de l’espace supplémentaire qu’il offre. Par ailleurs, cela ne sert pas les intérêts de l’utilisateur. Nous devons penser à afficher davantage de contenu, plutôt que le même contenu en plus gros caractères. Même sur une phablette, nous pourrions afficher davantage de lignes de contenu. Ainsi, nous pourrions exploiter l’espace supplémentaire afin d’afficher d’autres contenus (comme des publicités), ou nous pourrions remplacer la zone de liste par une vue de liste, qui pourrait placer les éléments dans plusieurs colonnes (si possible) afin de valoriser l’espace. Voir [Recommandations en matière de contrôles d’affichages de liste et d’affichages de grille](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists).

En plus de nouveaux contrôles, tels que le mode liste et la vue grille, la plupart des types de dispositions établis à partir de Windows Phone Silverlight ont des équivalents dans le plateforme Windows universelle (UWP). Exemples : [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) et [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel). Le portage de la majorité des fonctions de l’interface utilisateur qui utilisent ces types doit être clair. Toutefois, recherchez toujours d’autres méthodes pour tirer parti des fonctions de disposition dynamiques de ces panneaux de disposition, afin de proposer le redimensionnement et la redisposition automatiques sur les appareils de tailles différentes.

En allant au-delà de la disposition dynamique intégrée aux contrôles système et aux panneaux de disposition, nous pouvons utiliser une nouvelle fonctionnalité de Windows 10 appelée [Gestionnaire d’état visuel adaptatif](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="input-modalities"></a>Modalités d’entrée

Une interface Silverlight Windows Phone est spécifique à la tactile. L’interface de votre application portée doit évidemment proposer des interactions tactiles ; toutefois, vous avez la possibilité de proposer d’autres modalités d’entrée, comme des interactions par le biais d’un clavier et d’une souris. Dans UWP, les entrées effectuées à l’aide d’une souris, d’un stylet et de fonctions tactiles sont regroupées en une seule catégorie : les *entrées de pointeur*. Pour en savoir plus, voir [Gérer les entrées du pointeur](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input) et [Interactions avec le clavier](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions).

## <a name="maximizing-markup-and-code-re-use"></a>Valorisation de la réutilisation du code et du balisage

Reportez-vous à la liste [Valorisation de la réutilisation du code et du balisage](wpsl-to-uwp-porting-to-a-uwp-project.md) pour connaître les techniques de partage de votre interface utilisateur sur des appareils cibles présentant un large éventail de facteurs de forme.

## <a name="more-info-and-design-guidelines"></a>Informations supplémentaires et recommandations de conception

-   [Concevoir des applications UWP](https://developer.microsoft.com/windows/apps/design)
-   [Instructions pour les polices](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)
-   [Planifier différents facteurs de forme](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)

## <a name="related-topics"></a>Rubriques connexes

* [Mappages d’espaces de noms et de classes](wpsl-to-uwp-namespace-and-class-mappings.md)

