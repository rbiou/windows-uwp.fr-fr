---
description: Cet article explique comment prendre en charge le contrat de partage dans une application de plateforme Windows universelle (UWP).
title: Partager des données
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2be088edd732a22acb11be5fc209ff25c84bae17
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218349"
---
# <a name="share-data"></a>Partager des données


Cet article explique comment prendre en charge le contrat de partage dans une application de plateforme Windows universelle (UWP). Le contrat de partage constitue un moyen simple pour partager rapidement des données, telles que du texte, des liens, des photos et vidéos, entre les applications. Par exemple, un utilisateur peut partager une page web avec ses amis à l’aide d’une application de réseau social ou enregistrer un lien dans une application de prise de notes pour s’y référer plus tard.

> [!NOTE]
> Les exemples de code de cet article sont écrits pour les applications UWP. Les applications de bureau WPF C++, Windows Forms et/Win32 doivent utiliser l’interface [IDataTransferManagerInterop](https://docs.microsoft.com/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) pour obtenir l’objet [DataTransferManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) pour une fenêtre spécifique. Pour plus d’informations, consultez l’exemple [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) .

## <a name="set-up-an-event-handler"></a>Définir un gestionnaire d’événements

Ajoutez un gestionnaire d’événements [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) à appeler lorsque l’utilisateur appelle l’option Partager. Cela peut se produire lorsque l’utilisateur appuie sur un contrôle dans votre application (par exemple, une commande de barre d’application ou un bouton) ou automatiquement dans un scénario spécifique (par exemple, si l’utilisateur a terminé un niveau et obtient un score élevé).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

Lorsqu’un événement [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) survient, votre application reçoit un objet [**DataRequest**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest). Cet objet contient une classe [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) que vous pouvez utiliser pour fournir le contenu qu’un utilisateur souhaite partager. Vous devez fournir un titre et des données à partager. La description est facultative, mais recommandée.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>Choisir les données

Vous pouvez partager différents types de données, notamment :

-   Texte brut
-   Uniform Resource Identifiers (URI)
-   HTML
-   Texte mis en forme
-   Bitmaps
-   Files
-   Données personnalisées définies par le développeur

L’objet [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) peut contenir un ou plusieurs de ces formats, dans n’importe quelle combinaison. L’exemple suivant illustre le partage de texte.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>Définir les propriétés

Lorsque vous créez un package de données en vue de le partager, vous pouvez définir diverses propriétés qui fournissent des informations supplémentaires sur le contenu partagé. Ces propriétés aident les applications cibles à améliorer l’expérience utilisateur. Par exemple, une description se révèle utile lorsque l’utilisateur partage du contenu avec plusieurs applications. De même, un lien vers une page web ou une miniature ajoutée à une image partagée servent de référence visuelle à l’utilisateur. Pour plus d’informations, voir [**DataPackagePropertySet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Toutes les propriétés sont facultatives, à l’exception du titre. La propriété title est obligatoire et doit être définie.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>Lancer l’interface utilisateur de partage

Une interface utilisateur pour le partage est fournie par le système. Pour la lancer, appelez la méthode [**ShowShareUI**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>Gérer les erreurs

Dans la plupart des cas, le partage de contenu est un processus simple. Toutefois, un élément inattendu peut toujours se produire. Par exemple, l’application peut avoir besoin que l’utilisateur sélectionne du contenu à partager alors que l’utilisateur ne l’a pas fait. Pour gérer ces situations, utilisez la méthode [**FailWithDisplayText**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_), qui affiche un message destiné à l’utilisateur en cas de problème.

## <a name="delay-share-with-delegates"></a>Retarder le partage avec les délégués

Parfois, il est dénué de sens de préparer les données que l’utilisateur veut partager sur le champ. Par exemple, si votre application prend en charge l’envoi d’un fichier image de grande taille dans différents formats possibles, il n’est pas efficace de créer toutes ces images avant que l’utilisateur effectue sa sélection.

Pour résoudre ce problème, un [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) peut contenir un délégué (fonction appelée lorsque l’application réceptrice demande des données). Nous vous recommandons d’utiliser un délégué chaque fois que les données que souhaite partager un utilisateur font appel à des ressources importantes.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>Voir aussi 

* [Communication entre les applications](index.md)
* [Recevoir des données](receive-data.md)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [DataRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 

