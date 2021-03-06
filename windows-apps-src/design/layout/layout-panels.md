---
Description: Utilisez les panneaux de disposition pour organiser et regrouper des éléments d’interface utilisateur dans votre application.
title: Panneaux de disposition des applications Windows
ms.date: 04/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5a796f95efb418f7d70062ca2d73bbea7220d95
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234857"
---
# <a name="layout-panels"></a>Panneaux de disposition

Les panneaux de disposition sont des conteneurs qui vous permettent d’organiser et de regrouper des éléments d’interface utilisateur dans votre application. Les panneaux de disposition XAML intégrés incluent [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel), [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), [**VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) et [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas). Nous décrivons ici chaque panneau et la manière de les utiliser pour disposer des éléments d'interface utilisateur XAML.

Plusieurs éléments sont à prendre en considération lors du choix d’un panneau de disposition :
- la manière dont le panneau positionne ses éléments enfants ;
- la manière dont le panneau dimensionne ses éléments enfants ;
- la manière dont les éléments enfants superposés s’empilent les uns sur les autres (ordre de plan) ;
- le nombre et la complexité des éléments de panneau imbriqués nécessaires pour créer la disposition souhaitée.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, consultez les panneaux <a href="xamlcontrolsgallery:/item/RelativePanel">RelativePanel</a>, <a href="xamlcontrolsgallery:/item/StackPanel">StackPanel</a>, <a href="xamlcontrolsgallery:/item/Grid">Grid</a>, <a href="xamlcontrolsgallery:/item/VariableSizedWrapGrid">VariableSizedWrapGrid</a> et <a href="xamlcontrolsgallery:/item/Canvas">Canvas</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="panel-properties"></a>Propriétés des panneaux

Avant d’aborder les panneaux individuels, étudions certaines propriétés communes à tous les panneaux. 

### <a name="panel-attached-properties"></a>Propriétés jointes du panneau

La plupart des panneaux de disposition XAML utilisent des propriétés jointes pour permettre à leurs éléments enfants d’informer le panneau parent sur la manière dont ils doivent être placés dans l’interface utilisateur. Les propriétés jointes utilisent la syntaxe *AttachedPropertyProvider.PropertyName*. Si des panneaux sont imbriqués dans d’autres panneaux, les propriétés jointes des éléments d’interface utilisateur spécifiant les caractéristiques de disposition à un parent sont interprétées par le panneau parent le plus proche uniquement.

Voici un exemple de la façon dont vous pouvez définir la propriété jointe [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) sur un contrôle Button en XAML. Cela informe l’élément Canvas parent que le contrôle Button doit être positionné à 50 pixels effectifs du bord gauche de l’élément Canvas.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Pour plus d’informations sur les propriétés jointes, voir [Vue d’ensemble des propriétés jointes](../../xaml-platform/attached-properties-overview.md).

### <a name="panel-borders"></a>Bordures des panneaux

Les panneaux RelativePanel, StackPanel et Grid définissent les propriétés de bordure qui vous permettent de dessiner une bordure autour du panneau sans les encapsuler dans un autre élément Border. Les propriétés de bordure sont **BorderBrush**, **BorderThickness**, **CornerRadius** et **Padding**.

Voici un exemple illustrant comment définir les propriétés de bordure sur un élément Grid.

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![Un élément Grid avec des bordures](images/layout-panel-grid-border.png)

L’utilisation des propriétés de bordure intégrées réduit le nombre d’éléments XAML, ce qui peut améliorer les performances de l’interface utilisateur de votre application. Pour plus d’informations sur les panneaux de disposition et les performances de l’interface utilisateur, voir [Optimiser votre disposition XAML](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-your-xaml-layout).

## <a name="relativepanel"></a>RelativePanel

Le panneau [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) vous permet de disposer des éléments d’interface utilisateur en spécifiant leur emplacement en fonction d’autres éléments et par rapport au panneau. Par défaut, un élément est positionné dans le coin supérieur gauche du panneau. Vous pouvez utiliser RelativePanel avec [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) et [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) pour réorganiser votre interface utilisateur pour différentes tailles de fenêtre.

Ce tableau indique les propriétés jointes que vous pouvez utiliser pour aligner un élément par rapport au panneau ou à d’autres éléments.

Alignement du panneau | Alignement frère | Position sœur
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithpanelproperty) | [**AlignTopWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithproperty) | [**Above**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)  
[**AlignBottomWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithpanelproperty) | [**AlignBottomWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithproperty) | [**Below**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.belowproperty)  
[**AlignLeftWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel) | [**AlignLeftWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.getalignleftwith) | [**LeftOf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.leftofproperty)  
[**AlignRightWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithpanelproperty) | [**AlignRightWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithproperty) | [**RightOf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.setrightof)  
[**AlignHorizontalCenterWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) | [**AlignHorizontalCenterWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithproperty) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanelproperty) | [**AlignVerticalCenterWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithproperty) | &nbsp;   

 
Ce code XAML montre comment organiser des éléments dans un élément RelativePanel.

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

Le résultat se présente ainsi : 

![Panneau relatif](images/layout-panel-relative-panel.png)

Voici quelques informations à noter concernant le dimensionnement des rectangles :
- Le rectangle rouge est fourni dans une taille explicite de 44 x 44. Il est placé dans le coin supérieur gauche du panneau, qui est la position par défaut.
- Le rectangle vert est fourni dans une hauteur explicite de 44. Le côté gauche est aligné avec le rectangle rouge, et son côté droit est aligné avec le rectangle bleu, ce qui détermine sa largeur.
- Le rectangle orange n’a pas de taille explicite attribuée. Son côté gauche est aligné avec le rectangle bleu. Ses bords droit et inférieur sont alignés avec le bord du panneau. Sa taille est déterminée par ces alignements et elle est redimensionnées lorsque le panneau est redimensionné.

## <a name="stackpanel"></a>StackPanel

[ **StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) organise ses éléments enfants sur une seule ligne orientable horizontalement ou verticalement. StackPanel est généralement utilisé pour organiser une petite sous-section de l’interface utilisateur sur une page.

Vous pouvez utiliser la propriété [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) pour préciser l’orientation des éléments enfants. L’orientation par défaut est [**Vertical**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Orientation).

Le code XAML suivant indique comment créer un empilement StackPanel vertical des éléments.

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


Le résultat se présente ainsi :

![Panneau d’empilement](images/layout-panel-stack-panel.png)

Dans un panneau StackPanel, si la taille d’un élément enfant n’est pas définie explicitement, ce dernier s’étire pour remplir la largeur disponible (ou la hauteur si la propriété Orientation est définie sur **Horizontal**). Dans cet exemple, la largeur des rectangles n’est pas définie. Les rectangles se développent afin d’occuper toute la largeur du panneau StackPanel.

## <a name="grid"></a>Grille

Le panneau [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) prend en charge les dispositions fluides et vous permet d’organiser des contrôles dans des dispositions constituées de plusieurs lignes et colonnes. Vous pouvez spécifier les lignes et les colonnes d’un panneau Grid à l’aide des propriétés [**RowDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) et [**ColumnDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.columndefinitions).

Pour positionner les objets dans des cellules spécifiques du panneau Grid, utilisez les propriétés jointes [**Grid.Column**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.column) et [**Grid.Row**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.row).

Pour forcer le contenu à s’étendre sur plusieurs lignes et colonnes, utilisez les propriétés jointes [**Grid.RowSpan**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms605035(v=vs.95)) et [**Grid.ColumnSpan**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.columnspan).

Cet exemple XAML montre comment créer un élément Grid à deux lignes et deux colonnes.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


Le résultat se présente ainsi :

![Grille](images/layout-panel-grid.png)

Dans cet exemple, le dimensionnement fonctionne comme suit. 
- La deuxième ligne a une hauteur explicite de 44 pixels effectifs. Par défaut, la hauteur de la première ligne remplit l’espace restant disponible.
- La largeur de la première colonne est définie sur **Auto**, de manière à être suffisamment large pour ses enfants. Dans ce cas, elle est de 44 pixels efficaces pour s’adapter à la largeur du rectangle rouge.
- Il n’existe aucune autre contrainte de taille sur les rectangles, de manière à ce que chacun d’entre eux s’étire pour remplir la cellule de grille qui le contient.

Vous pouvez répartir l’espace au sein d’une colonne ou d’une ligne en utilisant le redimensionnement **Auto** ou proportionnel. Le dimensionnement automatique permet de redimensionner les éléments d’interface pour qu’ils s’adaptent à leur contenu ou à leur conteneur parent. Vous pouvez également utiliser le dimensionnement automatique avec les lignes et les colonnes d’une grille. Pour utiliser le dimensionnement automatique, définissez la propriété Height et/ou Width des éléments d’interface utilisateur sur **Auto**.

Le *dimensionnement proportionnel* sert à répartir l’espace disponible entre les lignes et les colonnes d’une grille par proportions pondérées. En XAML, les valeurs proportionnelles sont exprimées par \* (ou *n*\* pour le dimensionnement proportionnel pondéré). À titre d’exemple, dans une disposition à deux colonnes, pour spécifier qu’une colonne est cinq fois plus large que l’autre colonne, utilisez « 5\* » et « \* » pour les propriétés [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.columndefinition.width) des éléments [**ColumnDefinition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition).

Cet exemple combine le dimensionnement fixe, automatique et proportionnel dans un élément [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) avec 4 colonnes.

&nbsp;|&nbsp;|&nbsp;
------|------|------
Colonne_1 | **Auto** | La taille de la colonne s’adaptera à son contenu.
Colonne_2 | * | Une fois les colonnes Auto calculées, la colonne conserve une partie de la largeur restante. Colonne_2 sera deux fois moins large que Colonne_4.
Colonne_3 | **44** | La colonne aura une largeur de 44 pixels.
Colonne_4 | **2**\* | Une fois les colonnes Auto calculées, la colonne conserve une partie de la largeur restante. Colonne_4 sera deux fois plus large que Colonne_2.

La largeur par défaut de la colonne est « * », de sorte que vous n’avez pas besoin de définir explicitement cette valeur pour la deuxième colonne.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Dans le concepteur XAML de Visual Studio, le résultat se présente comme suit.

![Grille de 4 colonnes dans le concepteur Visual Studio](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[ **VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) est un panneau de disposition de style Grid dans lequel les lignes ou les colonnes sont automatiquement renvoyées à la ligne ou dans une nouvelle colonne lorsque la valeur [**MaximumRowsOrColumns**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns) est atteinte. 

La propriété [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.orientation) spécifie si la grille ajoute ses éléments en lignes ou en colonnes avant leur renvoi. L’orientation par défaut est **Vertical**, ce qui signifie que la grille ajoute des éléments de haut en bas jusqu’à ce qu’une colonne soit remplie, puis passe à une nouvelle colonne. Lorsque la valeur est **Horizontal**, la grille ajoute des éléments de gauche à droite, puis passe à la ligne suivante.

Les dimensions des cellules sont spécifiées par [**ItemHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight) et [**ItemWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth). Chaque cellule a la même taille. Si les valeurs ItemHeight ou ItemWidth ne sont pas spécifiées, la première cellule est redimensionnée pour s’adapter à son contenu, et toutes les autres cellules prennent la même taille.

Vous pouvez utiliser les propriétés jointes [**VariableSizedWrapGrid.ColumnSpan**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid) et [**VariableSizedWrapGrid.RowSpan**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.getrowspan) pour spécifier le nombre de cellules adjacentes devant être remplies par un élément enfant.

Voici comment utiliser un élément VariableSizedWrapGrid en XAML.

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


Le résultat se présente ainsi :

![Grille avec renvoi à la ligne à taille variable](images/layout-panel-variable-size-wrap-grid.png)

Dans cet exemple, le nombre maximal de lignes dans chaque colonne est 3. La première colonne contient seulement 2 éléments (les rectangles rouge et bleu), car le rectangle bleu s'étend sur 2 lignes. Le rectangle vert s’étend ensuite jusqu’en haut de la colonne suivante.

## <a name="canvas"></a>Canevas

Le panneau [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) positionne ses éléments enfants à l’aide de points de coordonnées fixes et ne prend pas en charge les dispositions fluides. Vous spécifiez les points des éléments enfants individuels en définissant les propriétés jointes [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) et [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) de chaque élément. L’élément Canvas parent lit les valeurs des propriétés jointes de ses enfants pendant la transmission de disposition [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange).

Dans un élément Canvas, les objets peuvent se chevaucher, auquel cas un objet est dessiné sur un autre objet. Par défaut, l’élément Canvas restitue les objets enfants dans l’ordre dans lequel ils sont déclarés, de sorte que le dernier enfant est restitué en haut (chaque élément a une valeur ZIndex par défaut de 0). Il en va de même pour les autres panneaux intégrés. Toutefois, l’élément Canvas prend également en charge la propriété jointe [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) que vous pouvez définir sur chacun des éléments enfants. Vous pouvez définir cette propriété dans le code pour changer l’ordre de dessin des éléments pendant l’exécution. L’élément avec la valeur Canvas.ZIndex la plus élevée est dessiné en dernier. Ainsi, il est dessiné au-dessus des autres éléments qui partagent le même espace ou qui se chevauchent. Notez que la valeur alpha (transparence) est respectée. Ainsi, même si des éléments se chevauchent, le contenu affiché dans les zones de chevauchement peut être fusionné si le contenu supérieur a une valeur alpha non maximale.

L’élément Canvas ne procède à aucun redimensionnement de ses enfants. Chaque élément doit spécifier sa taille.

Voici un exemple d’élément Canvas en XAML.

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Le résultat se présente ainsi :

![Canevas](images/layout-panel-canvas.png)

Utilisez le panneau Canvas en fonction de vos besoins. Bien qu’il soit pratique de pouvoir contrôler précisément les positions des éléments de l’interface utilisateur dans certains scénarios, un panneau de disposition positionné de manière fixe rend cette zone de l’interface utilisateur moins adaptable à l’ensemble des modifications de taille de fenêtre de l’application. Le redimensionnement des fenêtres d’application peut être dû au changement d’orientation de l’appareil, au fractionnement des fenêtres d’application, au changement de moniteur, ainsi qu’à plusieurs autres scénarios utilisateur.

## <a name="panels-for-itemscontrol"></a>Panneaux pour ItemsControl

Il existe plusieurs panneaux à usage spécifique qui peuvent être utilisés uniquement comme un contrôle [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) pour afficher des éléments dans un contrôle [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl). Il s’agit de [**ItemsStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel), [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid), [**VirtualizingStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel) et [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid). Vous ne pouvez pas utiliser ces panneaux pour créer une disposition générale de l’interface utilisateur.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.
