---
Description: Une fois vos packages correctement chargés, vous verrez un tableau indiquant les packages offerts aux familles d’appareils Windows 10 spécifiques (et aux versions antérieures du système d’exploitation, le cas échéant), classés par ordre.
title: Disponibilité de la famille d’appareils
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp, packages, télécharger, disponibilité famille d’appareils
ms.localizationpriority: medium
ms.openlocfilehash: 088fb859ae38e608182b22b94300b9c27063054e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320019"
---
# <a name="device-family-availability"></a>Disponibilité de la famille d’appareils

Une fois vos packages correctement chargés dans la page **Packages**, la section **Disponibilité de la famille d’appareils** affiche un tableau identifiant les packages offerts aux familles d’appareils Windows 10 spécifiques (et aux versions antérieures du système d’exploitation, le cas échéant), classés par ordre. Dans cette section, vous déterminez également si vous souhaitez offrir ou non la soumission aux clients sur des familles d’appareils Windows 10 spécifiques.

> [!NOTE]
> Si vous n’avez pas encore chargé les packages, la section **Disponibilité de la famille d’appareils** affiche les familles d’appareils Windows 10 avec des cases à cocher vous permettant de configurer la soumission pour les clients sur ces familles d’appareils. Le tableau apparaît une fois que vous avez chargé un ou plusieurs packages.

Cette section inclut également une case à cocher permettant d’indiquer si vous souhaitez permettre à Microsoft de rendre l’application disponible pour les futures familles d’appareils Windows 10. Nous recommandons de conserver cette case à cocher activée pour que votre application puisse être disponible pour davantage de clients potentiels à mesure que de nouvelles familles d’appareils seront introduites.


## <a name="choosing-which-device-families-to-support"></a>Sélection des familles d’appareils prises en charge

Si vous téléchargez des packages ciblant une famille d’appareils spécifique, nous allons cocher la case pour rendre ces packages disponibles pour les nouveaux clients sur ce type d’appareil. Par exemple, si un package cible Windows.Desktop, la case **Windows 10 Desktop** sera cochée pour ce package (et vous ne pourrez pas cocher les cases pour d’autres familles d’appareils).

Les packages ciblant la famille d’appareils Windows.Universal peuvent s’exécuter sur n’importe quel appareil Windows 10 (y compris Xbox One). Par défaut, nous allons rendre ces packages disponibles pour les nouveaux clients sur tous les types d’appareils *sauf* pour Xbox.

Vous pouvez décocher la case en regard d’une famille d’appareils Windows 10 si vous ne souhaitez pas proposer la soumission aux clients sur ce type d’appareils. Si la case associée à une famille d’appareils est décochée, les nouveaux clients sur ce type d’appareils ne pourront pas acquérir l’application (bien que les clients qui possèdent déjà l’application puissent toujours l’utiliser, et bénéficient des mises à jour soumises).

Si votre application les prend en charge, nous vous recommandons de conserver toutes les cases cochées, sauf si vous avez une raison spécifique de limiter les types d’appareils Windows 10 pouvant acquérir votre application. Par exemple, si vous savez que votre application n’offre pas une bonne expérience sur [Surface Hub](https://developer.microsoft.com/windows/surfacehub) et/ou [Microsoft HoloLens](https://developer.microsoft.com/mixed-reality), vous pouvez décocher la case **Windows 10 Collaboration** et/ou **Windows 10 Holographique**. Cela empêche tout nouveau client d’acquérir l’application sur ces appareils. Si vous estimez ultérieurement être prêt à proposer l’application à ces clients, vous pouvez créer une soumission en activant ces cases à cocher.

<span id="xbox" />

La seule famille d’appareils Windows 10 qui n’est pas activée par défaut pour les packages Windows.Universal est **Xbox Windows 10**. Si votre application n’est pas un jeu (ou s’il s’agit d’un jeu et que vous avez activé le [Programme Créateurs Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) ou que vous êtes passé par le processus d’[approbation de concept](../gaming/concept-approval.md)), et que votre soumission comprend des packages UWP neutres et/ou x64 compilés à l’aide de la version 14393 du SDK Windows 10 ou une version ultérieure, vous pouvez cocher la case **Xbox Windows 10** afin de proposer l'application aux clients sur Xbox One.

> [!IMPORTANT]
> Pour que votre application se lance sur les appareils Xbox, vous devez inclure un package neutre ou x64 compilé avec la version 14393 du SDK Windows ou une version ultérieure. Toutefois, si vous cochez la case **Xbox Windows 10**, votre package présentant la version la plus élevée applicable à Xbox (un package neutre ou x64 ciblant la famille d’appareils universelle ou Xbox) sera toujours proposé aux clients sur Xbox, même s’il est compilé avec une version antérieure du SDK. Pour cette raison, il est essentiel de vous assurer que le package présentant la version la plus élevée applicable à Xbox soit compilé avec la version 14393 du SDK Windows ou une version supérieure. Si ce n’est pas le cas, vous verrez un message d’erreur indiquant que les clients Xbox ne seront pas en mesure de lancer votre application. 
> 
> Pour résoudre ce problème, procédez comme suit :
> - Remplacez les packages applicables par les instances compilées à l’aide de la version 14393 du SDK Windows ou une version ultérieure.
> - Si vous disposez déjà d’un package qui prend en charge Xbox et qui est compilé à l’aide de la version 14393 du SDK Windows ou une version ultérieure, augmentez son numéro de version, de manière à en faire le package présentant la version la plus élevée de la soumission.
> - Désélectionnez la case à cocher **Xbox Windows 10**.
>   
> SI vous ne parvenez toujours pas à résoudre le problème, contactez le support technique.

Si vous soumettez une application UWP pour Windows 10 IoT Standard, vous ne devez pas modifier les sélections par défaut après avoir chargé vos packages ; il n’existe aucune case à cocher distincte pour Windows 10 IoT. Pour plus d’informations sur la publication d’applications UWP sous loT Standard, voir [Prise en charge dans le Microsoft Store pour les applications UWP sous IoT Standard](https://docs.microsoft.com/windows/iot-core/commercialize-your-device/installingandservicing).

Si votre soumission d’une application précédemment publiée inclut des packages qui peuvent s’exécuter sur **Windows 8/8.1** et **Windows Phone 8.x et versions antérieures**, ces packages seront disponibles aux clients sur ces systèmes d’exploitation versions. Pour arrêter de proposer votre application à ces clients, supprimez les packages correspondants de votre soumission.

> [!IMPORTANT]
> Pour empêcher complètement une famille de périphériques Windows 10 spécifique à partir de l’obtention de votre soumission, mettre à jour le [ **TargetDeviceFamily** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) élément dans votre manifeste pour cibler uniquement la famille de périphériques que vous souhaitez prise en charge (par exemple, Windows.Mobile ou Windows.Desktop), plutôt que de laisser en tant que la valeur Windows.Universal (pour la famille de périphériques universelle) que Microsoft Visual Studio inclut dans le manifeste par défaut.

Il est important de savoir que les sélections que vous effectuez dans la section **Disponibilité de la famille d’appareils** s’appliquent uniquement aux nouvelles acquisitions. Quiconque disposant déjà de votre application peut continuer à l’utiliser et obtenir les mises à jour que vous soumettez, même si vous supprimez sa famille d’appareils ici. Cela s’applique même aux clients ayant acquis votre application avant la mise à niveau vers Windows 10. Par exemple, si vous avez une application publiée avec les packages de Windows Phone 8.1, et que vous ajoutez un package de Windows 10 (UWP) ciblant la famille de périphériques Windows.Universal, les clients mobiles Windows 10 qui disposaient de votre package de Windows Phone 8.1 sont proposés une mise à jour pour cette Windows Package de 10 (UWP), même si vous avez désactivé la case à **Windows 10 Mobile**.

Pour plus d’informations sur les familles d’appareils, voir [**Vue d’ensemble des familles d’appareils**](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview).


## <a name="understanding-ranking"></a>Compréhension du classement

À part ce qui vous permet d’indiquer les familles d’appareils Windows 10 peuvent télécharger vos envois, le **disponibilité famille du périphérique** section affiche les packages qui seront apportées à des familles d’appareils différents. Si vous possédez plusieurs packages pouvant s’exécuter sur une famille d’appareils spécifique, le tableau indique l’ordre de proposition des packages, en fonction de leur numéro de version. Pour plus d’informations sur la façon dont le Store classe les packages en fonction de leur numéro de version, consultez la section [Numérotation des versions de packages](package-version-numbering.md). 

Par exemple, que vous disposez de deux packages : Package_A.appxupload et Package_B.appxupload. Pour une famille d’appareils donnée, si Package_A.appxupload est classé au premier rang et que Package_B.appxupload est classé au second rang, lorsqu’un client sur ce type d’appareil acquiert votre application, le Windows Store tentera dans un premier temps d’offrir Package_A.appxupload. Si l’appareil du client n’est pas en mesure d’exécuter Package_A.appxupload, le Windows Store propose Package_B.appxupload. Si l’appareil de client ne peut pas exécuter n’importe laquelle des packages pour cette famille de périphériques (par exemple, si le **MinVersion** votre application prend en charge est supérieure à la version sur l’appareil de client), puis le client ne sera en mesure de télécharger l’application sur Cet appareil.

> [!NOTE]
> Les numéros de version dans les packages .xap (pour les applications précédemment publiées) ne sont pas considérés lors de la détermination du lot à fournir un client donné. Pour cette raison, si vous disposez de plusieurs packages de rang égal, vous verrez un astérisque en lieu et place d’un numéro ; les clients peuvent recevoir les deux packages. Pour mettre à jour les clients d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.

