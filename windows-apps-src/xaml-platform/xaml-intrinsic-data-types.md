---
description: Indique la prise en charge au niveau du langage en XAML pour Windows Runtime pour certains types de données dans le Common Language Runtime (CLR) et dans d’autres langages de programmation tels que C++.
title: Types de données intrinsèques XAML
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19f297e731225d28f92f63ad9359bba70f0f0b32
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340448"
---
# <a name="xaml-intrinsic-data-types"></a>Types de données intrinsèques XAML


XAML pour Windows Runtime offre une prise en charge, au niveau du langage, de plusieurs types de données qui sont des primitives souvent utilisées dans le Common Language Runtime (CLR) et dans d’autres langages de programmation tels que C++.

Le cas le plus courant dans lequel vous verrez des utilisations de types de données intrinsèques XAML est celui où des ressources sont définies dans un dictionnaire de ressources XAML. Vous pouvez y définir des constantes, par exemple, des nombres que vous utilisez pour plusieurs valeurs. Ou vous pouvez utiliser une animation dans une table de montage séquentiel qui s’anime en utilisant une chaîne ou une valeur booléenne, puis vous aurez besoin d’un élément objet XAML représentant la chaîne ou la valeur booléenne pour remplir l’image clé de votre définition [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). Les modèles XAML par défaut Windows Runtime utilisent ces deux techniques.

XAML pour le Windows Runtime offre une prise en charge au niveau du langage pour les types suivants :

| Primitive XAML | Description |
|-------|-------------|
| **x:Boolean**  | Pour la prise en charge du CLR, correspond à [**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean). Le code XAML analyse les valeurs de **x:Boolean** sans tenir compte de la casse. Notez que « x:Bool » n’est pas une alternative acceptée. |
| **x:String**   | Pour la prise en charge du CLR, correspond à [**String**](https://docs.microsoft.com/dotnet/api/system.string). Le codage de la chaîne a pour valeur par défaut le codage XML environnant. |
| **x :Double**   | Pour la prise en charge du CLR, correspond à [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Outre les valeurs numériques, la syntaxe texte pour **x:Double** autorise le jeton « NaN », ce qui correspond à la manière dont vous pouvez stocker « Auto » pour le comportement de la disposition en tant que valeur de ressource. Les jetons sont traités en tenant compte de la casse. Vous pouvez utiliser une notation scientifique, par exemple « 1+E06 » pour `1,000,000`. |
| **x:Int32**    | Pour la prise en charge du CLR, correspond à [**Int32**](https://docs.microsoft.com/dotnet/api/system.int32). **x:Int32** est traité comme signé et vous pouvez inclure le symbole moins (« - ») pour un entier négatif. En XAML, l’absence de signe dans la syntaxe texte implique une valeur signée positive. |

Ces primitives de langage XAML sont en général les seuls cas où vous définissez un élément objet qui utilise le préfixe **x:** dans votre XAML. Toutes les autres fonctionnalités de langage XAML sont en général utilisées sous forme d’attributs ou en tant qu’extension de balisage.

**Notez**  par Convention, les primitives de langage pour XAML et tous les autres éléments de langage XAML sont indiquées par le préfixe « x : ». C’est ainsi que les éléments du langage XAML sont habituellement utilisés dans un balisage réel. Cette convention est suivie dans la documentation relative à XAML et également dans la spécification XAML.

## <a name="other-xaml-primitives"></a>Autres primitives XAML

La spécification XAML 2009 indique d’autres primitives au niveau du langage XAML, telles que **x:Uri** et **x:Single**. Sauf indication contraire dans le tableau de cette rubrique, les autres primitives du langage XAML telles que définies par les vocabulaires XAML ou par la spécification XAML 2009 ne sont actuellement pas prises en charge en XAML pour Windows Runtime.

**Notez**  les dates et les heures (les propriétés qui utilisent [**DateTime**](https://docs.microsoft.com/uwp/api/Windows.Foundation.DateTime) ou [**DateTimeOffset**](https://docs.microsoft.com/dotnet/api/system.datetimeoffset), [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) ou [**System. TimeSpan**](https://docs.microsoft.com/dotnet/api/system.timespan)) ne sont pas définissables avec une primitive XAML. En règle générale, ces propriétés ne sont pas du tout définissables en XAML, car, par défaut, l’analyseur XAML Windows Runtime ne prend pas en charge la conversion à partir d’une chaîne pour les dates et les heures. Pour les valeurs d’initialisation de toutes les propriétés de date et d’heure, vous devez utiliser du code-behind qui s’exécute au chargement d’une page ou d’un élément.

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Guide de syntaxe XAML](xaml-syntax-guide.md)
* [Animations de Storyboard](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
 

