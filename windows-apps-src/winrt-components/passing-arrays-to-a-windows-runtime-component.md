---
title: Transmission de tableaux à un composant Windows Runtime
description: Dans la plateforme universelle Windows (UWP), les paramètres sont destinés à l’entrée ou à la sortie, jamais aux deux. Cela signifie que le contenu d’un tableau qui est transmis à une méthode, ainsi que le tableau lui-même, sont destinés à l’entrée ou à la sortie.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49fb5ac5fbba5fad8123eb0167a2e00037725487
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340519"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Transmission de tableaux à un composant Windows Runtime




Dans la plateforme universelle Windows (UWP), les paramètres sont destinés à l’entrée ou à la sortie, jamais aux deux. Cela signifie que le contenu d’un tableau qui est transmis à une méthode, ainsi que le tableau lui-même, sont destinés à l’entrée ou à la sortie. Si le contenu du tableau est destiné à l’entrée, la méthode lit dans le tableau mais n’écrit pas dans celui-ci. Si le contenu du tableau est destiné à la sortie, la méthode écrit dans le tableau mais ne lit pas dans celui-ci. Cela pose un problème pour les paramètres de tableau, car les tableaux dans .NET sont des types référence et le contenu d’un tableau est mutable même lorsque la référence de tableau est passée par valeur (**ByVal** dans Visual Basic). L’[outil d’exportation de métadonnées Windows Runtime (Winmdexp.exe)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) requiert de spécifier l’utilisation prévue pour le tableau si elle n’est pas claire d’après le contexte en appliquant l’attribut ReadOnlyArrayAttribute ou WriteOnlyArrayAttribute au paramètre. L’utilisation du tableau est déterminée comme suit :

-   Pour la valeur de retour ou un paramètre out (un paramètre **ByRef** avec l’attribut [OutAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute) en Visual Basic), le tableau est toujours destiné à la sortie. N’appliquez pas l’attribut ReadOnlyArrayAttribute. L’attribut WriteOnlyArrayAttribute est autorisé sur les paramètres de sortie, mais il est redondant.

    > **Attention**  le compilateur Visual Basic n’applique pas les règles de sortie uniquement. Vous ne devez jamais lire un paramètre de sortie ; il peut contenir **Nothing**. Assignez toujours un nouveau tableau.
 
-   Les paramètres qui présentent le modificateur **ref** (**ByRef** en Visual Basic) ne sont pas autorisés. Winmdexp.exe génère une erreur.
-   Pour un paramètre transmis par valeur, vous devez indiquer si le contenu du tableau est destiné à l’entrée ou à la sortie en appliquant l’attribut [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) ou [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute). La spécification des deux attributs génère une erreur.

Si une méthode doit accepter un tableau pour l’entrée, modifier le contenu du tableau et retourner le tableau à l’appelant, utilisez un paramètre en lecture seule pour l’entrée et un paramètre en écriture seule (ou la valeur de retour) pour la sortie. Le code suivant montre un moyen d’implémenter ce modèle :

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

Nous vous conseillons d’effectuer une copie du tableau d’entrée immédiatement et de manipuler celle-ci. Cela permet de s’assurer que la méthode se comporte de la même façon, que votre composant soit appelé ou non par le code .NET.

## <a name="using-components-from-managed-and-unmanaged-code"></a>Utilisation des composants depuis du code managé et non managé


Les paramètres qui présentent l’attribut ReadOnlyArrayAttribute ou WriteOnlyArrayAttribute se comportent différemment selon que l’appelant est écrit en code natif ou en code managé. Si l’appelant est du code natif (JavaScript ou extensions des composants Visual C++), le contenu du tableau est traité comme suit :

-   ReadOnlyArrayAttribute : le tableau est copié lorsque l’appel traverse la limite de l’interface binaire d’application (ABI). Les éléments sont convertis si nécessaire. Par conséquent, toute modification accidentelle que la méthode effectue dans un tableau d’entrée uniquement n’est pas visible pour l’appelant.
-   WriteOnlyArrayAttribute : la méthode appelée ne peut pas faire d’hypothèses sur le contenu du tableau d’origine. Par exemple, le tableau que la méthode reçoit peut ne pas être initialisé ou peut contenir des valeurs par défaut. La méthode est censée définir les valeurs de tous les éléments du tableau.

Si l’appelant est du code managé, le tableau d’origine est disponible pour la méthode appelée, comme c’est le cas dans n’importe quel appel de méthode dans .NET. Le contenu du tableau étant mutable dans le code .NET, toute modification apportée par la méthode au tableau est visible par l’appelant. Il importe d’en tenir compte, car cela affecte les tests d’unité écrits pour un composant Windows Runtime. Si les tests sont écrits en code managé, le contenu d’un tableau apparaîtra mutable pendant le test.

## <a name="related-topics"></a>Rubriques connexes

* [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [Composants Windows Runtime avec C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
