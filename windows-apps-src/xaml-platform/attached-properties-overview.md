---
description: Explique le concept de propriété jointe en XAML et fournit quelques exemples.
title: Vue d’ensemble des propriétés jointes
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: d3892857baa29e2275845cb077e5ad9ea3166ada
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340604"
---
# <a name="attached-properties-overview"></a>Vue d’ensemble des propriétés jointes

Une *propriété jointe* est un concept XAML. Les propriétés jointes permettent de définir des paires propriété/valeur supplémentaires sur un objet, mais les propriétés ne font pas partie de la définition d’objet d’origine. Les propriétés jointes sont généralement définies comme une forme spécialisée de propriété de dépendance dont le modèle objet du type du propriétaire ne comporte pas de wrapper de propriété conventionnel.

## <a name="prerequisites"></a>Conditions préalables

Nous supposons que vous connaissez les principes de base des propriétés de dépendance et que vous avez lu le document [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md).

## <a name="attached-properties-in-xaml"></a>Propriétés jointes en XAML

En XAML, les propriétés jointes sont définies à l’aide de la syntaxe _AttachedPropertyProvider.PropertyName_. Voici un exemple montrant comment définir [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) en XAML.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> Nous utilisons simplement [**Canvas. Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) comme exemple de propriété jointe sans expliquer en détail pourquoi vous pouvez l’utiliser. Si vous voulez en savoir plus sur le rôle de **Canvas.Left** et la façon dont [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) gère ses enfants de disposition, voir la rubrique de référence [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) ou [Définir des dispositions avec XAML](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml).

## <a name="why-use-attached-properties"></a>Pourquoi utiliser des propriétés jointes ?

Les propriétés jointes sont un moyen d’échapper aux conventions de codage qui pourraient empêcher différents objets d’une relation d’échanger des informations au moment de l’exécution. Il est certainement possible de placer des propriétés sur une classe de base courante afin que chaque objet puisse juste obtenir et définir ces propriétés. Mais au final, le nombre considérable de scénarios où vous souhaiterez le faire encombrera vos classes de base de propriétés partageables. Cette approche pourrait même créer des situations où seuls deux des centaines de descendants essaieraient d’utiliser une propriété. Cela ne constitue pas une bonne conception de classe. Pour résoudre ce problème, le concept des propriétés jointes permet à un objet d’affecter une valeur pour une propriété que sa propre structure de classe ne définit pas. La classe de définition peut lire cette valeur à partir des objets enfants au moment de l’exécution, après que les différents objets ont été créés dans une arborescence d’objets.

Par exemple, des éléments enfants peuvent utiliser des propriétés jointes pour indiquer à leur élément parent la manière dont ils doivent être présentés dans l’interface utilisateur. C’est le cas avec la propriété jointe [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left). **Canvas.Left** est créée en tant que propriété jointe car elle est définie au niveau d’éléments contenus dans un élément [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), et non dans la classe **Canvas** proprement dite. Tous les éléments enfants possibles utilisent alors **Canvas.Left** et [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) pour spécifier leur décalage de disposition au sein du parent du conteneur de disposition **Canvas**. Les propriétés jointes permettent à ce scénario de fonctionner sans encombrer le modèle objet de l’élément de base de nombreuses propriétés qui s’appliquent chacune uniquement à un des nombreux conteneurs de disposition possibles. Au lieu de cela, la plupart des conteneurs de disposition implémentent leur propre jeu de propriétés jointes.

Pour implémenter la propriété jointe, la classe [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) définit un champ statique [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) nommé [**Canvas.LeftProperty**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.leftproperty). Ensuite, **Canvas** fournit les méthodes [**SetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.setleft) et [**GetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.getleft) comme accesseurs publics pour la propriété jointe, afin de permettre l’accès au paramètre XAML et à la valeur au moment de l’exécution. Pour XAML et le système de propriétés de dépendance, cet ensemble d’API obéit à un schéma qui autorise une syntaxe XAML spécifique pour les propriétés jointes et stocke la valeur dans la banque de propriétés de dépendance.

## <a name="how-the-owning-type-uses-attached-properties"></a>Utilisation des propriétés jointes par le type propriétaire

Bien que les propriétés jointes puissent être définies sur n’importe quel élément XAML (ou tout [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) sous-jacent), cela ne veut pas dire nécessairement que leur définition produit un résultat concret ou que leur valeur sera un jour utilisée. Le type qui définit la propriété jointe suit généralement l’un des scénarios suivants :

- Le type qui définit la propriété jointe est le parent dans une relation entre d’autres objets. Les objets enfants définissent les valeurs de la propriété jointe. Le type de propriétaire de la propriété jointe a un comportement inné qui itère au sein de ses éléments enfants, obtient les valeurs et agit sur ces valeurs à un moment donné dans la durée de vie des objets (une action de disposition, [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged), etc.)
- Le type qui définit la propriété jointe est utilisé en tant qu’élément enfant pour divers éléments parents et modèles de contenu possibles, mais les informations ne sont pas nécessairement des informations de disposition.
- La propriété jointe communique des informations à un service, et non à un autre élément d’interface utilisateur.

Pour plus d’informations sur ces scénarios et les types propriétaires, voir la section « En savoir plus sur Canvas.Left » de la rubrique [Propriétés jointes personnalisées](custom-attached-properties.md).

## <a name="attached-properties-in-code"></a>Propriétés jointes dans le code

Contrairement à d’autres propriétés de dépendance, les propriétés jointes ne disposent pas des wrappers de propriété classiques qui facilitent l’accès get et set. Cela est dû au fait que la propriété jointe ne fait pas nécessairement partie du modèle objet centré sur le code pour les instances où la propriété est définie. (Il est également possible, même si cela est rare, de définir une propriété qui est une propriété jointe définissable dans d’autres types et qui offre une utilisation de propriété classique dans le type propriétaire.)

Il existe deux façons de définir une propriété jointe dans le code : soit en utilisant les API du système de propriétés, soit en utilisant les accesseurs de modèle XAML. Ces techniques offrant des résultats très proches, le choix de l’une ou de l’autre est dicté essentiellement par le style de codage.

### <a name="using-the-property-system"></a>Utilisation du système de propriétés

Pour le Windows Runtime, les propriétés jointes sont implémentées en tant que propriétés de dépendance. De ce fait, le système de propriétés peut stocker les valeurs dans la banque de propriétés de dépendance partagée. Par conséquent, les propriétés jointes exposent un identificateur de propriété de dépendance dans la classe propriétaire.

Pour définir une propriété jointe dans le code, vous devez appeler la méthode [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) et passer le champ [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty), qui fait office d’identificateur pour cette propriété jointe. (Vous devez aussi passer la valeur à définir.)

Pour obtenir la valeur d’une propriété jointe dans le code, vous devez appeler la méthode [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) en passant à nouveau le champ [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) qui sert d’identificateur.

### <a name="using-the-xaml-accessor-pattern"></a>Utilisateur du modèle d’accesseur XAML

Un processeur XAML doit être en mesure de définir des valeurs de propriétés jointes lorsque le code XAML est analysé dans une arborescence d’objets. Le type propriétaire de la propriété jointe doit implémenter des méthodes d’accesseur dédiées nommées sous la forme **Get**_PropertyName_ et **Set**_PropertyName_. Ces méthodes d’accesseur dédiées offrent aussi un moyen d’obtenir ou de définir la propriété jointe dans le code. Du point de vue du code, une propriété jointe s’apparente à un champ de stockage doté d’accesseurs de méthode et non d’accesseurs de propriété, et ce champ de stockage peut exister dans n’importe quel objet au lieu de devoir être spécifiquement défini.

L’exemple suivant montre comment définir une propriété jointe dans du code via l’API d’accesseur XAML. Dans cet exemple, `myCheckBox` est une instance de la classe [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox). La dernière ligne correspond au code qui définit à proprement parler la valeur, tandis que les lignes qui la précèdent ne font qu’établir les instances et leur relation parent-enfant. La dernière ligne sans marques de commentaires est la syntaxe si vous utilisez le système de propriétés. La dernière ligne avec marques de commentaires est la syntaxe si vous utilisez le modèle d’accesseur XAML.

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>Propriétés jointes personnalisées

Pour obtenir des exemples de code illustrant comment définir des propriétés jointes personnalisées et pour plus d’informations sur les scénarios d’utilisation de propriété jointe, voir [Propriétés jointes personnalisées](custom-attached-properties.md).

## <a name="special-syntax-for-attached-property-references"></a>Syntaxe spéciale pour les références aux propriétés jointes

Le point figurant dans un nom de propriété jointe est un élément clé du modèle d’identification. Des ambiguïtés surviennent parfois quand une syntaxe ou une situation prête au point une toute autre signification. Par exemple, un point est considéré comme une traversée de modèle objet dans le cas d’un chemin de liaison. Dans la plupart des cas impliquant une telle ambiguïté, une propriété jointe est assortie d’une syntaxe spéciale qui permet au point intérieur d’être quand même analysé comme séparateur _owner_ **.** _property_ d’une propriété jointe.

- Pour spécifier une propriété jointe comme faisant partie du chemin cible d’une animation, mettez le nom de la propriété jointe entre parenthèses ("()") par exemple "(Canvas.Left)". Pour plus d’informations, voir [Syntaxe de PropertyPath](property-path-syntax.md).

> [!WARNING]
> Une limitation existante de l’implémentation XAML Windows Runtime est que vous ne pouvez pas animer une propriété jointe personnalisée.

- Pour spécifier une propriété jointe en tant que propriété cible d’une référence de ressource d’un fichier de ressources à **x :uid**, utilisez une syntaxe spéciale qui injecte une déclaration de code, entièrement qualifié **à l’aide de :** dans les crochets («\[\]»), afin de créer un saut de portée délibérée. Par exemple, en supposant qu’il existe un élément `<TextBlock x:Uid="Title" />`, la clé de ressource dans le fichier de ressources qui cible la valeur **Canvas. Top** de cette instance est « title ».\[à l’aide de : Windows. UI. Xaml. Controls\]Canvas. Top». Pour plus d’informations sur les fichiers de ressources et le code XAML, voir [Démarrage rapide : traduction des ressources d’interface utilisateur](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10)).

## <a name="related-topics"></a>Rubriques connexes

- [Propriétés jointes personnalisées](custom-attached-properties.md)
- [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
- [Définir des dispositions avec XAML](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml)
- [Démarrage rapide : traduction des ressources de l’interface utilisateur](https://docs.microsoft.com/previous-versions/windows/apps/hh943060(v=win.10))
- [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue)
- [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue)
