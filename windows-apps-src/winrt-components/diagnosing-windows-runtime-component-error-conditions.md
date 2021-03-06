---
title: Diagnostic des conditions d’erreur d’un composant Windows Runtime
description: Cet article fournit des informations supplémentaires sur les restrictions relatives aux composants Windows Runtime écrits avec du code managé.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55bf6360f09ba4ab6c7878543ecfa0c80c4558e3
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "72252310"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Diagnostic des conditions d’erreur d’un composant Windows Runtime

Cet article fournit des informations supplémentaires sur les restrictions relatives aux composants Windows Runtime écrits avec du code managé. Il développe les informations fournies dans les messages d’erreur de [Winmdexp. exe (outil d’exportation de métadonnées Windows Runtime)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)et complète les informations sur les restrictions fournies dans [Windows Runtime composants avec C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

Cet article ne couvre pas toutes les erreurs. Les erreurs décrites ici sont regroupées par catégorie générale, et chaque catégorie inclut un tableau des messages d’erreur associés. Recherchez le texte du message (en omettant les valeurs spécifiques des espaces réservés) ou le numéro du message. Si vous ne trouvez pas les informations dont vous avez besoin ici, veuillez nous aider à améliorer la documentation à l’aide du bouton de commentaire à la fin de cet article. Fournissez le message d’erreur. Autrement, vous pouvez déposer un bogue sur le site web Microsoft Connect.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>Message d’erreur pour l’implémentation d’interface asynchrone qui fournit un type incorrect

Les composants de Windows Runtime managés ne peuvent pas implémenter les interfaces plateforme Windows universelle (UWP) qui représentent des actions ou des opérations asynchrones ([IAsyncAction](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [IAsyncActionWithProgress&lt;TProgress&gt;](https://docs.microsoft.com/previous-versions/br205784(v=vs.85)), [IAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)ou [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)). Au lieu de cela, .NET fournit la classe [AsyncInfo](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime) pour générer des opérations asynchrones dans les composants Windows Runtime. Le message d’erreur affiché par Winmdexp.exe quand vous essayez d’implémenter une interface asynchrone de façon incorrecte fait référence à cette classe en utilisant son ancien nom, AsyncInfoFactory. .NET n’intègre plus la classe AsyncInfoFactory.

| Numéro d’erreur | Texte du message|       
|--------------|-------------|
| WME1084      | Le type'{0}'implémente Windows Runtime interface Async'{1}'. Les types Windows Runtime ne peuvent pas implémenter les interfaces asynchrones. Veuillez utiliser la classe System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory pour générer des opérations asynchrones afin d’exporter vers Windows Runtime. |

> **Notez** les messages d’erreur qui font référence à l’Windows Runtime utilisent une terminologie ancienne. On utilise maintenant l’appellation « plateforme Windows universelle (UWP) ». Par exemple, les types Windows Runtime sont désormais appelés types UWP.

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>Références manquantes à mscorlib.dll ou System.Runtime.dll

Ce problème se produit uniquement lorsque vous utilisez Winmdexp.exe à partir de la ligne de commande. Nous vous recommandons d’utiliser l’option/reference pour inclure des références à la fois à mscorlib. dll et à System. Runtime. dll à partir des assemblys de référence .NET Framework Core, qui se trouvent dans les assemblys de référence "% ProgramFiles (x86)%\\\\Microsoft\\Framework\\. Netcore\\v 4.5 "("% ProgramFiles%\\... " sur un ordinateur 32 bits).

| Numéro d’erreur | Texte du message                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | Aucune référence n’a été faite à mscorlib.dll. Une référence à ce fichier de métadonnées est requise pour exporter correctement.                               |
| WME1090      | Impossible de déterminer le principal assembly de référence. Assurez-vous que mscorlib.dll et System.Runtime.dll sont référencés à l’aide du commutateur /reference. |

## <a name="operator-overloading-is-not-allowed"></a>Surcharge d’opérateur non autorisée

Dans un composant Windows Runtime écrit en code managé, vous ne pouvez pas exposer les opérateurs surchargés sur les types publics.

> **Notez** dans le message d’erreur, l’opérateur est identifié par son nom de métadonnées, par exemple op\_addition, op\_multiplier, op\_exclusif, op\_implicite (conversion implicite), et ainsi de suite.

| Numéro d’erreur | Texte du message                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | '{0}'est une surcharge d’opérateur. Les types managés ne peuvent pas exposer les surcharges de l’opérateur dans le Windows Runtime. |

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>Constructeurs d’une classe ayant le même nombre de paramètres

Dans l’UWP, une classe ne peut avoir qu’un seul constructeur avec un nombre donné de paramètres ; par exemple, vous ne pouvez pas avoir un constructeur ayant un seul paramètre de type **String** et un autre possédant un seul paramètre de type **int** (**Integer** en Visual Basic). La seule solution consiste à utiliser un nombre différent de paramètres pour chaque constructeur.

| Numéro d’erreur | Texte du message                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | Le type'{0}'a plusieurs constructeurs avec un ou plusieurs arguments'{1}'. Les types Windows Runtime ne peuvent pas posséder plusieurs constructeurs ayant le même nombre d’arguments. |

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>Spécification obligatoire d’une valeur par défaut pour les surcharges ayant le même nombre de paramètres

Dans l’UWP, les méthodes surchargées peuvent avoir le même nombre de paramètres uniquement si une surcharge est spécifiée en tant que celle par défaut. Consultez « Méthodes surchargées » dans [Windows Runtime composants avec C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

| Numéro d’erreur | Texte du message                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | Plusieurs surcharges de paramètre de {0}de'{1}.{2}» sont décorés avec Windows. Foundation. Metadata. DefaultOverloadAttribute.                                                            |
| WME1085      | Surcharges de paramètre {0}de {1}.{2} doit avoir une seule méthode spécifiée en tant que surcharge par défaut en la décorant avec Windows. Foundation. Metadata. DefaultOverloadAttribute. |

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Erreurs d’espace de noms et noms non valides pour le fichier de sortie

Dans la plateforme Windows universelle, tous les types publics dans un fichier de métadonnées Windows (.winmd) doivent se trouver dans un espace de noms qui partage le nom du fichier .winmd, ou dans des sous-espaces de noms du nom de fichier. Par exemple, si votre projet Visual Studio est nommé A.B (autrement dit, votre composant Windows Runtime est A.B.winmd), il peut contenir des classes publiques A.B.Class1 et A.B.C.Class2, mais pas A.Class3 (WME0006) ou D.Class4 (WME1044).

> **Remarque**  Ces restrictions s’appliquent uniquement aux types publics, non aux types privés utilisés dans votre implémentation.

Dans le cas de A.Class3, vous pouvez déplacer Class3 dans un autre espace de noms ou remplacer le nom du composant Windows Runtime par A.winmd. Bien que WME0006 soit un avertissement, vous devez le traiter comme une erreur. Dans l’exemple précédent, le code qui appelle A.B.winmd ne pourra pas localiser A.Class3.

Dans le cas de D.Class4, aucun nom de fichier ne peut contenir à la fois D.Class4 et des classes dans l’espace de noms A.B ; la modification du nom du composant Windows Runtime n’est donc pas possible. Vous pouvez déplacer D.Class4 vers un autre espace de noms ou le placer dans un autre composant Windows Runtime.

Le système de fichiers ne peut pas effectuer la distinction entre majuscules et minuscules ; par conséquent, les espaces de noms dont la casse diffère ne sont pas autorisés (WME1067).

Votre composant doit contenir au moins un type **public sealed** (**Public NotInheritable** en Visual Basic). Si ce n’est pas le cas, vous obtiendrez WME1042 ou WME1043, selon si votre composant contient ou non des types privés.

Un type dans un composant Windows Runtime ne peut pas avoir un nom identique à un espace de noms (WME1068).

> **Attention**  Si vous appelez Winmdexp.exe directement et n’utilisez pas l’option /out pour spécifier un nom pour votre composant Windows Runtime, Winmdexp.exe essaie de générer un nom qui inclut tous les espaces de noms dans le composant. Le fait de renommer les espaces de noms peut modifier le nom de votre composant.

 

| Numéro d’erreur | Texte du message                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | '{0}'n’est pas un nom de fichier winmd valide pour cet assembly. Tous les types d’un fichier de métadonnées Windows doivent exister dans un sous-espace de noms de l’espace de noms impliqué par le nom de fichier. Les types qui n’existent pas dans un sous-espace de noms ne peuvent pas être localisés au moment de l’exécution. Dans cet assembly, le plus petit espace de noms commun pouvant faire office de nom de fichier est « {1} ». |
| WME1042      | Le module d’entrée doit contenir au moins un type public se trouvant à l’intérieur d’un espace de noms.                                                                                                                                                                                                                                                                   |
| WME1043      | Le module d’entrée doit contenir au moins un type public se trouvant à l’intérieur d’un espace de noms. Les seuls types présents dans des espaces de noms sont privés.                                                                                                                                                                                                               |
| WME1044      | Un type public a un espace de noms («{1}») qui ne partage aucun préfixe commun avec d’autres espaces de noms («{0}»). Tous les types d’un fichier de métadonnées Windows doivent exister dans un sous-espace de noms de l’espace de noms impliqué par le nom de fichier.                                                                                                                              |
| WME1067      | Les noms d’espaces de noms ne peuvent pas différer uniquement par la casse : '{0}', '{1}'.                                                                                                                                                                                                                                                                                                |
| WME1068      | Le type'{0}'ne peut pas avoir le même nom que l’espace de noms'{1}'.                                                                                                                                                                                                                                                                                                 |

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>Exportation des types qui ne sont pas des types de plateforme Windows universelle valides

L’interface publique de votre composant doit exposer uniquement les types UWP. Toutefois, .NET fournit des mappages pour plusieurs types couramment utilisés qui sont légèrement différents dans .NET et UWP. Cela permet au développeur .NET de travailler avec des types familiers au lieu d’en apprendre de nouveaux. Vous pouvez utiliser ces types .NET mappés dans l’interface publique de votre composant. Consultez « Déclaration des types dans les composants Windows Runtime » et « transmission de types plateforme Windows universelle au code managé » dans [Windows Runtime C# composants avec et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md), et [mappages .net de types Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

Bon nombre de ces mappages sont des interfaces. Par exemple, [IList&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1) mappe vers l’interface UWP [IVector&lt;T&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IVector_T_). Si vous utilisez List&lt;string&gt; (`List(Of String)` en Visual Basic) au lieu de IList&lt;string&gt; en tant que type de paramètre, Winmdexp.exe fournit une liste de possibilités qui comprend toutes les interfaces mappées implémentées par List&lt;T&gt;. Si vous utilisez des types génériques imbriqués, tels que List&lt;Dictionary&lt;int, string&gt;&gt; (List(Of Dictionary(Of Integer, String)) en Visual Basic), Winmdexp.exe offre des choix pour chaque niveau d’imbrication. Ces listes peuvent considérablement s’allonger.

En général, le meilleur choix est l’interface qui est la plus proche du type. Par exemple, pour Dictionary&lt;int, string&gt;, le meilleur choix est sans doute IDictionary&lt;int, string&gt;.

> **Important** JavaScript utilise l’interface qui s’affiche en premier dans la liste des interfaces implémentées par un type managé. Par exemple, si vous retournez Dictionary&lt;int, string&gt; au code JavaScript, il apparaît comme IDictionary&lt;int, string&gt;, quelle que soit l’interface que vous spécifiez comme type de retour. Cela signifie que si la première interface n’inclut pas un membre qui apparaît sur les interfaces ultérieures, ce membre n’est pas visible pour JavaScript.

> **Attention**  Évitez d’utiliser les interfaces non génériques [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist) et [IEnumerable](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) si votre composant doit être utilisé par JavaScript. Ces interfaces mappent vers [IBindableVector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.interop.ibindablevector) et [IBindableIterator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.interop.ibindableiterator), respectivement. Elles prennent en charge la liaison pour les contrôles XAML et sont invisibles dans JavaScript. JavaScript émet l’erreur d’exécution « La fonction “X” a une signature non valide et ne peut pas être appelée ».

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Numéro d’erreur</th>
<th align="left">Texte du message</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">La méthode «{0}» a le paramètre «{1}» de type «{2}». « {2} » n’est pas un type de paramètre Windows Runtime valide.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">La méthode «{0}» a un paramètre de type «{1}» dans sa signature. Bien que ce type ne soit pas un type Windows Runtime valide, il implémente les interfaces qui sont des types Windows Runtime valides. Modifiez la signature de la méthode pour utiliser l’un des types suivants à la place : « {2} ».</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>La méthode «{0}» a un paramètre de type «{1}» dans sa signature. Bien que ce type générique ne soit pas un type Windows Runtime valide, ce type ou ses paramètres génériques implémentent les interfaces qui sont des types Windows Runtime valides. [https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/]({2})</p>
> **Remarque**  Pour {2}, Winmdexp. exe ajoute une liste d’alternatives, telles que « la modification du type System. Collections. Generic. List&lt;T&gt;» dans la signature de méthode vers l’un des types suivants à la place : 'System. Collections. Generic. IList&lt;T&gt;, System. Collections. Generic. IReadOnlyList&lt;T&gt;, System. Collections. Generic. IEnumerable&lt;T&gt;'.»
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">La méthode «{0}» a un paramètre de type «{1}» dans sa signature. Au lieu d’utiliser un type de tâche managé, utilisez Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation ou l’une des autres interfaces asynchrones Windows Runtime. Le modèle .NET standard await s’applique également à ces interfaces. Pour plus d’informations sur la conversion d’objets de tâches managées en interfaces asynchrones Windows Runtime, voir System.Runtime.InteropServices.WindowsRuntime.AsyncInfo.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Structures contenant des champs de types non autorisés


Dans l’UWP, une structure peut contenir uniquement des champs, et seules les structures peuvent contenir des champs. Ces champs doivent être publics. Les types de champ valides incluent des énumérations, des structures et des types primitifs.

| Numéro d’erreur | Texte du message                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | La structure «{0}» a le champ «{1}» de type «{2}». « {2} » n’est pas un type de champ Windows Runtime valide. Les champs d’une structure Windows Runtime doivent uniquement avoir la valeur UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum, ou bien correspondre à une structure. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>Restrictions sur les tableaux dans les signatures de membre


Dans l’UWP, les tableaux dans les signatures de membre doivent être unidimensionnels avec une limite inférieure de 0 (zéro). Les types de tableaux imbriqués tels que `myArray[][]` (`myArray()()` en Visual Basic) ne sont pas autorisés.

> **Notez** cette restriction ne s’applique pas aux tableaux que vous utilisez en interne dans votre implémentation.

 

| Numéro d’erreur | Texte du message                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | La méthode «{0}» a un tableau de type «{1}» avec une limite inférieure différente de zéro dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime doivent avoir une limite inférieure de zéro. |
| WME1035      | La méthode «{0}» a un tableau multidimensionnel de type «{1}» dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime doivent être unidimensionnels.                  |
| WME1036      | La méthode «{0}» a un tableau imbriqué de type «{1}» dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime ne peuvent pas être imbriqués.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>Les paramètres de tableau doivent spécifier si le contenu du tableau est accessible en lecture ou en écriture


Dans l’UWP, les paramètres doivent être en lecture seule ou en écriture seule. Les paramètres ne peuvent pas être marqués **ref** (**ByRef** sans l’attribut [OutAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute) en Visual Basic). Cela s’applique au contenu des tableaux ; par conséquent, les paramètres de tableau doivent indiquer si le contenu du tableau est en lecture seule ou en écriture seule. La direction est claire pour les paramètres **out** (paramètre **ByRef** avec l’attribut OutAttribute en Visual Basic), mais les paramètres de tableau passés par valeur (ByVal en Visual Basic) doivent être marqués. Voir [Transmission de tableaux à un composant Windows Runtime](passing-arrays-to-a-windows-runtime-component.md).

| Numéro d’erreur | Texte du message         |
|--------------|----------------------|
| WME1101      | La méthode «{0}» a le paramètre «{1}» qui est un tableau, et qui a à la fois {2} et {3}. Dans le Windows Runtime, les paramètres du tableau de contenu doivent être accessibles en lecture ou en écriture. Supprimez l’un des attributs de « {1} ».                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | La méthode «{0}» a un paramètre de sortie «{1}» qui est un tableau, mais qui a {2}. Dans le Windows Runtime, le contenu des tableaux de sortie est accessible en écriture. Supprimez l’attribut de « {1} ».                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | La méthode'{0}'a le paramètre'{1}'qui est un tableau, et qui a un System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. Dans le Windows Runtime, les paramètres de tableau doivent avoir {2} ou {3}. Supprimez les attributs suivants ou remplacez-les par l’attribut de Windows Runtime approprié si nécessaire.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | La méthode'{0}'a le paramètre'{1}'qui n’est pas un tableau et qui a un {2} ou un {3}. Windows Runtime ne prend pas en charge le marquage des paramètres autres que des tableaux associés à {2} ou {3}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | La méthode'{0}'a le paramètre'{1}'avec un System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. Windows Runtime ne prend pas en charge le marquage des paramètres avec l’attribut System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Supprimez System.Runtime.InteropServices.InAttribute et remplacez System.Runtime.InteropServices.OutAttribute par le modificateur « out ». La méthode'{0}'a le paramètre'{1}'avec un System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. Windows Runtime ne prend en charge que le marquage des paramètres ByRef par System.Runtime.InteropServices.OutAttribute, et ne prend en charge aucune autre utilisation de ces attributs. |
| WME1106      | La méthode «{0}» a le paramètre «{1}» qui est un tableau. Dans le Windows Runtime, le contenu des paramètres du tableau doit être accessible en lecture ou en écriture. Appliquez {2} ou {3} à « {1} ».                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>Membre avec un paramètre nommé « value »


Dans l’UWP, les valeurs de retour sont considérées comme des paramètres de sortie, et les noms de ces paramètres doivent être uniques. Par défaut, Winmdexp.exe donne à la valeur de retour le nom « value ». Si votre méthode possède un paramètre nommé « value », vous obtiendrez l’erreur WME1092. Il existe deux façons de corriger cette situation :

-   Donnez à votre paramètre un nom différent de « value » (dans les accesseurs de propriété, un nom différent de « returnValue »).
-   Utilisez l’attribut ReturnValueNameAttribute pour modifier le nom de la valeur de retour, comme indiqué ci-après :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **Remarque**  Si vous modifiez le nom de la valeur de retour, et que ce nouveau nom est en conflit avec le nom d’un autre paramètre, vous obtenez l’erreur WME1091.

Le code JavaScript peut accéder aux paramètres de sortie d’une méthode par nom, notamment la valeur de retour. Pour obtenir un exemple, voir l’attribut [ReturnValueNameAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.returnvaluenameattribute).

| Numéro d’erreur | Texte du message |
|--------------|--------------|
| WME1091 | La méthode «\{0} » a la valeur de retour nommée «\{1} » qui est identique à un nom de paramètre. Les paramètres de méthode Windows Runtime et la valeur de retour doivent avoir des noms uniques. |
| WME1092 | La méthode «\{0} » a un paramètre nommé «\{1} » qui est identique au nom de la valeur de retour par défaut. Fournissez un autre nom pour le paramètre ou utilisez System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute pour spécifier explicitement le nom de la valeur de retour. |

**Remarque**  Le nom par défaut est « returnValue » pour les accesseurs de propriété, et « value » pour toutes les autres méthodes.

## <a name="related-topics"></a>Rubriques connexes

* [Composants Windows Runtime avec C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp. exe (outil d’exportation de métadonnées Windows Runtime)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)
