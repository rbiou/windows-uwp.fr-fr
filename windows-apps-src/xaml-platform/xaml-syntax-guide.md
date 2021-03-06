---
description: Nous expliquons les règles syntaxiques XAML et la terminologie qui décrit les restrictions ou les choix disponibles pour la syntaxe XAML.
title: Guide de la syntaxe XAML
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0351f3566d708b7a588fa77f9f15564f382de44f
ms.sourcegitcommit: 807dadf5eceb576aba3ad898a6e9bf12129e94a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72680745"
---
# <a name="xaml-syntax-guide"></a>Guide de la syntaxe XAML


Nous expliquons les règles syntaxiques XAML et la terminologie qui décrit les restrictions ou les choix disponibles pour la syntaxe XAML. Cette rubrique vous sera utile, si vous découvrez le langage XAML, si vous voulez revoir la terminologie ou une partie de la syntaxe, ou si vous souhaitez découvrir le fonctionnement du langage XAML et obtenir des informations supplémentaires sur le contexte et l’environnement.

## <a name="xaml-is-xml"></a>XAML et XML : des gênes identiques

La syntaxe de base du langage XAML (Extensible Application Markup Language) s’appuie sur le langage XML et, par définition, un code XAML valide doit être un code XML valide. Mais le langage XAML a aussi ses propres concepts syntaxiques qui étendent le langage XML. Une entité XML donnée peut être valide en langage XML brut, mais cette syntaxe peut avoir une signification différente et plus aboutie en XAML. Cette rubrique explique ces concepts de syntaxe XAML.

## <a name="xaml-vocabularies"></a>Vocabulaire lié au code XAML

Le code XAML se distingue de la plupart des utilisations du code XML dans un cas spécifique : il n’est pas habituellement appliqué avec un schéma, tel qu’un fichier XSD. Cela tient au fait que le code XAML a pour vocation d’être évolutif, c’est exactement ce que signifie le « X » de l’acronyme XAML. Une fois le code XAML analysé, une représentation du code de sauvegarde doit contenir les éléments et les attributs que vous référencez en XAML, soit dans les types principaux définis par Windows Runtime, soit dans les types qui étendent ou sont basés à l’extérieur de Windows Runtime. La documentation du Kit de développement logiciel fait référence aux types qui sont déjà intégrés à Windows Runtime et qui peuvent être utilisés en XAML en tant que *vocabulaire XAML* pour Windows Runtime. Microsoft Visual Studio vous aide à produire du balisage valide à l’aide de ce vocabulaire XAML. Visual Studio peut également inclure vos types personnalisés afin de les utiliser en XAML à condition que la source de ces types soit correctement référencée dans le projet. Pour plus d’informations sur le code XAML et les types personnalisés, voir [Espaces de noms XAML et mappage d’espaces de noms](xaml-namespaces-and-namespace-mapping.md).

##  <a name="declaring-objects"></a>Déclaration d’objets

Les programmeurs pensent souvent en termes d’objets et de membres, alors qu’un langage de balisage est conceptualisé en tant qu’éléments et attributs. En termes simples, un élément que vous déclarez dans du balisage XAML devient un objet dans une représentation d’objets d’exécution de stockage. Pour créer un objet d’exécution pour votre application, vous déclarez un élément XAML dans le balisage XAML. L’objet est créé lorsque Windows Runtime charge votre code XAML.

Un fichier XAML a toujours exactement un élément qui lui sert de racine et qui déclare un objet appelé à être la racine conceptuelle d’une structure de programmation telle qu’une page ou le graphique d’objet de la définition d’exécution complète d’une application.

En termes de syntaxe XAML, il existe trois façons de déclarer des objets en XAML :

-   **Directement, à l’aide de la syntaxe d’élément objet :** cette méthode utilise des balises d’ouverture et de fermeture pour instancier un objet en tant qu’élément de forme XML. Vous pouvez utiliser cette syntaxe pour déclarer des objets racines ou pour créer des objets imbriqués qui définissent des valeurs de propriétés.
-   **Indirectement, à l’aide de la syntaxe d’attribut :** cette méthode utilise une valeur de chaîne inline disposant d’instructions sur la création d’un objet. L’analyseur XAML utilise cette chaîne pour définir la valeur d’une référence nouvellement créée comme valeur d’une propriété. Cette prise en charge est limitée à certains objets courants et certaines propriétés courantes.
-   Utilisation d’une extension de balisage.

Cela ne veut pas dire que vous avez toujours le choix de la syntaxe de création d’objet dans un vocabulaire XAML. Certains objets peuvent être créés uniquement à l’aide de la syntaxe d’élément objet. D’autres objets peuvent être créés uniquement en étant définis initialement dans un attribut. En fait, les objets qui peuvent être créés avec la syntaxe d’attribut ou d’objet élément sont relativement rares dans les vocabulaires XAML. Même si les deux formes de syntaxes sont possibles, l’une des syntaxes sera en réalité plus courante pour des raisons de style.
Il existe également des techniques que vous pouvez utiliser en XAML pour référencer des objets existants plutôt que de créer de nouvelles valeurs. Les objets existants peuvent être définis dans d’autres parties du code XAML ou peuvent exister implicitement à travers le comportement de la plateforme et de ses modèles d’application ou de programmation.

### <a name="declaring-an-object-by-using-object-element-syntax"></a>Déclaration d’un objet à l’aide d’une syntaxe d’élément objet

Pour déclarer un objet à l’aide d’une syntaxe d’élément objet, vous écrivez des balises comme ceci : `<objectName>  </objectName>`, où *objectName* correspond au nom de type de l’objet que vous voulez instancier. Voici une utilisation d’élément objet pour déclarer un objet [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) :

```xml
<Canvas>
</Canvas>
```

If the object does not contain other objects, you can declare the object element by using one self-closing tag instead of an opening/closing pair: <ph id="ph1">`&lt;Canvas /&gt;`</ph>

### <a name="containers"></a>Conteneurs

Beaucoup d’objets utilisés comme éléments d’interface, tels que [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), peuvent contenir d’autres objets. Ces objets sont parfois désignés sous le nom de « conteneurs ». L’exemple suivant illustre un conteneur **Canvas** qui contient un élément, à savoir un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>Déclaration d’un objet à l’aide d’une syntaxe d’attribut

Comme ce comportement est lié au paramétrage des propriétés, nous en parlerons plus en détail dans de prochaines sections.

### <a name="initialization-text"></a>Texte d’initialisation

Pour certains objets, vous pouvez déclarer de nouvelles valeurs à l’aide de texte interne utilisé comme valeurs d’initialisation pour la construction. En langage XAML, cette technique (et syntaxe) est appelée *texte d’initialisation*. D’un point de vue conceptuel, le texte d’initialisation s’apparente à l’appel d’un constructeur doté de paramètres. Il s’avère utile pour définir les valeurs initiales de certaines structures.

Une syntaxe d’élément objet est souvent utilisée avec un texte d’initialisation pour définir une valeur de structure avec une valeur **x:Key**, afin de pouvoir exister dans un objet [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). Vous pouvez procéder de la sorte si vous partagez cette valeur de structure entre plusieurs propriétés cibles. Pour certaines structures, vous ne pouvez pas utiliser la syntaxe d’attribut pour définir les valeurs de la structure : le texte d’initialisation est le seul moyen de produire une ressource [**CornerRadius**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.CornerRadius), [**Thickness**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Thickness), [**GridLength**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.GridLength) ou [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color) utile et partageable.

Cet exemple abrégé utilise le texte d’initialisation pour spécifier les valeurs d’une structure [**Thickness**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Thickness). Dans ce cas, **Left** et **Right** prennent la valeur 20, tandis que **Top** et **Bottom** prennent la valeur 10. Cet exemple montre la structure **Thickness** créée en tant que ressource à clé, puis la référence à cette ressource. Pour plus d’informations sur le texte d’initialisation [**Thickness**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Thickness), voir [**Thickness**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Thickness).

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

<bpt id="p1">**</bpt>Note<ept id="p1">**</ept><ph id="ph1">  </ph>Some structures can't be declared as object elements. Le texte d’initialisation n’est pas pris en charge et ces structures ne peuvent pas être utilisées comme ressources. Vous devez utiliser une syntaxe d’attribut pour affecter aux propriétés ces valeurs en XAML. Ces valeurs sont les suivantes : [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration), [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) et [**Size**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size).

## <a name="setting-properties"></a>Définition de propriétés

Vous pouvez définir des propriétés d’objets que vous avez déclarés à l’aide de la syntaxe d’élément objet. Il existe plusieurs manières de définir des propriétés en XAML :

-   à l’aide de la syntaxe d’attribut ;
-   à l’aide de la syntaxe d’élément propriété ;
-   à l’aide de la syntaxe d’élément où le contenu (texte interne ou éléments enfants) définit la propriété de contenu XAML d’un objet ;
-   à l’aide d’une syntaxe de collection (qui est généralement la syntaxe de collection implicite).

Comme avec la déclaration d’objet, cette liste n’implique pas que toute propriété peut être définie avec chacune de ces techniques. Certaines propriétés prennent en charge une seule de ces techniques.
Certaines propriétés prennent en charge plusieurs formes ; par exemple, certaines peuvent utiliser la syntaxe d’élément propriété ou la syntaxe d’attribut. Les différentes possibilités dépendent à la fois de la propriété et du type d’objet utilisé par la propriété. Dans les informations de référence sur l’API Windows Runtime, vous verrez s’afficher les utilisations du code XAML dans la section **Syntaxe**. Une autre utilisation pourrait fonctionner mais elle serait plus détaillée. Ce type d’utilisation plus détaillée n’est pas toujours illustré, car nous essayons de vous montrer les meilleures pratiques ou les scénarios réels pour utiliser une propriété donnée en XAML. Des indications sur la syntaxe XAML sont présentées dans les sections **Utilisation XAML** des pages de référence relatives aux propriétés pouvant être définies en XAML.

Some properties on objects cannot be set in XAML by any means, and can only be set using code. Il s’agit généralement de propriétés plus adaptées pour une utilisation avec le code-behind, et non en XAML.

Une propriété en lecture seule ne peut pas être définie en XAML. Même dans du code, le type propriétaire doit prendre en charge une autre façon de la définir, comme une surcharge de constructeur, une méthode d’assistance ou la prise en charge des propriétés calculées. Une propriété calculée repose sur les valeurs d’autres propriétés définissables et aussi, parfois, sur un événement avec gestion intégrée ; ces fonctionnalités sont disponibles dans le système de propriétés de dépendance. Pour plus d’informations sur l’utilité des propriétés de dépendance pour la prise en charge des propriétés calculées, voir [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md).

La syntaxe de collection en langage XAML donne l’impression de définir une propriété en lecture seule, ce qui, en fait, n’est pas le cas. See "<bpt id="p1">[</bpt>Collection Syntax<ept id="p1">](#collection-syntax)</ept>" later in this topic.

### <a name="setting-a-property-by-using-attribute-syntax"></a>Définition d’une propriété à l’aide d’une syntaxe d’attribut

Pour définir une valeur de propriété dans un langage de balisage comme XML ou HTML, vous devez généralement définir une valeur d’attribut. La définition d’attributs XAML est similaire à la définition de valeurs d’attributs en XML. Le nom d’attribut est spécifié à un point quelconque à l’intérieur des balises après le nom d’élément, tous deux étant séparés d’au moins un espace. Le nom d’attribut est suivi d’un signe égal. La valeur d’attribut est entre guillemets droits. Les guillemets peuvent être doubles ou simples, pour autant que les guillemets encadrant la valeur soient identiques. La valeur d’attribut proprement dite doit pouvoir être exprimée sous forme de chaîne. La chaîne contient souvent des chiffres, mais en XAML, toutes les valeurs d’attributs sont des valeurs de chaîne jusqu’à ce que l’analyseur XAML intervienne et fasse une conversion de valeur de base.

Dans cet exemple, la syntaxe d’attribut est utilisée pour quatre attributs afin de définir les propriétés [**Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name), [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) et [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>Définition d’une propriété à l’aide d’une syntaxe d’élément propriété

Une bonne partie des propriétés d’un objet peut être définie à l’aide de la syntaxe d’élément propriété. Un élément propriété se présente ainsi : `<`*object*`.`*property*`>`.

Pour utiliser la syntaxe d’élément propriété, vous devez créer des éléments de propriété XAML pour la propriété à définir. En langage XML standard, cet élément serait simplement considéré comme un élément dont le nom contient un point. However, in XAML, the dot in the element name identifies the element as a property element, with <bpt id="p1">*</bpt>property<ept id="p1">*</ept> expected to be a member of <bpt id="p2">*</bpt>object<ept id="p2">*</ept> in a backing object model implementation. Pour utiliser la syntaxe d’élément propriété, il doit être possible de spécifier un élément objet pour « remplir » les balises d’élément propriété. Un élément propriété a toujours un contenu (un seul élément, plusieurs éléments ou du texte interne) ; il est inutile d’avoir un élément propriété de fermeture automatique.

Dans la grammaire suivante, *property* est le nom de la propriété que vous voulez définir et *propertyValueAsObjectElement* est un élément objet unique, lequel doit satisfaire les exigences de type de valeur.

<ph id="ph1">`&lt;`</ph><bpt id="p1">*</bpt>object<ept id="p1">*</ept><ph id="ph2">`&gt;`</ph>

<ph id="ph1">`&lt;`</ph><bpt id="p1">*</bpt>object<ept id="p1">*</ept><ph id="ph2">`.`</ph><bpt id="p2">*</bpt>property<ept id="p2">*</ept><ph id="ph3">`&gt;`</ph>

<bpt id="p1">*</bpt>propertyValueAsObjectElement<ept id="p1">*</ept>

<ph id="ph1">`&lt;/`</ph><bpt id="p1">*</bpt>object<ept id="p1">*</ept><ph id="ph2">`.`</ph><bpt id="p2">*</bpt>property<ept id="p2">*</ept><ph id="ph3">`&gt;`</ph>

<ph id="ph1">`&lt;/`</ph><bpt id="p1">*</bpt>object<ept id="p1">*</ept><ph id="ph2">`&gt;`</ph>

Dans l’exemple suivant, la syntaxe d’élément propriété est utilisée pour définir la propriété [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) avec un élément objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). (Dans **SolidColorBrush**, [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) est défini comme attribut.) Le résultat analysé de ce code XAML est identique à celui de l’exemple XAML précédent où **Fill** était défini en utilisant la syntaxe d’attribut.

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>Vocabulaires XAML et programmation orientée objet

Les propriétés et les événements, lorsqu’ils apparaissent en tant que membres XAML d’un type XAML Windows Runtime, sont souvent hérités de types de base. Prenons l’exemple suivant : `<Button Background="Blue" .../>` La propriété [**Background**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) n’est pas une propriété déclarée immédiatement sur la classe [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button). Au lieu de cela, **Background** est héritée de la classe de base [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control). En fait, si vous consultez la rubrique de référence concernant **Button**, vous verrez que les listes de membres contiennent au moins un membre hérité de chaque classe d’une chaîne de classes de base successives : [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase), [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control), [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement), [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Dans la liste **Propriétés**, toutes les propriétés de collection et propriétés en lecture seule sont héritées dans le sens des vocabulaires XAML. Les événements (comme les divers événements [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)) le sont également.

Si vous utilisez les informations de référence de Windows Runtime pour obtenir de l’aide sur le langage XAML, le nom d’élément affiché dans une syntaxe ou même dans un exemple de code correspond souvent au type qui définit initialement la propriété, car cette rubrique de référence est partagée par tous les types possibles qui en héritent à partir d’une classe de base. Si vous Microsoft IntelliSense de Visual Studio pour le code XAML dans l’éditeur XML, IntelliSense et ses listes déroulantes savent parfaitement fusionner l’héritage et fournir une liste précise des attributs pouvant être définis une fois que vous avez commencé à utiliser un élément objet pour une instance de classe.

### <a name="xaml-content-properties"></a>Propriétés de contenu XAML

Certains types définissent une de leurs propriétés de telle sorte que la propriété autorise une syntaxe de contenu XAML. Pour la propriété de contenu XAML d’un type, vous pouvez omettre l’élément propriété de cette propriété lorsque vous la spécifiez en XAML. Vous pouvez également affecter à la propriété une valeur de texte interne en entrant ce texte interne directement dans les balises d’élément objet du type propriétaire. Les propriétés de contenu XAML prennent en charge une syntaxe de balisage simple pour cette propriété et améliorent la lisibilité du code XAML en réduisant l’imbrication.

Si une syntaxe de contenu XAML est disponible, cette syntaxe est présentée dans les sections « XAML » de **Syntaxe** pour cette propriété dans la documentation de référence de Windows Runtime. Par exemple, la page de propriété [**Child**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child) pour [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) présente une syntaxe de contenu XAML et non une syntaxe d’élément propriété pour définir la valeur de la propriété **Border.Child** à objet unique d’un **Border**, comme ceci :

```xml
<Border>
  <Button .../>
</Border>
```

Si la propriété déclarée comme propriété de contenu XAML est le type **Object**, ou est le type **String**, la syntaxe de contenu XAML prend en charge ce qui est en fait le texte interne dans le modèle de document XML : une chaîne entre les balises objet d’ouverture et de fermeture. Par exemple, la page de propriétés [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) pour [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) présente une syntaxe de contenu XAML ayant une valeur de texte interne pour définir **Text**, mais la chaîne « Text » n’apparaît jamais dans le balisage. Voici un exemple d’utilisation :

```xml
<TextBlock>Hello!</TextBlock>
```

S’il existe une propriété de contenu XAML pour une classe, cela est indiqué dans la rubrique de référence correspondant à la classe, dans la section « Attributs ». Recherchez la valeur de l’attribut [**ContentPropertyAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute). Ce dernier utilise un champ nommé « Name ». La valeur de « Name » est le nom de la propriété de cette classe qui est la propriété de contenu XAML. Par exemple, sur la page de référence de [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border), vous verrez ceci : ContentProperty("Name=Child").

L’impossibilité de mélanger la propriété de contenu XAML et d’autres éléments propriétés définis sur l’élément constitue une règle de syntaxe XAML importante qu’il faut mentionner. La propriété de contenu XAML doit être définie totalement avant tout élément propriété, ou totalement après. Par exemple, le code XAML suivant n’est pas valide :

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>Syntaxe de collection

Toutes les syntaxes présentées jusqu’ici définissent des propriétés d’objets uniques. Mais dans bon nombre de scénarios d’interface utilisateur, un élément parent donné doit pouvoir avoir plusieurs éléments enfants. Par exemple, l’interface utilisateur d’un formulaire de saisie nécessite plusieurs éléments zone de texte, quelques étiquettes et éventuellement un bouton « Envoyer ». Malgré tout, si vous deviez utiliser un modèle objet de programmation pour accéder à ces divers éléments, il s’agirait logiquement d’éléments appartenant à une propriété de collection unique et chaque élément ne serait pas la valeur de différentes propriétés. Le langage XAML prend en charge aussi bien les éléments enfants multiples qu’un modèle de collection de stockage type en considérant les propriétés qui utilisent un type de collection comme étant implicite, et en réservant un traitement spécial aux éléments enfants d’un type de collection.

De nombreuses propriétés de collection sont également identifiées en tant que propriété de contenu XAML pour la classe. Il est fréquent de voir la combinaison du traitement de collections implicites et de la syntaxe de contenu XAML dans des types utilisés pour la composition de contrôles, tels que des panneaux, des vues ou des contrôles d’éléments. For example, the following example shows the simplest possible XAML for compositing two peer UI elements within a <bpt id="p1">[</bpt><bpt id="p2">**</bpt>StackPanel<ept id="p2">**</ept><ept id="p1">](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)</ept>.

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>Mécanisme de la syntaxe de collection XAML

Il peut sembler de prime abord que le code XAML active un « ensemble » de la propriété de collection en lecture seule. En réalité, le code XAML permet ici d’ajouter des éléments à une collection existante. Le langage XAML et les processeurs XAML implémentant la prise en charge XAML s’appuient sur une convention applicable aux types de collection de stockage pour permettre cette syntaxe. En principe, il existe une propriété de stockage telle qu’un indexeur ou une propriété **Items** qui fait référence à des éléments spécifiques de la collection. Généralement, cette propriété n’est pas explicite dans la syntaxe XAML. Pour les collections, le mécanisme sous-jacent d’analyse XAML n’est pas une propriété, mais une méthode (plus précisément, la méthode **Add**, dans la majorité des cas). Lorsque le processeur XAML rencontre un ou plusieurs éléments objet dans une syntaxe de collection XAML, chacun de ces objets est d’abord créé à partir d’un élément, puis chaque nouvel objet est ajouté dans l’ordre à la collection conteneur en appelant la méthode **Add** de la collection.

Lorsqu’un analyseur XAML ajoute des éléments à une collection, il est dans la logique de la méthode **Add** de déterminer si un élément XAML donné est un élément enfant autorisé de l’objet de collection. Nombreux sont les types de collection qui sont fortement typés par l’implémentation de stockage, ce qui signifie que le paramètre d’entrée de **Add** exige que le type de l’élément transmis corresponde au type du paramètre **Add**.

Pour les propriétés de collection, choisissez soigneusement le moment ou vous tentez de spécifier la collection explicitement en tant qu’élément objet. Un analyseur XAML crée un nouvel objet chaque fois qu’il rencontre un élément objet. Si la propriété de collection que vous tentez d’utiliser est en lecture seule, une exception d’analyse XAML peut être levée. Si vous utilisez simplement la syntaxe de collection implicite, aucune exception ne s’affiche.

## <a name="when-to-use-attribute-or-property-element-syntax"></a>Quand utiliser la syntaxe d’attribut ou d’élément propriété

Toutes les propriétés définissables en XAML prennent en charge la syntaxe d’attribut ou la syntaxe d’élément propriété pour la définition directe de valeurs. Toutefois, il n’est pas garanti qu’elles prennent en charge indifféremment l’une ou l’autre de ces syntaxes. Certaines propriétés prennent en charge l’une ou l’autre de ces syntaxes, et certaines propriétés prennent en charge des options de syntaxe supplémentaires comme une propriété de contenu XAML. Le type de syntaxe XAML pris en charge par une propriété dépend du type d’objet que la propriété utilise comme type de propriété. Si la propriété est de type primitif, par exemple double (flottant ou décimal), entier, booléen ou chaîne, la propriété prend toujours en charge la syntaxe d’attribut.

Vous pouvez également utiliser la syntaxe d’attribut pour définir une propriété si le type d’objet utilisé pour définir cette propriété peut être créé en traitant une chaîne. C’est toujours le cas pour les primitives : la conversion de type est intégrée à l’analyseur. Toutefois, certains autres types d’objet peuvent aussi être créés en utilisant une chaîne spécifiée comme valeur d’attribut, plutôt qu’un élément objet dans un élément propriété. Pour que cela fonctionne, une conversion de type sous-jacente, prise en charge soit par cette propriété particulière soit généralement par toutes les valeurs qui utilisent ce type de propriété, doit avoir lieu. La valeur de chaîne de l’attribut est utilisée pour définir les propriétés qui s’avèrent importantes pour l’initialisation de la nouvelle valeur d’objet. Potentiellement, un convertisseur de type spécifique peut également créer différentes sous-classes d’un type de propriété commun, selon la façon dont il traite exclusivement les informations contenues dans la chaîne. Pour les types d’objets qui prennent en charge ce comportement, une syntaxe spéciale sera présentée dans la section « Syntaxe » de la documentation de référence. À titre d’exemple, la syntaxe XAML pour [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) montre comment une syntaxe d’attribut peut être utilisée pour créer une nouvelle valeur [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pour toute propriété de type **Brush** (et il existe de nombreuses propriétés **Brush** dans le langage XAML Windows Runtime).

## <a name="xaml-parsing-logic-and-rules"></a>Logique et règles d’analyse XAML

Il est parfois instructif de lire le code XAML de manière similaire à la façon dont un analyseur XAML doit le lire : comme un ensemble de jetons de chaîne rencontrés dans un ordre linéaire. Un analyseur XAML doit interpréter ces jetons selon un ensemble de règles faisant partie de la définition du fonctionnement du langage XAML.

Pour définir une valeur de propriété dans un langage de balisage comme XML ou HTML, vous devez généralement définir une valeur d’attribut. Dans la syntaxe suivante, *objectName* est l’objet à instancier, *propertyName* est le nom de la propriété à définir dans cet objet et *propertyValue* est la valeur à définir.

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

Chacune des deux syntaxes vous permet de déclarer un objet et de définir une propriété de cet objet. Bien que le premier exemple corresponde à un balisage à un seul élément, il existe en réalité des étapes distinctes qui déterminent la façon dont le processeur XAML analyse ce balisage.

Tout d’abord, la présence de l’élément objet indique qu’un nouvel objet *objectName* doit être instancié. Only after such an instance exists can the instance property <bpt id="p1">*</bpt>propertyName<ept id="p1">*</ept> be set on it.

Une autre règle du langage XAML est que les attributs d’un élément doivent pouvoir être définis dans n’importe quel ordre. Par exemple, il n’y a pas de différence entre `<Rectangle Height="50" Width="100" />` et `<Rectangle Width="100"  Height="50" />`. L’ordre que vous utilisez est une question de style.

<bpt id="p1">**</bpt>Note<ept id="p1">**</ept><ph id="ph1">  </ph>XAML designers often promote ordering conventions if you use design surfaces other than the XML editor, but you can freely edit that XAML later, to reorder the attributes or introduce new ones.

## <a name="attached-properties"></a>Propriétés jointes

XAML étend XML en ajoutant un élément de syntaxe appelé *propriété jointe*. Semblable à la syntaxe d’élément de propriété, la syntaxe de propriété jointe contient un point qui revêt une signification particulière pour l’analyse XAML. Specifically, the dot separates the provider of the attached property, and the property name.

En XAML, on définit les propriétés jointes à l’aide de la syntaxe *AttachedPropertyProvider*.*PropertyName*. Voici un exemple illustrant comment définir la propriété jointe [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) en XAML :

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Vous pouvez définir la propriété jointe sur des éléments qui n’ont pas de propriété de ce nom dans le type de stockage, et en ce sens, elle fonctionne un peu comme une propriété globale, ou un attribut défini par un autre espace de noms tel que l’attribut **xml:space**.

Le langage XAML Windows Runtime comprend des propriétés jointes qui prennent en charge les scénarios suivants :

-   Les éléments enfants peuvent informer des panneaux de conteneur parent de la façon dont ils doivent se comporter dans la disposition : [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), [**VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid).
-   Les utilisations de contrôles peuvent influencer le comportement d’une partie importante des contrôles qui provient du modèle de contrôle : [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [**VirtualizingStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel).
-   L’utilisation d’un service qui est disponible dans une classe associée, où le service et la classe qui l’utilise ne partagent pas l’héritage : [**Typography**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Typography), [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager), [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties), [**ToolTipService**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTipService).
-   Ciblage d’animation : [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard).

Pour plus d’informations, voir [Vue d’ensemble des propriétés jointes](attached-properties-overview.md).

## <a name="literal--values"></a>Valeurs « { » littérales

Because the opening brace symbol <ph id="ph1">\{</ph> is the opening of the markup extension sequence, you use an escape sequence to specify a literal string value that starts with "<ph id="ph2">\{</ph>". The escape sequence is "<ph id="ph1">\{</ph><ph id="ph2">\}</ph>". For example, to specify a string value that is a single opening brace, specify the attribute value as "<ph id="ph1">\{</ph><ph id="ph2">\}</ph><ph id="ph3">\{</ph>". You can also use the alternative quotation marks (for example, a <bpt id="p1">**</bpt>'<ept id="p1">**</ept> within an attribute value delimited by <bpt id="p2">**</bpt>""<ept id="p2">**</ept>) to provide a "<ph id="ph1">\{</ph>" value as a string.

<bpt id="p1">**</bpt>Note<ept id="p1">**</ept><ph id="ph1">  </ph>"<ph id="ph2">\\</ph>}" also works if it's inside a quoted attribute.
 
## <a name="enumeration-values"></a>Valeurs d’énumération

De nombreuses propriétés de l’API Windows Runtime utilisent des énumérations comme valeurs. Si le membre est une propriété en lecture seule, vous pouvez définir une telle propriété en spécifiant une valeur d’attribut. Vous identifiez la valeur d’énumération à utiliser comme valeur de la propriété à l’aide du nom non qualifié de la constante. Par exemple, voici comment définir [**UIElement.Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) en XAML : `<Button Visibility="Visible"/>`. Ici, « Visible » en tant que chaîne est directement mappé à une constante nommée de l’énumération [**Visibility**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility), **Visible**.

-   N’utilisez pas une forme qualifiée, car elle ne fonctionnera pas. Par exemple, le code XAML suivant n’est pas valide :`<Button Visibility="Visibility.Visible"/>`
-   N’utilisez pas la valeur de la constante. En d’autres termes, ne vous fiez pas à la valeur entière de l’énumération qui dépend ici de façon explicite ou implicite du mode de définition de l’énumération. Même si cette pratique semble fonctionner, elle est déconseillée que ce soit en XAML ou dans le code, car vous comptez sur un détail d’implémentation peut-être temporaire. Par exemple, ceci est déconseillé : `<Button Visibility="1"/>`.

<bpt id="p1">**</bpt>Note<ept id="p1">**</ept><ph id="ph1">  </ph>In reference topics for APIs that use XAML and use enumerations, click the link to the enumeration type in the <bpt id="p2">**</bpt>Property value<ept id="p2">**</ept> section of <bpt id="p3">**</bpt>Syntax<ept id="p3">**</ept>. Vous faites référence à la page d’énumération où vous pouvez découvrir les constantes nommées pour cette énumération.

Les énumérations peuvent accepter les indicateurs, ce qui signifie qu’elles ont pour attribut **FlagsAttribute**. Si vous devez spécifier une combinaison de valeurs pour une énumération comprenant des indicateurs en tant que valeur d’attribut XAML, utilisez le nom de chaque constante d’énumération, avec une virgule (,) entre chaque nom et aucun espace intermédiaire. Les attributs comprenant des indicateurs ne sont pas courants dans le vocabulaire XAML Windows Runtime, mais [**ManipulationModes**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) est un exemple où la valeur d’énumération comprenant des indicateurs dans XAML est prise en charge.

## <a name="interfaces-in-xaml"></a>Interfaces en XAML

Dans de rares cas, vous verrez une syntaxe XAML où le type d’une propriété est une interface. Dans le système de type XAML, un type qui implémente cette interface est acceptable en tant que valeur lors de l’analyse. Il doit exister une instance créée d’un tel type disponible pour servir de valeur. Vous verrez une interface utilisée en tant que type dans la syntaxe XAML pour les propriétés [**Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) et [**CommandParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.commandparameter) de [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase). Ces propriétés prennent en charge les modèles de conception MVVM (Model-View-ViewModel) où l’interface **ICommand** représente le contrat régissant l’interaction entre les affichages et les modèles.

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Conventions sur les espaces réservés XAML dans les informations de référence sur Windows Runtime

Si vous avez examiné une section **Syntaxe** des rubriques de référence pour les API Windows Runtime qui peuvent utiliser XAML, vous avez probablement constaté que la syntaxe inclut un assez grand nombre d’espaces réservés. XAML syntax is different than the C#, Microsoft Visual Basic or Visual C++ component extensions (C++/CX) syntax because the XAML syntax is a usage syntax. Elle évoque votre utilisation finale dans vos propres fichiers XAML, mais sans être trop contraignante sur les valeurs que vous pouvez utiliser. Ainsi, l’utilisation décrit en général un type de grammaire qui combine des littéraux et des espaces réservés, et définit certains de ces espaces réservés dans la section **Valeurs XAML**.

Lorsque vous voyez des noms de types/noms d’éléments dans une syntaxe XAML pour une propriété, le nom affiché est relatif au type qui définit initialement la propriété. Cependant, la syntaxe XAML Windows Runtime prend en charge un modèle d’héritage de classe pour les classes [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Vous pouvez donc souvent utiliser un attribut sur une classe qui n’est pas littéralement la classe de définition, mais qui dérive d’une classe qui a au préalable défini la propriété/l’attribut. Par exemple, vous pouvez définir [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) en tant qu’attribut sur toute classe dérivée de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) avec un héritage complet. Par exemple : `<Button Visibility="Visible" />`. Ne considérez donc pas le nom d’élément affiché dans la syntaxe d’utilisation XAML de manière trop littérale. La syntaxe peut être viable pour des éléments représentant cette classe, mais aussi pour des éléments représentant une classe dérivée. Dans les cas où il est rare ou impossible que le type indiqué en tant qu’élément de définition soit dans une utilisation réelle, ce nom de type est délibérément en minuscules dans la syntaxe. Par exemple, la syntaxe affichée pour **UIElement.Visibility** est :

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

De nombreuses sections de syntaxe XAML incluent des espaces réservés dans « Utilisation » qui sont alors définis dans une section **Valeurs XAML** figurant directement sous la section **Syntaxe**.

Les sections d’utilisation XAML emploient également différents espaces réservés généralisés. Ces espaces réservés ne sont pas redéfinis chaque fois dans **Valeurs XAML**, car vous devinerez ou finirez par savoir ce qu’ils représentent. Comme nous pensons que la plupart des lecteurs se fatigueraient de les voir encore et toujours dans **Valeurs XAML**, nous les avons exclus des définitions. Pour référence, voici une liste de certains de ces espaces réservés et leur signification globale :

-   *object* : en théorie toute valeur d’objet, mais souvent en pratique limité à certains types d’objets, tels que le choix entre chaîne et objet, et vous devez consulter les remarques sur la page de référence pour obtenir d’autres informations.
-   <bpt id="p1">*</bpt>object<ept id="p1">*</ept> <bpt id="p2">*</bpt>property<ept id="p2">*</ept>: <bpt id="p3">*</bpt>object<ept id="p3">*</ept> <bpt id="p4">*</bpt>property<ept id="p4">*</ept> in combination is used for cases where the syntax being shown is the syntax for a type that can be used as an attribute value for many properties. For example, the <bpt id="p1">**</bpt>Xaml Attribute Usage<ept id="p1">**</ept> shown for <bpt id="p2">[</bpt><bpt id="p3">**</bpt>Brush<ept id="p3">**</ept><ept id="p2">](/uwp/api/Windows.UI.Xaml.Media.Brush)</ept> includes: &lt;<bpt id="p4">*</bpt>object<ept id="p4">*</ept> <bpt id="p5">*</bpt>property<ept id="p5">*</ept>="<bpt id="p6">*</bpt>predefinedColorName<ept id="p6">*</ept>"/&gt;
-   *eventhandler* : cet espace réservé apparaît en tant que valeur d’attribut pour toute syntaxe XAML affichée pour un attribut d’événement. Ce que vous indiquez ici est le nom d’une fonction de gestionnaire d’événements. Cette fonction doit être définie dans le code-behind pour la page XAML. Au niveau de la programmation, cette fonction doit correspondre à la signature du délégué de l’événement que vous gérez, ou le code de votre application ne sera pas compilé. Toutefois, comme il s’agit vraiment d’une considération relative à la programmation et non à XAML, nous n’allons en rien aborder le type délégué dans la syntaxe XAML. Si vous voulez connaître le délégué à implémenter pour un événement, la réponse se trouve dans la section **Informations sur les événements** de la rubrique de référence relative à l’événement, dans une ligne de tableau intitulée **Délégué**.
-   *enumMemberName* : affiché dans la syntaxe d’attribut pour toutes les énumérations. Il existe un espace réservé similaire pour les propriétés qui utilisent une valeur d’énumération, mais il comprend généralement en préfixe un indicateur du nom de l’énumération. Par exemple, la syntaxe affichée pour [**FrameworkElement.FlowDirection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) is <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>. Si vous vous trouvez sur l’une de ces pages de référence de propriétés, cliquez sur le lien vers le type d’énumération qui s’affiche dans la section **Valeur de propriété**, en regard du texte **Type :** . Pour connaître la valeur d’attribut d’une propriété qui utilise cette énumération, vous pouvez utiliser toute chaîne répertoriée dans la colonne **Membre** du tableau **Membres**.
-   *double*, *int*, *string*, *bool* : il s’agit des types de primitives connus en langage XAML. Si vous programmez en C# ou Visual Basic, ces types sont projetés en types équivalents Microsoft .NET comme [**Double**](https://docs.microsoft.com/dotnet/api/system.double), [**Int32**](https://docs.microsoft.com/dotnet/api/system.int32), [**String**](https://docs.microsoft.com/dotnet/api/system.string) et [**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean), et vous pouvez utiliser tous les membres sur ces types .NET lorsque vous employez vos valeurs définies en XAML dans du code-behind en .NET. Si vous programmez en C++/CX, vous utiliserez les types de primitives C++, mais vous pouvez également les considérer comme équivalents aux types définis par l’espace de noms [**Platform**](https://docs.microsoft.com/cpp/cppcx/platform-namespace-c-cx), par exemple [**Platform::String**](https://docs.microsoft.com/cpp/cppcx/platform-string-class). Il y aura parfois d’autres restrictions de valeurs pour des propriétés particulières. Cependant, celles-ci sont généralement indiquées dans une section **Valeur de propriété** ou Remarques et non dans une section XAML, car de telles restrictions s’appliquent à la fois aux utilisations de code et XAML.

## <a name="tips-and-tricks-notes-on-style"></a>Conseils et astuces, remarques sur le style

-   Les extensions de balisage en général sont décrites dans la principale [Vue d’ensemble du langage XAML](xaml-overview.md). Néanmoins, l’extension de balisage qui a le plus d’impact sur les recommandations données dans cette rubrique est l’extension de balisage [StaticResource](staticresource-markup-extension.md) (et le [ThemeResource](themeresource-markup-extension.md) associé). La fonction de l’extension de balisage StaticResource est de permettre la factorisation de votre code XAML en ressources réutilisables provenant d’un [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) XAML. Vous définissez presque toujours des modèles de contrôle et des styles associés dans un **ResourceDictionary**. De même, vous définissez souvent les plus petites parties d’un modèle de contrôle ou un style spécifique à l’application dans un **ResourceDictionary**, par exemple un élément [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pour une couleur que votre application utilise plusieurs fois pour différentes parties de l’interface utilisateur. Lorsqu’un élément StaticResource est utilisé, toute propriété qui, sans cela, nécessiterait l’utilisation d’un élément propriété pour être définie peut désormais être définie dans la syntaxe d’attribut. Mais les avantages de la factorisation du code XAML pour la réutilisation vont au-delà de la seule simplification de la syntaxe au niveau de la page. Pour plus d’informations, voir [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).
-   Des exemples de code XAML présentent plusieurs conventions différentes sur la façon dont les espaces et les sauts de ligne sont appliqués. En particulier, il existe différentes conventions sur la façon de diviser des éléments objets pour lesquels beaucoup d’attributs différents sont définis. C’est juste une question de style. L’éditeur XML Visual Studio applique certaines règles de style par défaut lorsque vous modifiez du code XAML, mais vous pouvez les changer dans les paramètres. Dans un petit nombre de cas, l’espace blanc dans un fichier XAML est considéré comme significatif. Pour plus d’informations, voir [XAML et espace blanc](xaml-and-whitespace.md).

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* <bpt id="p1">[</bpt>XAML namespaces and namespace mapping<ept id="p1">](xaml-namespaces-and-namespace-mapping.md)</ept>
* [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
 

