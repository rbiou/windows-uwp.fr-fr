---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Cet article décrit comment afficher rapidement le flux d’aperçu de l’appareil photo sur une page XAML dans une application UWP.
title: Afficher l’aperçu de l’appareil photo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f35cbab511912bd9cf6616330f3e9e7737189fd
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339742"
---
# <a name="display-the-camera-preview"></a>Afficher l’aperçu de l’appareil photo


Cet article décrit comment afficher rapidement le flux d’aperçu de l’appareil photo sur une page XAML dans une application UWP. La création d’une application qui capture des photos et des vidéos à l’aide de l’appareil photo nécessite que vous effectuiez des tâches telles que la gestion de l’orientation de l’appareil et de la caméra ou la définition des options de codage pour le fichier capturé. Pour certains scénarios d’application, vous pouvez simplement afficher le flux d’aperçu à partir de l’appareil photo sans tenir compte de ces autres considérations. Cet article vous montre comment effectuer cette opération avec un minimum de code. Vous devez toujours arrêter le flux d’aperçu correctement lorsque vous avez fini de l’utiliser en suivant les étapes ci-dessous.

Pour plus d’informations sur l’écriture d’une application d’appareil photo qui capture les photos ou les vidéos, consultez la section [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Ajouter des déclarations de fonctionnalités au manifeste de l’application

Afin que votre application puisse accéder à l’appareil photo d’un appareil, vous devez déclarer que cette application utilise les fonctionnalités de l’appareil *webcam* et *microphone*. 

**Ajouter des fonctionnalités au manifeste d’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.

## <a name="add-a-captureelement-to-your-page"></a>Ajouter un objet CaptureElement à votre page

Utilisez un [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) pour afficher le flux d’aperçu sur votre page XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Utiliser MediaCapture pour démarrer le flux d’aperçu

L’objet [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) est l’interface de votre application pour l’appareil photo de l’appareil. Cette classe est membre de l’espace de noms Windows.Media.Capture. L’exemple de cet article utilise également des API à partir des espaces de noms [**Windows.ApplicationModel**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel) et [System.Threading.Tasks](https://docs.microsoft.com/dotnet/api/system.threading.tasks), en plus de celles fournies par le modèle de projet par défaut.

Ajoutez des directives using pour inclure les espaces de noms suivants dans le fichier .cs de votre page.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Déclarez une variable de membre de classe pour l’objet **MediaCapture** et une valeur booléenne afin de détecter l’activation de l’aperçu sur l’appareil photo. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Déclarez une variable de type [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest), qui sera utilisée pour garantir que l’affichage ne s’éteindra pas pendant l’aperçu.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Créez une méthode d’assistance pour démarrer l’aperçu de l'appareil photo, appelée **StartPreviewAsync** dans cet exemple. En fonction du scénario de votre application, vous lancerez peut-être votre appel à partir du gestionnaire d’événements **OnNavigatedTo** qui est appelé lorsque la page est chargée ou vous attendrez pour lancer la version d’évaluation en réponse aux événements de l’interface utilisateur.

Créez une instance de la classe **MediaCapture** et appelez [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) pour initialiser l’appareil de capture. Cette méthode pouvant par exemple échouer sur des appareils n’ayant pas d’appareil photo, vous devez donc l’appeler à partir d’un bloc **try**. Une **UnauthorizedAccessException** est levée lorsque vous tentez d’initialiser l’appareil photo si l’utilisateur a désactivé l’accès à ce dernier dans les paramètres de confidentialité de l’appareil. Vous verrez également cette exception au cours du développement si vous n’avez pas ajouté les fonctionnalités appropriées au manifeste de votre application.

**Important** Sur certaines familles d’appareils, l’utilisateur doit approuver une invite de consentement avant que votre application puisse avoir accès à l’appareil photo de l’appareil. Pour cette raison, vous devez uniquement appeler [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) à partir du thread d’interface utilisateur principal. Toute tentative d’initialiser l’appareil photo à partir d’un autre thread peut entraîner un échec de l’initialisation.

Connectez **MediaCapture** à **CaptureElement** en définissant la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.captureelement.source). Démarrez l’aperçu en appelant [**StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Cette méthode permet de lever une **FileLoadException** si une autre application dispose d’un contrôle exclusif du périphérique de capture. Consultez la section suivante pour en savoir plus sur l’écoute des modifications du contrôle exclusif.

Appelez la méthode [**RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) afin de vous assurer que l’appareil ne se met pas en veille pendant l’aperçu. Enfin, définissez la propriété [**DisplayInformation.AutoRotationPreferences**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) sur [**Landscape**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) afin de prévenir toute rotation de l’interface utilisateur et de **CaptureElement** lorsque l’utilisateur modifie l’orientation de l’appareil. Pour plus d’informations sur le traitement des modifications d’orientation de l’appareil, consultez la rubrique [**Gérer l’orientation de l’appareil à l’aide de MediaCapture**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>Gérer les modifications du contrôle exclusif
Comme indiqué dans la section précédente, **StartPreviewAsync** lève une **FileLoadException** si une autre application dispose d’un contrôle exclusif du périphérique de capture. Depuis Windows 10, version 1703, vous pouvez inscrire un gestionnaire pour l’événement [MediaCapture.CaptureDeviceExclusiveControlStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged), qui est déclenché à chaque fois que l’état du contrôle exclusif du périphérique change. Dans le gestionnaire de cet événement, vérifiez la propriété [MediaCaptureDeviceExclusiveControlStatusChangedEventArgs.Status](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) pour voir l’état actuel. Si le nouvel état est **SharedReadOnlyAvailable**, vous savez que vous ne pouvez pas démarrer l’aperçu et vous pouvez mettre à jour votre interface utilisateur pour alerter l’utilisateur. Si le nouvel état est **ExclusiveControlAvailable**, vous pouvez essayer de redémarrer l’aperçu de l’appareil photo.

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>Arrêter le flux d’aperçu

Lorsque vous avez fini d’utiliser le flux d’aperçu, vous devez toujours arrêter le flux et supprimer correctement les ressources associées afin de garantir que l’appareil photo est disponible pour d’autres applications sur l’appareil. Les étapes nécessaires à l’arrêt du flux d’aperçu sont les suivantes :

-   Si l’aperçu est activé sur l’appareil photo, appelez [**StopPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.stoppreviewasync) afin d’arrêter le flux d’aperçu. Une exception est levée si vous appelez **StopPreviewAsync** alors que l’aperçu n’est pas exécuté.
-   Définissez la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.captureelement.source) de **CaptureElement** sur null. Utilisez [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) pour vous assurer que cet appel est exécuté sur le thread de l’interface utilisateur.
-   Appelez la méthode [**Dispose**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.dispose) de l’objet **MediaCapture** pour libérer l’objet. Là encore, utilisez [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) pour vous assurer que cet appel est exécuté sur le thread de l’interface utilisateur.
-   Définissez la variable membre **MediaCapture** sur null.
-   Appelez la méthode [**RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) pour activer l’extinction de l’écran en cas d’inactivité.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Vous devez arrêter le flux d’aperçu lorsque l’utilisateur quitte votre page en remplaçant la méthode [**OnNavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Vous devez également arrêter le flux d’aperçu correctement si votre application est interrompue. Pour ce faire, inscrivez un gestionnaire pour l’événement [**Application.Suspending**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.suspending) dans le constructeur de votre page.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

Dans le gestionnaire d’événements **Suspending**, vérifiez que la page est affichée par le [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) de l’application en comparant le type de page à la propriété [**CurrentSourcePageType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.currentsourcepagetype). Si la page n’est pas affichée, l’événement **OnNavigatedFrom** doit déjà avoir été déclenché et le flux d’aperçu arrêté. Si la page est affichée, obtenez un objet [**SuspendingDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingDeferral) à partir des arguments d’événement transmis au gestionnaire pour vous assurer que le système n’interrompt pas votre application avant l’arrêt du flux d’aperçu. Après l’arrêt du flux, appelez la méthode [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingdeferral.complete) de report afin que le système continue d’interrompre votre application.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture de photos, vidéo et audio de base avec MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Obtenir un aperçu du frame](get-a-preview-frame.md)
