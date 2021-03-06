---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Recherchez les encodeurs et décodeurs audio et vidéo installés sur un appareil.
title: Requête pour les codecs installés
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, codec, encodeur, décodeur, requête
ms.localizationpriority: medium
ms.openlocfilehash: e447f39258a4a0439bcbd3cca7aeb4407a9b1d26
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318614"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Rechercher les codecs installés sur un appareil
La classe **[CodecQuery](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery)** vous permet de rechercher les codecs installés sur l’appareil actuel. Les codecs qui sont fournis dans Windows 10 pour les différentes familles d’appareils sont répertoriés dans l’article [Codecs pris en charge](supported-codecs.md) ; toutefois, étant donné que les utilisateurs et les applications peuvent installer des codecs supplémentaires sur un appareil, vous pouvez avoir besoin de rechercher les codecs pris en charge au moment de l’exécution afin de déterminer les codecs qui sont disponibles sur l’appareil actuel.

L’API CodecQuery est un membre de l’espace de noms **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** . Vous devrez donc inclure cet espace de noms dans votre application.

L’API CodecQuery est un membre de l’espace de noms **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** . Vous devrez donc inclure cet espace de noms dans votre application.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

Initialisez une nouvelle instance de la classe **CodecQuery** en appelant le constructeur.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

La méthode **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery.findallasync)** renvoie tous les codecs installés qui correspondent aux paramètres fournis. Ces paramètres comportent une valeur **[CodecKind](https://docs.microsoft.com/uwp/api/windows.media.core.codeckind)** indiquant si vous recherchez des codecs audio, vidéo ou les deux, une valeur **[CodecCategory](https://docs.microsoft.com/uwp/api/windows.media.core.codeccategory)** précisant si vous recherchez des encodeurs ou des décodeurs, ainsi qu’une chaîne représentant le sous-type d’encodage multimédia sur lequel porte votre recherche, tel que vidéo H.264 ou audio MP3.

Pour rechercher les codecs correspondant à tous les sous-types, spécifiez une chaîne vide ou une valeur Null pour le sous-type. L’exemple ci-après répertorie tous les encodeurs vidéo installés sur l’appareil.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

La chaîne de sous-type que vous transmettez dans **FindAllAsync** peut être une représentation de chaîne du GUID de sous-type défini par le système ou un code FOURCC relatif au sous-type. Les GUID de sous-type multimédia pris en charge sont répertoriés dans les articles [GUID de sous-type audio](https://docs.microsoft.com/windows/desktop/medfound/audio-subtype-guids) et [GUID de sous-type vidéo](https://docs.microsoft.com/windows/desktop/medfound/video-subtype-guids), mais la classe **[CodecSubtypes](https://docs.microsoft.com/uwp/api/windows.media.core.codecsubtypes)** fournit des propriétés qui renvoient les valeurs de GUID pour chaque sous-type pris en charge. Pour plus d’informations sur les codes FOURCC, consultez l’article [Codes FOURCC](https://docs.microsoft.com/windows/desktop/DirectShow/fourcc-codes). 

L’exemple ci-après indique le code FOURCC « H264 » pour déterminer si un décodeur vidéo H.264 est installé sur l’appareil. Vous pouvez exécuter cette requête avant d’essayer de lire du contenu vidéo H.264. Vous pouvez également gérer les codecs non pris en charge au moment de la lecture. Pour plus d’informations, consultez l’article [Gérer les codecs non pris en charge et les erreurs inconnues à l’ouverture des éléments multimédias](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

L’exemple ci-après cherche à déterminer si un encodeur audio FLAC est installé sur l’appareil actuel. Si tel est le cas, un objet **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** est créé pour le sous-type qui peut être utilisé pour la capture de contenu audio dans un fichier ou le transcodage du contenu audio d’un autre format vers un fichier audio FLAC.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Rubriques connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcoder des fichiers multimédias](transcode-media-files.md)
* [Codecs pris en charge](supported-codecs.md)
 

 




