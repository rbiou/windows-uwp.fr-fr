---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les capacités des différents appareils de capture vidéo. Cela inclut les tâches telles que la sélection des profils qui prennent en charge des résolutions ou de fréquences d’images spécifiques, des profils qui prennent en charge un accès simultané à plusieurs appareils photos et des profils qui prennent en charge la capture HDR.
title: Détecter et sélectionner des fonctions de l’appareil photo à l’aide de profils d’appareil photo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8fb73d917124f776fbb4ca1e47799804e4f635e2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358958"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>Détecter et sélectionner des fonctions de l’appareil photo à l’aide de profils d’appareil photo



Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les capacités des différents appareils de capture vidéo. Cela inclut les tâches telles que la sélection des profils qui prennent en charge des résolutions ou de fréquences d’images spécifiques, des profils qui prennent en charge un accès simultané à plusieurs appareils photos et des profils qui prennent en charge la capture HDR.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

 

## <a name="about-camera-profiles"></a>À propos des profils d’appareil photo

Les appareils photo offrent différentes fonctionnalités, notamment l’ensemble des résolutions de capture, la fréquence d’images pour la capture vidéo, et si les vidéos HDR ou les captures de fréquence d’images variable sont prises en charge. L’infrastructure de capture multimédia de plateforme Windows universelle (UWP) stocke cet ensemble de fonctionnalités dans une [**MediaCaptureVideoProfileMediaDescription**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription). Un profil d’appareil photo, représenté par un objet [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile), comporte trois ensembles de descriptions du média : un premier pour la capture photo, un deuxième pour la capture vidéo et un dernier pour l’aperçu vidéo.

Avant d’initialiser votre objet [MediaCapture](capture-photos-and-video-with-mediacapture.md), vous pouvez interroger les appareils de capture de l’appareil actuel pour découvrir les profils pris en charge. Lorsque vous sélectionnez un profil pris en charge, vous savez que l’appareil de capture prend en charge toutes les fonctionnalités dans les descriptions du média du profil. Cela permet de déterminer directement les combinaisons de capacités prises en charge sur un appareil particulier.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

Les exemples de code dans cet article remplacent cette initialisation minimale par la découverte de profils d’appareil photo prenant en charge différentes fonctionnalités, qui sont ensuite utilisées pour initialiser l’appareil de capture multimédia.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>Trouver un appareil vidéo prenant en charge des profils d’appareil photo

Avant de rechercher des profils d’appareil photo pris en charge, vous devez trouver un appareil de capture qui prend en charge l’utilisation de profils d’appareil photo. La méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** définie dans l’exemple ci-dessous utilise la méthode [**DeviceInformaion.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) pour récupérer la liste de tous les appareils de capture vidéo disponibles. Elle parcourt tous les appareils de la liste en appelant la méthode statique [**IsVideoProfileSupported**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported) pour chaque appareil afin de déterminer s’il prend en charge les profils vidéo. En outre, la propriété [**EnclosureLocation.Panel**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.panel) vous permet de spécifier pour chaque appareil si vous préférez que l’appareil photo se situe à l’avant ou à l’arrière.

Si un appareil prenant en charge des profils d’appareil photo est détecté dans le panneau de configuration spécifié, la valeur [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) contenant la chaîne d’ID de l’appareil, est renvoyée.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Si l’ID d’appareil renvoyé par la méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** est null ou une chaîne vide, il n’existe aucun appareil sur le panneau de configuration spécifié prenant en charge des profils d’appareil photo. Dans ce cas, vous devez initialiser votre appareil de capture multimédia sans utiliser de profils.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>Sélectionner un profil en fonction de la résolution et de la fréquence d’images prises en charge

Pour sélectionner un profil avec des fonctionnalités particulières, comme la possibilité d’atteindre une résolution et une fréquence d’images données, vous devez d’abord appeler la méthode d’assistance définie ci-dessus pour obtenir l’ID d’un appareil de capture qui prend en charge l’utilisation des profils de l’appareil photo.

Créez un objet [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings), en passant l’ID d’appareil sélectionné. Ensuite, appelez la méthode statique [**MediaCapture.FindAllVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) pour obtenir la liste de tous les profils d’appareil photo pris en charge par l’appareil.

Cet exemple utilise une méthode de requête Linq, incluse dans l’espace de noms using **System.Linq**, pour sélectionner un profil qui contient un objet [**SupportedRecordMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) dans lequel les propriétés [**Width**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height) et [**FrameRate**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) correspondent aux valeurs demandées. Si une correspondance est trouvée, [**VideoProfile**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) et [**RecordMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) du **MediaCaptureInitializationSettings** sont définies sur les valeurs de type anonyme renvoyées par la requête Linq. Si aucune correspondance n’est trouvée, le profil par défaut est utilisé.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Une fois que vous avez rempli **MediaCaptureInitializationSettings** avec le profil d’appareil photo souhaité, il vous suffit d’appeler [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) sur l’objet de capture multimédia pour le configurer sur le profil de votre choix.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="use-media-frame-source-groups-to-get-profiles"></a>Utiliser des groupes de sources d’images multimédias pour obtenir des profils

À partir de Windows 10, version 1803, vous pouvez utiliser la classe [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup) pour obtenir des profils d’appareil photo avec des fonctionnalités spécifiques avant d’initialiser l’objet **MediaCapture**. Les groupes de sources d’images permettent aux fabricants d’appareil de représenter des groupes de capteurs ou de capturer des fonctionnalités en tant que périphérique virtuel unique. Cela permet d’activer les scénarios de photographie computationnelle tels que l’utilisation conjointe d’appareils photo de profondeur et couleur, mais peut également être utilisé pour sélectionner des profils d’appareil photo pour les scénarios de capture simples. Pour plus d’informations sur l’utilisation de **MediaFrameSourceGroup**, voir [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

L’exemple de méthode ci-dessous montre comment utiliser les objets **MediaFrameSourceGroup** pour rechercher un profil d’appareil photo qui prend en charge un profil vidéo connu, par exemple un profil qui prend en charge la séquence de photos HDR ou variables. Commencez par appeler [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) pour obtenir la liste de tous les groupes de sources d’images multimédias disponibles sur l’appareil actuel. Parcourez chaque groupe de sources et appelez [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) pour obtenir la liste de tous les profils vidéo pour le groupe de sources actuel qui prend en charge le profil spécifié, dans ce cas HDR avec photo WCG. Si un profil répondant aux critères est trouvé, créez un objet **MediaCaptureInitializationSettings** et définissez le **VideoProfile** sur le profil de sélection et le **VideoDeviceId** sur la propriété **Id** du groupe de sources d’images multimédias actuel. Ainsi, par exemple, vous pouvez transmettre la valeur **KnownVideoProfile.HdrWithWcgVideo** dans cette méthode pour récupérer les paramètres de capture multimédia qui prennent en charge la vidéo HDR. Transmettez **KnownVideoProfile.VariablePhotoSequence** pour obtenir les paramètres qui prennent en charge la séquence de photos variables.

 [!code-cs[FindKnownVideoProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindKnownVideoProfile)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>Utiliser des profils connus pour rechercher un profil qui prend en charge la vidéo HDR (technique héritée)

> [!NOTE] 
> Les API décrites dans cette section sont déconseillées à compter de Windows 10, version 1803. Consultez la section précédente, **Utiliser des groupes de sources d’images multimédias pour obtenir des profils**.

La sélection d’un profil prenant en charge la vidéo HDR commence comme tous les autres scénarios. Créer un **MediaCaptureInitializationSettings** et une chaîne contenant l’ID de périphérique de capture. Ajoutez une variable booléenne qui déterminera si la vidéo HDR est prise en charge.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Utilisez la méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** définie ci-dessus pour obtenir l’ID d’un appareil de capture prenant en charge les profils d’appareil photo.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

La méthode statique [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) renvoie les profils d’appareil photo pris en charge par l’appareil spécifié classé par la valeur [**KnownVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.KnownVideoProfile) spécifiée. Pour ce scénario, la valeur **VideoRecording** est spécifiée de manière à limiter les profils d’appareil photo renvoyés à ceux prenant en charge l’enregistrement vidéo.

Parcourez la liste renvoyée des profils d’appareil photo. Pour chaque profil d’appareil photo, parcourez chaque [**VideoProfileMediaDescription**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) dans le profil pour vérifier si la propriété [**IsHdrVideoSupported**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) est définie sur true. Une fois qu’une description du média appropriée est détectée, quittez la boucle et attribuez le profil et les objets de description à l’objet **MediaCaptureInitializationSettings**.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>Déterminer si un appareil prend en charge la capture simultanée de photo et de vidéo

De nombreux appareils prennent en charge la capture simultanée de photos et de vidéos. Pour déterminer si un appareil de capture prend cette fonctionnalité en charge, appelez [**MediaCapture.FindAllVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) pour obtenir tous les profils d’appareil photo pris en charge. Utilisez une requête Linq pour rechercher un profil comportant au moins une entrée pour [**SupportedPhotoMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) et pour [**SupportedRecordMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription), ce qui indiquera que le profil prend en charge la capture simultanée.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Vous pouvez affiner la requête pour rechercher des profils qui prennent en charge des résolutions spécifiques ou autres fonctionnalités en plus de l’enregistrement vidéo simultané. Vous pouvez également utiliser le [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) et spécifier la valeur [**BalancedVideoAndPhoto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.KnownVideoProfile) pour récupérer les profils prenant en charge la capture simultanée, mais vous obtiendrez des résultats plus complets en interrogeant tous les profils.

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




