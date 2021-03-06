---
Description: Si votre compte de développeur a reçu les autorisations appropriées, vous pouvez générer et télécharger des packages de préinstallation pour permettre à un fabricant d’ordinateurs OEM d’inclure votre application dans son image de système d’exploitation.
title: Génération des packages de préinstallation pour les fabricants OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1ab17adc80a643c04ac7793945486c3ff975fde5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643134"
---
# <a name="generate-preinstall-packages-for-oems"></a>Génération des packages de préinstallation pour les fabricants OEM

Si votre compte de développeur a reçu les autorisations appropriées, vous pouvez générer et télécharger des packages de préinstallation pour permettre à un fabricant d’ordinateurs OEM d’inclure votre application dans son image de système d’exploitation. Les autorisations de préinstallation sont uniquement activées sur les comptes de développeur qui sont sponsorisés par des OEM.


## <a name="important-preinstall-policy--limitations"></a>Politique et limitations importantes en matière de préinstallation

Applications de pré-installation doivent être certifiées dans [partenaires](https://partner.microsoft.com/dashboard) pour que la licence Store dernières afin qu’ils soient en mesure de se connecter au Store et de recevoir des mises à jour de l’application.

Toute application préinstallée doit être et rester gratuite dans tous les marchés.


## <a name="generating-preinstall-packages"></a>Génération de packages de préinstallation

Une fois qu'un compte a été activé avec des autorisations de préinstallation, suivez la procédure ci-après :

1.  Dans le centre de partenaires, accédez à l’application qui doit être préinstallé.
2.  Dans le menu de navigation de gauche, développez l’option **Gestion des applications**, puis sélectionnez **Packages actuels**.
3.  Dans la section **Demander des packages pour la préinstallation du système d’exploitation**, sélectionnez **Activer les packages téléchargeables**.
4.  Dans la boîte de dialogue de confirmation, sélectionnez **Activer**.
5.  Recherchez le package à télécharger, puis sélectionnez le lien **Générer le package** approprié.

    > [!NOTE]
    > Le temps nécessaire à la génération des packages de préinstallation varie en fonction de la taille du package que vous avez sélectionné. Vous pouvez quitter cette page et y revenir par la suite, ou vous pouvez laisser la page ouverte pendant la génération de votre package.

6.  Une fois le package généré, un lien **Télécharger le package** s’affiche. Sélectionnez ce lien pour télécharger le fichier .zip.

Vous pouvez alors fournir le fichier .zip à l’OEM pour qu’il puisse l’inclure dans son image de système d’exploitation.


## <a name="support"></a>Support

Si vous avez d’autres questions sur la génération de packages de préinstallation, envoyez un e-mail <partnerops@microsoft.com>.

 

 




