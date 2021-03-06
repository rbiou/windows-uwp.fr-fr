---
description: Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers.
title: Copier et coller
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dfa5c15c2bd4d82588e0b197dc265c4b529e64c9
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302683"
---
# <a name="copy-and-paste"></a>Copier et coller

Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers. Le copier-coller est la méthode classique d’échanger des données entre les applications, ou dans une application, et presque chaque application peut prendre en charge les opérations du Presse-papiers dans une certaine mesure. Pour obtenir des exemples de code complets qui illustrent différents scénarios de copie et de collage, consultez l' [exemple du presse-papiers](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard).

## <a name="check-for-built-in-clipboard-support"></a>Rechercher la prise en charge intégrée du Presse-papiers

Le plus souvent, vous n’avez pas besoin d’écrire de code supplémentaire pour fournir une prise en charge des opérations du Presse-papiers. De nombreux contrôles XAML par défaut disponibles pour créer les applications offrent déjà une prise en charge des opérations du Presse-papiers. 

## <a name="get-set-up"></a>Se préparer

Tout d’abord, incluez l’espace de noms [**Windows.ApplicationModel.DataTransfer**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer) dans votre application. Ensuite, ajoutez une instance à l’objet [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Cet objet contient les données que l’utilisateur souhaite copier ainsi que les propriétés (telles qu’une description) que vous voulez ajouter.

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Copier et Couper

Copier et Couper (également appelé *déplacement*) fonctionne presque exactement de la même manière. Choisissez l’opération que vous souhaitez effectuer à l’aide de la propriété [**RequestedOperation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation).

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>Définir le contenu copié

Vous pouvez ensuite ajouter les données sélectionnées par l’utilisateur dans l’objet [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Si ces données sont prises en charge par la classe **DataPackage** , vous pouvez utiliser l’une des [méthodes](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods) correspondantes de l’objet **DataPackage** . Voici comment ajouter du texte à l’aide de la méthode [**SetText**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) :

```cs
dataPackage.SetText("Hello World!");
```

La dernière étape consiste à ajouter l’objet [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) dans le Presse-papiers en appelant la méthode statique [**SetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent).

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>Coller

Pour obtenir le contenu du Presse-papiers, appelez la méthode statique [**GetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent). Cette méthode renvoie un objet [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) avec son contenu. Cet objet est identique à l’objet [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage), sauf qu’il est en lecture seule. Avec cet objet, vous pouvez utiliser la propriété [**AvailableFormats**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) ou la méthode [**Contains**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) pour identifier les formats disponibles. Ensuite, appelez la méthode [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) correspondante pour obtenir les données.

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>Suivi des modifications dans le Presse-papiers

En plus des commandes copier et coller, vous pouvez également effectuer le suivi des modifications dans le Presse-papiers. Vous pouvez le faire en gérant l’événement [**ContentChanged**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) du Presse-papiers.

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>Articles associés

* [Exemple de presse-papiers](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [Communication entre les applications](index.md)
* [DataTransfer](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [ControlsList](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)
* [SetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [AvailableFormats](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Comprend](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [ContentChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)

