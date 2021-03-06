---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Couche visuelle
description: L’API Windows.UI.Composition vous donne accès à la couche de composition comprise entre la couche d’infrastructure (XAML) et la couche graphique (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac41d461982a39a939e460b7a81b144e5a08fdb3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255522"
---
# <a name="visual-layer"></a>Couche visuelle

La couche visuelle fournit une performance élevée, une API en mode retenu pour les graphiques, les effets et les animations. Elle constitue la base de l’ensemble de l’interface utilisateur sur les appareils Windows. Vous définissez votre interface utilisateur de façon déclarative et la couche visuelle s’appuie sur l’accélération matérielle graphique pour s’assurer que le contenu, les effets et les animations sont rendus de manière lisse et sans problème, indépendamment du thread d’interface utilisateur de l’application.

Éléments essentiels :

* API WinRT familières
* Conçue pour des interactions et une interface utilisateur plus dynamiques
* Concepts harmonisés avec les outils de conception
* Évolutivité linéaire sans chute brutale des performances

Vos applications UWP Windows utilisent déjà la couche visuelle via l’une des infrastructures d’interface utilisateur. Vous pouvez également tirer parti directement de la couche visuelle pour personnaliser très simplement vos rendus, effets et animations.

![Couche d’infrastructure d’interface utilisateur : la couche d’infrastructure (Windows.UI.XAML) s’appuie sur la couche visuelle (Windows.UI.Composition) qui repose sur la couche graphique (DirectX).](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>Nouveautés de la couche visuelle

Principales fonctions de la couche visuelle :

1. **Contenu** : composition légère de contenu dessiné personnalisé
1. **Effets** : système d’effets d’interface utilisateur en temps réel dont les effets peuvent être animés, chaînés et personnalisés
1. **Animations** : animations expressives, indépendantes de l’infrastructure en cours d’exécution, indépendamment du thread d’interface utilisateur

### <a name="content"></a>Content

Le contenu est hébergé, transformé et peut être utilisé par le système d’effets et d’animations à l’aide d’éléments visuels. La classe [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) figure à la base de la hiérarchie de classes. Il s’agit d’un proxy léger, agile de thread dans le processus d’application pour l’état visuel du compositeur. Les sous-classes de Visual incluent  [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) pour permettre aux enfants de créer des arborescences d’éléments visuels et [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) qui contiennent du contenu et qui peuvent être peints avec des couleurs unies, du contenu dessiné personnalisé ou des effets visuels. Ensemble, ces types Visual constituent la structure de l’arborescence des éléments visuels de l’interface utilisateur 2D et soutiennent les éléments FrameworkElements XAML les plus visibles.

Pour plus d’informations, consultez la vue d’ensemble [Éléments visuels de composition](composition-visual-tree.md).

### <a name="effects"></a>Effets

Le système d’effets de la couche visuelle vous permet d’appliquer une chaîne d’effets de filtre et de transparence à un élément visuel ou à une arborescence d’éléments visuels. Il s’agit d’un système d’effets d’interface utilisateur, à ne pas confondre avec les effets d’image et multimédias. Les effets fonctionnent conjointement avec le système d’animations, ce qui permet aux utilisateurs d’obtenir des animations fluides et dynamiques des propriétés d’effet, dont le rendu est indépendant du thread d’interface utilisateur. Les effets de la couche visuelle fournissent les blocs de construction créative qui peuvent être combinés et animés pour créer des expériences personnalisées et interactives.

Outre les chaînes à effet animable, la couche visuelle prend en charge un modèle d’éclairage qui permet aux éléments visuels de reproduire des propriétés matérielles en répondant à des éclairages animables. Les éléments visuels peuvent également projeter des ombres. L’éclairage et les ombres peuvent être combinés pour créer une perception de profondeur et de réalisme.

Pour plus d’informations, consultez la vue d’ensemble [Effets de composition](composition-effects.md).

### <a name="animations"></a>Animations

Le système d’animations de la couche visuelle vous permet de déplacer des éléments visuels, d’animer des effets, et d’actionner des transformations, des clips et d’autres propriétés.  Il s’agit d’un système agnostique du cadre qui a été conçu dès le départ, avec des performances à l’esprit.  Il s’exécute indépendamment du thread d’interface utilisateur pour assurer la fluidité et l’évolutivité.  Bien qu’il vous permette d’utiliser des animations d’image clé familières pour piloter les modifications de propriétés dans le temps, il vous permet également de définir des relations mathématiques entre différentes propriétés, y compris les entrées utilisateur, vous permettant de créer directement des expériences chorégraphiés transparents.

Pour plus d’informations, consultez la vue d’ensemble [Animations de composition](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Utilisation de votre application UWP XAML

Vous pouvez accéder à un élément visuel créé par l’infrastructure XAML, et soutenant un élément FrameworkElement visible, à l’aide de la classe [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) de [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting). Notez que les éléments visuels créés pour vous par l’infrastructure présentent certaines limites de personnalisation. Cela tient au fait que l’infrastructure gère les décalages, les transformations et les durées de vie. Vous pouvez toutefois créer vos propres éléments visuels et les associer à un élément XAML existant via ElementCompositionPreview, ou en l’ajoutant à un élément ContainerVisual existant quelque part dans la structure de l’arborescence des éléments visuels.

Pour plus d’informations, consultez la vue d’ensemble [Utilisation de la couche visuelle avec XAML](using-the-visual-layer-with-xaml.md).

### <a name="working-with-your-desktop-app"></a>Utilisation de votre application de bureau

Vous pouvez utiliser la couche visuelle pour améliorer l’apparence, la convivialité et les fonctionnalités de vos applications de bureau WPF C++ , Windows Forms et Win32. Vous pouvez migrer des îlots de contenu pour utiliser la couche visuelle et conserver le reste de l’interface utilisateur dans son infrastructure existante. Cela signifie que vous pouvez apporter des mises à jour et des améliorations significatives à l’interface utilisateur de votre application sans avoir à apporter de modifications importantes à votre base de code existante.

Pour plus d’informations, consultez [Moderniser votre application de bureau à l’aide de la couche Visuel](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Ressources supplémentaires

* [**Documentation de référence complète sur l’API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples).
* [Galerie d’exemples Windows. UI. composition](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [flux Twitter @windowsui](https://twitter.com/windowsui)
* Lisez l’article MSDN de Kenny Kerr sur cette API : [Graphismes et animation : l’API Composition Windows passe à Windows 10](https://msdn.microsoft.com/magazine/mt590968)
