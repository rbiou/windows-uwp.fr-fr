---
description: Explique comment implémenter une propriété jointe XAML en tant que propriété de dépendance et comment définir la convention d’accesseur nécessaire pour que votre propriété jointe soit utilisable en XAML.
title: Propriétés jointes personnalisées
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.date: 07/18/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f23d66acc9371fd7b23b6770a0c7be6d16f86be4
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340615"
---
# <a name="custom-attached-properties"></a>Propriétés jointes personnalisées

Une *propriété jointe* est un concept XAML. Les propriétés jointes sont généralement définies comme une forme spécialisée de propriété de dépendance. Cette rubrique explique comment implémenter une propriété jointe en tant que propriété de dépendance et comment définir la convention d’accesseur nécessaire pour que votre propriété jointe soit utilisable en XAML.

## <a name="prerequisites"></a>Prérequis

Nous supposons que vous comprenez les propriétés de dépendance du point de vue d’un consommateur de propriétés de dépendance existantes et que vous avez lu la [vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md). Vous devez aussi avoir lu la [vue d’ensemble des propriétés jointes](attached-properties-overview.md). Pour suivre les exemples de cette rubrique, vous devez également comprendre le langage XAML et savoir comment écrire une application Windows Runtime de base en C++, C# ou Visual Basic.

## <a name="scenarios-for-attached-properties"></a>Scénarios de propriétés jointes

Vous pourriez créer une propriété jointe quand il y a une raison de disposer d’un mécanisme de définition de propriété pour les classes autres que la classe de définition. Les scénarios les plus courants concernent la prise en charge des services et de la disposition. [  **Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) et [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) sont des exemples de propriétés de disposition existantes. Dans un scénario de disposition, les éléments qui existent en tant qu’éléments enfants d’éléments de contrôle de disposition peuvent exprimer des exigences de disposition à leurs éléments parents individuellement, chacun définissant une valeur de propriété que le parent définit comme une propriété jointe. Un exemple de scénario de prise en charge de services dans l’API Windows Runtime est l’ensemble de propriétés jointes de [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), tel que [**ScrollViewer.IsZoomChainingEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoomchainingenabled).

> [!WARNING]
> Une limitation existante de l’implémentation XAML de Windows Runtime est que vous ne pouvez pas animer votre propriété jointe personnalisée.

## <a name="registering-a-custom-attached-property"></a>Inscription d’une propriété jointe personnalisée

Si vous définissez la propriété jointe pour une utilisation strictement sur d’autres types, il n’est pas obligatoire que la classe dans laquelle la propriété est inscrite dérive de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). En revanche, vous devez faire en sorte que le paramètre cible des accesseurs utilise **DependencyObject** si vous suivez le modèle ordinaire selon lequel votre propriété jointe est également une propriété de dépendance, de manière à pouvoir utiliser la banque de propriétés de stockage.

Définissez votre propriété jointe en tant que propriété de dépendance en déclarant une propriété **ReadOnly** **statique** **publique** de type [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty). Vous définissez cette propriété à l’aide de la valeur de retour de la méthode [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached). Le nom de la propriété doit correspondre au nom de la propriété jointe que vous spécifiez comme paramètre **RegisterAttached** *Name* , avec la chaîne « Property » ajoutée à la fin. Il s’agit de la convention établie pour l’affectation de noms aux identificateurs de propriétés de dépendance en fonction des propriétés qu’ils représentent.

La définition d’une propriété jointe personnalisée et d’une propriété de dépendance personnalisée diffèrent principalement dans la manière dont vous définissez les accesseurs ou wrappers. Au lieu d’utiliser la technique d’enveloppement décrite dans [Propriétés de dépendance personnalisées](custom-dependency-properties.md), vous devez également fournir des méthodes **Get**_PropertyName_ et **Set**_PropertyName_ statiques en tant qu’accesseurs pour la propriété jointe. Les accesseurs sont utilisés principalement par l’analyseur XAML, bien que n’importe quel autre appelant puisse aussi les utiliser pour définir des valeurs dans les scénarios non-XAML.

> [!IMPORTANT]
> Si vous ne définissez pas correctement les accesseurs, le processeur XAML ne peut pas accéder à la propriété jointe et toute personne qui essaie de l’utiliser obtiendra probablement une erreur d’analyse XAML. En outre, les outils de conception et de codage s’appuient souvent sur les conventions « \*Property » pour nommer des identificateurs lorsqu’ils rencontrent une propriété de dépendance personnalisée dans un assembly référencé.

## <a name="accessors"></a>Accesseurs

La signature de l’accesseur **Get**_PropertyName_ doit être la suivante.

`public static` _ValueType_ **obten**_PropertyName_ `(DependencyObject target)`

Pour Microsoft Visual Basic, il s’agit de ceci.

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_ValueType_`)`

L’objet *target* peut être d’un type plus spécifique dans votre implémentation, mais il doit dériver de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). La valeur de retour *valueType* peut aussi être d’un type plus spécifique dans votre implémentation. Le type **Object** de base est acceptable, mais bien souvent vous souhaiterez que votre propriété jointe applique la sécurité de type. Le recours au typage dans les signatures getter et setter est une technique de sécurisation de type recommandée.

La signature de l’accesseur **Set**_PropertyName_ doit être la suivante.

`public static void Set`_PropertyName_` (DependencyObject target , `_ValueType_` value)`

Pour Visual Basic, il s’agit de ceci.

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_ValueType_`)`

L’objet *target* peut être d’un type plus spécifique dans votre implémentation, mais il doit dériver de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). L’objet *value* et son *valueType* peuvent aussi être d’un type plus spécifique dans votre implémentation. Souvenez-vous que la valeur de cette méthode est l’entrée qui provient du processeur XAML quand elle rencontre votre propriété jointe dans le balisage. Il doit exister une conversion de type ou une prise en charge de l’extension de balisage existant pour le type que vous utilisez, afin que le type approprié puisse être créé à partir de la valeur d’un attribut (qui n’est en fin de compte qu’une chaîne). Le type **Object** de base est acceptable, mais bien souvent vous souhaiterez bénéficier d’une sécurité de type supplémentaire. Pour cela, placez l’application du type dans les accesseurs.

> [!NOTE]
> Il est également possible de définir une propriété jointe dans laquelle l’utilisation prévue s’effectue par le biais de la syntaxe d’élément de propriété. Dans ce cas, vous n’avez pas besoin de conversion de type pour les valeurs, mais vous devez vous assurer que les valeurs envisagées peuvent être construites en XAML. [**VisualStateManager. VisualStateGroups**](https://docs.microsoft.com/dotnet/api/system.windows.visualstatemanager) est un exemple de propriété jointe existante qui prend uniquement en charge l’utilisation des éléments de propriété.

## <a name="code-example"></a>Exemple de code

Cet exemple illustre l’inscription de propriété de dépendance (à l’aide de la méthode [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)), ainsi que les accesseurs **Get** et **Set**, pour une propriété jointe personnalisée. Dans l’exemple, la propriété jointe se nomme `IsMovable`. Par conséquent, les accesseurs doivent se nommer `GetIsMovable` et `SetIsMovable`. Le propriétaire de la propriété jointe est une classe de service nommée `GameService` qui ne possède pas sa propre interface utilisateur. Son seul objectif est de fournir les services de propriété jointe lorsque la propriété jointe **GameService.IsMovable** est utilisée.

La définition de la propriété C++jointe dans/CX est un peu plus complexe. Vous devez décider de la factorisation entre fichier de code et en-tête. De plus, vous devez exposer l’identificateur en tant que propriété uniquement avec un accesseur **get**, pour les raisons discutées dans [Propriétés de dépendance personnalisées](custom-dependency-properties.md). Dans C++/CX, vous devez définir explicitement cette relation de champ de propriété plutôt que de vous appuyer sur le Keywords **ReadOnly** .net et la sauvegarde implicite des propriétés simples. Vous devez également inscrire la propriété jointe au sein d’une fonction d’assistance. Celle-ci n’est exécutée qu’une seule fois au démarrage de l’application, avant le chargement de toute page XAML nécessitant la propriété jointe. En général, l’appel des fonctions d’assistance d’inscription de propriété pour une partie ou la totalité des propriétés jointes ou de dépendance s’effectue au sein du constructeur **App** / [**Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.-ctor) dans le code de votre fichier app.xaml.

```csharp
public class GameService : DependencyObject
{
    public static readonly DependencyProperty IsMovableProperty = 
    DependencyProperty.RegisterAttached(
      "IsMovable",
      typeof(Boolean),
      typeof(GameService),
      new PropertyMetadata(false)
    );
    public static void SetIsMovable(UIElement element, Boolean value)
    {
        element.SetValue(IsMovableProperty, value);
    }
    public static Boolean GetIsMovable(UIElement element)
    {
        return (Boolean)element.GetValue(IsMovableProperty);
    }
}
```

```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    [default_interface]
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        static Boolean GetIsMovable(Windows.UI.Xaml.DependencyObject target);
        static void SetIsMovable(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// GameService.h
...
    static Windows::UI::Xaml::DependencyProperty IsMovableProperty() { return m_IsMovableProperty; }
    static bool GetIsMovable(Windows::UI::Xaml::DependencyObject const& target) { return winrt::unbox_value<bool>(target.GetValue(m_IsMovableProperty)); }
    static void SetIsMovable(Windows::UI::Xaml::DependencyObject const& target, bool value) { target.SetValue(m_IsMovableProperty, winrt::box_value(value)); }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}

// GameService.cpp
#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="setting-your-custom-attached-property-from-xaml-markup"></a>Définition de votre propriété jointe personnalisée à partir du balisage XAML

> [!NOTE]
> Si vous utilisez C++/WinRT, passez à la section suivante (en[définissant la propriété jointe personnalisée de manière C++impérative avec/WinRT](#setting-your-custom-attached-property-imperatively-with-cwinrt)).

Après avoir défini votre propriété jointe et inclus ses membres de prise en charge dans le cadre d’un type personnalisé, vous devez rendre les définitions accessibles pour l’utilisation XAML. Pour cela, vous devez mapper un espace de noms XAML qui fera référence à l’espace de noms de code qui contient la classe pertinente. Dans les cas où vous avez défini la propriété jointe dans le cadre d’une bibliothèque, vous devez inclure cette dernière dans le package de l’application.

Un mappage d’espace de noms XML pour XAML se place généralement dans l’élément racine d’une page XAML. Par exemple, pour la classe nommée `GameService` dans l’espace de noms `UserAndCustomControls` qui contient les définitions des propriétés jointes indiquées dans les extraits de code précédents, le mappage peut ressembler à ce qui suit.

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

À l’aide du mappage, vous pouvez définir votre propriété jointe `GameService.IsMovable` sur tout élément qui correspond à votre définition cible, y compris un type existant défini par le Windows Runtime.

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

Si vous définissez la propriété sur un élément qui se trouve également dans le même espace de noms XML mappé, vous devez quand même inclure le préfixe dans le nom de la propriété jointe, car le préfixe qualifie le type de propriétaire. On ne peut pas supposer que l’attribut de la propriété jointe se trouve dans le même espace de noms XML que l’élément où l’attribut est inclus, bien que, selon les règles XML normales, les attributs puissent hériter de l’espace de noms des éléments. Par exemple, si vous définissez `GameService.IsMovable` sur un type personnalisé `ImageWithLabelControl` (définition non illustrée), et même si tous deux ont été définis dans le même espace de noms de code mappé au même préfixe, le XAML sera tout de même le suivant.

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> Si vous écrivez une interface utilisateur XAML avec C++/CX, vous devez inclure l’en-tête pour le type personnalisé qui définit la propriété jointe, chaque fois qu’une page XAML utilise ce type. Chaque page XAML est associée à un en-tête code-behind (. Xaml. h). C’est là que vous devez inclure (à l’aide de **\#include**) l’en-tête pour la définition du type de propriétaire de la propriété jointe.

## <a name="setting-your-custom-attached-property-imperatively-with-cwinrt"></a>Définition de votre propriété jointe personnalisée de C++manière impérative avec/WinRT

Si vous utilisez C++/WinRT, vous pouvez accéder à une propriété jointe personnalisée à partir du code impératif, mais pas à partir du balisage XAML. Le code ci-dessous montre comment procéder.

```xaml
<Image x:Name="gameServiceImage"/>
```

```cppwinrt
// MainPage.h
...
#include "GameService.h"
...

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    GameService::SetIsMovable(gameServiceImage(), true);
}
...
```

## <a name="value-type-of-a-custom-attached-property"></a>Type de valeur d’une propriété jointe personnalisée

Le type utilisé comme type de valeur d’une propriété jointe personnalisée affecte l’utilisation, la définition ou les deux à la fois. Le type de valeur de la propriété jointe est déclaré à plusieurs endroits : dans les signatures des méthodes d’accesseur **Get** et **Set**, et également comme paramètre *propertyType* de l’appel [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached).

Le type de valeur le plus courant pour les propriétés jointes (personnalisées ou autres) est une chaîne simple. En effet, les propriétés jointes sont généralement destinées à des attributs XAML et l’utilisation d’une chaîne comme type de valeur garantit la légèreté des propriétés. D’autres primitives qui offrent une méthode de conversion native en chaînes, telles qu’entier, double ou valeur d’énumération, sont également des types de valeurs courants pour les propriétés jointes. Vous pouvez utiliser d’autres types de valeurs (qui ne prennent pas en charge la conversion de chaîne native) comme valeur de propriété jointe. Toutefois, cela implique de faire un choix en matière d’utilisation ou d’implémentation :

- vous pouvez laisser la propriété jointe telle quelle, mais elle peut prendre en charge l’utilisation uniquement quand il s’agit d’un élément de propriété, et la valeur est déclarée en tant qu’élément objet. Dans ce cas, le type de propriété doit prendre en charge l’utilisation de XAML en tant qu’élément objet. Pour les références existantes au Windows Runtime, vérifiez la syntaxe XAML pour vous assurer que le type prend en charge l’utilisation des éléments objets XAML.
- Vous pouvez laisser la propriété jointe telle quelle, mais utilisez-la uniquement dans le cadre d’un attribut par le biais d’une technique de référence XAML telle que **Binding** ou **StaticResource** qui peut être exprimée sous forme de chaîne.

## <a name="more-about-the-canvasleft-example"></a>En savoir plus sur l’exemple de **Canvas.Left**

Dans les précédents exemples d’utilisations de propriétés jointes, nous avons vu différentes façons de définir la propriété jointe [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left). Mais qu’est-ce que cela change à la façon dont une classe [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) interagit avec votre objet, et quand cela se produit-il ? Nous examinerons plus en détail cet exemple particulier, car si vous implémentez une propriété jointe, il est intéressant de voir ce qu’une classe propriétaire de propriété jointe classique a l’intention de faire d’autre avec les valeurs de sa propriété jointe si elle les trouve sur d’autres objets.

La fonction principale d’une classe [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) est d’être un conteneur de disposition à position absolue dans l’interface utilisateur. Les enfants d’une classe **Canvas** sont stockés dans une propriété définie par une classe de base [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.children). De tous les panneaux, **Canvas** est le seul qui utilise le positionnement absolu. Il aurait encombré le modèle objet du type [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) courant pour ajouter des propriétés qui ne présenteraient de l’intérêt que pour **Canvas** et les cas d’**UIElement** particuliers où ils sont des éléments enfants d’un **UIElement**. La définition des propriétés de contrôle de disposition d’une classe **Canvas** en tant que propriétés jointes utilisables par n’importe quelle classe **UIElement** maintient le modèle objet plus propre.

Pour être un panneau pratique, [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) a un comportement qui remplace les méthodes [**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) et [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) au niveau de l’infrastructure. C’est en fait l’endroit où **Canvas** recherche les valeurs de propriétés jointes sur ses enfants. Une partie des deux modèles **Measure** et **Arrange** est une boucle qui effectue une itération sur n’importe quel contenu, et un panneau a la propriété [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.children) qui rend explicite ce qui est supposé être considéré comme l’enfant d’un panneau. Ainsi, le comportement de la disposition de **Canvas** procède à une itération dans ces enfants, et effectue des appels des méthodes [**Canvas.GetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.getleft) et [**Canvas.GetTop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.gettop) statiques sur chaque enfant pour voir si ces propriétés jointes contiennent une valeur autre que la valeur par défaut (la valeur par défaut est 0). Ces valeurs sont alors utilisées pour positionner de façon absolue chaque enfant dans l’espace réservé à la disposition disponible dans l’objet **Canvas** en fonction des valeurs spécifiques fournies par chaque enfant, et validées à l’aide de la méthode **Arrange**.

Le code ressemble à ce pseudocode.

```syntax
protected override Size ArrangeOverride(Size finalSize)
{
    foreach (UIElement child in Children)
    {
        double x = (double) Canvas.GetLeft(child);
        double y = (double) Canvas.GetTop(child);
        child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
    }
    return base.ArrangeOverride(finalSize); 
    // real Canvas has more sophisticated sizing
}
```

> [!NOTE]
> Pour plus d’informations sur le fonctionnement des panneaux, consultez [vue d’ensemble des panneaux personnalisés XAML](https://docs.microsoft.com/windows/uwp/layout/custom-panels-overview).

## <a name="related-topics"></a>Rubriques connexes

* [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)
* [Vue d’ensemble des propriétés jointes](attached-properties-overview.md)
* [Propriétés de dépendance personnalisées](custom-dependency-properties.md)
* [Vue d’ensemble du langage XAML](xaml-overview.md)
