---
title: Utilisation des objets Windows Runtime dans un environnement multi-thread | Documentation Microsoft
description: Cet article décrit la façon dont le .NET Framework gère les appels C# de et Visual Basic le code aux objets fournis par le Windows Runtime ou par Windows Runtime composants.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: windows 10, uwp, minuteur, threads
ms.localizationpriority: medium
ms.openlocfilehash: 948b16c4e093f7b638ba9abc5bf18e30da8047a2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259801"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Utilisation des objets Windows Runtime dans un environnement multi-thread
Cet article décrit la façon dont le .NET Framework gère les appels C# de et Visual Basic le code aux objets fournis par le Windows Runtime ou par Windows Runtime composants.

Dans le .NET Framework, vous pouvez accéder à n’importe quel objet à partir de plusieurs threads par défaut, sans un traitement particulier. Vous avez uniquement besoin d'une référence à l’objet. Dans l'environnement Windows Runtime, ces objets sont appelés *agiles*. La plupart des classes Windows Runtime sont agiles, mais certaines ne le sont pas, et même les classes agiles peuvent nécessiter un traitement particulier.

Dans la mesure du possible, le CLR traite les objets provenant d’autres sources, telles que l'environnement Windows Runtime, comme s'il s'agissait d'objets .NET Framework :

- Si l’objet implémente l'interface [IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) ou détient l'attribut [MarshalingBehaviorAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingbehaviorattribute.aspx) avec [MarshalingType.Agile](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingtype.aspx), le CLR traite l'objet comme étant agile.

- Si le CLR peut ordonner un appel à partir du thread dans lequel il a été effectué pour le contexte de thread de l’objet cible, il le fait en toute transparence.

- Si l’objet détient l'attribut [MarshalingBehaviorAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingbehaviorattribute.aspx) avec [MarshalingType.None](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingtype.aspx), la classe ne fournit pas les informations de rassemblement. Le CLR ne peut pas ordonner l’appel, donc il lève une exception [InvalidCastException](/dotnet/api/system.invalidcastexception), avec un message indiquant que l’objet peut être utilisé uniquement dans le contexte de thread où il a été créé.

Les sections suivantes décrivent les conséquences de ce comportement sur les objets provenant de différentes sources.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objets provenant d’un composant Windows Runtime écrit en code C# ou Visual Basic
Tous les types dans le composant qui peuvent être activés sont agiles par défaut.

> [!NOTE]
>  L'agilité n’implique pas la sécurité des threads. Dans les environnements Windows Runtime et .NET Framework, la plupart des classes ne sont pas thread-safe car la sécurité des threads implique une baisse des performances, et la plupart des objets ne sont jamais accessibles par plusieurs threads. Il est plus efficace de synchroniser l’accès à des objets individuels (ou d’utiliser des classes thread-safe) uniquement si cela est nécessaire.

Lorsque vous créez un composant Windows Runtime, vous pouvez remplacer la valeur par défaut. Reportez-vous aux interfaces [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) et [IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject).

## <a name="objects-from-the-windows-runtime"></a>Objets issus de l'environnement Windows Runtime
La plupart des classes du Windows Runtime sont agiles, et le CLR les traite comme étant agiles. La documentation de ces classes répertorie « MarshalingBehaviorAttribute(Agile) » parmi les attributs de classe. Toutefois, les membres de certaines de ces classes agiles, tels que les contrôles XAML, lèvent des exceptions s’ils ne sont pas appelés sur le thread de l’interface utilisateur. Par exemple, le code suivant tente d’utiliser un thread d’arrière-plan pour définir une propriété du bouton sur lequel l’utilisateur a cliqué. La propriété du bouton [Contenu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) lève une exception.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

Vous pouvez accéder au bouton en toute sécurité à l’aide de sa propriété [Répartiteur](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.dispatcher.aspx) ou de la propriété `Dispatcher` d’un objet existant dans le contexte de thread d’interface utilisateur (tel que la page où se trouve le bouton). Le code suivant utilise l'objet [CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) de la méthode [RunAsync](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) pour envoyer l’appel sur le thread d’interface utilisateur.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  La propriété `Dispatcher` ne lève pas d'exception lorsqu’elle est appelée à partir d’un autre thread.

La durée de vie d’un objet Windows Runtime qui est créé sur le thread d’interface utilisateur est limitée par la durée de vie du thread. N’essayez pas d’accéder aux objets sur un thread d’interface utilisateur après la fermeture de la fenêtre.

Si vous créez votre propre contrôle en héritant d’un contrôle XAML ou en composant un ensemble de contrôles XAML, votre contrôle est agile, car il s’agit d’un objet .NET Framework. Toutefois, s'il appelle les membres de sa classe de base ou des classes de composants, ou si vous appelez les membres hérités, ces membres lèveront des exceptions lorsqu’ils sont appelés à partir de n’importe quel thread à l’exception du thread d’interface utilisateur.

### <a name="classes-that-cant-be-marshaled"></a>Classes qui ne peuvent pas être appelées
Les classes Windows Runtime qui ne fournissent pas d’informations de rassemblement sont dotées de l'attribut [MarshalingBehaviorAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingbehaviorattribute.aspx) avec [MarshalingType.None](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingtype.aspx). La documentation pour une telle classe répertorie « MarshalingBehaviorAttribute(None) » parmi ses attributs.

Le code suivant crée un objet [CameraCaptureUI](https://msdn.microsoft.com/library/windows/apps/windows.media.capture.cameracaptureui.aspx) sur le thread d’interface utilisateur, puis tente de définir une propriété de l’objet à partir d’un thread de pool de threads. Le CLR ne peut pas obtenir l’appel, donc il lève une exception [System.InvalidCastException](/dotnet/api/system.invalidcastexception), avec un message indiquant que l’objet peut être utilisé uniquement dans le contexte de thread où il a été créé.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

La documentation de [CameraCaptureUI](https://msdn.microsoft.com/library/windows/apps/windows.media.capture.cameracaptureui.aspx) répertorie également « ThreadingAttribute(STA) » parmi les attributs de la classe, car il doit être créé dans un contexte de thread unique, tel que le thread d’interface utilisateur.

Si vous souhaitez accéder à l'objet [CameraCaptureUI](https://msdn.microsoft.com/library/windows/apps/windows.media.capture.cameracaptureui.aspx) à partir d’un autre thread, vous pouvez mettre en cache l'objet [CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) pour le thread d’interface utilisateur et l'utiliser ultérieurement pour envoyer l’appel sur ce thread. Ou bien, vous pouvez obtenir le répartiteur d’un objet XAML tel que la page, comme illustré dans le code suivant.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Objets à partir d’un composant Windows Runtime écrit en C++
Par défaut, les classes dans le composant qui peut être activé sont agiles. Toutefois, C++ permet un vaste contrôle sur les modèles de thread et le comportement de rassemblement. Comme décrit précédemment dans cet article, le CLR reconnaît les classes agiles, tente de regrouper les appels lorsque les classes ne sont pas agiles et lève une exception [System.InvalidCastException](/dotnet/api/system.invalidcastexception) lorsqu'une classe ne dispose d'aucune information de rassemblement.

Concernant les objets qui s’exécutent sur l’interface utilisateur de thread et qui lèvent des exceptions lorsqu’ils sont appelés à partir d’un thread autre que le thread d’interface utilisateur, vous pouvez utiliser l'objet [CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) du thread d’interface utilisateur pour distribuer l’appel.

## <a name="see-also"></a>Articles associés
[Guide C#](/dotnet/csharp/)

[Guide de Visual Basic](/dotnet/visual-basic/)
