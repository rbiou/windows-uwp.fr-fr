---
author: TerryWarwick
title: Héberger l'aperçu du scanneur de codes-barres à caméra
description: Découvrez comment héberger un aperçu de scanneur de codes-barres à caméra dans votre application
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 4e5765a725ad99a1092ad8c56cef674ec6210a2c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833140"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Héberger un aperçu de scanneur de codes-barres à caméra dans votre application
## <a name="step-1-setup-your-camera-preview"></a>Étape1: Installer l'aperçu de votre caméra
Vous pouvez accomplir la première étape de l’ajout d’un aperçu à votre application de scanneur de codes-barres à caméra en suivant les instructions fournies dans la rubrique [Afficher l’aperçu de la caméra](../audio-video-camera/simple-camera-preview-access.md).  Une fois que vous avez terminé cette étape, revenez à cette rubrique pour voir les modifications spécifiques du scanneur de code-barres à caméra.

## <a name="step-2-update-capability-declarations"></a>Étape2: Mettre à jour les déclarations de fonctionnalités
Pour empêcher vos utilisateurs de recevoir l'invite de consentement du microphone, vous pouvez l'exclure des fonctionnalités répertoriées dans le manifeste de votre application.

1. Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2. Sélectionnez l’onglet **Fonctionnalités**.
3. Désélectionnez la case à cocher **Microphone**.

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Étape3: Ajouter des directives d'utilisation supplémentaires pour la capture multimédia

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Étape4: Configurer vos paramètres d’initialisation de MediaCapture
L’exemple suivant initialise les paramètres [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings). 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = ClaimedBarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
    
    if (_deviceList.Count > 0)
    {
        _captureInitSettings.VideoDeviceId = _deviceList[0].Id;
    }
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Étape5: Associer votre objet MediaCapture avec le scanneur de codes-barres à caméra
Remplacez le mediaCapture.InitializeAsync() existant dans *StartPreviewAsync()* par ce qui suit:

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Consultez [Afficher l’aperçu de la caméra](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest) pour voir des rubriques plus approfondies sur l’hébergement d’un aperçu de la caméra dans votre application.