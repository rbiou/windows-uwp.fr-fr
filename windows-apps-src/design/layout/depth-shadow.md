---
author: serenaz
description: Z en profondeur, ou relatif profondeur et ombre existe deux façons de profondeur d’intégrer dans votre application pour aider les utilisateurs à se concentrer naturellement et plus efficacement.
title: Z en profondeur et ombre pour les applications UWP
template: detail.hbs
ms.author: sezhen
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: chigy
design-contact: balrayit
ms.localizationpriority: medium
ms.openlocfilehash: a1433b131b994ee2b1323909bc7c195e00f43cde
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4501019"
---
# <a name="z-depth-and-shadow"></a>Z en profondeur et ombre

![profondeur true](images/elevation-shadow/depth.svg)

Le système de profondeur Fluent utilise des concepts physiques, tels que 3D de positionnement, la lumière, et ombre réinventer numérique interface utilisateur permettre être perçu dans un environnement physique stratifié plus. Z en profondeur, ou relatif profondeur et ombre sont deux façons d’incorporer de profondeur dans votre application UWP.

## <a name="what-is-z-depth"></a>Qu’est profondeur z?

Z en profondeur est la distance entre deux surfaces le long de l’axe z, et il illustre comment fermer un objet est à l’Observateur d’événements.

![profondeur-z](images/elevation-shadow/elevation.svg)

### <a name="why-use-z-depth"></a>Pourquoi utiliser z en profondeur?

Dans le monde physique, nous ont tendance à se concentrer sur les objets qui sont plus proches nous. Nous pouvons appliquer ce instinct spatial à l’interface utilisateur numérique, également. Par exemple, si vous concevez un élément le plus proche pour l’utilisateur, puis l’utilisateur sera instinctivement centrée sur l’élément. Par déplacement éléments d’interface utilisateur plus proche de l’axe z, vous pouvez établir une hiérarchie visuelle entre les objets, aider les utilisateurs à effectuer des tâches naturellement et efficacement dans votre application. 

![z en profondeur dans le menu de contenu](images/elevation-shadow/whyelevation.svg)

En plus de fournir significative hiérarchie visuelle, z en profondeur permet également de vous permettent de créer expériences fluides en toute transparence de la 2D pour les environnements 3D, mise à l’échelle de votre application sur tous les périphériques et facteurs de forme. 

![z en profondeur en 2d pour 3d](images/elevation-shadow/elevation-2d3d.svg)

### <a name="how-is-z-depth-perceived"></a>Comment z en profondeur est perçue?

Selon la façon dont nous percevoir profondeur dans le monde physique, Voici plusieurs techniques qui peuvent être utilisés pour afficher la proximité dans l’interface utilisateur numérique.

- **Mise à l’échelle** Les objets loin apparaissent plus petits que les objets plus près de la même taille. Cette méthode est difficile à illustrer efficacement dans l’espace 2D, afin qu’il n’est généralement pas recommandé. Toutefois, vous pouvez utiliser la mise à l’échelle et [ombre](#what-is-shadow) pour créer une simulation efficace des objets en déplacement de plus près à l’utilisateur en 2D.

    ![proximité avec une échelle](images/elevation-shadow/elevation-scale.svg)

- **Atmosphère** Les objets peuvent apparaître plus loin et en dehors de la mise au point avec une superposition «enfumée» ou un autre effet atmosphérique.

    ![proximité avec atmosphère](images/elevation-shadow/elevation-atmosphere.svg)

- **Mouvement** Vitesse relative peut être utilisée pour illustrer la proximité: déplacent des objets plus proches plus rapidement que les objets distants en arrière-plan. Pour savoir comment implémenter cet effet, consultez [parallaxe](../motion/parallax.md).

    ![proximité avec le mouvement](images/elevation-shadow/elevation-motion.svg)

### <a name="recommendations-for-z-depth"></a>Recommandations en matière de profondeur-z

Réduire le nombre de plans avec élévation de privilèges pour fournir le focus visuel claire. Pour la plupart des scénarios, deux plans est suffisant: un pour les éléments de premier plan (proximité élevée) et une autre pour les éléments de l’arrière-plan (proximité faible). Si vous avez plusieurs éléments avec élévation de privilèges qui ne se chevauchent pas, les regrouper le même plan (autrement dit, le premier plan) afin de réduire le nombre de plans.

![z en profondeur au sein d’une application](images/elevation-shadow/app-depth.svg)

## <a name="what-is-shadow"></a>Nouveautés d’ombre?

![ombre](images/elevation-shadow/shadow.svg)

Ombre est un moyen de percevoir une élévation. Lorsqu’il est lumière au-dessus d’un objet avec élévation de privilèges, il existe une ombre sur la surface ci-dessous. Plus l’objet, la plus grande et plus l’ombre devient. Notez que les objets avec élévation de privilèges ne doivent avoir des ombres, mais les ombres indiquent une élévation.

Dans les applications UWP, les ombres doivent être délibérée, pas esthétique. Si les ombres détournent l’attention de mise au point et la productivité, puis limiter l’utilisation de l’ombre.

Vous pouvez utiliser des ombres avec la ThemeShadow ou DropShadow APIs.

## <a name="themeshadow"></a>ThemeShadow

Le ThemeShadow type peut être appliqué à n’importe quel élément XAML pour dessiner des ombres en fonction de manière appropriée sur x, y, z coordonnées. ThemeShadow ajuste automatiquement pour les autres conditions ambiantes:

- S’adapte aux changements de l’éclairage, thème de l’utilisateur, environnement de l’application et interpréteur de commandes.
- Ombres automatiquement en fonction de leur élévation des éléments.
- Maintient les éléments synchronisées comme ils de déplacement et de modifier une élévation.
- Conserve les ombres cohérent tout au long d’et entre les applications.

Voici quelques exemples des ThemeShadow à différents élévations avec les thèmes clair et foncées:

![ombres actives avec le thème clair](images/elevation-shadow/smartshadow-light.svg)

![ombres actives avec le thème foncé](images/elevation-shadow/smartshadow-dark.svg)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow contrôles communs

Les contrôles communs utilisent automatiquement ThemeShadow pour projeter des ombres:

- [Boîtes de dialogue et menus volants](../controls-and-patterns/dialogs.md)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [Contrôle de transport de média](../controls-and-patterns/media-playback.md)
- [Menu contextuel](../controls-and-patterns/menus.md)
- [Barre de commandes](../controls-and-patterns/app-bars.md)
- [Suggestion automatique](../controls-and-patterns/auto-suggest-box.md), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), les [sélecteurs de calendrier/Date/heure](../controls-and-patterns/date-and-time.md), [info-bulle](../controls-and-patterns/tooltips.md)
- [Touches d’accès rapide](../input/access-keys.md)

### <a name="themeshadow-in-popups"></a>ThemeShadow dans des fenêtres contextuelles

ThemeShadow convertit automatiquement les ombres lorsqu’il est appliqué à n’importe quel élément XAML dans un [menu contextuel](/uwp/api/windows.ui.xaml.controls.primitives.popup). Il sera projeter des ombres sur le contenu en arrière-plan derrière elle et n’importe quel autres ouvrir les fenêtres contextuelles juste en dessous.

Pour utiliser ThemeShadow avec fenêtres contextuelles, utilisez le `Shadow` propriété à laquelle appliquer un ThemeShadow à un élément XAML. Ensuite, élever l’élément à partir d’autres éléments d’arrière-plan, par exemple en utilisant le composant z de le `Translation` propriété.
Pour la plupart des interface utilisateur du menu contextuel, l’élévation par défaut recommandé par rapport à du contenu en arrière-plan de l’application est de 32 pixels effectifs.

Cet exemple montre un Rectangle dans une fenêtre contextuelle projette une ombre sur le contenu en arrière-plan et tous les autres menus contextuels derrière elle:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="White" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![clichés instantanés à partir de l’exemple de code](images/elevation-shadow/smartshadow-example.svg)

### <a name="themeshadow-in-other-elements"></a>ThemeShadow dans d’autres éléments

Pour effectuer un cast une ombre à partir d’un élément XAML qui n’est pas dans un menu contextuel, vous devez spécifier explicitement les autres éléments qui peuvent recevoir l’ombre dans le `ThemeShadow.Receivers` collection.

Cet exemple montre deux boutons qui projettent des ombres sur une grille derrière les:

```xaml
<Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Button x:Name="Button1" Content="Button 1" Shadow="{StaticResource SharedShadow}" Margin="10" />

    <Button x:Name="Button2" Content="Button 2" Shadow="{StaticResource SharedShadow}" Margin="120" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Button1.Translation += new Vector3(0, 0, 16);
Button2.Translation += new Vector3(0, 0, 32);
```

### <a name="performance-best-practices-for-themeshadow"></a>Recommandations en matière de performances pour ThemeShadow

1. Limitez le nombre d’éléments de récepteur personnalisée au minimum nécessaire. 

2. Si plusieurs éléments récepteur sont à l’élévation même essayer de les combiner en ciblant un élément parent unique à la place.

3. Si plusieurs éléments effectuer un cast le même type d’ombre sur les mêmes éléments récepteur ajouter l’ombre comme une ressource partagée et le réutiliser.

## <a name="drop-shadow"></a>Ombre portée

DropShadow n’est pas réactive automatiquement à son environnement et n’utilise pas de sources de lumière. Par exemple les implémentations, consultez la [Classe de DropShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Lequel les clichés instantanés dois-je utiliser?

| Propriété | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | RS5 | 14393 |
| **Capacité d’adaptation** | Oui | Non |
| **Personnalisation** | Non | Oui |
| **Source de lumière** | Automatique (globale par défaut, mais peut remplacer par application) | None |
| **Prise en charge dans les environnements 3D** | Oui | Non |

- En règle générale, nous vous recommandons d’utiliser ThemeShadow, qui s’adapte automatiquement à son environnement.
- Si vous avez plus avancées des scénarios d’ombres personnalisées, utilisez DropShadow, ce qui permet une plus grande personnalisation.
- Pour vers l’arrière compatibilité, utilisez DropShadow.
- Pour les problèmes de performances, limiter le nombre d’ombres ou utilisez DropShadow.
- Sur HMDs en 3D true, utilisez ThemeShadow. Dans la mesure où DropShadow dessine avec un décalage spécifié à partir de l’élément visuel. elle est apparentée à, à partir du côté, il se présente comme il flotte dans l’espace. En revanche, ThemeShadow est restituée au-dessus les effets visuels définis en tant que récepteurs.