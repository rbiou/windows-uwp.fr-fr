---
title: Mode Fractionné
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Un contrôle de mode Fractionné inclut un volet pouvant être développé/réduit ainsi qu’une zone de contenu.
label: Split view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e5df29f4ce91bd272fd921b23b5fc05b6655d4c0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081541"
---
# <a name="split-view-control"></a>Contrôle de mode fractionné

Un contrôle de mode Fractionné inclut un volet pouvant être développé/réduit ainsi qu’une zone de contenu.

> **API importantes** : [classe SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView)

Voici un exemple de l’application Microsoft Edge utilisant SplitView pour afficher son Hub.

![Exemple de mode Fractionné Microsoft Edge](images/split_view_Edge.png)


 La zone de contenu du mode Fractionné est toujours visible. Le volet peut être développé ou réduit ou rester ouvert, et peut s’afficher à gauche ou à droite de la fenêtre d’application. Le volet comporte quatre modes :

-   **Overlay**

    Le volet est masqué jusqu’à ce qu’il soit ouvert. Lorsqu’il est ouvert, il recouvre la zone de contenu.

-   **Inline**

    Le volet est toujours visible et ne recouvre pas la zone de contenu. Les zones de volet et de contenu divisent l’espace disponible à l’écran.

-   **CompactOverlay**

    Une mince partie du volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes. Par défaut, la largeur du volet fermé est de 48 pixels et peut être modifiée avec `CompactPaneLength`. Si le volet est ouvert, il se superpose à la zone de contenu.

-   **CompactInline**

    Une mince partie du volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes. Par défaut, la largeur du volet fermé est de 48 pixels et peut être modifiée avec `CompactPaneLength`. Si le volet est ouvert, l’espace disponible pour le contenu est réduit, le poussant en dehors.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Le contrôle de mode Fractionné peut servir à créer toute expérience « à tiroirs » (c’est-à-dire que les utilisateurs peuvent ouvrir et fermer le volet supplémentaire). Par exemple, vous pouvez utiliser SplitView pour créer le modèle [maître/détails](master-details.md).

Si vous voulez créer un menu de navigation avec un bouton développer/réduire et une liste d’éléments de navigation, utilisez le contrôle [NavigationView](navigationview.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/SplitView">ouvrir l’application et voir l’objet SplitView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-split-view"></a>Créer une vue en mode Fractionné

Voici un contrôle SplitView avec un volet ouvert, qui s’affiche en ligne en regard du contenu.
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes
- [Modèle de volet de navigation](navigationview.md)
- [Vue Liste](lists.md)
- [Maître/détails](master-details.md)